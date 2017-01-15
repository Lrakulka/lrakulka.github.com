---
layout: post
title:  "Skype Web-turret"
date:   2014-06-14 00:00:00 +0100
categories: IoT Webcam Microcontroller SkyprAPI
previewImg: http://lrakulka.github.io/images/EfguIR5wruZfjfLPKfSB.jpg
info: Web-turret is a webcam which can be rotated in horizontal and vertical axes by commands from Skype chat.
---

![](/images/EfguIR5wruZfjfLPKfSB.jpg)

**About this project**

Web-turret is a webcam which can be rotated in horizontal and vertical axes by commands from Skype chat.

**Brief project description**

For the rotation of a webcam we need two engines and two reduction drives. We need a reduction drive to decrease the speed of the camera rotation and for increasing the rotation power. We need to use a microcontroller and a motor drive for the control engines (The Power of a microcontroller is not enough to supply the engines with the energy that is why we use a motor driver). Also we need to write a program for the microcontroller and for the Skype chat which can read the commands from the chat and send them to the microcontroller.

**The device for the rotation webcam**

I was lucky to find the engines and reduction drives in the toys.

![The motor and the reduction drive](/images/4YpiG1CVazxVKregACsy.jpg)

I faced a big problem during the creation of the device which can rotate in any direction. The problem was in the winding of the cable on the axis. I decided to use the bearings to solved this problem. I soldered one wire to the outer ring, and another to the inner ring but it didn't help because during bearing rotation the connection between webcam and a computer disappeared for a couple of milliseconds and it was enough for computer to disconnect the webcam and only the physical reconnection of the webcam helps to return the connection between the webcam and a computer.

![My fail experiment with the bearing](/images/x1jAkMgQPj2Fo3cP6SQJ.jpg)

In another try I decided to try the sliding contacts. I created from copper foil seven tracks for the sliding contacts but I had to stop on this position because of the quality of tracks. I understood that I need more qualification work than I could do.

![My second try with the sliding contacts](/images/k37RKlx8aNi3KkloaYjj.jpg)

I needed help from the outside. I created the scheme of the sliding contacts and found people who could created it for me (The process of gathering all together you can see below).

![1](/images/lAsx8A0nH3Ohi0grx5ef.jpg)
![2](/images/WfgS7xVsobpwUikzjtvf.jpg)
![3](/images/Kh8PW24rFuEUxnk0ixQR.jpg)
![4](/images/rJ9HhCu0bdsLFe7pBfCG.jpg)

(I covered the slighting contacts with the aluminium foil to protect against the electromagnetic fields)

This variant worked much better than the variant with the bearing but it had the same problem with the disconnection that is why I decided to refuse the full rotation.

**The engine controller**

I chose the breadboard AVR-USB-TINY45 which has microcontroller ATtiny85 and a nice USB connector.

![The breadboard AVR-USB-TINY45(85)](/images/WI6g82IPbzHcrpNTDxcc.jpg)

Also I created an adapter for programming the microcontroller because the breadboard AVR-USB-TINY45 has a connector ISP6 and my programator has ISP10.

![The adapter](/images/SqMJsLcgIzWHZz6VQ51i.jpg)

I used PROTEUS to create the scheme of the controller and for the simulation.

![The scheme and real look of the controller](/images/0g6c9aOeIEBroWlfhTv1.jpg)

**Program for the microcontroller**

The program is controls the motor driver by setting four legs of the microcontroller in logical one or zero. Also the program sets all legs to logical zero if it doesn't get new commands during 200 milliseconds (I did it for protection against looping of the last command)

```C
#define Max 230 
class Device {    
    // Class is uses as container for exchange information between PC and web-turret
    class dataexchange_t { 	
    public: 	   
	char b1;        
	char b2;     // We need only 4 bits to contain commands for all 4 legs but 
	char b3;     // I decided to use 4 bytes because it's more comfortable to use.
	char b4; 	
    } data;    
    class Position { 	
    public: 		
	float x; 		
	float y;    
    } positionTransform, cameraPosition;     
/* 
 * positionTransform - contains transformed coordinates 
 * cameraPosition - contains current camera position 
*/    
    HIDLibrary <dataexchange_t> hid; 
    unsigned char processTime;   // Variable is decreasing number of request to device
    FILE *f;                     // I save last position of the camera in this file
    int SendData(dataexchange_t *);  // This method sends commands to the device 
public:    
    bool Connect();                  
    void SetCameraPosition(short int *, short int *);   
    bool MoveCamera();    
    bool StopMoveCamera();    
    Device() { 	   
	processTime = 10; 	   
	(int&) data = positionTransform.x = positionTransform.y = 0; 	   
	f = fopen("CameraPosition.txt","r+"); 	   
	fscanf(f,"%f %f", &cameraPosition.x, &cameraPosition.y); 	
	// Limits check   
	if (cameraPosition.x < -Max || cameraPosition.y < -Max || 
	        cameraPosition.y > Max || cameraPosition.x > Max) 		   
	    cameraPosition.x = cameraPosition.y = 0;    
    }    
    ~Device() {    	   
	fclose(f);    
    } 
}; 
...
```

I created the specific coordinate axis system to control the camera rotating process. This system decreases amount of commands which are sent over the communication channel. The principle of this system is next: the user types how many degrees he wanted to rotate the camera from the current position then the program converts entered degrees to amount of commands that with an interval of 2 seconds must be sent to the device.

