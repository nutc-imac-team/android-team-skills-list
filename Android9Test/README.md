# Android 9 Version Function Test
## Environment
- mas os 10.13.3
- android 9(API28)
- Virtul Device Pixel 2
## Research 
- Lock down any app to a device
- Management employee 
- Avoid employee data theft

## That's Begin
### First.
Build a project 
### Second.
Create an xml like this :

![xml](https://raw.githubusercontent.com/chistine1018/android9/master/image/螢幕快照%202018-09-17%2020.16.00.png)

### Third.
The content of lock_screen.xml

![xml](https://raw.githubusercontent.com/chistine1018/android9/master/image/螢幕快照%202018-09-17%2019.42.17.png)

### Fourth.
Declare the receiver in Manifest.xml, in your <applicatio>n element :
  
![xml](https://raw.githubusercontent.com/chistine1018/android9/master/image/螢幕快照%202018-09-17%2019.41.59.png)

### Fifth.
Before you call startLockTask(), you need to allow the your app to pin the screen as device owner

### Sixth.

![xml](https://raw.githubusercontent.com/chistine1018/android9/master/image/螢幕快照%202018-09-17%2019.41.00.png)







