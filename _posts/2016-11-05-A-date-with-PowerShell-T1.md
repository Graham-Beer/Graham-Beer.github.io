---
layout: post
title:  "A date with PowerShell"
date:   2016-11-05 19:00
comments: true
description: "A date with PowerShell"
categories: 
    - PowerShell
tags: 
    - PowerShell
---

Also seen on PowerShell.org !

At the beginning of July, we welcomed our 3rd son into the world. As days past my wife and I would say, "wow, he's 11 days old. Can you believe it?!". I'm sure parents out there are relating to this!
This gave me an idea for a fun script that would get your age in years, months and days, tell you how many days until your birthday and your star sign.
I wanted date of birth passed to the function as 'dd/MM/yy'. To keep to this format, I’m using the 'ValidatePattern' Advanced Parameter with a Regular Expression (Regex). The regular expression, "^(0[1-9]|[12]\d|3[01])/(0[1-9]|1[0-2])/(\d{2})$", will only allow a date in the format of 01/01/16, for example. 

Briefly, here is regex syntax I used in some of the expression:

^ Start of string
( .. ) Capturing group
(0[1-9] Match two digits that make up the day. This accepts numbers from 01 to 09
| Acts like a Boolean OR.
/d match any digital character
[12] match any character in the set
/ used to divide the date numbers
{2} Exactly two times
$ End of string

Now that my function parameter variable $Bday has a date, its passed to get-date to be converted from a string to a date. The date in variable $cDate will look like this, '01 January 2016 00:00:00'. The next line in the code will use todays date and subtract the date passed in $cDate variable. The $diff variable will contain the following data which we will use to get our age in years, months and days:

Days : 212
Hours : 12
Minutes : 40
Seconds : 20
Milliseconds : 533
Ticks : 183624205335135
TotalDays : 212.528015434184
TotalHours : 5100.67237042042
TotalMinutes : 306040.342225225
TotalSeconds : 18362420.5335135
TotalMilliseconds : 18362420533.5135

I've contained this first part in our Begin block. The Process block does the main code. 
Now I need to get my age in Years, Months and Days. This is where the [math] data type is used. I'm using the 'Truncate' property as 
I don't want to do anything fancy like round up my numbers. Adding the .typename of Days to my $diff variable and dividing by $daysInYear variable I can get my age in years.
The next two, months and days required a tweak to the algorithm. 
I ended up using a maths term called a 'Mod'. Now I’m not talking about youth culture and style in the sixties (Mods and rockers 
anyone ??), but the Modulus Math Operator. Basically the Modulus Operator returns the remainder when the first number is divided
by the second. So for example:

1 mod 3 = 1 (or 1 % 3 = 1)
2 mod 3 = 2
3 mod 3 = 0
4 mod 3 = 1

The operator sign used is % for Modulus. Not to be confused for the alias of foreach in PowerShell. For days in a month, I used the average of 30.
I thought it would be fun to add the star sign as well. I was after something that could tell me, "is this date in this date range?". One of the properties of 'get-date' is DayOfYear.

Finding if a number is in a range is pretty straight forward, For example:

```PowerShell
 5 -in 1..10 
 ```
 
 Which gives a Boolean result.
Now if I convert my date ranges into days of the year then I can match the day of the year I was born against the ranges of days for star signs. I've used a switch statement to check against multiple conditions. Within a scriptblock I’ve asked if the value
I’m passing is 'in' the array of dates for each star sign. The match will return the star sign and is held in the $starSign variable.
The Final part of the process block is to work out how many days until your next birthday. By capturing the current date, formatting 
the date of birth by removing the year born, adding the current year and finally subtract the amended date of birth against the current date. Phew!
This will leave a number of days until your next birthday. The 'if' statement is added if your birthday has already happened at the 
time of the code, it simply reverses the sum to give a positive number.
The end block displays the three captured results to the host. 
I hope you have enjoyed this post and can see the many options possible for dates in PowerShell.
Feel free to download the script from my GitHub
[](https://github.com/Gbeer7/Get-Age.git)

<script src="https://gist.github.com/Graham-Beer/96089f8273f4b0facc9f4fe669c12c78.js"></script>
