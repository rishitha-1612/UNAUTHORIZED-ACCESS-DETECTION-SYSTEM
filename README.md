ABSTRACT - This project focuses on enhancing user account security by implementing a system that detects unauthorized access attempts and takes immediate action. It combines basic user authentication with automated monitoring, image capturing, and email alerting functionalities to create a robust security application.If a user fails to input the correct password within three attempts, the application captures an image using the computer's webcam, saves it, and sends an alert email with the captured image attached to the registered email address of the user. The email notification includes a warning message about the unauthorized access attempt. This project leverages the OpenCV library for capturing images and the smtplib library for sending emails, making it a simple yet effective security measure for protecting user accounts.

INTRODUCTION - This project aims to enhance user account security by integrating multiple technologies to detect and respond to unauthorized access attempts. Utilizing Python, it combines user authentication, webcam image capture through OpenCV, and email notifications using smtplib. Upon detecting multiple failed login attempts, the system captures an image of the potential intruder and sends it to the registered user's email, thereby providing real-time alerts and reinforcing security measures. This approach not only prevents unauthorized access but also raises user awareness about potential security breaches.

KEY FEATURES -
1. User Authentication:
The application prompts the user to enter their username and password.It verifies the entered credentials against a predefined list of valid users.If the username is invalid, the system displays an "Invalid username" message and exits.
2. Login Attempts Management:
Users are allowed up to three attempts to enter the correct password.After each incorrect password entry, the system informs the user of the remaining attempts.A specific warning is given on the final attempt.
3. Unauthorized Access Handling:
After three failed login attempts, the system assumes an unauthorized access attempt.The computer's webcam is activated to capture an image of the person attempting access.This image is saved locally for further action.
4. Email Notification:
An email is prepared with the subject "Unauthorized Access Attempt" and a warning message in the body.The captured image is attached to the emailUsing the smtplib library, the system sends this email to the registered email address of the user associated with the login attempt.

TECHNICAL DETAILS - 
1. Image Capture: The OpenCV library is used to interface with the webcam and capture an image of the unauthorized user.
2. Email Sending: The smtplib library establishes a secure connection with the Gmail SMTP server to send the email with the captured image attached.
3. HTML Email: The email body is formatted using HTML to highlight the warning message in red, making it easily noticeable.
   
USER INTERACTION - 
1. Login Prompt:
Users are prompted to enter their username and password.The system provides feedback for incorrect attempts, including the number of attempts left and specific warnings on the last attempt.
2. Unauthorized Access Alert:
Upon reaching the maximum number of failed attempts, the system captures an image and informs the user that an email has been sent to their registered address.

OBJECTIVES - 
1. Security Enhancement: The project aims to add a security layer by detecting and responding to unauthorized access attempts.
2. Real-Time Monitoring: The system automates the detection of multiple failed logins attempts and takes immediate action.
3. User Notification: It ensures users are promptly informed of potential security breaches through real-time email notifications.
4. Integration Demonstration: The project showcases the practical integration of image processing (OpenCV) and email communication (smtplib) libraries in Python.
5. Educational Tool: It serves as a hands-on example for understanding and implementing security measures in a Python application.

CONCLUSION - 
Overall, this project presents a straightforward yet effective approach to improving account security by combining user authentication, real-time monitoring, and automated alerting through image capture and email notifications. The integration of OpenCV and smtplib demonstrates how these libraries can be used in practical security applications, making this project an excellent educational tool for learning and implementing basic security features in Python.
