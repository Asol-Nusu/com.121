Safety Management, Practice #1
==============================

In this course, we study the influence of the C and C++ programming languages on
the modern world. The reason is that C and C++ were and still are the
irreplaceable technologies that are used to build fundamental software such as
Operating System kernels, hardware drivers, and high-performance software such
as browsers that we use every day. In the beginning, we study the fundamental
structured and object-oriented programming elements of the languages. Next, we
explore how those languages' design decisions influence the world from the point
of safety and what extra cost people have to bear for some of the unique
features.

## Developer Tools

The following tool is required to be installed to work on this course. It
contains the Git version control system that you will use to submit your work to
the instructor. The Git installation package also includes a virtual terminal
Mintty, the command interpreter Bash, and an SSH client to connect and work
with the course server.

* [Git SCM](https://git-scm.com)

On macOS, it is recommended to install the official command-line developer
tools from Apple by opening the `Terminal.app` and running the following
command.

```bash
xcode-select --install
```

The following code editor is optional. It is not required, but it can be helpful
if you will not feel comfortable working with the command-line interface for
long periods. You have to install the 'Remote - SSH' extension from the editor
to use it with our remote server.

* [Microsoft Visual Studio Code](https://code.visualstudio.com)

This virtualization software and the OS images will allow you to set up your
development environment on your computer to not depend on the course server. It
is optional to set it up. You are on your own here as we will not help you
figure out what to do to get your environment up and running. Internet is full
of information about the process, though.

* [Oracle VM VirtualBox](https://www.virtualbox.org)
* [Ubuntu 20.04 Desktop](https://ubuntu.com/#download) with GUI or the
  [server](https://ubuntu.com/download/server) version without it

## Development Environment

To setup, a development environment, open the terminal (for example, through
`git-bash` on Windows) and run the Secure Copy (`scp`) program to download
credentials to your machine to login to the course server. Your credentials will
include public and private RSA cryptographic keys with a configuration file to
point to our server with your login information. To download the keys, you will
have to agree with any prompts by typing `yes` and pressing enter. You will also
have to type the password when prompted. The password will be given to you
during the class. The passwords will be disabled two weeks after the start of
the course. Ensure to get your keys before that. You will not be able to see
your password when you type it. It is normal not to leak it to your command-line
history file or show it to people nearby. Continue typing the password. If the
system does not accept your password multiple times, type it into a text editor,
cut and paste it into the terminal window. Remember that `CTRL+C` and `CTRL+V`
key combinations do not work in most Unix terminals. You will have to use other
key combinations that are different for different terminals. Mintty on Windows
uses `CTRL+INSERT` and `SHIFT+INSERT` by default. macOS terminal users are
luckier as they can use the standard `COMMAND+C` and `COMMAND+V` key
combinations.

```bash
# WARNING: If you have existing .ssh keys or configuration files, make a backup of them first.
scp -r <AUCA login>@auca.space:~/.ssh ~/
```

Do not forget to replace `<AUCA login>`, including the `<` and `>` character,
with your actual AUCA login. For example, `toksaitov_d` would be the text the
instructor of this course would write to download the keys to access the server.

From now on, you can access the server with all the necessary tools installed by
issuing the following command in your terminal.

```bash
ssh auca
```

To terminate the connection from the remote server, you can issue the `exit`
command.

It is a good idea to create a separate directory for the course work on the
server and give it a meaningful name (e.g., `com-121`). You should probably
keep different labs (experiments) in separate directories, too. Name them
appropriately. Use the commands and programs such as `cd`, `mkdir`, `mv`,
`touch`, `nano`, `cat`, `man`, and `ls`. 

## Problem #1: "Hello, World"

Write a C program that outputs the "hello, world" message to the screen.
Compile the source code with the GNU C Compiler (GCC). Ensure that you have no
compilation errors.

```bash
gcc -o 01 01.c
```

Run the program.

```bash
./01
hello, world
```

Entities that start with the `#` symbol in C sources are preprocessor
directives. The preprocessor scans for such entities and performs simple text
replacement or insertions operations to generate the final C source code ready
for compilation for the target platform. We can look at what the preprocessor
will generate for our program by forcing GCC to run the preprocessor on our
sources and nothing else.

```bash
gcc -E -o 01.i 01.c
```

Open the `01.i` file in any editor that your instructor is using. Study the
file. Compile the file without the preprocessing step again.

```bash
gcc -o 01 01.i
```

Ensure that the program still produces the same result.

```bash
./01
hello, world
```

Now, force the compiler to generate the assembly source file from your C source
code.

```bash
gcc -S -o 01.s 01.c
```

Open the `01.s` file in any editor that your instructor is using. Study the
file. Translate the assembly file into the machine code. Ensure that the
generated program still works.

```bash
gcc -o 01 01.s
./01
hello, world
```

Use the optimization facilities of your compiler to generate a more efficient
(performant) assembly code. You can try any flags such as `-O1`, `-O2`, `-O3`,
`-Os`, or `-Ofast`. The flags such as `-O1-3` enable a different number of
optimization techniques. The `-Os` flag tells GCC to make the program binary
as small as possible. The `-Ofast` is `-O3` with some extra optimizations that
may lead to fast but not accurate math calculations with floating-point numbers.
GCC has more flags to enable specific optimizations. Most of them are new and
experimental or may not work well for all programs.

```bash
gcc -O3 -S -o 01.O3.s 01.c
gcc -o 01 01.O3.s
./01
hello, world
```

Compare the optimized and unoptimized assembly files with any `diff` program
such as `vimdiff`. Note how we have fewer instructions in the optimized source
file. Press `ESC`, then type `:q!` to exit from the editor `vim`.

```
vimdiff 01.s 01.O3.s
```

Finally, let's take a look at machine instructions that the CPU uses to run the
program. Run a disassembly program `objdump` on your compiled binary. It will
try to reconstruct the assembly instructions from the machine code and print
the machine code alongside the assembly. Send (pipe) the output of `objdump`
into the pager `less` to make it possible to scroll through and search for the
main entry point with the `/main` command in the terminal.

```
objdump -D 01 | less
```

Compare the machine code to the assembly code. Press `q` to exit from `less`.

Reflect on the process of generating an executable file for C. Compare the
process to that of generating a Java class file. What is the difference? Try to
answer what runs the machine code generated by the C compiler. What runs the
compiled Java bytecode?

With all that, consider the following hypothetical situation. What if all
high-performance programs in the world written in C would be rewritten into
Java. Theorize, would all those programs consume much more or less electricity?

## Problem #2: "A Message in a Rectangle"

Create a program that prints the "hello, world" greeting surrounded by the
asterisk symbols. To print a line of asterisks, write a separate function
called `void print_decor_line(void)`. Note the naming compared to Java.

Perform several experiments. Every time you have to recompile the program, try
running it if the compilation was successful.

### Experiments

1. Try putting the function before and after `main(...)`. You will find that the
   order of function definition in C matters, compared to Java.
2. Try putting the function into a separate header file, `utilities.h`. Include
   the file before main once. Add library includes into `utilities.h` if
   necessary.
3. Include the `utilities.h` file before main twice. You will see that without
   the *Include Guard* in the header files, you will not compile the program.
   Fix it by adding the [Include Guard](https://en.wikipedia.org/wiki/Include_guard)
   into the header file.

For large source files, the compilation can take a lot of time. For example, the
Linux kernel takes on average one hour to be recompiled from scratch on a modern
personal computer. In large projects, to allow developers to recompile only those
modified files, programs in C are split into multiple `.c` files, preprocessed
and compiled separately, and then combined with a linker program. Even for your
current application that consists of only one file, the linker is called to
combine your object file with the system libraries' machine code and the C
runtime (that prepares your program's environment to run).

Even though it is not necessary for such a small program to do it, let us split
our program into two `.c` files, where the main file references entities from
another through the `.h` header. Create a `utilities.c` file, and move the
`print_decor_line` function to it. Leave the functions declaration in the header
file without the body. Do not forget to include the header file in the newly
created `.c` file. Compile the files separately. Finally, link (combine) them
together. Use the `-c` flag to tell the compiler to generate an object file
without performing any linking for the first two steps.

```bash
gcc -c -o 02.o 02.c # Step 1: compiling into an object file
gcc -c -o utilities.o utilities.c # Step 2: compiling into an object file
gcc -o 02 02.o utilities.o # Step 3: linking object files, system libraries, and the C runtime to build a final executable
./02
**************
|hello, world|
**************
```

Try to change the function in the `utilities.c` file to generate. Figure out
what steps outlined in the comments above (`Step 1:...`, `Step 2:...`, `Step 3...`)
do you only have to repeat to rebuild your program?

```bash
# TODO: figure out which steps above are only required to be repeated to recompile the program
./02
--------------
|hello, world|
--------------
```

It is great that we can only recompile parts that were changed, but what if we
don't want to track manually which files have changed since the last full
rebuild? What if we don't want to run the compiler and linker manually multiple
times? We can delegate this task to a build system. A build system is a program
that tracks modification dates of the files and runs the commands to rebuild the
files (artifacts), such as object files and executables. Build systems usually
require a configuration file, where you specify the build rules for all the
artifacts of your system. There are many build systems. The most popular
software products for the C and C++ programming languages are Make and CMake.
We will use Make on this course.

Create a file named `Makefile` with the following content

```make
CC=gcc
CFLAGS=-O3
LDLIBS=

02: 02.o utilities.o

02.o: 02.c

utilities.o: utilities.c

.PHONY: clean
clean:
    rm -rf 02.o utilities.o 02
```

Ensure that you have a `<TAB>` character befor `rm`.

The `CC` variable allows you to specify a C compiler that you want to use. You
can try replacing `gcc` with `clang`. Clang is another popular C compiler that
we have installed on our server.

The `CFLAGS` variable allows you to specify compiler flags. We have provided the
flag to turn optimizations on as an example. You can add more compiler flags here
by separating them with a whitespace character.

In some of our programs, we will have to tell the compiler to use additional
system libraries that are not linked by default. For example, to use the `Math`
library on our OS in C, we have to add a `-lm` flag to the `LDLIBS` library.

Next, we have several targets, such as `02`, `02.o`, `utilities.o`, and `clean`.
For many targets, we have prerequisites specified after the colon. Prerequisites
tell Make to track the file's modification date with the specified name with the
target's modification date and rerun the receipt if the dates are considerably
different. The targets `02`, `02.o`, and `utilities.o` have implied receipts
(you don't see them). Based on the prerequisites' extension, Make will figure
out how to properly start the compiler to build the implied receipts' target.
The `clean` rule has a receipt specified on the next line after the `<TAB>`
character. The `rm -rf 02.o utilities.o 02` instruction is executed by Make in
the command interpreter. The command interpreter runs the `rm` program to remove
all the generated files. `clean` is a utility target useful to clean up the
directory from all the generated files.

You can now rebuild the program by only compiling the modified files with only
one command by using Make.

```bash
make
```

You can also remove all the generated artifacts with just the following

```bash
make clean
```

Compile the project with Make. Note how many times the compiler front-end was
called. Try modifying the `print_decor_line` back to print stars. Recompile the
program. How many times was the compiler called this time?

Recall what steps do you need to do to compile a Java program. How do you think,
do we need a build system to compile large Java apps?

## GitHub Checkpoint #1

For the first GitHub Checkpoint, you need to prepare, commit, and push Problem
1 for Lab 1 to your private course repository on GitHub. You have to get the
repository from the instructor if you don't have one. Submit the last commit ID
without any extra characters here on Canvas, pointing to the snapshot where all
the code is ready. You may make new commits and resubmit before the deadline
multiple times.

You must create a `Readme.md` file in the root of your course folder. The Readme
file should contain the following text for now.

```
# COM-121 Private Course Directory

Here you can find all the works of FULLNAME for the COM-121 course.
```

Replace `FULLNAME` with your real name.

You must also record your work with the server for the first lab problem. You can
use Zoom to record the sessions to the disk. The recording must be concise and
must have your commentaries explaining the most important steps of the process.
Do the operations that the instructor was doing. You may pretend that you are an
instructor now, and you are trying to create a series of educational videos. You
have to upload them to the AUCA Google Drive folder (create one, name it appropriately) and share
them with the instructor (you can find his E-mail in Syllabus). You must not
remove the videos until the end of the course. If it is not possible to download
or watch the video, you will get zero for the work. Recording URLs (and nothing
else) must be stored in `rec.txt` files for every problem. [Here](https://drive.google.com/file/d/1Q_jFnOCQbJGYS1Ky8rfQ-F389PVioOYV)
you can find the video that shows how to record, upload, share, and get the URL
to write to `rec.txt`.

Here is the directory structure with the names of the files that you must use.

```
<Your private GitHub repository>
├── lab-01
│   ├── problem-01
│   │   ├── 01.c
│   │   ├── 01.i
│   │   ├── 01.s
│   │   ├── 01.O3.s
│   │   └── rec.txt
└── Readme.md
```

Here you can find the commands that will be used to compile your code.

| Problem       | Compilation Command |
| :------------ | :------------------ |
| p01: 01.c     | gcc -o 01 01.c      |

Files `01.i`, `01.O3.s`, and `01.s` from `problem-01` may be checked manually.
Ensure that you have them in the repository. `01.i` should include some
preprocessing output generated from `01.c`. `*.s` files must include x86-64
assembly generated with and without optimizations.

Ensure not to submit any binary files (object files and executables). Your grade
will be lowered for that. You will get zero for a late submission. You will get
zero if the auto-grading script cannot parse your commit, clone your repository,
check out the commit, find your source files under the specific names the
instructor was using during the class, build the sources, run your programs. You
will also get zero if your programs' output format is not the same as that
outlined in the samples.

### Documentation

    man make
    man gcc
    man as
    man gdb
    man objdump

### Links

#### C

* [Beej's Guide to C Programming](https://beej.us/guide/bgc)

#### x86 ISA

* [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://software.intel.com/en-us/articles/intel-sdm)
* [X86 Opcode Reference](http://ref.x86asm.net/index.html)
* [X86 Instruction Reference](http://www.felixcloutier.com/x86)

#### Tools

* [GAS Syntax](https://en.wikibooks.org/wiki/X86_Assembly/GAS_Syntax)
* [GDB Quick Reference](https://users.ece.utexas.edu/~adnan/gdb-refcard.pdf)
* [Pro Git](https://git-scm.com/book/en/v2)

### Books

#### C

* C Programming: A Modern Approach, 2nd Edition by K. N. King
