>What should I know to implement a login system for my website?

I am implementing a login model for my website and went on an exploratory path on what is safe and what is not.

>End-user perspective:

What is secure?
How should passwords be?
Why do websites require passwords to be strong?
From a end-user perspective, how should a password be?
Ok, I typed my password and hit login, what happens next? Can't people snoop over the net connection and get my password?
What's http and https?
Why can't I just get my old password by mail? Why do we have to reset at all?
How many username-passwords do I have to remember?
My chrome remembers all my passwords, and I don't click on forgot password ever. Is this safe?
Banking websites don't allow passwords to be stored, is this something that the website has imposed, or it something that chrome does?


>Server implementor's perspective:

Do I need to do anything special in my server to support SSL/misc?
Ok, the password came to my server safely. What do I do next?
How should I validate the username and password at the time of signup?
How should I validate the username and password at the time of login?
Do I need a captcha? 
How should I handle the username and password at the time of signup?
Do we need email verification at the time of signup?
 Good for security, but usability overhead.
How should I handle the username and password at the time of login?
What should I do if my user forgot their password?
How can I implement login for nodejs?
What is secure?
How should passwords be?
People talk about salt and other edibles but can hackers only access db? Can they not access servers?
What is hashing? Why do they say that it is one way street?
What information do I have to verify before sending a reset link email? Why do most websites just ask a mail id? Can it be that a mail is id compromised and the worst?
Obviously email verification is one that happens. Question is, is it enough?
Why should reset link have timeouts?
What is an ideal timeout time?
Ok, the user clicked on the link, what next?
Ok, the user reset their password, should they relogin or directly be logged in?
Ok, the user reset the password, what happens to the record in db?
So, everything rests on my mail account? That's scarry! What happens if someone gets access to my mail account?
Some sites have secret questions on clicking on link. How should a secret question be?
Where and how should we store these secret questions and answers for a user?
Do we need a username at all, in the login screen and forgot password screen? 
Who(IP) initiated the reset? Do we need to tell this in email?
What are OpenID and OAuth?
How should we handle wrong password entry? How many attempts should we give? Infinite?
Ok, my code is all secure now, am I done? Not yet, who is your hosting? Who is having your database?
Some websites send code to registered mobile number, if they hash the mobile number, then how can they un-hash it , to send me the code? Or do they store mobile number un-hashed, and hash only mail id and username?

>Final question:

All this seems abstractable, what are some good libraries out there that I use readily?

If you have had one of these questions yourself, stick around, we will find it out.
