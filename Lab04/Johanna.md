## PY - 01

The task provided us with a code snippet and a text file with the output after running the code with a secret input, which was the flag. In order to find the flag, it was necesseray to analyze the code carefully. 

![image](https://user-images.githubusercontent.com/72946914/161009970-6701dbae-d10b-41cc-9a30-3cd1018fe92b.png)

As we can see from the code snippet, there are two unknown variables: the flag and a secret number. The secret number makes the task more difficult to solve, as it provides a variation which can affect the output completly. 

The text file had the following content: 
```python
0xb4,0xb7,0xc1,0xa2,0xa3,0xa9,0xec,0xe4,0xa3,0xe8,0xd6,0xea,0xdc,0xd7,0xe6,0xad,0xda,0xc8,0xae,0xe9,0xe4,0xdf,0xe4,0xe7,0xe4,0xfe,0xff,0xba,0xf9,0xe7,0x100,0xbb,0xfe,0xf4,0xec,0xf6,0xc2,0xef,0xf5,0xc3,0xf7,0x111
```
To start, one could either import the text file by reading it like a file in the code, or just copy its content and insert it like a variable. As the content was not too long, the latter option was chosen. Then, it was necessary to have a variable to catch the flag, called `flag`. 

In the provided code snippet, the secret number is added to the flag's characters after it is turned into an integer representing the characters Unicode. Then the integer is turned into the corresponding hex decimal value. In every interation, the secret number is increased by one. In order to decrypt the output, we simply wrote a code which did the operations in the opposite order, starting by subtracting the secret number, then followed by the other operations: 

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

## Py - 02

We were provided with the same material here; a code snippet and a text file with the output. This time, the output was a list consisting of the following integers: 
```python
111,88,37,87,81,90,81,97,81,96,81,90,91,91,81,90,81,102,87,88,34,101,43,38,61,102,37,37,94,87,102,97,102,86,83,102,89,91,100,87,102,34,81,94,88,90,109,38,70,59
```
After reading through the bellow code snippet, we got an idea of what was happening. First, every character of the flag is turned into the corresponding Unicode character and added the value of `secret_number_1`. Then the encrypted list, `enc`, is flipped around before every other element is added to a new list, `cne2`. Then the remaining elements is added to `cne2`, as the list's second half. At last, the numbers are subtracted with `secret_number_2`. 

![image](https://user-images.githubusercontent.com/72946914/161036498-dcd84999-a449-4877-bcd4-4e55bce35e75.png)

This time it is not as simple as flipping the code around, like in the previous task. We are dealing with two unknown numbers and flag that has been completly turned round and twisted. 

For starters, we defined the two variables `secret_number_1` and `secret_number_2` to both equal zero, which makes it easier to understand which output we are dealing with before adding arbitrary values to the variables. To start of where the code snippet left off, we iterated through the elements of the output and subtracted `secret_number_2`. 

![image](https://user-images.githubusercontent.com/72946914/161040129-c89f599c-347c-43ec-b0ec-7a1ffcf2d023.png)

As `cne2` is divided into two parts, containg the first added and the second added numbers from `cne`, we started by dividing the list into two parts, trying to reconstruate the original list. Then we flipped both of them to get their original order. 

![image](https://user-images.githubusercontent.com/72946914/161040570-b4c471d6-349c-4175-bfc0-9a25dde042d0.png)

By going through the code with pen and paper, with `IKT449` as the flag (which we knew was the flag's starting characters), we understood that the list would be something like this:

```python
first_half = K49
second_half = IT4
```
In order to zip them together to obtain the right order, we used the Python function called `zip()` which does exactly that. Then, we went through the values of the zipped list and turned them into charachter after subtracting `secret_number_1`. The complete code now looks like this: 

![image](https://user-images.githubusercontent.com/72946914/161042062-905ea0af-4a4c-4578-b798-6601ed78a6d4.png)

Giving us the following result: 

```python
Result:  [(59, 61), (70, 38), (38, 43), (109, 101), (90, 34), (88, 88), (94, 87), (81, 102), (34, 81), (102, 90), (87, 81), (100, 91), (91, 91), (89, 90), (102, 81), (83, 96), (86, 81), (102, 97), (97, 81), (102, 90), (87, 81), (94, 87), (37, 37), (37, 88), (102, 111)]
Flag:  ;=F&&+meZ"XX^WQf"QfZWQd[[[YZfQS`VQfaaQfZWQ^W%%%Xfo
```

As the previous task, we want the first number in the list `Result` to be 73, which equals the letter `I`. Since 73 - 59 = 14, we knew we someplace had to add 14 to either of the secret numbers. Trying `secret_number_2 = 14` did not change much, but setting `secret_number_1 = -14` (minus since this value is subtracted from the elements in the list, and we want it added), we obtanied the flag: 

```python
Flag:  IKT449{sh0ffle_t0_the_riiight_and_too_the_le333ft}
```




