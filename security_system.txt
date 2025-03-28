import cv2
import smtplib
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.image import MIMEImage
from datetime import datetime
import os
import requests

def send_email(image_path, receiver_email, login_attempts, location, is_owner=False):
    sender_email = "pythonminiproject24@gmail.com"  
    password = "ufjw fdqy bmyb bacw"  

    message = MIMEMultipart()
    message['From'] = sender_email
    message['To'] = receiver_email

    if is_owner:
        message['Subject'] = "Password Reset Request"
        body = (f"<b>HELLO..<br></b><p style='color:red;'>It seems like you've entered the wrong password.</p>"
                f"<br><p>Please <a href='https://example.com/reset-password'>click here</a> to reset your password.</p>"
                f"<br><p>Location: {location}</p>")
        message.attach(MIMEText(body, 'html'))
    else:
        message['Subject'] = "Unauthorized Access Attempt"
        timestamp = datetime.now().strftime("%d %B %Y at %H:%M:%S")
        body = (f"<b>HELLO..<br></b><p style='color:red;'>Warning: Unauthorized access attempt detected!.</p>"
                f"<br><p>Time of attempt: {timestamp}</p>"
                f"<br><p>Number of login attempts: {login_attempts}</p>"
                f"<br><p>Location: {location}</p>")
        message.attach(MIMEText(body, 'html'))

        if image_path is not None:
            with open(image_path, 'rb') as f:
                image = MIMEImage(f.read())
                message.attach(image)

    with smtplib.SMTP_SSL('smtp.gmail.com', 465) as server:
        server.login(sender_email, password)
        server.sendmail(sender_email, receiver_email, message.as_string())

def capture_image(folder_path, filename):
    if not os.path.exists(folder_path):
        os.makedirs(folder_path)

    cap = cv2.VideoCapture(0)
    ret, frame = cap.read()
    image_path = os.path.join(folder_path, filename)
    cv2.imwrite(image_path, frame)
    cap.release()
    
    return image_path

def detect_face(image_path):
    face_cascade = cv2.CascadeClassifier(cv2.data.haarcascades + 'haarcascade_frontalface_default.xml')
    img = cv2.imread(image_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    faces = face_cascade.detectMultiScale(gray, 1.3, 5)
    return len(faces) > 0

def get_location():
    try:
        response = requests.get('http://ipinfo.io')
        data = response.json()
        return f"{data['city']}, {data['region']}, {data['country']}"
    except:
        return "Unknown"

def main():
    folder_path = "captured_image"
    filename = "captured_image.jpg"
    captured_image_path = capture_image(folder_path, filename)
    is_owner_image = detect_face(captured_image_path)
    location = get_location()

    users = {
        "python": {"email": "pythonminiproject24@gmail.com", "password": "welcome"},
        "user2": {"email": "user2@gmail.com", "password": "password2"},
    }

    username = input("Enter your username: ")
    if username not in users:
        print("Invalid username.")
        return

    user_data = users[username]

    login_attempts = 0
    max_attempts = 3
    intruder_detected = not is_owner_image

    while login_attempts < max_attempts:
        user_input = input(f"Enter your password {username}: ")
        if user_input != user_data["password"]:
            login_attempts += 1
            attempts_left = max_attempts - login_attempts
            if attempts_left == 1:
                print("Last attempt left...")
            elif attempts_left == 0:
                print("Try again later")
            else:
                print(f"Incorrect password. Attempts left: {attempts_left}")
        else:
            print(f"Login successful, {username}!")
            if intruder_detected:
                send_email(captured_image_path, user_data["email"], login_attempts, location)
                print(f"Intruder detected but password guessed correctly. Image sent to {user_data['email']}.")
            return

    if is_owner_image:
        send_email(None, user_data["email"], login_attempts, location, is_owner=True)
        print("Owner detected. Password reset email sent.")
    else:
        send_email(captured_image_path, user_data["email"], login_attempts, location)
        print(f"Unauthorized access attempt detected for {username}. Image sent to {user_data['email']}.")

    cv2.waitKey(0)
    cv2.destroyAllWindows()

if __name__ == "__main__":
    main()
    
