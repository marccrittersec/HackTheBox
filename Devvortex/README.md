![image](https://github.com/marccrittersec/HackTheBox/assets/131484050/3c2cf65c-9db5-40ff-b999-cdc5e2d8a29f)

# Process
### - Perform an NMAP Scan to determine open ports

Nmap Scan Results:

![image](https://github.com/marccrittersec/HackTheBox/assets/131484050/46fe4030-7cc8-43c8-a1e1-2eccd247e9cb)

Here we see an open port of 80, we also see the url "http://devvortex.htb/"

### - Add URL to /etc/hosts file

    echo "10.10.11.242 devvortex.htb" | sudo tee -a /etc/hosts   

### - FFUF scan

Lets now scan for possible vhosts,

    ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/bitquark-subdomains-
    top100000.txt:FUZZ -u http://devvortex.htb -H 'Host: FUZZ.devvortex.htb' -fw 4 -
    t 100

Results: We discover "dev"

Dont forget to add dev.devvortex.htb to the /etc/hosts file

Once we do we are presented with a page:

![image](https://github.com/marccrittersec/HackTheBox/assets/131484050/87f1a3ae-9faf-48d4-acef-f359d3fbb7d6)

Lets try to scan and reveal a path.

    ffuf -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt:FFUZ -u http://dev.devvortex.htb/FFUZ -ic -t 100


Results: We find /adminstrator

This takes us to a login page, and we try to perform a login of admin:admin but with no success.



![image](https://github.com/marccrittersec/HackTheBox/assets/131484050/e1592933-1f02-42c1-9a3e-6d4a96881b08)

Lets try to see what version of Joomla is being ran, lets go to /administrator/manifests/files/joomla.xml as we navigate here we see the .xml that indeed does give us the running version.

![image](https://github.com/marccrittersec/HackTheBox/assets/131484050/933611ec-a8dd-4cb7-910d-dde318e95552)
