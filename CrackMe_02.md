# 1. Meta Information 
- Name: Crackme Level 1
- Platform: Crackmes.one
- Difficulty: Easy / Level 1
- Tools Used: Ghidra, Command Prompt

# 2. Triage / Initial Analysis
I ran the binary in Windows. It prompted "*Enter Your Number :*" Entering a random value caused the console window to close instantly without displaying an error message.

# 3. Deep Dive (The Code Analysis)
I loaded the binary into Ghidra and analyzed the main function. The decompiler revealed a nested if structure checking two hardcoded hex values: **0x21** and **0x66.**

<img width="937" height="601" alt="image" src="https://github.com/user-attachments/assets/a7d96f78-6c3b-4eb9-93bf-c9daf083703d" />

# 4. The Solution / Exploitation
I converted the hexadecimal values to decimal using Ghidra's built-in converter. 0x21 became 33 and 0x66 became 102. To prevent the window from closing instantly, I executed the binary via a persistent Command Prompt window.

# 5. Conclusion & Flags
Entering 33 followed by 102 successfully triggered the success message: _Congratulations, you have completed the challenge!_

# Personal Thoughts
...okay maybe it was too easy. 😐 I'll hunt for better one. But at least I did it after ghosting it for months. 😁
