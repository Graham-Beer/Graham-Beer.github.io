---
layout: post
title:  "What is my name in Binary?"
date:   2017-03-05 20:00
comments: true
description: "How to convert your name to binary code"
categories: 
    - PowerShell
tags: 
    - PowerShell
---

This idea came to me with a book i've just started reading by Charles Petzold. The book is called 'Code'. 
It was actually the front cover of the book, the word Code written and below it binary code underneath each letter. How could i convert
letters to binary ? Even better could I convert my name to Binary ?

![](/images/Blog_Pictures/Code Charles Petzold.jpg)

The first part of the script was a function that prompted for a name. This was quite neat and it was something new to me as well.
When calling the function I wanted the user to be prompted to add their names. By name I mean firstname, middlename and lastname. 
There is no enforcement to add all names, in fact you could just put your initials !

In the param, I used a parameter with 'DontShow'. This is quite cool, as when you put in the function name and try and add the -name 
parameter won't be visable, so you will assume your only option is 'Get-NameInBinary' which is what I wanted.
The parameter variable 'Name' is then set by the read-host cmdlet, which prompts for a value. (You could still do 'Get-NameInBinary Graham' 
which would bypass the read-host by setting the variable 'name').

```PowerShell
param(
        [parameter(DontShow)] # hide paramter to allow read host to promt for name(s)
        [string]$name = $(read-host -Prompt "Enter Name(s)")
    )
```

The next line just displays the name you entered.
I wanted to be able to split firstname and lastname with a space. Turns out that even binary produces code for a whitespace. 

The next part is the main body of the script. 
 The 'output' variable capture's each letter in Binary.
The 'name' variable is passed to a foreach loop. By using a for statement, it starts a counter at zero and stops at the given Length of 
characters in variable 'name'.
```PowerShell
$name | %{ for($i=0;$i -lt $name.Length;$i++) {
```
The names are split by each 'for' count to char number:
```PowerShell
[int][char]$_[$i] | %{
```
The Char integer value is converted to a base of 2. 2 being the base for binary number conversion. The 'Padleft' adds a 0 to the front 
of the number generated to create the 8 digit binary code.
```PowerShell
[System.Convert]::ToString($_,2).PadLeft(8,'0')
```

Each binary number is added to an array.
The last part of the script is to join all the binary codes together and with the 'WhiteSpace' variable set at the start of the script,
to split the names with a space (Should there be more than one name).

By using my name, the script output looks like so:
```PowerShell
Enter Name(s): Graham Beer
Your Name 'Graham Beer' in Binary is :
010001110111001001100001011010000110000101101101 01000010011001010110010101110010
```

Script is on my GitHub page should you want to find your binary name ! 

<script src="https://gist.github.com/Graham-Beer/574f1b508e313d8078248645f23e1087.js"></script>
