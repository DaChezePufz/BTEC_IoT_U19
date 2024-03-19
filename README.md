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

We need to define some MIME stuff:

    message = MIMEMultipart()
    message["Subject"] = "Motion has been detected"
    message["From"] = email_address
    message["To"] = recipient_email_address

Next, we have to set up the text of the email

    message_text = MIMEText("""\
    There has been motion detected from the camera!
    Please see the attached image for more details.""")

We need to then attach this text to the message, as we're later attaching an image.

    message.attach(message_text)

Now, we need to open, read and attach the image

    if os.name == "nt":
        picPath = "snapshot.jpg"
    else:
        picPath = "/var/lib/motion/snapshot.jpg"
    
    with open("picPath, "rb") as f:
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
> Pass: radxa

Next, open up Terminal with:

> Ctrl + Alt + T

## Step 5: Install Motion v4.3.2

Open up a terminal window and run the following command,

    sudo apt-get update

    sudo apt-get install motion

*Note, you may have to press enter if there is a prompt about using up disk space*

## Step 6: Copying program over to RockPi

Once you have written and tested the program on your PC, you should place it on an empty usb stick and transfer it to the RockPi.

You may need to mount the USB before you can access its contents, so if when plugging in a box appears in the bottom left of your screen, click on it and click the mount button next to the USB.

Open up a terminal window and type

    'ls /mnt
    ls /media/radxa/

You should see a singlar device listed, probably named "USB" then type

    'cd /mnt/*device*
    cd /media/radxa/USB/
    ls

You should see the Python program saved, as

> sendEmail.py

You will then want to run the following command

    cp sendEmail.py /etc/motion/sendEmail.py

*If you receive an error along the lines of: "cp: cannot create regular file '/etc/motion/sendEmail.py': Permission denied"; append sudo to the beginning:

    sudo cp sendEmail.py /etc/motion/sendEmail.py

This will copy the python program to the Rock.

## Step 7: Editing the Motion Config File

In a terminal window

    nano /etc/motion/motion.conf

Set Line 103 to

    on_event_end "python /etc/motion/sendEmail.py"

*Note, the default keyboard layout is American English, so the '@' and '"' keys will be swapped around.*

Set line 113 to

    picture_output on

Set line 116 to

    picture_filename snapshot

Set line 123 to

    movie_output off

## Step 8: Finalising

You will then have to run Motion with the following command:

    sudo motion

You can then go to "localhost:8080" in a web browser to check that the service is running and the camera has been detected.

Once you have completed all the previous steps you should be able to then check your emails and it should all be working.