```C
...  
// My transformation of the camera position coordinates 
// Device has different speed of rotation that is why I use two constants  
// Axis X
if(*x > 0) 		
    positionTransform.x = (*x * 0.20832);  // The constant for clockwise rotation 
else positionTransform.x = (*x * 0.225); // The constant for counterclockwise rotation
// Axis Y
if(*y > 0) 		
    positionTransform.y = (*y * 0.20832);  
else positionTransform.y = (*y * 0.225);   
...
```

I made the device to rotate the camera in a horizontal direction first , and then in a vertically axis, I did so because I wanted to avoid overloading of the USB power lines by two motors working simultaneously.

**Skype**

I wanted to have possibility to control Web-turret remotely. I decided to use the Skype API to control the device and to transmit video and sound from web-camera. If you decided to use the Skype API you should download Skype4COM.dll and put it into Windows carnal directory (I used Windows 7 for this project).

I wrote the program for Skype which monitors Skype chat with the owner. If the owner write in chat with the web-turret command "0000", the program will start a video call and turn on the web-turret. The user can control the camera by typing the following commands in the chat:

StopMove - stops the web-turret rotation

MoveTo('X'@'Y') - rotation by the vertical(X) axis and the horizontal(Y) axis

VideoOn - turn on video

VideoOff - turn off video

> I must note that I use old version of Skype API (This version was outdated even in 2014).

```C
... 
char comandName[] = {"###"};        // Skype login of the owner  
char comandStart[] = {"0000"};      // Start command    
int timeWait = 500;                 // Maximum time of the owner inactivity   
CoInitialize(NULL);   
  
SKYPE4COMLib::ISkypePtr pSkype(__uuidof(SKYPE4COMLib::Skype));  // Create Skype object      
pSkype->Attach(6,VARIANT_TRUE); // Connection to Skype API   
   
_bstr_t comand;   
IChatMessagePtr curMessage, privMessage = NULL;   
COleDateTime startTime(time(NULL));     
// Monitoring of the Skype chat with owner
for (;;) { // Dummy loop 
    // Get last message from client	  
    curMessage = pSkype->GetMessages((_bstr_t)comandName)->GetItem(1); 
    // Get text from last message
    comand = curMessage->GetBody();
    // Check if message is new
    // Check for start command
    if ((privMessage == NULL || privMessage->GetId() != curMessage->GetId()) && 
            (!strcmp((char*) comand, comandStart)) &&
            (startTime.m_dt < curMessage->GetTimestamp())) { 
        privMessage = curMessage; 			
        curMessage->PutSeen(true); // Set message as read
	ControlDevice(pSkype, comandName, timeWait, privMessage);		 
    } else {
        Sleep(5000); 
    }  
} 
...
_bstr_t comand;   
IChatMessagePtr curMessage;   
int timeToSendMessage = (timeWait / 100) * 80;   
Device device;   
ICallPtr call;   
// Check if we making call to the owner then put down call
for (int i = pSkype->GetActiveCalls()->GetCount(); i != 0 ; i--) { 	  
    call = pSkype->GetActiveCalls()->GetItem(i); // Get all current calls 
    if (!strcmp((char*) (call->GetPartnerHandle()), comandName)) { 		              call->Finish(); // Put down call		  
        Sleep(2000); 		  
        break; 	  
    }   
}   
// Start call to the owner 
call = pSkype->PlaceCall(_bstr_t(comandName), L"", L"", L"");  
// Sent message to the owner
pSkype->SendMessage(_bstr_t(comandName), _bstr_t(L"Connection started")); 
// Connection to web-turret   
if (!device.Connect()) { 
    pSkype->SendMessage(_bstr_t(comandName), 
        _bstr_t(L"The web-turret not found"));	 
}  
// Wait until the owner take call 
for (int i = 0; i < timeWait && call->Status != clsInProgress; i++) {      
    Sleep(20);   
}
if (call->Status == clsInProgress) {     
    Sleep(2000); 	
    call->StartVideoSend();  // Turn video on (Sometimes not works)
    for(int time = 0; call->Status == clsInProgress;time++) { 	    	
        if(time >= timeWait) {  // If time of waiting ends then put down call 	                  call->Finish(); 			
            break; 		
        } 		
        curMessage = pSkype->GetMessages((_bstr_t)comandName)->GetItem(1); 		if (curMessage->GetId() != privMessage->GetId()) { //Catch the owners messages 	    privMessage = curMessage; 		  
            if (time > 0) { 	        
                time = 0; 
            }		  
            // Sent command to the web-turret
            if (!ComandToDevice((char*) curMessage->GetBody(),
                        &device, pSkype, comandName, &time, call)) {
                pSkype->SendMessage(_bstr_t(comandName), 
                        _bstr_t(L"The command not executed"));  
            }		  
            curMessage->PutSeen(true); 		
        } 		
        Sleep(200); 		
        if (!device.MoveCamera()) { 			
            pSkype->SendMessage(_bstr_t(comandName), 
                _bstr_t(L"Some problem with the web-turret"));	 		
            device.StopMoveCamera(); 		
        } 		
        if (timeToSendMessage == time) {			
                pSkype->SendMessage(_bstr_t(comandName), 
                        _bstr_t(L"Time of a command expecting is ended")); 
        } 
    } 
}     
pSkype->SendMessage(_bstr_t(comandName), _bstr_t(L"Connection is over"));  
...
```

<iframe width="638" height="365" src="https://www.youtube.com/embed/e8YDh1pZeuA" frameborder="0" allowfullscreen></iframe>

[Git source](https://github.com/Lrakulka/Web-turret)
