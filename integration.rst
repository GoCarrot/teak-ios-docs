Integration
===========

Dependencies
------------
The following frameworks are required by Teak

* AdSupport.framework
* AVFoundation.framework
* ImageIO.framework
* MobileCoreServices.framework
* StoreKit.framework
* UserNotifications.framework
* CoreGraphics.framework
* UIKit.framework
* SystemConfiguration.framework

This is the list of dependencies as compiler flags::

    -framework AdSupport -framework AVFoundation -framework MobileCoreServices -framework StoreKit -framework UserNotifications -framework ImageIO -framework CoreGraphics -framework UIKit -framework SystemConfiguration

Edit Info.plist
---------------
.. highlight:: xml

Add the following keys and values to your Info.plist file::

    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleTypeRole</key>
            <string>Editor</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>teakYOUR_TEAK_APP_ID</string>
            </array>
        </dict>
    </array>

.. note:: Replace ``YOUR_TEAK_APP_ID`` with your game's value.

    Your Teak App Id and API Key can be found in the Settings for your app on the Teak dashboard.

This will add the ``teak123456://`` URL scheme in order for deep link fallbacks to work properly.

Edit Entitlements
-----------------
You'll need to add your Teak subdomain to the associated domains in order for deep linking to work properly on iOS. Add the following to your entitlements::

    <key>com.apple.developer.associated-domains</key>
    <array>
        <string>applinks:YOUR_SUBDOMAIN.jckpt.me</string>
    </array>

.. note:: Replace ``YOUR_SUBDOMAIN`` with your game's subdomain.

Your Teak Subdomain can be found in the Settings for your app on the Teak dashboard.

Initialize Teak
---------------
Initialize Teak in your ``main.m`` file using ``[Teak initForApplicationId:withClass:andApiKey:]``, for example::

    // Step 1:
    // Import Teak into the main.m file to use the initialization method.
    #import <Teak/Teak.h>

    int main(int argc, char* argv[]) {
      @autoreleasepool {
        // Step 2:
        // Initialize Teak inside the @autoreleasepool but before UIApplicationMain() is called.
        [Teak initForApplicationId:@"YOUR_TEAK_APP_ID"                    // Use your Teak Application Id here.
                         withClass:[AppDelegate class]                    // Use the name of your main UIApplicationDelegate here.
                         andApiKey:@"YOUR_TEAK_API_KEY"];                 // Use your Teak API Key here.

        // Continue to our AppDelegate.m file for the next steps.

        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
      }
    }

.. important:: You must initialize Teak inside the ``@autoreleasepool`` but before UIApplicationMain() is called.

Teak also exposes this functionality via a C API::

    extern void Teak_Plant(Class appDelegateClass, NSString* appId, NSString* appSecret);

Identify User, and Add Observers
--------------------------------
.. highlight:: objc

As soon as your game knows how it will identify the current user in your own backend, you should tell Teak that identifyer.

Teak will wait until ``[identifyUser:]`` is called before it will send ``NSNotificationCenter`` notifications to inform you about events (covered later).

::
    // Step 3:
    // Import Teak into your UIApplicationDelegate implementation.
    #import <Teak/Teak.h>

    @implementation AppDelegate

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {

      // Register a deep link that opens the store to the specific SKU
      // Routes use pattern matching to capture variables. Variables are prefixed with ':', so ':sku' will create
      //    a key named 'sku' in the dictionary passed to the block.
      // Name and Description are optional, but will show up in the Teak Dashboard to help identify the deep link
      [TeakLink registerRoute:@"/store/:sku"
                         name:@"Store SKU"
                  description:@"Will open the In App Purchase for the specified SKU"
                        block:^(NSDictionary* _Nonnull parameters) {
                          NSLog(@"%@", parameters);
                          NSLog(@"IT CALLED THE THING!! SKU: %@", parameters[@"sku"]);
                        }];

      // Step 4:
      // In your game, you will want to use the same user id that you use in your database.
      //
      // These user ids should be unique, no two players should have the same user id.
      // Call identifyUser as soon as you know the user id of the current player.
      [[Teak sharedInstance] identifyUser:ASSIGNED_USER_ID];

      // Step 5:
      // Tell Teak that you want to be notified when your game has been launched via a Push Notification.
      //
      // See the bottom of this file for an example of a handler function.
      [[NSNotificationCenter defaultCenter] addObserver:self
                                               selector:@selector(handleTeakNotification:)
                                                   name:TeakNotificationAppLaunch
                                                 object:nil];

      return YES;
    }

    // This is an example of a handler function that will be called when your app
    // is launched from a Push Notification.
    - (void)handleTeakNotification:(NSNotification*)notification {
      NSLog(@"TEAK TOLD US ABOUT A NOTIFICATION, THANKS TEAK!");
    }

    @end

