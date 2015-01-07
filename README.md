What should I know to implement a login system for my website?
=============================================================

I am implementing a login model for my website and went on an exploratory path on what is safe and what is not. There are two perspectives to look at: the server's implementor and the end user of your website. This Q and A is to get you thinking on these lines. 

******

Server implementor's perspective
--------------------------------

 1. Ok, the password came to my server safely. What do I do next?

    Did it? think again.

 2. What is SSL?

    SSL is an abbreviation for Secure Sockets Layer. SSL is a term commonly used in connection with HTTPS, where HTTPS is just HTTP coupled with SSL. When important information is sent over the internet, it is important to send it encrypted to prevent eavesdropping. It is vital to encrypt in a way that only the correct server should be able to decrypt it. At the same time, from a user perspective, it is important to be sure that you are talking to the REAL server and not some fake server masquerading as facebook.com. Thus, you would want to send information encypted and also be sure of the identity of the server to whom you are communicating. HTTPS solves these two problems. Without going into gory details, be aware that encryption can be done with asymmetric keys. That is, usually a key which is used to encrypt data, can be used again in the reverse way to decrypt it; however, there is something called Public Key Cryptyography which has a concept of two keys: one for encryption, and the other for decryption. The Public Key is publicly distributed so everyone has access to it, whereas the Private Key is kept secretely by the server. The pair of keys is such that information encrypted with the Private Key can be decrypted only by the Public Key and the information encrypted by the Public Key can be decrypted only by the Private Key. When you do a HTTPS GET request to a server, it will send you it's public key along with a bunch of information. The bunch of information contains something called the SSL certificate information, which will help you confirm the identity of the server sending you the information. SSL certificate thus confirms the identity of the server, and the public key passed lets you encrypt a key that you want to use for this HTTPS session. Once the server receives this new key, it will send back a agreed message, and you both can start communicating using the new key.

 3. How can I get a SSL certificate? Do I need to do anything special in my server to support SSL/HTTPS?

    There are two ways to get a SSL certificate: one is called a Self-Signed Certificate and the other is a CA(Certificate Authority) provided SSL certificate. The way SSL manages identity assurance is by making the Browser trust a few(2/3) CA(Certificate Authorities). These CA then delegate the responsibility to few of their friend CAs. In this way in the end of the delegation chain, one CA will actually give you the SSL certificate saying "This is really xyz.com and I have verified that.". Based on the delegation, it is as if the topmost(trusted) CA had signed your identity. You can view these SSL certificates in the browser. In Chrome, you can simply go to a website using https, and click on the green part in the address bar. This green part could be a small lock, or a long bar showing the name of the company. You can view the chain of CA's who have approved the website's identity: Click on the green part->Click on Connection tab-> Click on Certificate Information->Certification Path. When you try to get a SSL crtificate(which for some reason you need to purchase), this is the process that happens. First, the certificate issuing authority will confirm your identity by using one of the following ways:confirmation mail sent to a mail @your_domain.com, or a confirmation mail sent to a mail id given when you registered your domain(whois mail id). Alternatively you could be asked to host a CA provided file to your server. You could also be asked to do a CNAME change with a hashed domain address provided by CA. Secondly, you pay them and they will give you a installation file, which will basically give you a few files(with extension like .pem). Thirdly and finally, you might need to change your code to do two things: one, to always redirect http to https and two, to associate the certificate files(.pem) to encrypt and decrypt data when communication happens with the server.


 4. How should I validate the mail id at the time of signup? If the same mail id exists in the database, should you say the same mail id exists?

    Yes, there seems no other way to deny the new person this mail id for signup. You can give an option to claim that account.

 5. Do I need a captcha?

    Captchas look fool-proof but a captcha can be solved in less than 17 seconds by simply transmitting the image to some real human on some sites. They solve 1000 captchas for a dollar. And remember the last time you solved a captcha, did it take you about 20 seconds to work out the weirdly curly letters? Captchas apart from ruining the user experience, add very little to distinguish humans from bots, yet this is the most effective way available to this day, and facebook and a lot of popular websites uses it. reCaptcha seems better but most sites have not started using it.

 6. Do we need email verification at the time of signup?

    In terms of security, this is always good. For instance, a user coulld have mis-typed their E-Mail Id, and you would have no way to reach them in the future. Or a user could type in an arbitrary E-Mail Id to just get past the signup page, and this is bad too. It's definitely a good idea to check on signup, and this does hinder the initial process, so keep as many features of your site free-access(without login) as possible. Or atleast tell what your site is all about and give a feel of it, if you want your first-time user to go to the trouble of confirming their E-Mail Id.[GR-3] 

 7. What should I do if my user forgot their password?

    Tradionally this has been handled by asking the username and sending a mail(with the actual password, sometimes) to the registered mail id. This has real problems: Sending passwords via E-Mail is a bad practice as the connection between your server and the E-Mail account could be un-encrypted and the E-Mail Client can have a local storage of mails which would expose your actual password to attackers. Another bottleneck is that people tend to forget their usernames, but remember their E-Mail Ids better(as they usually have just one or two of them). Hence, a better way would be to get their mail id and see if there is a registered user with this mail id and if yes, to send a reset password link to the mail id[Don't even tell if the Mail Id does not exist on your database, simply say that the mail has been sent]. Once the user clicks on the link in his E-Mail, they should be directed to a page which asks their secret question and then asks for the New Password. Also, it's good to send a confirmation mail to the E-Mail account saying that the Password has been changed.


 8. People talk about salt and other edibles but can hackers only access db? Can they not attack servers?


 9. What is hashing? Why do they say that it is one way street?

    Hashing is something like a magic trick, where you take a string(data) and cast a magic spell on it and it somehows becomes exactly N bits. For example, a simple hashing algorithm could be MOD 7. Suppose you give an input of 8, it will return 1. It's basically giving the remainder when you divide by 7. It is a one way street as there a zillion possibilities of input string and zillion possibilities of hashing algorithm. For instance, in our own algorithm, given the 1, you would have a tough time figuring out if the input was 1 or 8 or 15 or 22 or ...just any multiple of 7 plus 1. All this, if you knew the actual hashing algorithm in the first place. When you hash a string, if several input strings hash to the same value, it is called a collision. Good hashing algorithms have very less collisions. 

 10. What information do I have to verify before sending a reset link email? Why do most websites just ask a mail id? Can it be that a mail is id compromised and the worst?

     E-Mail id is enough, provided you check with a secret question on clicking of the link, you could even ask the secret question here at the time of asking E-Mail Id.


 11. Obviously email verification is one that happens. Question is, is it enough?



 12. Why should reset link have timeouts?

     Just a logical way to ensure, the email account is not hacked. However if this were the case, then the E-Mail account would have been hacked, and then the "Forgot Password" would have been clicked. Not really helpful in this sense.
Another viewpoint, is if you have reset your password, then the link should be disabled. This is so because many E-Mail clients have local system storage of mails(in plain text), and having access to that link for prolonged time is only inviting trouble. Thus, it is recommended to have some reasonable timeout so that the intended person can reset and the link goes dead as soon as possible.


 13. What is an ideal timeout time?

     This is debatable, but anything above 5 mins, and anything below 3 days could suit your needs. 


 14. Ok, the user clicked on the link, what next?

     Always remember to de-activate the link when the click has been clicked once. 
Also, there was this site called 4shared.com, where I never bothered to reset the password, as the Forgot Password link always sent me to a page where I was logged in. I never had to type a New Password at all; don't do this. I could use it for 30 days. Clearly, after 30 days, I needed to get another reset password link. So, as soon as someone clicks on your "Forgot Password" link, deactivate the link and ask for a new password before logging them in. 


 15. Ok, the user reset their password, should they re-login or directly be logged in?

     Even after asking for a New Password it's always good to ask them to re-login, so they can have a chance to memorize their password(if it's a sensitive website).


 16. Ok, the user reset the password, what happens to the record in db?

     Invalidate the reset password token but keep the history. In this way if repeated forgot passwords links are clicked by same user, or if humongous "Forgot Password" requests come up, then we can keep a closer eye and stop further requests from that IP.


 17. So, everything rests on my mail account? That's scarry! What happens if someone gets access to my mail account?

     True, if you haven't clicked the link soon, AND the mail messages are stored locally, AND if you have a not-so-talented-but-determined individual, they would be able to change your password, and lock you out of your account. By the time you realize this and rescue your account, it could just be too late. The same would be the case, for a website which sends a Reset Password link based on just mail id input. Imagine the same  not-so-talented-but-determined individual going to a bunch of important websites and keying in your mail id to lock all your accounts. That would indeed be terrible. Turns out we have already solved this problem.


 18. Some sites have secret questions on clicking on link. How should a secret question be?

     Yes, this seems a good way to prevent attacks from people who have access to your E-Mail accounts/to your local system and know your E-Mail Id. Obviously you need to be smarter than Sarah in choosing your secret question(http://en.wikipedia.org/wiki/Sarah_Palin_email_hack). So, from a server implementor perspective, give questions that are not everyday conversation questions, and questions whose answers cannot be easily wheedled from the person.


 19. Where and how should we store these secret questions and answers for a user?

     Clearly we need to store them in some encrypted/hashed form. Or else, in the case of database intrusion, an atacker has the all the information on a silver platter.


 20. Do we need a username at all, in the login screen and forgot password screen?

     Sometimes, people forget the user name, while this serves as an extra layer of protection, this is more burden on the user.

 21. Who(IP) initiated the reset? Do we need to tell this in email? Some websites send a mail saying "A password reset was requested from IP(location: Australia)". Question is "Is this needed? Does this help for the user?"

     It does help, if the user had not requested the reset. Then again, he/she does not need the location to confirm that they never requested the reset. It would be a good idea to place a "Cancel this Request" link in the mail. Possibly a "Report this" would help too. 

 22. What are OpenID and OAuth?

     Remember the password for site xyz? Remember the password for site xyyz? Oh god, how many passwords do I have to remember? To solve this usability issue, openID and OAuth were invented. Both these attack the problem in different ways.
     OpenID  says: "Sign up with me, and just remember my site's password. In any site, you would find a sign in option with the OpenID provider and you would need to provide this one password to sign to any site." This didn't sell well across the internet, so the support for this is not complete.
     OAuth says: "Don't remember any new passwords, just remember your facebook/twitter password, and sign in with that password for all sites."
     OAuth is more widely used, more in terms of the pressure that the social media giants have tons of people already on their bandwagons and not providing an easy way to access your site for them, is just losing out on traffic. OAuth is also convenient in that you can get a authorize a website to access your facebook and all you would need is to be logged into facebook to use the website. A user can always revoke the rights given to the website to access your facebook profile.

 23. How should we handle wrong password entry? How many attempts should we give? Infinite?

     Probably 3. what happens after 3 attempts? How long would you stop the user from trying again?

 24. Ok, my code is all secure now, am I done? Not yet, who is your hosting? Who is having your database?

     Many of us use third party hosting and database storages. We all go by the untold assumption, that our third party providers are all secure and no database and server can be hacked. Indeed this is a huge huge assumption. Who can be trusted? It is important to think of these things. 

 25. Some websites send code to registered mobile number, if they hash the mobile number, then how can they un-hash it , to send me the code? Or do they store mobile number un-hashed, and hash only mail id and username?

******

End-user perspective
--------------------

 1. What is secure?

    Keep your OS password difficult as once someone knows your OS password, they can just go to Chrome and get all your saved passwords. 


 2. How should passwords be?


 3. Why do websites require passwords to be strong?

    In early days when encryption was used this was the way to prevent brute-force/dictionary attacks, however with the use of bcrypt and other cost-adaptive hashing algorithms, all passwords are considered strong. The fact that websites ask you to enter one uppercase letter, one special character etc is all a way to keep attackers believing that the brute-force attack would be harder.

 4. From a end-user perspective, how should a password be?

    These days almost all websites pose restrictions on how the password should be. Aside from that, avoid using the same password for multiple websites. 

 5. Ok, I typed my password and hit login, what happens next? Can't people snoop over the net connection and get my password?

    No, as mentioned in a question, there is something called HTTPS, that takes care of this.

 6. What's http and https?

    http is a protocol for transmission of data between the server and the client(you). Data sent using the http is not secure. https tries to address the security issues; https is just http along with SSL. https provides two things: identity verification of the server and proper encryption of data when transmitted over the network. So, if a site is using http when you give your password or other sensitive information, you might want to back off.

 7. Why can't I just get my old password by mail? Why do we have to reset at all?

    E-Mail clients store the mail messages in the local system. So when you get your old password and use it, the mail still remains in your system, and anyone who is determined could get it and use your account on the website.

 8. How many username-passwords do I have to remember?

    With Chrome, not so much. With OAuth, not so much. Several people have tried to solve this problem, but we still have a long way to go. 

 9. My chrome remembers all my passwords, and I don't click on forgot password ever. Is this safe?

    Much debate has gone into how secure this is. Considering that the logged in user can see the passwords in plain text, isn't it really bad. They have a made an improvement by asking for the OS password everytime you try to access the "Saved Passwords" screen. My question to Chrome is: If you are going to remember my password and auto-fill them anyway, why do you want to show me the saved passwords anyway? So even if Chrome takes down this option of seeing Saved Passwords, then again, Chrome is "helping" by auto-filling the username and passwords. So a person who has logged into your system cannot be stopped. In this case, it seems best to avoid both Auto-Fill and Save Password features. So basically storing passwords by any browser is only as safe as the OS login credentials are safe. Clearly, this is not very safe. For sensitive sites, use the "Never for this site" option. For other sites it is okay to save your passwords.

 10. Banking websites don't allow passwords to be stored, is this something that the website has imposed, or it something that chrome does?

     This is probably something that you have done, by selecting the "Never for this site" option, all password entries are tracked by Chrome.

******

Final question
--------------

All this seems abstractable, what are some good libraries out there that I use readily?


If you have had one of these questions yourself, stick around, we will find it out.

If you have another question, please raise an issue, and we will add them to the list. If you want to answer any question, please do a pull request.

*******

Refereces:
=========

HOWTO
-----

[1] http://www.akadia.com/services/ssh_test_certificate.html

Private key is stored encrypted using Triple DES(a method of encryption in which three keys are used) in the server.
Explains nicely for APACHE.

[2] https://docs.nodejitsu.com/articles/HTTP/servers/how-to-create-a-HTTPS-server

Explains for nodeJS.
cert.pem, key.pem.
Simple tutorial to get started.


General References:
------------------

[1] http://en.wikipedia.org/wiki/Triple_DES

Highly recommend the references section, good links.

[2] http://cs.jhu.edu/~sdoshi/crypto/papers/p465-merkle.pdf

Paper discussing security of double and triple DES(using only 2 keys).

[3] http://stackoverflow.com/questions/1931769/why-is-it-important-to-do-email-verification-upon-sign-up-and-is-it-mandatory
[3] https://support.office.com/en-us/article/Encrypt-e-mail-messages-84d7e382-5f76-4d71-8705-324489b710a2?ui=en-US&rs=en-US&ad=US

Email encryption. You would need to get a Digital ID for about 20 dollars. This is basically SSL for mails; bear in mind both sides need certificates to send encrypted data.

[4] https://www.youtube.com/watch?v=jwslDn3ImM0

Intro to Re-captcha. Funny.

[5] http://www.shieldsquare.com/blog/sorry-google-captcha-recaptcha-doesnt-stop-bots/

Problems with reCaptcha.

[6] http://solvemedia.com/security/captcha

Problems with Captcha. It seems funny that there are captchas that we can't solve. Funny. Google agree. "CAPTCHA (a squiggly word with a box below it)."

[7] https://support.google.com/recaptcha/



Github repos
------------

[1] https://github.com/coolaj86/node-ssl-root-cas
[2] https://github.com/ptigas/simple-captcha-solver
