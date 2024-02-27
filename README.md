# BTEC_IoT_U19
Files and documentation for the BTEC Unit 19 IoT practical

## Step 1: Image the MicroSD Card

*To be added later by Jacob*

## Step 2: Install and set up the OS

*To be added later by Jacob*

## Step 3: Install Motion v4.3.2

*To be added later by Keiran*

## Step 4: The Python Script

### 4.2: Writing the script on a PC

Firstly, we need to import all the required libaries.
> import smtplib, ssl, os
> from email.mime.text import MIMEText
> from email.mime.multipart import MIMEMultipart
> from email.mime.image import MIMEImage

Then, we need to input our GMail credentials. *Note, the password is not the account password, but the app password.*
> email_address = "example@gmail.com"
> password = "xxxx xxxx xxxx xxxx"

We also need to define the recipients E-Mail address.
> recipient_email_address = "student_number@student.bcot.ac.uk"

Next, we have to set up the text of the email
> message_text = MIMEText("""\
> There has been motion detected from the camera!
> Please see the attached image for more details.""")

We need to then attach this text to the message, as we're later attaching an image.
> message.attach(message_text)

Now, we need to open, read and attach the image
> with open("\etc\lib\motion\snapshot.jpg", "rb") as f:
>     img_data = f.read()
> 
> image = MIMEImage(img_data, name=os.path.basename("snapshot.jpg"))
> message.attach(image)

Now we just have to so some SSL and SMTO stuff to send the E-Mail
> context = ssl.create_default_context()
> 
> with smtplib.SMTP_SSL("smtp.gmail.com, 465, context=context) as server:
>     server.login(email_address, password)
>     server.sendmail(email_address, recipient_email_address, message.as_string())