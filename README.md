# Simple Ghidra Python Plugin to Decode XOR-Obfuscated Strings

This Ghidra plug-in is a continuation of the original post, which was written for the IDA disassembler. The IDA plugin was inspired by a recent XLS document that drops and executes a DLL using RUNDLL32. The DLL is small and only used to download the next stage. However, it employs rather straight-forward string obfuscation using the bitwise XOR operation. An important skill for any reverse engineer/malware analyst is to be able to create plugins to assist in statically decoding these strings and doing so across the entire disassembly database. This plugin is intended to get you started creating IDA Plugins with Python, recognize the importance of deobfuscating strings and work on translating assembly to a higher-level language (i.e. Python).

Original sample and DLL: [https://github.com/jstrosch/malware-samples/tree/master/maldocs/unknown/2020/December](https://github.com/jstrosch/malware-samples/tree/master/maldocs/unknown/2020/December)  
Analysis on YouTube: [https://youtu.be/un8I6dfuDVQ](https://youtu.be/un8I6dfuDVQ)

## Obfuscated Strings

Below is a sample of the obfsucated string pattern. The function called to deobfucate the strings is *FUN_10001210* and takes three arguments - the size of the string to decode, the key and obfuscated string (in that order).

![pre_deobf](https://user-images.githubusercontent.com/69214982/117378054-829cd180-ae89-11eb-9180-9a817cd3e28a.png)

Function *FUN_10001210* allocates memory for the deobfuscated string using *LocallAloc* and a loop. The loop takes each letter of the key and XORs with the obfuscated string. If the string is longer than the key, it uses modulo division to repeat back over the key and continue until the string is fully deobfuscated.

![ghidraLoopGraph](https://user-images.githubusercontent.com/69214982/118728926-d395b980-b7e9-11eb-9981-cdbfc4c4f5b4.png)

Finally, the pointer to the allocated memory that contains the deobfucated string is returned and assigned to a global variable. The default behavior for this plugin is to add the deobfuscated string value as a comment next to this assignment.

![postDeobf](https://user-images.githubusercontent.com/69214982/117378523-7bc28e80-ae8a-11eb-8f26-89c4814f4bf3.png)


## Note
This plugin is not intended to decode all XOR obfuscated strings you encounter, but should serve as a good starting point to implement the logic you encounter and recover those strings!
