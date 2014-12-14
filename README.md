What should I know to implement a login system for my website?
==============================================================

I am implementing a login model for my website and went on an exploratory path on what is safe and what is not.

End-user perspective
--------------------

 1.) What is secure?

 2.) How should passwords be?

 3.) Why do websites require passwords to be strong?

 4.) From a end-user perspective, how should a password be?

 5.) Ok, I typed my password and hit login, what happens next? Can't people snoop over the net connection and get my password?

 6.) What's http and https?

 7.) Why can't I just get my old password by mail? Why do we have to reset at all?

 8.) How many username-passwords do I have to remember?

 9.) My chrome remembers all my passwords, and I don't click on forgot password ever. Is this safe?

10.) Banking websites don't allow passwords to be stored, is this something that the website has imposed, or it something that chrome does?


Server implementor's perspective
--------------------------------

 1.) Do I need to do anything special in my server to support SSL/misc?

 2.) Ok, the password came to my server safely. What do I do next?

 3.) How should I validate the username and password at the time of signup?

 4.) How should I validate the username and password at the time of login?

 5.) Do I need a captcha? 

 6.) How should I handle the username and password at the time of signup?

 7.) Do we need email verification at the time of signup?

 8.) How should I handle the username and password at the time of login?

 9.) What should I do if my user forgot their password?

10.) How should passwords be?

11.) People talk about salt and other edibles but can hackers only access db? Can they not access servers?

12.) What is hashing? Why do they say that it is one way street?

13.) What information do I have to verify before sending a reset link email? Why do most websites just ask a mail id? Can it be that a mail is id compromised and the worst?

14.) Obviously email verification is one that happens. Question is, is it enough?

15.) Why should reset link have timeouts?

16.) What is an ideal timeout time?

17.) Ok, the user clicked on the link, what next?

18.) Ok, the user reset their password, should they relogin or directly be logged in?

19.) Ok, the user reset the password, what happens to the record in db?

20.) So, everything rests on my mail account? That's scarry! What happens if someone gets access to my mail account?

21.) Some sites have secret questions on clicking on link. How should a secret question be?

22.) Where and how should we store these secret questions and answers for a user?

23.) Do we need a username at all, in the login screen and forgot password screen? 

24.) Who(IP) initiated the reset? Do we need to tell this in email?

25.) What are OpenID and OAuth?

26.) How should we handle wrong password entry? How many attempts should we give? Infinite?

27.) Ok, my code is all secure now, am I done? Not yet, who is your hosting? Who is having your database?

28.) Some websites send code to registered mobile number, if they hash the mobile number, then how can they un-hash it , to send me the code? Or do they store mobile number un-hashed, and hash only mail id and username?

Final question
--------------

All this seems abstractable, what are some good libraries out there that I use readily?


If you have had one of these questions yourself, stick around, we will find it out. 

If you have another question, please raise an issue, and we will add them to the list. If you want to answer any question, please do a pull request.
  