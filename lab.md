**Action Mailer Lab / Homework**

Implement reset password feature in simple login app (or app of your choice)

**Prerequisite:** 

-  The welcome email feature works
-  Action Mailer is configured and sending emails


**Requirements:**

- On signin view, user should be able to click a **forgot password link** rendering a forgot password view

- On the **forgot password view**, user should be able to enter and submit email address to request a new password

- System sends **reset password** email containing unique link to reset user's password on the site 


- On **reset password view**, user should be able to enter new password and password confirmation

- Upon success, user is logged in and shown profile page

**Tips:**

- Use the user's remember_token to generate unique reset password link in reset password email. The link should look something like this: 

```http://localhost:3000/reset/xmbCPqeCfqxossoOcxciSw```


- You will need at least three new routes backed up by three controller methods (tip: use user_controller)
  - A route responding to forgot password link, renders forgot password view
  - A route responding to send reset password email request
  - A route responding to reset password link contained in reset password email, renders reset password view with password/password confirmation inputs. Also display the user name.
  
- Also: reset password form in reset password view updates the user, i.e. it posts to user_controller update method, if it doesn't exist, create it


