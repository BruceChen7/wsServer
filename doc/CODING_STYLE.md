# wsServer Coding Style

wsServer is a simple project, and therefore I don't believe there is so much
interest in it. However, as I have received a few contributions over the years,
it may be interesting to write something about it.

(much of what is here is (heavily-)based on
[Nanvix](https://github.com/nanvix/documentation/blob/master/6-contrib/3-coding-style.md)'s
and
[Linux](https://www.kernel.org/doc/html/v4.10/process/coding-style.html)'s
coding style.)

## Indentation
- _Always_ use **tab** characters for indentation.
- Tab width: 4 characters.
- Indent case labels in switch statements, such as:
```c
switch (var)
{
	case bar:
		something;
		break;
	case beef:
		something
		break;
	default:
		break;
}
```
- Do not put multiples statements nor multiples assignments in a single line.

## Long lines length
- Even though Mr. Torvalds has [said](https://www.theregister.com/2020/06/01/linux_5_7/)
that nowadays it is ok to use large lines, I'll keep to 80 columns with five (85)
of tolerance. However, use common sense here.

- While breaking long lines, indent the next line at one level.

## Placing Braces and Spaces
- Allman style: whether functions or control statements, place the braces always
in the next line. Statements within the braces are indented at one level.

Ex:
```c
void foo(void)
{
	if (something)
	{
		bar();
		baz();
	}
	else
		deadbeef();

	something_else(); /* put always a blank line between a control block
				without braces and the next statement. */
}
```
- If one line is enough (like function call or variable assignment), do not put
braces. Keep in mind the readability: put a blank line between statements.

## Spaces
- Use one space after these keywords: `if, switch, case, for, do, while`.
- Use one space around (on each side of) most binary and ternary operators:
`= + - < > * / % | & ^ <= >= == != ? :`.
- But no space after unary operators: `& * + - ~ ! sizeof`.
- No space after/before the postfix/prefix increment & decrement unary operators:
`++ --`.

## Variables and Naming
- Do not use mixed-cases to name your variables. Always use lower-case for
variables and functions name. Underscores are acceptable to give a more descriptive
name, e.g., `int next_frame(...)`.
- Always use uppercase for macros.
- Do not use _typedef_ s unless for totally opaque objects. The reasoning is
simple: it _hides_ the original meaning of the variable. Is it a union, struct,
an integer?.
- (Try to) do not mix variable declaration with your code. Declare your variables
at the very beginning of the scope of that variable.

## Commenting/Documenting
- Use always `/* c89 style. */` in your code, even for a single line.
- Use Doxygen syntax to document functions and structures, even for the internal
API.

---

## Using clang-format
`clang-format` is a command-line tool, part of the LLVM project that, for a
pre-defined set of rules or a given file, automatically formats source code.
wsServer comes with a .clang-format file in the root directory that tries
to embrace most of these rules above.

Please note that the .clang-format file does not fully cover everything described
here and even in cases that do, it is always important to carefully evaluate the
output generated by it, since sometimes it may be far from ideal, use common
sense here.

My suggestion would be something like:
- Commit what you need to commit (**locally**).
- Apply clang-format over the file: `clang-format -style=file -i src/file.c`.
- Manually check changes on the file.
- Make a commit amend if applicable.

Similarly, instead of performing an amend, you can also generate a newly formatted
file and then perform a diff of the original with the new one:
```bash
clang-format -style=file file.c > newfile.c
diff -u file.c newfile.c > diffs.patch        # see this file
```
Pick what you think is best.
