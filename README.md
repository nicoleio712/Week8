# Project 8 - Pentesting Live Targets

Time spent: 4 hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:
* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each version of the site has been given two of the six vulnerabilities. (In other words, all six of the exploits should be assignable to one of the sites.)

## Blue

Vulnerability #1: Session Hijacking/Fixation

Using the provided php script to change the session ID and two browsers, this attack can be performed. On the first browser, Google Chrome, log into the Blue site as an admin. Getting the session ID from "inspect page" > Applications, copy and save that somewhere. Opening a new browser (Microsoft Edge), navigate to the public homepage for the Blue site. Add the php script extension to the url and paste the admin session ID into the "change to" box. Re-navigate back to the public homepage and click Login. The generic user now has access to admin features, effectively exploiting a vulnerability. In the Red and Green sites, this attack attempt results in normal behavior -- clicking the login simply takes you to the login page.

Gif: <img src="my_gif_walkthrough_url" width="800">

Vulnerability #2: SQL Injection (SQLi)

The hints page reccommended using a blind attack: ' OR SLEEP(5)=0--'
Knowing this, I was able to change the ID in public/salesperson.php?id=XX url (where XX is the ID of the salesperson) in any salesperson's page. From the "Find a Salesperson" section of the Blue site (not logged in as an admin), click on any salesperson and make the SQL edit in the url. This causes the site to stall for 5 seconds, effectively injecting code into the site's database instructing it to do so. The Red and Green sites prevented this attack by re-routing the attacker back to the "Find a Salesperson" home page once the attacks was attempted.

Gif: <img src="my_gif_walkthrough_url" width="800">


## Green

Vulnerability #1: Cross-Site Scripting (XSS)

For this attack, a generic user can input an alert() attack inside <script> tags in the "Feedback" part of the "Contact Us" form. Nothing will happen right away, but when an admin logs in and checks their submitted feedback forms, the malicious Javascript code will execute and the alert message will pop-up. This, of course, doesn't happen on the Red or Blue sites -- instead, checking the submitted feedback simply shows the text version of the attempted Javascript.

Gif: <img src="my_gif_walkthrough_url" width="800">

Vulnerability #2: Username Enumeration

Username enumeration attacks happen when the attacked system offers clues about whether or not a username or password (or combination of the two) exists. Note that in the gif below, jmunroe99, pperson, and lbtables are the only usernames in existence.
On the Green site's login screen, if the entered username does not exist, the "Log in unsuccessful." message is not bolded -- if the username does exist, but the password is wrong, the message IS bolded. This gives attackers clues about which portion of the login is incorrect and therefore allows them the opportunity to enumerate all passwords once they have a correct username. On Blue and Red sites, the error message is bolded in both cases, offering no clues to the attacker.

Gif: <img src="my_gif_walkthrough_url" width="800">

## Red

Vulnerability #1: Insecure Direct Object Reference (IDOR)

This IDOR vulnerability allows a non-admin user to see any account - even deactivated or not-yet-active accounts. Without logging in, an attacker can look at the "Find a Salesperson" page and click on a name. This reveals the public/salesperson.php?id=XX url (XX is the ID of the salesperson). From here, the attacker can guess and type in arbitrary numbers until an inactive account is revealed. Typing in ID=10 reveals "Testy McTesterson," for example. This account isn't supposed to be public until September 1st. Since all the ID numbers are low and sequential, this guessing method is extremely successful. The Blue and Green sites avoid this by simply redirecting the user back to the list of salespeople even if a non-existent or deactivated ID is inputted.

Gif: <img src="my_gif_walkthrough_url" width="800"> 

Vulnerability #2: Cross-Site Request Frogery (CSRF)

This attack involves editing users in the "Users" section of the admin page. In the vulnerable Red site, in the edit screen of a user, once you inspect and change the csrf_token, you can then change any part of the user's information and the changes will be saved. This is obviously a vulnerability. In the Blue and Green sites, the attempted attack results in an "invalid request" screen since the new session ID is not valid.

Gif: <img src="my_gif_walkthrough_url" width="800">


## Notes

Describe any challenges encountered while doing the work
