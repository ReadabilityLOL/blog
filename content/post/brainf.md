+++
title = "Let's make a Brainf*** interpreter"
date = 2026-02-16T20:08:00-05:00
draft = false
tags = ["development", "tutorial"]
+++

## What is Brainf***?

Brainf*** is a esoteric, Turing complete programming language involving only 8 commands and two lists. Because it is Turing complete, it is theoretically possible to compute any task doable by a computer in Brainf***, though it is significantly harder to do tasks here than in say, Python or C. It was initially designed with the goal of making the smallest possible compiler, which should make it a pretty simple project to implement.

## Begin setup

As with my last project, we'll be doing this in Python, because I don't feel like trying anything else right now.

The language consists of two lists, an instruction list, and a data list. Each gets a pointer, which tells it what index it is on. Let's start by making these lists. We'll make the instructions list a string, since a string is just a list of characters anyway. We'll also make the data list an array of 30,000 values, though theoretically it should be infinite.

```python
instructions = ""
data = [0]*30000
```

That's basically all of the setup.

## Basically all of the instructions
We can easily replicate movement, (the "<" and ">" instructions) by adding and subtracting from the instructionPointer variable. 

Incrementing and decrementing (the "+" and "-" instructions) can be done in the same way, by adding and subtracting from the element we are currently on in the data list. Since typical Brainf implementations wrap around, meaning that -1 becomes 255, and 256 becomes 0, we'll make sure to take the modulus of the value after addition or subtraction.

Displaying output is also really easy to implement because of the inbuilt `print` function. We also need to run the `chr` function, which converts a number to Unicode, just like what the "." function is supposed to do.

Taking input (the "," instruction) is also quite simple. For this one, we can use the `input` function, and just grab the first character. It's not perfect, but it works. We'll convert this character into an integer using the `ord` function.

We'll add the "[" and "]" instructions later, because they're a bit more complicated.

## Interpreting code

Since every command in Brainf is one character, we don't have to do any special parsing.

We just need to loop through every character in the instruction list and do a different action based on the value. By the way, did you know that Python has switch statements now? Since we're doing all this in a function, move instructionPointer and datapointer inside the parse function. Since anything that's not one of these commands is considered a comment, we can just add a final case that does nothing.

```python
def parse():
    instructionPointer = 0
    datapointer = 0
    for x in range(len(instructions)):
        match instructions[x]:
            case "+":
                data[datapointer] += 1
                data[datapointer] %= 256

            case "-":
                data[datapointer] -= 1
                data[datapointer] %= 256

            case "<":
                datapointer -= 1

            case ">":
                datapointer += 1

            case ".":
                print(chr(data[datapointer]),end="")

            case ",":
                data[datapointer] = ord(input()[0])
                x += 1

            case _:
                pass
```

Testing it out, here it has an input statement at the end that I typed the letter h into

```python
parse()
print(data[:100])
```
```bash
h
h [8, 104, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

## Loops

The last two instructions are a bit more complicated, since they actually involve changing control flow. Brainf *does* match brackets, so we'll need to add two counters that state how many left and right brackets deep we are.

The way the "[" instruction works is this: If the number inside is 0, it jumps to right after its matching "]", and if it isn't, it just continues on. The "]" instruction works similarly, except that it proceeds on if the number inside is 0, and jumps back if the number is nonzero.

```python
case "[":
    if data[datapointer] == 0:

        foundRBrackets = 0
        foundLBrackets = 1

        while foundLBrackets > foundRBrackets:
            instructionPointer += 1

            if instructions[instructionPointer] == "]":
                foundRBrackets += 1

            elif instructions[instructionPointer] == "[":
                foundLBrackets += 1

case "]":
    if data[datapointer] != 0:

        foundLBrackets = 0
        foundRBrackets = 1

        while foundLBrackets < foundRBrackets:
            instructionPointer -= 1

            if instructions[instructionPointer] == "]":
                foundRBrackets += 1

            elif instructions[instructionPointer] == "[":
                foundLBrackets += 1
```

Now, to test it out with the classic "Hello World!"

```python
instructions = "++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++."

parse()
```

```bash
Hello World!
```

## Final Steps

First off, let's make the function a bit more pure by having it accept instructions as an argument.

```python
def parse(instructions):
...

commands = "++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++."
parse(commands)
```

```bash
Hello World!
```

Now, why not make a little Brainf repl?

```python
While True:
    parse(input("repl> "))
    print()
    data = [0] * 30000
```
```bash
repl> ++++++++[>++++[>++>+++>+++>+<<<<-]>+>+>->>+[<]<-]>>.>---.+++++++..+++.>>.<-.<.+++.------.--------.>>+.>++.
Hello World!

repl> ^C
```

We could add a way to gracefully quit, but I think adding extra helpful features is antithetical to Brainf's purpose (also I'm feeling a bit lazy).

Let's modify it a bit so you can run your own Brainf code from a file:

First, at the top: 

```python
import sys
```
Then, modify the code making our repl.

```python
if len(sys.argv) > 1:
    for x in sys.argv[1:]:
        with open(x, "r") as file:
            parse(file.read())
        data = [0] * 30000

else:
    while True:
        parse(input("repl> "))
        print()
        data = [0] * 30000
```

Let's stick the "Hello World!" code in hello.bf, and try running it.

```bash
python brainf.py hello.bf
```
```bash
❯ python brainf.py hello.bf

Hello World!
```

## All done?

Thanks for reading this tutorial. If I've made a mistake, or if you just want to say something, feel free to contact me.
