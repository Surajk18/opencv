# Virtual Drag & Drog using OpenCV


In this project we will be using OpenCV to virtually drag a rectangle and drop it at a different location. It will be further used for Virtual Mouse.

Project Description:
In this project, I am using the live feed coming from the webcam of my system to drag and drop a rectangle on the screen. I am detecting two things: hand and a Click will be detected when the fore finger and the middle finger come close to each other. We will create a rectangle on the screen and then drag it and drop it to a different location.


Important Note:
I faced some issues during this project. The issues and their solutions are as follows:

FindPositon() not found: This is because you are running a newer version of cvzone. In newer versions, they removed findpositions and added findhands. So you need to install the version cvzone v1.4.1. You could click the above link or simply use "pip install cvzone==1.4.1".



# Virtual Zoom gesture


This is a python project based on real-time hand-gesture detection, to zoom in or out, using the distance between the right index finger and the right thumb from left index finger and left thumb. It's based on OpenCV, Mediapipe . MediaPipe offers open source cross-platform, customizable ML solutions for live and streaming media, created by Google.To start using this, clone this repository and run

pip install -r requirement.txt

Then, run virtualzoom.py to start camera and use your right index, thumb fingers and left index, thumb fingers to zoom in and out an image.

Press any key to exit.

# Virtual Keyboard

Implementation of Virtual Keyboard Using OpenCV
Let us create a virtual Keyboard.

First, let us install the required modules.

---> pip install numpy

---> pip install opencv-python

---> pip install cvzone

Here we are importing the HandDetector module from cvzone.HandTrackingModule and then in order to make the virtual keyboard 

cap = cv2.VideoCapture(0, cv2.CAP_DSHOW)
cap.set(3, 1280)
cap.set(4, 720)
Now let’s take real-time input from cv2.Videocapture

detector = HandDetector(detectionCon=0.8)
keyboard_keys = [["Q", "W", "E", "R", "T", "Y", "U", "I", "O", "P"],
                  ["A", "S", "D", "F", "G", "H", "J", "K", "L", ";"],
                  ["Z", "X", "C", "V", "B", "N", "M", ",", ".", "/"]]
final_text = ""
We initialize HandDetector with detection confidence of 0.8 and assign it to the detector. Then we create an array of lists according to the layout of our keyboard and define an empty string to store the typed keys.

Defining Draw Function
keyboard = Controller()
def draw(img, buttonList):
    for button in buttonList:
        x, y = button.pos
        w, h = button.size
        cvzone.cornerRect(img, (button.pos[0], button.pos[1],
                                                   button.size[0],button.size[0]), 20 ,rt=0)
        cv2.rectangle(img, button.pos, (int(x + w), int(y + h)), (255, 144, 30), cv2.FILLED)
        cv2.putText(img, button.text, (x + 20, y + 65),
                    cv2.FONT_HERSHEY_PLAIN, 4, (0, 0, 0), 4)
    return img
 

Initialize the keyboard controller, and define a function with name draw() and it takes two arguments that is an image and the buttonList and return the image. Here Inside the draw() function, we are using cvzone’s cornerRect function to draw rectangle edges at the corner of each keys. It is in order to make our keyboard layout look better. It will look something like the below images.

W key Virtual Keyboard Using OpenCV
You can also try changing different colours.

 

Inside the while loop the main function takes place, first we read the real-time input frames and store it in a variable called img. Then we pass that image to the detector.findHands() in order to find the hand in the frame. Then in that image, we need to find the position and bounding box information of that detected hand.

Here we can find the distance between the top point of our index finger and middle finger, if the distance between the two is less than a certain threshold, then we can type the letter on which we are indicating. Once we get the position then we loop through the entire position list. From that list, we find button position and button size and then we plot it on the frame according to a well-defined manner.

hand landmark model | Virtual Keyboard Using OpenCV
Image 1: Hand Landmark Model  
After that, we need to find the distance between the top point of our index finger and middle finger. In the above image, you can see the top points which we require are point 8 and point 12. Hence we need to pass 8, 12 inside a distance finding function in order to get the distance between them. In the above code you can see detector.findDistance() and there we passed 8, 12, and image in order to find the distance and we set the draw flag to false so that we do not need any line between the two points.

If the distance between the points is very less we will use press() function to press the keys. In the above code keyboard.press() and we are passing button.text in order to display that pressed key. And finally, we draw a small white rectangular box just below our keyboard layout in order to display the pressed key.

Once you execute the whole code it looks something like this.

final output | Virtual Keyboard Using OpenCV
After you bring the index finger and middle finger close to each other on top of a particular letter, you can type that letter.

use virtual keyboard 
If you need the keyboard layout to be more customized, we can make the keyboard layout transparent. We just need to add a transparent layout function and replace the draw() function with transparent_layout() function.

Let us define the transparent_layout() function. Below is the function, it takes the same input as that of the draw() function. Here we assign a numpy’s zero_like() function to a variable called imgNew and perform the desired operation on that, like having the corner rectangle, creating the rectangle box for each key, and putting the text inside the box. After that, we copy that image to a new variable and create a mask of imgNew and we use OpenCV’s addWeighted() function to place the mask on top of the actual image. Hence this makes the keyboard layout to be transparent.
