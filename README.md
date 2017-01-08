# cms-detector.py

cms-detector.py is a python port of the Get-CMS powershell script. Due to the flexibility of Pythons Requests module, this version is considerably more accurate and less likely to report false positives due to web server/code based redirects.

This script is designed to be used in addition to the normal checks an analyst performs in investigating AL events and can help determine if the attack that alerted applies to the site being attacked, ie Joomla attack on a Drupal site.

It is important to note that the script is only as good as the data put into it, so make sure the site that is entered is actually the destination site. Some sites will immediately redirect elsewhere (site.com to www.site.com) and you may not get the expected results. I have added an additional check to detect if the site is redirecting and then attempt to output the url that should be scanned instead, you should see something like the following if this happens:

[+] Checking to see if the site is redirecting...
[!] The site entered appears to be redirecting, please verify the destination site to ensure accurate results!
[!} It appears the site is redirecting to https://www.google.com/?gws_rd=ssl

Also, if the host is *nix based, the URL/URI will be case sensitive, for example site.com/wp and site.com/WP are not the same thing. 

Requirements:
Python 2.7
Python-Requests
Python-Argparse

Usage:
python cms-detector.py --site domain.com
python cms-checkor.py -s domain.com

Sample output:
```
PS C:\Users\administrator.LAB\desktop> python cms-detector.py --site robwillis.info

[+] Checking to see if the site is online...
 |  http://robwillis.info appears to be online.

Beginning scan...

[+] Checking to see if the site is redirecting...
 | Site does not appear to be redirecting...

[+] Attempting to get the HTTP headers...
 | Cache-Control : max-age=3594, public
 | Pragma : public
 | Content-Length : 10192
 | Content-Type : text/html; charset=UTF-8
 | Content-Encoding : gzip
 | Expires : Thu, 03 Nov 2016 20:26:41 GMT
 | Last-Modified : Thu, 03 Nov 2016 19:26:41 GMT
 | Vary : Accept-Encoding
 | Server :
 | X-Powered-By : ,
 | Set-Cookie : ARRAffinity=76c2dbc02886538d418cc1925d451591c338b4c3057cff39c8bbd03a0804d855;Path=/;Domain=robwillis.info
 | Date : Thu, 03 Nov 2016 19:26:47 GMT

[+] Running the WordPress scans...
[!] Detected: WordPress WP-Login page: http://robwillis.info/wp-login.php
[!] Detected: WordPress WP-Admin page: http://robwillis.info/wp-admin
[!] Detected: WordPress WP-Admin/upgrade.php page: http://robwillis.info/wp-admin/upgrade.php
[!] Detected: WordPress Readme.html: http://robwillis.info/readme.html
[!] Detected: WordPress wp- style links detected on index

[+] Running the Joomla scans...
 |  Not Detected: Joomla administrator login page: http://robwillis.info/administrator/
 |  Not Detected: Joomla Readme.txt: http://robwillis.info/readme.txt
 |  Not Detected: Generated by Joomla tag on index
 |  Not Detected: Joomla strings on index
 |  Not Detected: Joomla media/com_joomlaupdate directories: http://robwillis.info/media/com_joomlaupdate/

[+] Running the Magento scans...
 |  Not Detected: Magento administrator login page: http://robwillis.info/index.php/admin
 |  Not Detected: Magento Release_Notes.txt: http://robwillis.info/RELEASE_NOTES.txt
 |  Not Detected: Magento cookies.js: http://robwillis.info/js/mage/cookies.js
 |  Not Detected: Magento strings on index
 |  Not Detected: Magento styles.css: http://robwillis.info/skin/frontend/default/default/css/styles.css
 |  Not Detected: Magento error page design.xml: http://robwillis.info/errors/design.xml

[+] Running the Drupal scans...
 |  Not Detected: Drupal Readme.txt: http://robwillis.info/readme.txt
 |  Not Detected: Generated by Drupal tag on index
 |  Not Detected: Drupal COPYRIGHT.txt: http://robwillis.info/core/COPYRIGHT.txt
 |  Not Detected: Drupal modules/README.txt: http://robwillis.info/modules/README.txt
 |  Not Detected: Drupal strings on index

[+] Running the phpMyAdmin scans...
 |  Not Detected: phpMyAdmin index page
 |  Not Detected: phpMyAdmin pmahomme and pma_ style links on index page
 |  Not Detected: phpMyAdmin configuration file: http://robwillis.info/config.inc.php

Scan is now complete!
```
