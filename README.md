
Mail intro

Sending email from Rails applications is easy thanks to ActionMailer but there are a few issues we can run into when using it. In this episode we’ll show you how it works and how to avoid some common problems. To demonstrate it we’ll use a simple application with name and email address fields (to keep this example simple there aren’t the usual password fields).

Create mailer:

	rails g mailer user_mailer signup_confirmation
	
	
This command creates an app/mailers directory with a user_mailer.rb file in it and we can use this to send out our signup confirmation email. The generator also creates a view for the message which contains some default text.

	/app/views/user_mailer/signup_confirmation.text.erb
	
	
Note that we can share instance variables between the view and the mailer itself much like we can with a controller.		


It’s important that the signup_confirmation method ends with a call to mail as this will generate the email and return it. We can pass a variety of options to this method including who we send it to and the message’s subject. A comment near the top of the class shows us that we can also set the subject in the internationalization file but we’ll set it directly here as we don’t need to support multiple languages.

mailer/user_mailer:


```
  def signup_confirmation
    @greeting = "Hi"

    mail to: "to@example.org", subject: "Sign Up Confirmation"
  end
```  
  
The API documentation shows us a list of all the options we can pass in to the mail method.

We still need to specify who to send the email to; this should be the email specified in the form. By design mailer classes don’t have access to request parameters so we’ll have to pass in the User model in different way. We’ll alter the signup_confirmation method so that it takes a user argument and pass the user in that way. We can then call user.email to get their email address. We’ll also set an instance variable to that user so that we can use it in the view.

```
def signup_confirmation(user)
  @user = user

  mail to: user.email, subject: "Sign Up Confirmation"
end
```

/app/views/user_mailer/signup_confirmation.text.erb

```
<%= @user.name %>,

Thank you for signing up.
```

Our email is now pretty much complete and we just need to send it from our controller. We could send email through a model observer or a callback but we’ll send the email in the controller so that we don’t unintentionally send any email when we’re interacting with the model in other ways.
  

In User:

```  
def create
  @user = User.new(params[:user])
  if @user.save
    UserMailer.signup_confirmation(@user).deliver
    redirect_to @user, notice: "Signed up successfully."
  else
    render :new
  end
end
```

Configuration:

/config/environments/development.rb

  
  
  

The API documentation shows us a list of all the options we can pass in to the mail method.