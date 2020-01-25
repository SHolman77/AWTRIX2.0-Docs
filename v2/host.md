!> The server software is closed Source!

!> Please note that Java is not very resource friendly.  
AWTRIX will use round about 200MB of RAM. 160MB of it are only the Java Runtiume.   
If you want to use a Raspberry i recommend at least a Raspberry 3+ or better. It should run on a ZeroW but a bit slower. 


AWTRIX 2.0 can run on any platform (Windows, MacOS, Linux), the only requirement is the support of Java8. It is a non-GUI application so you doesnt need an desktop environment.   
This Tutorial describes the installation on a Linux machine.  

## **Beta**
I highly recommend the current Beta Server!  
[All about Beta Server](https://forum.blueforcer.de/d/295)  

You can use the installer by adding the beta parameter:  
 ```wget -N https://blueforcer.de/awtrix/awtrix.sh ; sudo sh awtrix.sh beta```  
Or via manual download: [AWTRIX BETA application](https://blueforcer.de/awtrix/beta/awtrix.jar)  



## **Raspberry Installer**
Enter following command into your terminal for automatic installation  
 ```wget -N https://blueforcer.de/awtrix/awtrix.sh ; sudo sh awtrix.sh```

?> Shortly after the start the web interface can be called via http://awtrix_ip:7000.  
You can also use this command to update your AWTRIX.  


## **Other platforms**

Install the latest [Java Runtime](https://www.oracle.com/technetwork/java/javase/downloads/index.html) 
  
Download the latest [AWTRIX Java application](https://blueforcer.de/awtrix/stable/awtrix.jar)

 **Linux & MacOS:**  
 ``` sudo java -jar awtrix.jar ```    
 **Windows:**  
 ``` java -jar awtrix.jar ```  

   
!> **sudo** is not always needed. It depends on the folder in which you want to start the Application. AWTRIX need to create new folders and files, so in few cases AWTRIX has no write permissions.

Shortly after the start the web interface can be called via http://awtrix_ip:7000.



## **Manual install on a Linux machine with Autostart**


First check if Java is installed  
```java -version```  
  
Otherwise install the latest Java Runtime :  
```sudo apt install default-jre```  

Set your timezone: e.g  
``` sudo timedatectl set-timezone Europe/Berlin```  


### **Download AWTRIX Server**

```sudo mkdir /usr/local/awtrix```  
```cd /usr/local/awtrix```    
```sudo wget https://blueforcer.de/awtrix/stable/awtrix.jar```


### **Autostart**

Create a file under  **/etc/systemd/system/** with nano or vi. eg.  
```sudo nano /etc/systemd/system/awtrix.service```  
  
Paste the code below in this new file:
```
[Unit]
Description=AWTRIX SERVER
After=network.target

[Service]
Type=simple
WorkingDirectory=/usr/local/awtrix/
ExecStart=/usr/bin/java -jar /usr/local/awtrix/awtrix.jar

[Install]
WantedBy=multi-user.target

```

If everything is working, enable the service with the command

```sudo systemctl enable awtrix```  


To run awtrix  
```sudo systemctl start awtrix ```   
To stop awtrix   
```sudo systemctl stop awtrix```   
To restart awtrix   
```sudo systemctl restart awtrix``` 


### **Update**  
```sudo -i```  
```cd /usr/local/awtrix```  
```systemctl stop awtrix.service```  
```wget -N awtrix.jar https://blueforcer.de/awtrix/stable/awtrix.jar```  
```systemctl start awtrix.service```  


## **Run AWTRIX on android device**
!> This needs a powerful device. Ive tested it on a Galaxy S8+ and it runs without any problems. This feature is not officially supported!

- [Download Termux](https://play.google.com/store/apps/details?id=com.termux) from Google Playstore
- open it and run follwing command:  
```pkg install wget proot -y && wget https://raw.githubusercontent.com/EXALAB/AnLinux-Resources/master/Scripts/Installer/Debian/debian.sh && bash debian.sh```
- start debian console with   
```./start-debian.sh```
- update debian   
```apt-get update```
- install Java   
```apt-get install default-jre```
- download AWTRIX   
```wget https://blueforcer.de/awtrix/stable/awtrix.jar```
- run AWTRIX   
```java -jar awtrix.jar```
