In order to scan all ports of the target system, `nmap` together with the target IP has to be utilized. This can be done using the following command: `-nmap -Pn -T4 -A -p- 10.225.149.170`. Nmap scans all the ports, and confirms that ports `22`, `1337` and `7331` are open. Port `1337`returns `Wrong port :)`, and therefore we proceed to the next port, `7331` to search for the flag. 

<img width="749" alt="164413183-b75a837e-0178-47c0-93b5-fd7dfcb6c662" src="https://user-images.githubusercontent.com/46780028/167447678-d76b9015-70f4-4ac6-b4c7-34410ca43583.png">


When the target IP and port number 7331 is entered into the browser (http://10.225.149.170:7331/), a webpage with the flag is revealed:

![image](https://user-images.githubusercontent.com/46780028/167448426-1add0bfd-b57a-426d-912a-f53189655ab5.png)

Flag: IKT449{check_all_the_ports}
