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


