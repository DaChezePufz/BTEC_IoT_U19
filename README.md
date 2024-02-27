# BTEC_IoT_U19

Files and documentation for the BTEC Unit 19 IoT practical

## Step 1: Creating the GMail account

The first step is to create a new GMAil account.

Once you have created this, go to the >Manage your account< Option

Then, go to the security tab, where we will have to enable 2FA/ 2 Step Verification.

Select 2-Step verification, then scroll down to App Passwords. Create a new one and keep track of the 16 digit password. If you loose this, delete the previous one and create a new one. You will use this later.

## Step 2: The Python Script

Firstly, we need to import all the required libaries.

    import smtplib, ssl, os
    from email.mime.text import MIMEText
    from email.mime.multipart import MIMEMultipart
    from email.mime.image import MIMEImage

Then, we need to input our GMail credentials. *Note, the password is not the account password, but the app password.*

    email_address = "example@gmail.com"
    password = "xxxx xxxx xxxx xxxx"

We also need to define the recipients E-Mail address.

    recipient_email_address = "student_number@student.bcot.ac.uk"

Next, we have to set up the text of the email

    message_text = MIMEText("""\
    There has been motion detected from the camera!
    Please see the attached image for more details.""")

We need to then attach this text to the message, as we're later attaching an image.

    message.attach(message_text)

Now, we need to open, read and attach the image

    with open("\var\lib\motion\snapshot.jpg", "rb") as f:
        img_data = f.read()

    image = MIMEImage(img_data, name=os.path.basename("snapshot.jpg"))
    message.attach(image)

Now we just have to so some SSL and SMTO stuff to send the E-Mail

    context = ssl.create_default_context()

    with smtplib.SMTP_SSL("smtp.gmail.com, 465, context=context) as server:
        server.login(email_address, password)
        server.sendmail(email_address, recipient_email_address, message.as_string())

## Step 3: Image the MicroSD Card

Retreive the image from the USB

> rock-4c-plus_debian_bullseye_kde_b60.img.xz

Using Balena Etcher, flash the image to the MicroSD card

## Step 4: Install and set up the OS

When booting up, you will be presented with the login screen, use the following credentials:

> User: radxa
> 
> Pass: radxa

Next, open up Terminal with 

> Ctrl + Alt + T

## Step 5: Install Motion v4.3.2

Open up a terminal window and run the following command,

    sudo apt install motion

*Note, you may have to press enter if there is a prompt about using up disk space*

## Step 6: Copying program over to RockPi

Once you have written and tested the program on your PC, you should place it on an empty usb stick and transfer it to the RockPi.

Open up a terminal window and type

    ls /mnt

You should see a singlar device listed, then type

    cd /mnt/*device*
    ls

You should see the Python program saved, as

> sendEmail.py

You will then want to run the following command

    cp sendEmail.py /etc/motion/sendEmail.py

This will copy the python program to the Rock.

## Step 7: Editing the Motion Config File

In a terminal window

    nano /etc/motion/motion.conf

Set Line 103 to

    on_event_end "python /etc/motion/sendEmail.py"

Set line 113 to

    picture_output on

Set line 123 to

    movie_output off
