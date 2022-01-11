## Week 16 Homework Submission File: Penetration Testing 1

#### Step 1: Google Dorking


- Using Google, can you identify who the Chief Executive Officer of Altoro Mutual is:
```
Using the Google search "site:demo.testfire.net "Cheif Exective Officer"" I identified the CEO is Karl Fitzgerald.
```

- How can this information be helpful to an attacker:
```
This information has many vulneribilities from a social engineering position. An attacker could use 
this name to impersonate an executive member to gather more sensitive information from lower ranked employees.  
An attacker could use email to conduct this impersonation.
```

#### Step 2: DNS and Domain Discovery

Enter the IP address for `demo.testfire.net` into Domain Dossier and answer the following questions based on the results:

  1. Where is the company located: 
  ```
  This company is located at 9725 Datapoint Drive, Suite 100 San Antonio TX, 78229
  ```

  2. What is the NetRange IP address:
  ```
  The NetRange of this IP address is 65.61.137.64 - 65.61.137.127
  ```

  3. What is the company they use to store their infrastructure:
  ```
  Rackspace Backone Engineering
  ```

  4. What is the IP address of the DNS server:
  ```
  The IP address of the DNS server is 65.61.137.117
  ```

#### Step 3: Shodan

- What open ports and running services did Shodan find:
```
Ports 80 and 443 are open.
```

#### Step 4: Recon-ng

- Install the Recon module `xssed`. 
- Set the source to `demo.testfire.net`. 
- Run the module. 

Is Altoro Mutual vulnerable to XSS: 
```
Altoro Mutual has 1 vulnerability found.
```

### Step 5: Zenmap

Your client has asked that you help identify any vulnerabilities with their file-sharing server. Using the Metasploitable machine to act as your client's server, complete the following:

- Command for Zenmap to run a service scan against the Metasploitable machine: 
```
nmap -sV 192.168.0.10
```
 
- Bonus command to output results into a new text file named `zenmapscan.txt`:
```
nmap -sV -oN zenmapscan.txt 192.168.0.10
```

- Zenmap vulnerability script command:
```
nmap -p 139,445 --script smb-enum-shares 192.168.9.10
```

- Once you have identified this vulnerability, answer the following questions for your client:
  1. What is the vulnerability:
  ```
  The open shares is the vulnerability
  ```

  2. Why is it dangerous:
```
This vulnerability could allow attackers the ability to access writable files. This could allow the attacker to place a Trojan or to infect a file.
```
  3. What mitigation strategies can you recommendations for the client to protect their server:
```
Update and patch the server.
```
---
© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.  
