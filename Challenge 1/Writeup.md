### Challenge 1 

Download the challenge fom this repo.  
![](https://github.com/dmanasa6/CTF-Challenges/blob/master/Images/Challenge1/Me_again_lol%20.jpg)  
The image in the challenge can be examined by various forensic tools like `binwalk`,`strings`,`exiftool`,`xxd`,`hexdump` etc.
By using binwalk tool, it was clear that the image had two JPEG file in it.  
```  
binwalk Me_again_lol .jpg
```  
By using dd tool, the second image can be extracted.  
```  
dd if=Me_again_lol.jpg of=secondpic skip=17334 bs=1
```   
![](https://github.com/dmanasa6/CTF-Challenges/blob/master/Images/Challenge1/binwalk_dd_1.png)  

The image extracted is :  
![](https://github.com/dmanasa6/CTF-Challenges/blob/master/Images/Challenge1/secondpic.jpg)

After looking into the hexdump of both images, we observe a string "FLE" in the beginning of second image's data.  
"FLE" is nothing but the reverse of ELF format, Executable and Linked Format.  
![](https://github.com/dmanasa6/CTF-Challenges/blob/master/Images/Challenge1/FLE_string.png)  
We need to extract a file from that data and convert it into a ELF file and find the flag from it using reversing.  

By using dd tool, a file `text` is extracted till the FLE string, so as to convert it into ELF format.  
```
dd if=Me_again_lol .jpg of=text count=17734 bs=1
```  
![](https://github.com/dmanasa6/CTF-Challenges/blob/master/Images/Challenge1/dd_text.png)  

On viewing the hexdump and data of `text`, we notice "ELF" at the end of the file.  
[Reverse the contents](https://unix.stackexchange.com/questions/416401/how-to-reverse-the-content-of-binary-file) of the file by using xxd command :  
```
<text xxd -p -c1 | tac | xxd -p -r > flag
```  
![](https://github.com/dmanasa6/CTF-Challenges/blob/master/Images/Challenge1/revers_elf.png)  

Now, the file `flag` is an Executable file. To find the flag, we will have to reverse it.  
I used gdb, to find the flag.  
```
gdb flag
```
```
disas main
```
![](https://github.com/dmanasa6/CTF-Challenges/blob/master/Images/Challenge1/char_gdb_pyshell.png)

On finding the ascii character for each instruction, the flag for the challenge is obtained.  
Flag :
```
CODEBATTLE{For3ns1c_is_also_needed_R3vers1ng_Sk1ll_to_analyze_Malwar3}
```
