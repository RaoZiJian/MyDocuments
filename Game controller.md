#How to support game controller in Cocos2d-x

---

## Overview ##

We have added game controller feature since Cocos2d-x v3.2 rc0. This tutorial  will teach you how to support game controllers in iOS and Android.

## Game-Controller-Test ##
The sample test for game controller is in the path: `Cocos2d-x path/build/cocos2d_tests.xcodeproj`. Select the project `game-controller-test` and the target is the iOS Device which have connected your game controller.

![game-controller-text iOS](http://discuss.cocos2d-x.org/uploads/default/6646/949394e25469580c.jpeg)

Then compile and run the `game-controller-test`. You will see the appearance on your screen like the picture below. If you press any keys on your controller device. The corresponding keys on your screen will highlight.

![Mou icon](http://discuss.cocos2d-x.org/uploads/default/6647/e94806f5561af0c3.png)


##Game controller event##

Game controller is a event listener called `EventListenerController` in Cocos2d-x. There are 6 events of controller listener: `onConnected` ,`onDisconnected`, `onKeyDown`, `onKeyUp`, `onAxisEvent`, `onKeyRepeat`. 'onAxisEvent' is the event when the analog stick changed. The `KeyCode` refers to the key type in your interaction. The list and picture shows the `KeyCode` and its corresponding key of the controller. 

```cpp

    enum Key
    {
    	JOYSTICK_LEFT_X = 1000,
        JOYSTICK_LEFT_Y,
        JOYSTICK_RIGHT_X,
        JOYSTICK_RIGHT_Y,
        
        BUTTON_A,
        BUTTON_B,
        BUTTON_C,
        BUTTON_X,
        BUTTON_Y,
        BUTTON_Z,
          
        BUTTON_DPAD_UP,
        BUTTON_DPAD_DOWN,
        BUTTON_DPAD_LEFT,
        BUTTON_DPAD_RIGHT,
        BUTTON_DPAD_CENTER,

        BUTTON_LEFT_SHOULDER,
        BUTTON_RIGHT_SHOULDER,

        AXIS_LEFT_TRIGGER,
        AXIS_RIGHT_TRIGGER,

        BUTTON_LEFT_THUMBSTICK,
        BUTTON_RIGHT_THUMBSTICK,

        BUTTON_START,
        BUTTON_SELECT,

        BUTTON_PAUSE,
        KEY_MAX
    };

```
![Key Code](http://discuss.cocos2d-x.org/uploads/default/_optimized/3a6/62a/dec9ce3e5b_690x388.png)

There are 3 kinds statuses of a key. If `isAnalog` is true, the `value` might be a float from -1 to 1. If `isAnalog` is false, the `value` would be a centain number like -1 or 1. If depends on your key and the game controller. For example, if your key is `JOYSTICK_LEFT_X`, then its `isAnalog` would be true and the `value` is a float from -1 to 1 which represent your left joystick's X axis value.

```cpp
   typedef struct _keyStatus
    {
        bool isPressed;
        float value;
        bool isAnalog;
    }KeyStatus;
```

The codes below shows how to use these events in your iOS project.

```cpp

	void HelloWorld::registerControllerListener()
	{
		//create an evnetListenerController
	    _listener = EventListenerController::create();
	    
	    //bind onConneected event call function
	    _listener->onConnected = CC_CALLBACK_2(HelloWorld::onConnectController,this);

	    //bind disconnect event call function
	    _listener->onDisconnected = CC_CALLBACK_2(HelloWorld::onDisconnectedController,this);
	    
	    //bind onKeyDown event call function	    
	    _listener->onKeyDown = CC_CALLBACK_3(HelloWorld::onKeyDown, this);
	    
	    //bind onKeyUp event call function   
	    _listener->onKeyUp = CC_CALLBACK_3(HelloWorld::onKeyUp, this);

	    //bind onAxis event call function, onAxis will be called when analog stick is changed
	 	_listener->onAxisEvent = CC_CALLBACK_3(HelloWorld::onAxisEvent, this);
	    
	   	//Activate the listener into the event dispatcher
	    _eventDispatcher->addEventListenerWithSceneGraphPriority(_listener, this);
	    
	   	//This function should be called for iOS platform
	   	Controller::startDiscoveryController();
	}
	
	//Controller is the obejects of the Controller，keyCode means the keycode of the controller you click down
	void HelloWorld::onKeyDown(cocos2d::Controller *controller, int keyCode, cocos2d::Event *event)
	{
	    CCLOG("KeyDown:%d", keyCode);
	}
	
	void HelloWorld::onKeyUp(cocos2d::Controller *controller, int keyCode, cocos2d::Event *event)
	{
		//You can get the controller by tag, deviceId or devicename if there are multiple controllers
		CCLOG("tag:%d DeviceId:%d DeviceName:%s", controller->getTag(), controller->getDeviceId(), controller->getDeviceName().c_str());

	    CCLOG("KeyUp:%d", keyCode);
	}
	
	//The axis includes X axis and Y axis and its range is from -1 to 1. X axis is start from left to right and Y axis is bottom to top.
	void HelloWorld::onAxisEvent(cocos2d::Controller* controller, int keyCode, cocos2d::Event* event)
	{
	    const auto& keyStatus = controller->getKeyStatus(keyCode);
	    CCLOG("Axis KeyCode:%d Axis Value:%f", keyCode, keyStatus.value);
	}
	
	void HelloWorld::onConnectController(Controller* controller, Event* event)
	{		    
	    CCLOG("Game controller connected");
	}
	
	void HelloWorld::onDisconnectedController(Controller* controller, Event* event)
	{
	    CCLOG("Game controller disconnected");
	}
		
	bool HelloWorld::init()
	{
	    //////////////////////////////
	    // 1. super init first
	    if ( !Layer::init() )
	    {
	        return false;
	    }
	    
	    ......
	    ......
	        
	    registerControllerListener();
	    
	    return true;
	}


```

##How to integrate game controller to iOS devices 

###1. Connect device

To support game controller in iOS devices, you need `iOS 7.0` above. And you need to connect your controller to you iOS device. Different devices may have dissimilar methods to link. (Please read the instructions of your game controller). In this sample, we used PowerShell, which supported iPhone 5/5C/5S and iPod touch 5th generation. It is easy to connect PowerShell, just insert your devices in it.

![PowerShell](http://store.storeimages.cdn-apple.com/7792/as-images.apple.com/is/image/AppleInc/aos/published/images/H/E0/HE018/HE018?wid=800)

###2. Add GameController.Framework 

You need to add GameController.Framework (iOS 7.0 above). See the picture below.

![Mou icon](http://discuss.cocos2d-x.org/uploads/default/6649/1edfdce7d63272f4.jpeg)

###3. Write your controller event 

[a](#user-content-how-to-support-game-controller-in-cocos2d-x)

## How to integrate game controller to Android devices

###1. Connect device


