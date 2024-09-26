## Solution 1 (JADX)
First of all, download JADX 
After that open the APK file attached to the shuffle
And examine the structure of the project

Often everything we need is stored in package ID in our case it is com -> KopohGames.Scheduler (com.KopohGames.Scheduler).
Since our first problem is the application login screen, we need to find this screen in the code of the decompiled application.
![Assets/1.png](Assets/1.png).
By examining the ==LoginScreen== file.
Find the code fragment that is responsible for login (named ==LoginResponse==).

	The lines of code that relate to the processing of logging into the application are marked with a blue square.

Find the lines that are responsible for the state of the variable ==LoginResponse==, which are taken from such an entity as ViewModel with the name ==LoginViewModel==.

	Using ViewModel is a standard application development practice, the MVVM pattern 
	The lines of reference to the ViewModel entity are marked with green squares.

Next we find the code section responsible for successful authorisation in the application (circled in magenta) 
![Assets/2.png](Assets/2.png)
Go to ==LoginViewModel== to study the login mechanism, here we find the function ==loginUser()==, which accepts login and password inside itself, we are on the right track.
![Assets/3.png](Assets/3.png)
We see an appeal to the ==LoginRepository== file.
Further clicking on the method ==userLogin(login, password)== we get a link to this method
![Assets/3.png](Assets/3.png)
Going to the method already inside ==LoginRepository==, we see the familiar ==LoginResponse== going down a little further, we see a conditional expression that literally means that if(login == ‘ZEROCTF’ && password == ‘Reverse is useless!’)
![Assets/4.png](Assets/4.png)

The login data for login is ZEROCTF and password is Reverse is useless!

Then we log into the application, we see that nothing is there, and nothing really happens.
![Assets/5.jpg|300](Assets/5.jpg)

But if you really want to, you can scroll to the 30th of October, and after that you can see a clue.
![Assets/6.jpg|300](Assets/6.jpg)
Or if you want very badly and study the mechanism of setting the schedule, you can find this hint in ==SharedViewModel==
![Assets/7.jpg](Assets/7.png)

Next we start exploring the settings screen ==SettingsScreen==, and we see a strange entity called ==KonamiCodeBox==, starting to explore this strange entity which has a lot of different code in it (well in fact it means that it is obviously responsible for most of the screen)
![Assets/8.jpg](Assets/8.png)
Examining ==KonamiCodeBox==, we see that the implementation of Konami code checking is done by swipes and taps, as a result, in order to understand what sequence of actions should be performed, we need to dig further into enum class ==Gesture==
![Assets/9.jpg](Assets/9.png)
![Assets/10.jpg](Assets/10.png)
To find the sequence we have to find all the uses of the class ==Gesture==, and we see a list that specifies the sequence of action swipes: left, right, left, right, top, them, tap, tap, tap.
![Assets/11.jpg](Assets/11.png)
We enter this sequence of gestures on the settings screen (may not work the first time)
And in the end you'll see this field 
![Assets/12.jpg|300](Assets/12.jpg)
And the flag is this 
```
kpkCTF{$ch3dule_w@s_t3ribb1e}
```

## Solution 2 (IDA)

If it can be solved via JADX/ApkTools and other tools for decompiling AKP files, then you can find the right places and code blocks in IDA too
