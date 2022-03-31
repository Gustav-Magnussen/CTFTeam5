## PY - 01

The task provided us with a code snippet and a text file with the output after running the code with a secret input, which was the flag. In order to find the flag, it was necesseray to analyze the code carefully. 

![image](https://user-images.githubusercontent.com/72946914/161009970-6701dbae-d10b-41cc-9a30-3cd1018fe92b.png)

As we can see from the code snippet, there are two unknown variables: the flag and a secret number. The secret number makes the task more difficult to solve, as it provides a variation which can affect the output completly. 

The text file had the following content: 
```python
0xb4,0xb7,0xc1,0xa2,0xa3,0xa9,0xec,0xe4,0xa3,0xe8,0xd6,0xea,0xdc,0xd7,0xe6,0xad,0xda,0xc8,0xae,0xe9,0xe4,0xdf,0xe4,0xe7,0xe4,0xfe,0xff,0xba,0xf9,0xe7,0x100,0xbb,0xfe,0xf4,0xec,0xf6,0xc2,0xef,0xf5,0xc3,0xf7,0x111
```
To start, one could either import the text file by reading it like a file in the code, or just copy its content and insert it like a variable. As the content was not too long, the latter option was chosen. Then, it was necessary to have a variable to catch the flag, called `flag`. 

In the provided code snippet, the secret number is added to the flag's characters after it is turned into an integer representing the characters Unicode. In every interation, the secret number is increased by one. In order to decrypt the output, we simply wrote a code which did the operations in the opposite order, starting by subtracting the secret number, then followed by the other operations: 

![image](https://user-images.githubusercontent.com/72946914/161015249-33c331e8-f7e8-464b-90b5-a983284a92b8.png)

Having `secret_number = 0`, we consentrated around the first six integers provided by printing `real_int`, since we knew that the flag would start with `IKT449`: 
```python
180
182
191
159
159
164 
```
The decimal value of `I` is 73, thus, subtracting that number from the number we got, resulted in 107. Entering 107 as the secret number, gave the following output: 
```python
Flag:  IKT449{r0tate_m3_L1ke_ceazz4r_w1sh_h3_d1d}
```
