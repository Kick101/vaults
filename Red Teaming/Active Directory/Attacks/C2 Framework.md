### Implants
https://dominicbreuker.com/post/learning_sliver_c2_02_beacons_and_sessions/
#### Session mode
- Implants create a _permanent network connection from the target to the C2 server_. You can type commands and get immediate feedback, almost as if you would be on an SSH or PowerShell Remoting session. This is very convenient to work with, but of course such long-running connections might one day cause a few eyebrows to raise.

#### Beacon mode
- Implants create _temporary network connections back to the target at somewhat regular intervals_. In between, the implants go to sleep. If you interact with a beacon, your commands are not executed immediately. They will instead be queued up and executed next time the beacon checks in. This is a little bit less convenient as you’ll have to wait for the feedback but it may also look a bit less suspicious on the wire (depending on the environment, of course).





