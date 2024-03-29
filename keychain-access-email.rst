.. _keychain_access_email:

Potential Loss of Keychain Access
=================================
If your iOS App Id Prefix has been changed inbetween builds uploaded to the App Store, or you have used a different prefix for building the app than the app extension, you will receive an automated email from Apple that looks like this::

    Potential Loss of Keychain Access - The previous version of software has an application-identifier value of ['D6xxxxxx.xxxxxx.TeakNotificationContent'] and the new version of software being submitted has an application-identifier of ['Z2xxxxxx.com.xxxxxx.TeakNotificationContent']. This will result in a loss of keychain access.

    Potential Loss of Keychain Access - The previous version of software has an application-identifier value of ['D6xxxxxx.xxxxxx.TeakNotificationService'] and the new version of software being submitted has an application-identifier of ['Z2xxxxxx.xxxxxx.TeakNotificationService']. This will result in a loss of keychain access.

First and foremost, this is not an error; it is simply a warning. It will not have any impact on your game, unless you use keychain access, Handoff, or UIPasteboard sharing. Here is Apple's documentation on this issue: `https://developer.apple.com/library/archive/qa/qa1726/_index.html <https://developer.apple.com/library/archive/qa/qa1726/_index.html>`_.

**It is fine to ignore this warning.**

If you want to fix this issue, you need to investigate the process you use for building your game, and make sure that the Apple Team Id prefix is consistent for all targets, and across builds.

Teak cannot help you resolve this issue as it pertains to your game's build process, and not to any factors under Teak's control.
