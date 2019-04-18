====================
SDK Getting Started
====================

Usage Instructions
====================

.. image::  ../images/image12.jpg

- Step 1: Initialize the screen orientation supported by the game in the AppDelegate system and set the supported embedded community screen orientation. See Demo Project
- Step 2: SDK initialization interface

.. code-block:: c

    + (void)initGameCommunity:(WeGamersSDKParams*)param showCommunityRed:(void (^)(BOOL bShow))showNotifyRedBlock supportGameCommunity:(void (^)(BOOL bSupport))supportBlock;

- Step 3: Set in-game pop-up community notification (UI is supported by SDK, please set the content at the backend. The notification can be called in the game interface that requires this function. Suggest to detect the notification after the game finished the starting. Please DO NOT use polling and avoid to detect it at the tutorial and during the gaming process.)

.. code-block:: c

    + (void)checkGameCommunityNotice:(UIWindow *)window completionBlock:(void (^)(NSError * _Nullable error))completionHandler;

Function: Pop-up notifications are displayed in the game interface, players could see the relative content in the community after they click the pop-up window. It provides an efficient way for community operation staff to disseminate  important content in the community. (As shown below.)

.. image::  ../images/image13.jpg

- Step 4: Open the game embedded community interface

.. code-block:: c

    + (GameCommunityEntryResult *)openGameCommunityHomePageAndwillExitLive:(void (^)(void))blockWillExit

Call this interface in-game via game community event


Demo Project
=============

Demo: Please refer to the attached GameCommunityDemo project.