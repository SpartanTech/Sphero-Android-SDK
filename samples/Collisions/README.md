![logo](http://update.orbotix.com/developer/sphero-small.png)

# Collision Detection





These values are dependent on the types of collisions you want to detect.  For example, if you only want to detect serious impacts when the ball hits a wall you should set these parameters:

	Wall Hits: Xt=200, Xsp=0, Yt=125, Ysp=0, deadTime=100 (1 second)
	
These values suggest only pay attention to a power threshold over 125 in the y-direction. And a x threshold over 200 is too high to occur while driving.  The deadTime means that after the ball detects a collision, it will delay any more collisions for 1 second after.  This is to avoid multiple collision async packets from one collision.

Ball to ball collisions is a little bit tougher to determine, since Sphero may trigger collisions just driving around. However, these parameters will get you fairly accurate collisions at any point on the ball.

	Ball to Ball Hits: Xt=40, Xsp=60, Yt=40, Ysp=60, deadTime=40 (400 ms)
	
These values suggest a low collision threshold when you are not moving, and a higher one when Sphero is driving at full power. That way, we will minimize the false collisions when just driving around.  This also will trigger collisions on both axis.  We don't want the deadTime to be as high here, because if we get a false collision before an actual collision, we don't want to ignore the real one.

Collision Detection is enabled via the Configure Collision Detection command.  The Method field should be set to RKCollisionDetectionMethod1, and the X and Y axis impact thresholds should be set, along with the deadTime. The code to configure collision detection is:

    ConfigureCollisionDetectionCommand.sendCommand(mRobot, 		ConfigureCollisionDetectionCommand.DEFAULT_DETECTION_METHOD, Xt, Yt, Xspd, Yspd, deatTime);   
                                              
## Registering for Collision Async Data

As with data streaming, you have to register for async data notifications with this line of code after you configure the collision detection:

	DeviceMessenger.getInstance().addAsyncDataListener(mRobot, mCollisionListener);                                        

		public void onDataReceived(DeviceAsyncData asyncData) {
			if (asyncData instanceof CollisionDetectedAsyncData) {
				final CollisionDetectedAsyncData collisionData = (CollisionDetectedAsyncData) asyncData;
				// Analyze the Data
			}
				
	}


	final CollisionDetectedAsyncData collisionData = (CollisionDetectedAsyncData) asyncData;
	
	collisionData.hasImpactXAxis()
	collisionData.hasImpactYAxis();
		
	CollisionPower power = collisionData.getImpactPower();
	power.x;
	power.y;


	Acceleration acceleration = collisionData.getImpactAcceleration();
	acceleration.y;           
	collisionData.getImpactSpeed();

For questions, please visit our developer's forum at [http://forum.gosphero.com/](http://forum.gosphero.com/)