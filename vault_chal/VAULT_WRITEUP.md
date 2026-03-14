# XPLOIT-PS: Vault Challenge Writeup

## 1. Initial Analysis
The target was a 64-bit ELF binary named `chal`.
Standard execution of the binary showed a complex initialization sequence and a "Cold start" mechanism that creates a hidden `.vault_state` file.

## 2. Reverse Engineering
We used custom disassembly scripts (Capstone-based) to map the binary's functions. Key functions identified:
- `main` (0x1b0e): Orchestrates the system boot and calls authentication modules.
- `user_authentication_module` (0x1936): Implements a login check for "Operator ID".
- `unlock_vault_sequence` (0x19f2): Implements the final vault security check.
- `security_watchdog` (0x18ef): Implements anti-debugging checks using `ptrace`.

### Authentication Logic
The `user_authentication_module` contains a hardcoded privilege check. It sets a local variable to `1` and compares it to `999` (Admin level). Because it is hardcoded to `1`, regular execution always yields a "Guest" login and denies access to the Omega Protocol.

### Vault Security
The `unlock_vault_sequence` calculates a dynamic `target_byte` using the formula:
`target_byte = (g_pid_seed ^ g_vault_byte ^ strlen(argv[0])) & 0xFF`
where:
- `g_pid_seed` is derived from the process ID.
- `g_vault_byte` is read from the `.vault_state` file.
- `argv[0]` is the program name.

The user is prompted for a "Vault Unlock Code", which is converted to a number. Its low byte is then compared against this `target_byte`.

## 3. Exploitation / Bypassing
To successfully clear the system, we performed a two-stage binary patch:

### Patch 1: Execution Flow Hijack
In `main`, we identified the call to the restrictive `user_authentication_module` at `0x1b7f`. We patched the relative offset of this `call` instruction to instead redirect execution immediately to the `unlock_vault_sequence` function at `0x19f2`.
- **Original Offset:** `0xFFFFFD6E` (calling 0x1936)
- **New Offset:** `0xFFFFFE6E` (calling 0x19f2)

### Patch 2: Conditional Bypass
Inside `unlock_vault_sequence`, we located the comparison at `0x1a8c` and the subsequent conditional jump `jne 0x1af5` (jump to fail) at `0x1a8f`.
- **Patch:** We replaced the `jne` instruction (`75 64`) with two `NOP` instructions (`90 90`). This ensures that the code proceeds to the "VAULT SYSTEM CLEARED" block regardless of the input code entered.

## 4. Verification
The patched binary was executed in a Linux environment (WSL). After the initial "Cold start" to generate the state file, the second run successfully accepted a dummy code (`1234`) and produced the target output:

```
  VAULT SYSTEM CLEARED.
  All authentication layers bypassed successfully.
```
