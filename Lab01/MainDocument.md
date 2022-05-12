# CTFTeam5

>**Flag`IKT449{eaarlyyy_m000rr0r0rning}`**

## Introduction

>*The attack occured from Jan 8, 2022 @ 23:55:00.000 to Jan 9, 2022 @ 00:10:00.000... [truncated]*

To conform to the rules and regulations to perform the tasks, we replied `IKT449{i_will_behave}`.

## Attacker 

### Attacker

>*From which IP does the attack originate?*

From the task given, it was apparent that the victim's IP address was `10.13.37.4`. To find the attacker's IP address, we filtered for `source.ip` within Elastic, where it showcased two IPs. Since `.4` was the victim, and `127.0.0.1` is the loopback adapter of the localhost, the `10.13.37.5` is the attacker since it has the most requests as well:

![bilde](https://user-images.githubusercontent.com/70077872/150950686-69d9b703-fb9a-4ab6-921d-56da8ecf9b63.png)


### Search
>*The attackers performed a few searches on the site, what was the last thing he searched for?*

Most often, searches are viewed as `/?s=` in the URL. By entering this as a filter, in addition to the attacker's IP address, we were left with two hits. 

![image](https://user-images.githubusercontent.com/72946914/151540795-78551aae-f0e3-492f-8c8f-e91a2477b07b.png)

Checking the search result in the newest hit, it was discovered that the search was `/?s=hello+this+is+a+test`. Thus, the answer to the challenge was `hello this is a test`.

![image](https://user-images.githubusercontent.com/72946914/151540857-79cc6c7b-d8f4-4dbc-98bb-1d8b2c62ef87.png)

## Bruteforce

### Bruteforce Activity
>*How many unique passwords were tried during the brute force?*

To find the total amount of unique passwords tried during the brute force attack the total amount of unsuccessful and the successful attempt have to be added. Filtering the logs by setting the `request.keyword` to `Incorrect` only shows entries where there was an unsuccessful login attempt. Knowing that there were `670` unsuccessful login attempts and one successful one the total amount of tried passwords is `671`.

![B2](https://user-images.githubusercontent.com/59768512/151540063-dbbc6310-1125-4f41-9ff3-fe896770a84d.PNG)

### Bruteforce Tool
>*From the logs one can conclude that a brute force has taken place. What tool has been used to perform this attack? (all lowercase)*

To find out what type of tool was used to perform the brute force attack the phrase `password` was searched in Elastic. This resulted in several entries where the phrase `Incorrect username or password` was present. Expanding one of these entries and looking at the `user-agent.original` field shows that a tool called `wpscan` is used, which is a WordPress brute-forcing tool.

![B1](https://user-images.githubusercontent.com/59768512/151540034-fc6fc174-c4ee-4a93-9d71-f642c7939536.PNG)

### Bruteforce Success

>*What is the content of the _id field where the correct password was guessed from brute-forcing?*

We previously found out that the password for the admin user was "purple1" (from Admin Password). To find the "id" field in which the correct username was found, we searched for "purple1" and looked at the packet. This was done since the system most likely accepts the correct password and gives the successful message back in the same packet. From there, the correct answer is `ObjrO34BeTOdFhttNk4J`:

![bilde](https://user-images.githubusercontent.com/70077872/149674201-88ff335d-3b7f-4bf3-892c-d52e6e6fc5cd.png)

This can be verified later in the packet since the variable "isAdmin" returns a boolean value of "1", meaning the Admin password was correct:

![bilde](https://user-images.githubusercontent.com/70077872/149674301-1cf71a85-5fea-4175-a3c6-8e0c84467359.png)

## Files

### File Read 1
>*What file was read by the attacker's initial user in /home/user ?*

Knowing that the attacker initially gained access to the user `www-data` and that the file that is read is in the `/home/user` directory the logs can be filtered using `user.name:www-data and process.working_directory:/home/user`. This gives a total of three entries and looking at one of these shows that the attacker used the executable `/usr/bin/cat` on a file called `note.txt`.

![LS](https://user-images.githubusercontent.com/59768512/151540006-b7c136b1-a698-4d42-bfb5-c41483940d7c.PNG)

### File Read 2
>*What file did the attacker read once logged in as 'user' ?*

Knowing that the attacker used the executable `/usr/bin/cat` to read file the logs can be filtered using `user.name:user and process.executable:/usr/bin/cat`. This provides a total of 13 entries where only one is using the `/usr/bin/cat` executable. Expanding this entry shows that the user `user` used the executable `/usr/bin/cat` on a file called `user.txt`.

![LS](https://user-images.githubusercontent.com/59768512/151539978-198070f1-be9b-42b0-8b83-2feb8708f28b.PNG)

### Strange File
>*The attacker created a strange file once access to the machine was gained, can you find the file? Submit only the filename once you find it.*

Since we know that the attacker gained access to the `www-data` user first on the machine we can search for entries in the log where the user name is `www-data`, this can be done using the filter `user.name IS www-data`. 

![S1](https://user-images.githubusercontent.com/59768512/151539916-1ed93b71-260c-4d4c-8a4f-1c45970b9d88.PNG)

As this is still providing a lot of entries in the logs and expanding the entries shows that mostly all of them have `/usr/sbin/apache2` in the `process.executable` field. These are entries that account for traffic the webserver itself conducts and using a filter like `process.executable IS NOT /usr/sbin/apache2` reduces the matches down from 363 hits to 21 where one of the entries include the creation of a strange file called `IKT449{this_is_a_perfectly_benign_file}`

![S2](https://user-images.githubusercontent.com/59768512/151539945-46977e0f-1447-4ab3-b4ca-b0e911bddbe6.PNG)

## Firefox

### Firefox

>*What version of firefox is the attacker using? (two digits)*

Since we previously know that the attacker's IP is `10.13.37.5`, we searched for `firefox` within the search field. Additionally, to filter out the attacker's agent, we added a filter with `source.ip.keyword: 10.13.37.5`. This yields several results, where it is apparent that the Firefox version is `91`:

![bilde](https://user-images.githubusercontent.com/70077872/151168512-5e94022f-3351-4515-af98-a60197222131.png)


## Framework 

### name

>*What framework is used to host the site running on port 80 on the victim? (all lowercase)*

As found in Bruteforce Tool, the tool to perform the scanning was `wp-scan`. After researching WP-scan, it was apparent that it is used for brute-forcing the WordPress framework's login page. As such, the underlying framework running on port 80 is WordPress.

### Themes
>*The attacker replaced the 404.php page of one of the themes with a reverse shell. Which theme was compromised? (all lowercase, one word)*

It was quickly discovered that the page in question was in fact a WordPress site.  By entering "theme and 404" as a search word, we were left with 16 hits. For the attacker to fetch the desired theme, an HTTP `GET` request was required. By filtering on just GET requests, there were 5 hits left. 

![image](https://user-images.githubusercontent.com/72946914/151540643-0b794f82-bf29-457c-86bc-e1d63c8dc95b.png)

By expanding one of the five, it was discovered that the compromised theme was *twentytwentyone*.

## Kibana

### Kibana

>*What version of Kibana is the site running?*

Kibana lets you visualize the Elastic search, and uses the KQL syntax language. To find the version number, we searched around Elastic and pressed the button at the top right corner. This tells the version number to be `7.16.2`:

![bilde](https://user-images.githubusercontent.com/70077872/150954177-5b57d0e3-dd93-4d85-8cae-b588395799b7.png)

## Logs

### Log Count
>*How many logs are found in the interesting timeframe?*

Looking at the discovery section of Elastic it can be seen that a total of `6788` entries are present when not filtering for anything.

![LC](https://user-images.githubusercontent.com/59768512/151539880-5cd658fa-688b-4928-b1ef-239d8a83ac12.PNG)

### Log Sources
>*What data shippers from Elastic have been used to log all the different logs found in Kibana? Sort the values in alphabetical order, separated with a comma, all lowercase. Ex: IKT449{blue,red,zebra}*

Expanding the `agent.type` field within Elastic show that in total there are three different data shippers that have been used meaning that the correct answer is `IKT449(auditbeat,filebeat,packetbeat)`.

![LS](https://user-images.githubusercontent.com/59768512/151539840-cc789217-b9b3-4a49-afd7-aea1fbe4d054.PNG)

## Port

### Got Port?
>*What port is the most commonly requested one in the logs?*

Expanding the `destination.port` field within Elastic shows that the most commonly requested port is port `80`.

![LS](https://user-images.githubusercontent.com/59768512/151539801-1451c88a-1178-4c2a-ae34-f520b53f4d1c.PNG)

### Reverse Port
>*The attacked managed to get a reverse shell from the machine, on which port did the attacker receive this shell?*

Filtering the logs for the phrase `reverse shell` displays several entries where a pentest monkey reverse shell is mentioned.

![LS](https://user-images.githubusercontent.com/59768512/151539745-90b30c7c-12da-4383-ac34-3eb49926df0f.PNG)

Expanding the `HTTP.request.body.content` field of one of these entries shows a lot of text where the port `9001` is mentioned showing that this is the port the attacker used.

![LS](https://user-images.githubusercontent.com/59768512/151539694-e0850cf0-c208-45f5-a02c-fa4ff7feae12.PNG)

### More Ports

>*It seems to have occurred a portscan at the start of the attack, how many ports were tested?*

Firstly, we had to find the exact time frame in which the scanning occurred, which was done by looking at the table on the `Discover` page. There is a time frame that stands lonely and separated:

![bilde](https://user-images.githubusercontent.com/70077872/149670992-a3512899-d5be-4cc1-9b4a-79be78e1a922.png)

Secondly, it was important to filter out the port the attacker used to scan with. This was done to filter the interesting time frame and filter out the `source.port` that was used for scanning. After filtering, port `55437` is mostly used: 

![bilde](https://user-images.githubusercontent.com/70077872/150955489-00505b57-87e4-4f6c-b993-5e33ed7539d0.png)

Then, entering this timeframe into the Dashboard, and choosing the specified port, with filtering the unique destination ports (`Unique count of destination.port`) got the result of 100 ports:

![bilde](https://user-images.githubusercontent.com/70077872/149670940-425cf730-fad2-467c-8bec-bbb71adcc62e.png)

## Users

### Admin Password
>*What is the correct password for the admin account?*

By searching in Elastic for `http.response.status_code:200 and response:isAdmin` we are able to see the entries where the status code for the http response is `200 OK` and where the `isAdmin()` is set.

![Admin Password](https://user-images.githubusercontent.com/59768512/151539645-a41a5da4-66f4-4923-8bc8-41a1de78115b.PNG)

Expanding this entry we can see that the admin password is `purple1`.

![Admin Password2](https://user-images.githubusercontent.com/59768512/151539614-6bda64cf-6c50-4811-9c67-4a50b636156b.PNG)

### User what now?
>*What was the first user they got access to? (on the machine itself)*

As is found earlier the framework `WordPress` is running on port 80 on the machine, given that the attacker gained access to the webserver framework first it would make sense that landing the victim machine the attacker would initially have access to the user running the web server. Using best practice when setting up a webserver on a Ubuntu platform is to use the `www-data` user. Filtering the logs using `user.name:www-data` shows that several commands have been executed on the machine originally centered around webserver running (running /usr/bin/apache2) and later including commands that set up a more user-friendly environment for further escalation. Therefore the first user the attacker got access to on the machine is `www-data`.

![U1](https://user-images.githubusercontent.com/59768512/151539551-760f5cf1-220e-4f34-8c32-19d6f0dfbe8f.png)

### Login Time

>*At what time did the attacker login to the user: 'user'? (Answer down to the second as: xx:yy:zz)*

To find the correct login time, we had to find a login time in which the auditd.result was a success. Many of the audits before the correct point of time were successfully executed, by the "user" as the primary actor. However, they only got access to "root" as the secondary actor. Therefore, by filtering the times the audit was successful, as well as investigating at which times the user "user" acted as the secondary actor consistently, we were able to find out at which time the login was successful. This first occurred at 00:08:23.248, from which point the logins further consistently returned the audit successful with "user" as the secondary actor". Therefore, the correct login time is 00:08:23.

![image](https://user-images.githubusercontent.com/46780028/150975179-b76b9820-0cae-4c28-9b58-47ea3240e92c.png)
