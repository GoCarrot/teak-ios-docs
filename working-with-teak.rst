.. highlight:: objc

Working with Teak on iOS
========================
The Teak SDK is exposed both via Objective-C as well as C.

Identify User
-------------
This tells Teak how the user should be referenced in the Teak system.

All Teak events will be delayed until ``identifyUser`` is called.

.. important:: This should be the same way that you identify the user in your system, so that when you export data from Teak, it will be easy for you to associate with your own data.

::

    - (void)identifyUser:(nonnull NSString*)userId

::

    - (void)identifyUser:(nonnull NSString*)userId
               withEmail:(nonnull NSString*)email

::

    - (void)identifyUser:(nonnull NSString*)userId
          withOptOutList:(nonnull NSArray*)optOut

::

    - (void)identifyUser:(nonnull NSString*)userId
          withOptOutList:(nonnull NSArray*)optOut
                andEmail:(nullable NSString*)email

::

    void TeakIdentifyUser(const char* userId, const char* optOutJsonArray, const char* email)

Parameters
    :userId: User identifier, required.

    :email: Email address to associate with the user, optional.

    :optOut: An array of privacy-related data items that should not be collected, optional.

        :TeakOptOutIdfa: Will not collect IDFA for the users. If this is not collected, you will not be able to add this user to Teak-created Ad Audiences.

        :TeakOptOutFacebook: Will not collect Facebook Access Token. If this is not collected, Teak will not be able to corilate users across multiple devices.

        :TeakOptOutPushKey: Will not collect push key. If this is not collected, Teak will not be able to send push notifications to this user.

.. note:: In the case of the C ``TeakIdentifyUser`` the ``optOutJsonArray`` parameter should be a JSON encoded array (or NULL). ``email`` can also be NULL.

Example
::

    [[Teak sharedInstance] identifyUser:@"someuserid"];

User Attributes
---------------
Teak allows you to add a limited number of attributes to users. A maximum of 16 string and 16 numeric attributes can be used.

::

    - (void)setNumericAttribute:(double)value
                         forKey:(NSString* _Nonnull)key

::

    void TeakSetNumericAttribute(const char* cstr_key, double value)

::

    - (void)setStringAttribute:(NSString* _Nonnull)value
                        forKey:(NSString* _Nonnull)key

::

    void TeakSetStringAttribute(const char* cstr_key, const char* cstr_value)

Parameters
    :key: The name of the user attribute.

    :value: Value to assign to the attribute.

Example
::

    [[Teak sharedInstance] setStringAttribute:@"unicorn_slots" forKey:@"last_played_slot"];
    [[Teak sharedInstance] setNumericAttribute:@123456 forKey:@"last_bet_amount"];

Notifications
-------------

`TeakNotification Class Documentation <objc_output/Classes/TeakNotification.html>`_

Scheduling a Notification
^^^^^^^^^^^^^^^^^^^^^^^^^

::

    + (nullable TeakNotification*)scheduleNotificationForCreative:(nonnull NSString*)creativeId
                                                      withMessage:(nonnull NSString*)message
                                                   secondsFromNow:(int64_t)delay

::

    TeakNotification* TeakNotificationSchedule(const char* creativeId, const char* message, int64_t delay)

::

    + (nullable TeakNotification*)scheduleNotificationForCreative:(nonnull NSString*)creativeId
                                                   secondsFromNow:(int64_t)delay
                                                       forUserIds:(nonnull NSArray*)userIds

:: 

    TeakNotification* TeakNotificationScheduleLongDistanceWithNSArray(const char* creativeId, int64_t delay, NSArray* userIds)

::

    TeakNotification* TeakNotificationScheduleLongDistance(const char* creativeId, int64_t delay, const char* inUserIds[], int inUserIdCount)

Canceling a Scheduled Notification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
::

    + (nullable TeakNotification*)cancelScheduledNotification:(nonnull NSString*)scheduleId

::

    TeakNotification* TeakNotificationCancel(const char* scheduleId)

Cancel All Client-Scheduled Notifications
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
This will cancel all of the notifications which were scheduled from client-side code using one of the ``scheduleNotificationForCreative`` variants.

::

    + (nullable TeakNotification*)cancelAll

::

    TeakNotification* TeakNotificationCancelAll()

