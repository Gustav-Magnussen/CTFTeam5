## 4002 - secret
> I forgot the password to my backdoor running at port 4002, can you find it for me?

Trying to execute the attached binary provides the greater than symbol, `>`. This seems to ask for user input and upon entering something the binary responds with `no`. 

![image](https://user-images.githubusercontent.com/59768512/161137298-4634d986-ba55-4802-a910-759d42c359c5.png)

Most likely when prompted to enter something a correct string or password has to be entered. Using the strings command in Linux on the binary reveals any strings within the file. One specific string stands out; `s00p3r_secret_k3ys`. This might be the needed password.

![image](https://user-images.githubusercontent.com/59768512/161137203-af194da4-97c0-48c8-b381-caa9350325b0.png)

Using this password as the user input gives a shell on the machine.

![image](https://user-images.githubusercontent.com/59768512/161138275-cdade4b8-e50c-4331-ad1e-cec6f10e044f.png)

Doing this on the attached port provides a shell on the machine and looking in the current directory shows a `flag.txt` file that contains the flag.

![image](https://user-images.githubusercontent.com/59768512/161138897-826fd8a9-b430-427e-bca5-a9636aef8f3a.png)

>**Flag:`IKT449{nice_l00gin}`**

## 4004 - INTelligence
> I have moved from passwords to PINs for my backdoors, is it still secure though?

Running the attached file seems to ask for a PIN code and return `no` if the PIN code is wrong.

![image](https://user-images.githubusercontent.com/59768512/158981968-c814230a-f6c9-4b50-b2a9-14f72a1b6ee6.png)

Opening the file in IDA Freeware and generating pseudocode for the main functions gives a bit of understanding of what happens when running the binary.

![image](https://user-images.githubusercontent.com/59768512/158981863-611d4047-8441-4754-8e95-3e1637705216.png)

The `fgets(s, 30, stdin9;` seems to take the user input and puts it into the variable `s`, while the variable `&v5[13]` seems to be what it is compared to. Renaming the `s` variable to `user-input`  and the `v5` to `password` will help with readability. Looking at lines 16 and 17, the main function calls the library function `strcpy` and adds the integer values to the `password` and `src` variables. At this point in the code, the `password` variable is `123454652311` and the `src` variable is `22`.

![image](https://user-images.githubusercontent.com/59768512/158984054-94d38097-f91a-4ddf-8e00-e37ae5ebd863.png)

Looking at lines 20 and 21 the library function `strcat` gets called and adds the string of the `src` variable to the `password` variable and later the string of the `password` variable as well. This means that the final `&password[13]` variable contains the string `22123454652311` which should be the pin.

![image](https://user-images.githubusercontent.com/59768512/158984306-202f5e3a-22e2-4a8c-be52-188914d4742a.png)

Using this pin confirms this theory and gives shell. 

![image](https://user-images.githubusercontent.com/59768512/161140284-e93681a6-2b31-4d91-865f-6ef0978d959b.png)

Using this pin on the attached port gives a shell on the machine in the directory where the `flag.txt` file is located.

![image](https://user-images.githubusercontent.com/59768512/161140454-0d8cb553-6f7f-4aa2-aaab-27ffdb1f713d.png)

>**Flag:`IKT449{P1N_RIGHT_UP_T1s_c5all}`**




## 4005 - screw it

210

![image](https://user-images.githubusercontent.com/59768512/159141285-91a3e03a-2321-4c94-92b1-1dd764d771ea.png)

## PY - 01

String to decimal to hexadecimal, add `secret_number` to decimal value.

![image](https://user-images.githubusercontent.com/59768512/159143200-5955d8aa-e21c-42d6-810d-35c8518c852f.png)


Starts with "I"

Unicode character of "I"

![image](https://user-images.githubusercontent.com/59768512/159143136-40e81d0f-f3c7-4d1a-9199-36cf3dffcb28.png)

First encrypted is `0xb4`

![image](https://user-images.githubusercontent.com/59768512/159143239-1d9948de-fbcd-4bfe-aa71-c9e252878720.png)

180-73= 107

`secret_number=107`

code 

![image](https://user-images.githubusercontent.com/59768512/159143186-f7f26125-4620-4193-9d12-1c1558f269b8.png)

## PY - 02

secret contains `secret_number_1`, `secret_number_2` and `flag`

![image](https://user-images.githubusercontent.com/59768512/159176591-d50a7a3a-cbda-43ea-9eb2-648c18bb5dce.png)

Adds unicode character decimal representative to file in addition to a adding secret_number_1.

![image](https://user-images.githubusercontent.com/59768512/159176654-cb2f4cb7-a955-4a31-b1ba-b32847bc2aab.png)

Reverses `enc` list into a list called `cne`.

![image](https://user-images.githubusercontent.com/59768512/159176749-e4b2545b-971c-4b56-bbe4-c9ad6c27eac0.png)

First adds all odd entries into list, then all even entries.

![image](https://user-images.githubusercontent.com/59768512/159176780-38fdd619-b9e3-423c-9385-8fc5d70c90c9.png)

Final prints (or in the case of the `task7.txt` writes to file) while subtracting `secret_number_2` from the individual entries.

![image](https://user-images.githubusercontent.com/59768512/159176855-8e37c7be-18dc-4d3c-bfdd-49327144fc7b.png)


73-59 = 14, secret_number_1 - secret_number_2 = 14


code 

![image](https://user-images.githubusercontent.com/59768512/159176468-9e8d28ae-7c06-4c0b-9f82-cc91ae5e1b94.png)

