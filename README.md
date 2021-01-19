# Introduction

Have you ever had to parse a file line by line and do operations on it?
Don't answer that - I know you have.
awk is a wonderful little command line utility purpose-built for parsing and doing operations on a file line-by-line.
The purpose of this tutorial is to convince you that you should be reaching for awk next time you need to read and transform a file instead of writing a python or js script.

# Structure

The structure of an awk program is `pattern {action}`.
That means that for each line, awk will evaluate the pattern and then take an action.
So for instances, if you want to print the 5th column (say the URL) of a log file when the 3rd column (maybe the status code) equals 500, you might do `awk '$3 == 500 { print $5 }'`.

`BEGIN` and `END` are two special keywords that, when proceeded with an action, will take that action, before any lines are evaluated and after all lines are evaluated, respectively.

For instance:

```bash
awk 'BEGIN {print "hello"} END {print "goodbye"}' sample.txt
```

# Built-in variables

In the above example, notice how I was able to just print the 5th column by writing `print $5`.
`$5` refers to the 5th field, or column, in a line given a separator.
The default separator is whitespace.
You can refer to any column with `$n` where `n` is the 1-indexed column number.
`$0` refers to the entire line.

awk also includes a number of built-in variables:

- NR - This variable keeps track of the number of lines seen.
  For example, at the 5th line, `print NR` would print 5.
  mnemonic: Number of Records.

- NF - Number of fields in the current record.
  mnemonic: Number of Fields

- FS - The field separator being used for this script.
  By default, it'll be whitespace.
  mnemonic: Field Separator

There are other special variables, but these give you the 80/20 you need to be effective with awk.

## Examples

You could print the number of fields in each line like so.

```bash
awk '{print NF}' sample.txt
```

And to print the number of lines a file:

```bash
awk 'END {print NR}' sample.txt
```

# Conditionals

## No Conditionals

The simplest conditional is no conditional.
For instance if you want to just print the 1st field of every line, do the following.

```bash
awk '{ print $1 }' sample.txt
```

## ==, >, <

As mentioned above, you can print a line or a field if some field is equal to some value.

```bash
awk '$2 == "1" { print $1}' sample.txt
```

Or maybe you want to print lines whose length is greater than some value.

```bash
awk 'length($0) > 9' sample.txt
```

Notice how I didn't include an action in that last command?
The default behavior when no action is specified is to just print the whole line.

## Regular Expressions

A common task you might have is to print some lines of text where some field matches a regular expression.
With awk, you can test a string against a regex with the `~` operator.
For instance, if I want to print the line if the first field contains an `a`, I'd do the following.

```bash
awk '$1 ~ /a/' sample.txt
```

To print the lines where the first field starts with `a`, I'd do the following.

```bash
awk '$1 ~ /^a/' sample.txt
```

And finally, if I wanted to match against an entire line, I could do:

```bash
awk '/9/' sample.txt
```

# User Variables

## Aggregations

Let's say you wanted to sum the values of a column in a file.
With awk, that's easy peasy.

```bash
awk '{ sum += $4 } END { print sum }' sample.txt
```

## Input Variables

You can define variables that can be used throughout the program using the `-v` flag.
For instance, if you wanted to re-use some string, you could do the following.

# References

I used a few resources for this tutorial.

- https://www.grymoire.com/Unix/Awk.html
- https://www.gnu.org/software/gawk/manual/gawk.html
- https://gist.github.com/0/5486125
- https://www.geeksforgeeks.org/awk-command-unixlinux-examples/
