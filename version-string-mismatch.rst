.. _version_string_mistatch_email:

CFBundle(Short)VersionString Mismatch
=====================================
If the ``CFBundleShortVersionString`` or ``CFBundleVersionString`` in the notification service or content extension do not match their respective values in the host application, Apple will send an warning email.

**It is fine to ignore this warning.**

The reason for this warning is that the values of these keys, in the ``Info.plist`` files for all App Extensions and the main application, must all have the same value.

If you want to fix this issue, ensure that these values match.

Teak cannot help you resolve this issue as it pertains to your game's build process, and not to any factors under Teak's control.
