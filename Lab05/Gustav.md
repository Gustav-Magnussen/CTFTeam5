## 5 - Root

> It is game over, the attacker got root access.
> Could you help us figure out how this happened by getting to root yourself?

Having gained user access on the machine it is smart to check if the user has any admin privileges. Executing the command `sudo -l` shows the user administrative privileges on the machine.

<img width="796" alt="image" src="https://user-images.githubusercontent.com/59768512/167270262-887ad0d2-eedc-48c9-a667-8a5d37d74742.png">

As it turns out the user account can run the command `sudo /usr/bin/python3 /home/user/safe_backup.py *`.  The `*` here essentially means that the command can end with anything. It also seems that the person adding the admin rights to the user account has been very specific on the path of the executable and the file probably to avoid having users run fabricated files and executables by manipulating pathing or altering files. Looking at the permissions of the `safe_backup.py` it is owned by the root account and can therefore not be removed or altered.

<img width="320" alt="image" src="https://user-images.githubusercontent.com/59768512/167715284-3230f41f-471d-4670-8c3b-89ac95e277c7.png">

To get some idea of what is possible, looking at the `safe_backup.py` file is probably a good place to start.

<img width="870" alt="image" src="https://user-images.githubusercontent.com/59768512/167270179-3574d9be-a110-491c-8208-d4e76893d452.png">

Looking at the code within the file it seems like it archives the whole `/home/user` folder and names it after the current datetime. It also seems like a user can provide some input on files or folders they do not want to have archived using the `--exclude` option. In this python file the user can specify these files/folders using the option `-e` and if it is present in the command the user input is put into the command using the variable `args.e`. Another thing to point out is that there seems to be sanitation on what the user enters, which means that there is a possibility for injection of something into the command.

Looking at the `os.system` method the user input seems to be passed before the main part of the tar command, this could be manipulated as in Linux it is possible to run more than one command in a single line. One way to do this that does not account for if the first command is successful is using the `;` sign to separate commands. Therefore within the `--exclude` part of the command, we can inject commands. One way to do this is by simply adding `sudo su` which switches the account to the root account. The whole command would then be:

`sudo /usr/bin/python3 /home/user/safe_backup.py -e ";sudo su; tar"`

Adding the `; tar` at the end also ensures that the second part of the command runs smoothly without errors. 

<img width="578" alt="image" src="https://user-images.githubusercontent.com/59768512/167680228-faee620b-631b-4767-bb49-582f91864c6a.png">

While an error message is present for the first part of the command the shell has successfully been upgraded to the root account. Looking in the root directory the file `root.txt` is found which contains the flag.

<img width="312" alt="image" src="https://user-images.githubusercontent.com/59768512/167680481-26fcd5f2-b437-4c09-8d92-02b9a56c9a68.png">

>**Flag:`IKT449{root_doing_things_often_goes_bad}`**
