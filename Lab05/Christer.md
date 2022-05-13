In order to scan all ports of the target system, `nmap` together with the target IP has to be utilized. This can be done using the following command: `nmap -Pn -T4 -A -p- 10.225.149.170`. `-Pn` meaning Dont Ping, enables nmap to perform its function regardless if the target is active or not. `-T4` sets the speed of the scan, which scans the ports quickly without omitting important information. `-A` means Agressive Scan, and enables nmap to scan important information like OS, version, script and traceroute. Finally `-p-` specifies that all the ports of the target system should be scanned.

Using the given commands, Nmap scans all the ports of the target system, and confirms that ports `22`, `1337` and `7331` are open. However, it also states that port `1337` is the `Wrong port :)`, and therefore we proceed to the next port. `7331` is the last open port, and it is returning data. This means that the webpage should be located in this last open port.

<img width="749" alt="164413183-b75a837e-0178-47c0-93b5-fd7dfcb6c662" src="https://user-images.githubusercontent.com/46780028/167447678-d76b9015-70f4-4ac6-b4c7-34410ca43583.png">


The target webpage is found when the target IP and the correct port number 7331 is entered into the browser (http://10.225.149.170:7331/). On this page, the flag is revealed:

![image](https://user-images.githubusercontent.com/46780028/167448426-1add0bfd-b57a-426d-912a-f53189655ab5.png)

Flag: IKT449{check_all_the_ports}
