SimplePID
=========

A very simple Proportional-Integral-Derivative (PID) control system written in Java for game/physics engine control.

This video is a great intruction to PID control:
http://www.youtube.com/watch?v=UR0hOmjaHp0


Use Excerpt:
============

In the constructor, we set everything up.  Classes are from the LibGDX implementation of Box2D:
    
    //This controller adjusts the downward thrust magnitude of a flying saucer
    //from Flight of the Abductor (Ludum Dare 25)

    targetY = 0f;
  	thrustWeight = 2;  //Weighting factor for clamping
		levelThrust = getBody().getMass()*GameWorld.GRAVITY;  // The thrust needed for level flight
		
    //Instantiate the controller
    // P, I, D constants, integral/history size size
		thrustController = new SimplePID(1f, 0.05f, 210f, 16, 0.25f);

    // Set maximum/minimum outputs
  	thrustController.setClamping(thrustWeight*levelThrust, levelThrust*0.25f);

    // Set default offset - this simplifies math outside the class
		thrustController.setOffset(levelThrust);

So in the update loop, get feedback/set point and update the controller:

    thrustController.update(pos.y, targetY, deltaTime);

  	tScratch.y = thrustController.getOutput(); //Set thrust from controller output
		tScratch.x = 0;

    body.applyForceToCenter(tScratch);
