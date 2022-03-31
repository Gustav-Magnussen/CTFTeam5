## 4001 - Connection Established

![oppgave1-lab4](https://user-images.githubusercontent.com/46780028/160909838-0c82df23-ca60-4cc9-8aec-ce8d6945b3a7.PNG)

In order to establish a connection to the machine and the correct port, we use NetCat to scan the port. This is done by using the command `nc`, followed by the target ip. Finally, the port to be scanned is specified.

To be able to confirm that the port has been scanned and that the target machine has been accessed, we use the command `whoami` to be able to see who we are and which privileges can be used. Then `ls` is used to list the available files, including `flag.txt`.

Finally, "cat" is used on `flag.txt` fiile to open it, thereby revealing the flag.


## 4003 - All Strings Attached

![image](https://user-images.githubusercontent.com/46780028/160911603-3691bb57-8752-4a26-b3cc-273604ea5573.png)

When the file is opened, it lists a numbher of strings. Some of these are strings that will be used in the password, but as the task mantions, some of these are also false. 

![image](https://user-images.githubusercontent.com/46780028/161066505-2a162913-684f-43c9-8c13-b2dfd5fba987.png)

The password will be composed with three strings. To choose these strings, the commands `first`, `second` and `&second` are written in front of the selected strings. However, `second` value, `"dr4h_ti."` has to be reveresed to retrieve the correct password. When reveresed, it the string changes to `it_h4rd`. When the three selected strings are sucsessfully connected, the command `concatenate`followed by the three selected strings will form the final password string; `00ps_d1d_im444k33-3rrrr-it_h4rd_with_ErrR0r-str11nnnz`.

![image](https://user-images.githubusercontent.com/46780028/161066318-b988b0fe-1f7d-4618-93fa-ab984b7887ae.png)

If the password entered into `userInput`is correct, the user will gain acsess to the shell. In this case, the previo If the password is incorrect, the system will simply return the message `"no"`. When permission is gained by using the previously mentioned password, we use `cat` on the `flag.txt` file to reveal the flag.
