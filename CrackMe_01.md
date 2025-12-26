First Write-up. 
...No,it's not my first crackme, but I'm lazy..shhh. Just read if you want, I'm already tired 🫩.

# Crackme name: crackme-v3.exe

I downloaded another crackme of 2 level difficulty and then I ran it to see whats they asking and then after typing wrong key it directly roasts "Error0xDEAD: Skill not found"..bruh.
Then after staring at the code and trying to find atleast one thing that makes sense, I found this line-

_pcVar3 = fgets((char *)&local_118,0x100,pFVar2);_

And I guessed it takes input.

Then I was looking at some more things and then I came across a function 'FUN_140002a6b'. One line caught my eyes which says-

_if (local_20 == 0x10) {_

And 0x10 means 16 and in the .exe file, it was asking me lisence of 16 number length. So I got it that thats it.
What does this function exactly do?? "It takes the 16-byte input and converts it into four 32-bit integers.
This is not random.
This is exactly how crypto / hash / block ciphers start." -- not my words, ChatGPT said it. Yeah, I used it, Fight me.

Then I opened a function named ' FUN_1400028e5', changed the variable names into simple one and tried to simplify it.
This is the version I got-

_
void FUN_1400028e5(longlong const_table,longlong input_word,longlong output_word)

{
  undefined4 j;
  undefined4 i;
  
  for (i = 0; i < 4; i = i + 1) {        
    *(undefined4 *)(output_word + (longlong)i * 4) = 0;
    for (j = 0; j < 4; j = j + 1) {
      *(int *)(output_word + (longlong)i * 4) =
           *(int *)(const_table + ((longlong)j + (longlong)i * 4) * 4) *
           *(int *)(input_word + (longlong)j * 4) + *(int *)(output_word + (longlong)i * 4);
    }
  }
  return;
}
_

This...is the matrix form, isnt it?? Then ChatGPT said that I gotta find the values of the matrix, 16 values to be precise. SO...I went back and searched that which function called this 'matrix' function. And then ...I saw the values like these-


local_88 = 0x7a2b1c4d;
local_84 = 0x3e5f6a7b;
local_80 = 0x1c2d4e5f;
local_7c = 0x6a3b2c1d;

local_78 = 0x4e5f6a3b;
local_74 = 0x2c1d7e4f;
local_70 = 0x5a3b2c6d;
local_6c = 0x7e1f5a4b;

local_68 = 0x3c2d1e7a;
local_64 = 0x4b5c6d7e;
local_60 = 0x2b1c3d4e;
local_5c = 0x5f6a7b8c;

local_58 = 0x1d2e3f4a;
local_54 = 0x5b6c7d8e;
local_50 = 0x9a8b7c6d;
local_4c = 0x5e4f3a2b;

Which are..16! means...these are the matrix I need. Now...I put them in order as a good hooman. 
with this rule: "*(int *)(const_table + ((j + i*4) * 4))" which means Row-major order: 

### Row 0: local_88 local_84 local_80 local_7c

### Row 1: local_78 local_74 local_70 local_6c

### Row 2: local_68 local_64 local_60 local_5c

### Row 3: local_58 local_54 local_50 local_4c

So my table becomes:

### [ 7a2b1c4d  3e5f6a7b  1c2d4e5f  6a3b2c1d ]

### [ 4e5f6a3b  2c1d7e4f  5a3b2c6d  7e1f5a4b ]

### [ 3c2d1e7a  4b5c6d7e  2b1c3d4e  5f6a7b8c ]

### [ 1d2e3f4a  5b6c7d8e  9a8b7c6d  5e4f3a2b ]

{wow..this .md file is so annoying I hate it...}
