Provided Files : broken_compiler.exe, program.wut, expected_output.txt

to dos:
1. note the current output when the program.wut is run using current compiler
2. fix/rewrite the compiler so that running it produces excepted_output.txt
3. write a program in wut to showcase your understanding of it. to get points for this section, your program must atleast be printing a string like your team's name. additional functionality (loops and complex arithmetic) showcasing understanding of language will secure you more points.

deliverables:
1. fixed compiler 
2. your new program
3. thorough documentation of process

---

## ✅ Our Solution — Team **Circuits Labs**

**Bugs Found:**
1. `^` (print operator) incorrectly **popped** the top of the stack after printing, corrupting subsequent operations.
2. `$` (swap/dup operator) **duplicated** the top value instead of properly swapping the top two values.

**Deliverables:**
- `compiler.py` — Fixed, fully working compiler for `.wut` programs
- `circuits_labs.wut` — Custom program showcasing loops, arithmetic, and printing team name "Circuits Labs"
- `WRITEUP.md` — Full documentation: bug analysis, fix explanation, and verification

**How to run:**
```bash
python compiler.py circuits_labs.wut
```
