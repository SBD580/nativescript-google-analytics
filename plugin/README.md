# Nativescript Google Analytics #
## [Release Notes](https://github.com/sitefinitysteve/nativescript-googleanalytics) ##
<iframe width="560" height="315" src="https://www.youtube.com/embed/5xIlbvT7j2g" frameborder="0" allowfullscreen></iframe>

## Add Plugin ##
```
tns plugin add nativescript-google-analytics 
```

## Get Config files ##
* [iOS instructions](https://developers.google.com/analytics/devguides/collection/ios/v3/#initialize-analytics-for-your-app)
* [Android instructions](https://developers.google.com/analytics/devguides/collection/android/v4/#add-screen-tracking)
* Click the "Get a Configuration File" instrutctions
* Add the platform specific config file you just downloaded to its respective App_Resources/{platform} folder

## Initalize the tracker in app.js ##
### PLAIN JS ###
``` js
var application = require("application");
var googleAnalytics = require("nativescript-google-analytics");
application.mainModule = "main-page";
application.cssFile = "./app.css";

if (application.ios) {
    //iOS
    var __extends = this.__extends || function (d, b) {
        for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p];
        function __() { this.constructor = d; }
        __.prototype = b.prototype;
        d.prototype = new __();
    };
    
    var appDelegate = (function (_super) {
        __extends(appDelegate, _super);
        function appDelegate() {
            _super.apply(this, arguments);
        }
        
        appDelegate.prototype.applicationDidFinishLaunchingWithOptions = function (application, launchOptions) {
            initAnalytics(); //Module Code to initalize
        };
        
        appDelegate.ObjCProtocols = [UIApplicationDelegate];
        return appDelegate;
    })(UIResponder);
    application.ios.delegate = appDelegate;
}else{
    //ANDROID
    application.on(application.launchEvent, function (args) {
        initAnalytics(); //Module Code to initalize
    });

}

application.start();

function initAnalytics(){
    googleAnalytics.initalize({
                trackingId: "UA-XXXXXXXX-1", //YOUR Id from GA
                //userId: "9ac7a034-ffde-4783-8374-f78b3df39d32", //Optional
                dispatchInterval: 5,
                logging: {
                    native: true,
                    console: false
                }
            });
}

```

#### Typescript ###
```js
var application = require("application");
import * as googleAnalytics from "nativescript-google-analytics";
application.mainModule = "main-page";
application.cssFile = "./app.css";

if (application.ios) {
    //iOS
    class MyDelegate extends UIResponder implements UIApplicationDelegate {
        public static ObjCProtocols = [UIApplicationDelegate];

        applicationDidFinishLaunchingWithOptions(application: UIApplication, launchOptions: NSDictionary): boolean {
            initAnalytics(); //Module Code to initalize
            return true;
        }

    }

    application.ios.delegate = MyDelegate;

}else{
    //ANDROID
    application.on(application.launchEvent, function (args) {
        initAnalytics(); //Module Code to initalize
    });

}

application.start();

function initAnalytics(){
    googleAnalytics.initalize({
                trackingId: "UA-XXXXXXXX-1", //YOUR Id from GA
                //userId: "9ac7a034-ffde-4783-8374-f78b3df39d32", //Optional
                dispatchInterval: 5,
                logging: {
                    native: true,
                    console: false
                }
            });
}

```

## Methods ##
### Initalize Options ###
``` js
// category and action are not optional, label and value are
googleAnalytics.initalize(
    {
      trackingId: "UA-XXXXXXX-1",
      userId: "(some userid value)", //Optional! Needs to be enabled on the tracking account: https://support.google.com/analytics/answer/3123666#FindTheUserID
      dispatchInterval: 30, //(Value in seconds)...Default Android is 30 minutes, default iOS is 2 minutes (120 seconds).  Disable by setting to 0.
      logging: {
         native: false, //Default false, should not be used in production
         console: false   
      }
    });
```

### Log Event ###
``` js
// category and action are not optional, label and value are
googleAnalytics.logEvent(
    {
      category: "MyCategory",
      action: "MyAction",
      label: "MyLabel",
      value: 7
    });
```

### Log ScreenView ###
``` js
// category and action are not optional, label and value are
googleAnalytics.logView("Secondary-Page");
```

### Flush Message queue (dispatch)  ###
``` js
googleAnalytics.dispatch();
```

### Log exceptions  ###
``` js
googleAnalytics.logException({
        description: "Cat tried to divide by 0...",
        fatal: true //Optional, default false... if true will be a "Crash" in GA.  False is an "Exception"
    });
//or
googleAnalytics.logException("Ergmagerd excerpshern");
```

### Log timings  ###
``` js
    //OPTION 1: Auto (Time is stored internally, just call stopTimer when you're done)
    googleAnalytics.startTimer("Logo Timer", {
                                category: "Animations",
                                name: "Rotate the logo", //Optional
                                label: (application.ios) ? "iOS" : "Android"  //Optional
                            });
    /* ... time passes as you do something ... */
    googleAnalytics.stopTimer("Logo Timer");
    

    //OPTION 2: Raw, calculate and send yourself
    googleAnalytics.logTimingEvent({
        category: "Animations",
        value: diffMilliseconds, //Milliseconds, example 4000
        name: "Rotate the logo", //Optional
        label: "Some Label" //Optional
    }); 
```

## Issues
#### Android
Error: Could not find com.google.android.gms:play-services-analytics:8.4.0

Resolution:
*Open the Android Studio SDK manager, make sure all of your build tools are up to date. Then make sure your Google Play Services and Google Repository packages are up to date. In the Android Studio sdk manager, you'll find these under the "SDK Tools" tab. If you are using the standalone sdk manager, you would scroll down to the "Extras" section at the bottom and update them there 
*
