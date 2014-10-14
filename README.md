ECE382_Lab3
===========

##Lab3

####Objective

Learn how to use the logic analyzers and Nokia displays in conjunction with the MSP430s.  Draw a 8 by 8 pixel square on the Nokia 1202 display everytime that SW3 is pressed and released.

####MEGA PRELAB

See attachment in my lab 3 files.


####Lab 3 Questions

I was tasked by the lab with filling in the following table.  This is what I had to do: "When SW3 is detected as being pressed and released (lines 56-62), the MSP430 generates 4 packets of data that are sent to the Nokia 1202 display, causing a vertical bar to be drawn. Complete the following table by finding the 4 calls to writeNokiaByte that generate these packets. In addition, scan the nearby code to determine the parameters being passed into this subroutine. Finally, write a brief description of what is trying to be accomplished by each call to writeNokiaByte."
Here is that table:

|__Line__|__R12__|__R13__|__Purpose__|
|:-----|:-----|:-----|:-----|
|66|1h|E7h|Draws a vertical bar with 2 spaces in the middle|
|276|0h|B1h|mask out any weird upper nibble bits and mask in "B0" as the prefix for a page address|
|288|0h|10h|mask out upper nibble, 10 is the prefix for a upper column address|
|294|0h|0h|Write a command, setup call to make a copy of the top of the stack|

I was also tasked with filling in this table.  This is what I had to do: "Configure the logic analyzer to capture the waveform generated when the SW3 button is pressed and released. Decode the data bits of each 9-bit waveform by separating out the MSB, which indicates command or data. Explain how the packet contents correspond to what was drawn on the display. Be specific with the relationship between the data values and what and where the pixels are drawn"
Here is that table:

|__Line__|__Command/Data__|__8-Bit Packet__|
|:-----|:-----|:-----|
|66|write/E7h|11100111|
|276|read/B1h|10110001|
|288|read/10h|00010000|
|294|read/0h|00000000|

Finally, I had to go into paint and fill in the following picture to show different writing modes:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/ECE382_Lab3/master/paint.PNG "Paint")

####Logic Analyzer

Here is the logic analyzer after running the code:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/ECE382_Lab3/master/logic1.jpg "Logic 1")

There was a time gap of 1.65 micro seconds.

Here is the logic analyzer after running the reset:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/ECE382_Lab3/master/logic2.jpg "Logic 2")

There was a time gap of 1.94 micro seconds.

####Required Functionaly Code

Here is my code I wrote to get required functionality for the lab.  It is a whopping 7 lines long.

```
;------------------------------------------------------------------------------------------------
; This is the code I wrote for my required functionality. I drew an 8 pixel high line
; and then drew it 8 times
;------------------------------------------------------------------------------------------------
mov	#NOKIA_DATA, R12	; For testing just draw an 8 pixel high
mov	#0xFF, R13
mov	#0x08, R5	; beam with a 2 pixel hole in the center
block	call	#writeNokiaByte
dec	R5
cmp	#0x00, R5
jnz	block
;------------------------------------------------------------------------------------------------
```

Essentially what I'm doing in this little snippet of code is writing a for loop that runs 8 times, with the program drawing a row containing 8 blacked out pixels every time.  This gives me a 8 by 8 pixel black box on the Nokia display.
In order to draw a row containing 8 pixels, I put #0xFF into R13.  The required functionality for this lab was surprisingly simple and it made me happy.

####A Functionality

For the first time in over a month, I had a weekend where I was not doing homework from the time I woke up until taps.  I actually had time to relax this weekend, and took advantage of this glorious new-found time and freedom.  Unfortunately, in the process of keeping myself sane, I did not attempt A functionality.  Although I probably could have used the points, I regret nothing, and am finally fresh again and ready to take on lab 4!


####Debugging/Testing

Basically I took a quick look at how the original program drew the line with the gap and then modified that section of code to write a 8 by 8 pixel block to the Nokia display.  This was the result I got the first time running my code:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/ECE382_Lab3/master/reqFunc1.jpg "first try")

That thumb nail needs to be trimmed!  This made me chuckle because it was so obviously wrong.  After studying my code, I realized that I had created an infinite loop because I was not decrementing R5, my counting register.

After fiving this mistake, I ran the program again.  This is what I got the second time through:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/ECE382_Lab3/master/recFunc2.jpg "second try")

Wrong again, but closer this time.  After studying my code, I realized I was still using the code given to us that passed in #0xE7 into R13.  I changed it to pass #0xFF into R13 and ran the program again.  Here was my result:

![alt text](https://raw.githubusercontent.com/JeremyGruszka/ECE382_Lab3/master/recFunc3.jpg "required functionality")

It worked!  That is a beautiful 8 by 8 pixel block!  Required functionality accomplished!  Shortly after snapping this picture, I was called by Derek Zoolander and asked if I would like to join him and his model friends as a thumb model.  Great success!

####Conclusions/Results

After a few trials, I was able to run my code and get a 8 by 8 pixel box to show up on the Nokia display. Thus I know that my code and program worked.

I would like to work with the logic analyzer again to better understand how it works and what we can do with it.

Documentation:  Class notes and handouts, Lab 3 handout, My previous work and labs
