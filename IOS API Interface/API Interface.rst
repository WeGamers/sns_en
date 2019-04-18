====================================
API Interface Call and Description
====================================

Call Example
==================

- Step 1: Configure parameters, call initialization, and detect community notification interface 

.. code-block:: c

    __weak typeof(self)weakSelf = self;  
    WeGamersSDKParams *gcParam = [[WeGamersSDKParams alloc] init];
    gcParam.sdkId = @"WeGamers embedded community assigned community SDK number";
    gcParam.sessionKey = @"WeGamers embedded community assigned community sessionKey";
    gcParam.gameAccountId = @"Game account";
    gcParam.nickName = @"Player’s nickname";
    gcParam.skinType = GameCommunityThemDark;//Five skin types. Choose default UI type here

    [GameCommunityEntry initGameCommunity:gcParam showCommunityRed:^(BOOL bShow) {
        [weakSelf showNotifyRed:bShow];//The game itself handles the red display dots’ UI callback block
    }supportGameCommunity:^(BOOL bSupport) {
        if (bSupport) {//Callback the current system environment here and check if it supports the game community function
            //Supports game community and the game entrance is revealed
        }
        else{
            //Game community not supported and the game entrance can be hidden
        }
    }];

.. code-block:: c

    //Register game community pop-up notification
    AppDelegate *delegate = (AppDelegate *)[UIApplication sharedApplication].delegate; 
    [GameCommunityEntry checkGameCommunityNotice:delegate.window completionBlock:^(NSError * _Nullable error) {
        //Detects if the community notification is successful
    }];

For more details on parameter types, please refer to the interface methods.

- Step 2: Open game community interface

.. code-block:: c

    - (void)onGotoCommunity:(id)sender
    {
        GameCommunityEntryResult* result = [GameCommunityEntry openGameCommunityHomePageAndwillExitLive:^{
            NSLog(@"Close game community");
        }];

        if (result.error && result.error.code == -1)
        {
            //TODO: Failed to open prompt
            //...
        } 
        else if (result.error && ((result.error.code == -2) || (result.error.code == -3)))
        {
            //TODO: Take out the home page view and pop-up on the specified page 
            UIViewController* liveHomePage = result.wgHomePageViewController;//... 
        }
    }

- Step 3: Set game embedded community that supports screen orientation

Add and implement methods in AppDelegate:

.. code-block:: c

    - (UIInterfaceOrientationMask)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
    {//AppDelegate 的supportedInterfaceOrientationsForWindow Process callback interface
        return [GameCommunityEntry appOrientationMask];//Or call return UIInterfaceOrientationMaskAllButUpsideDown; call UIInterfaceOrientationMaskAllButUpsideDown game's own interface needs to handle its own horizontal and vertical screen state
    }


Interface Description
========================

Include header file：#import <GameCommunitySDK/GameCommunitySDK.h>

- GameCommunityParam Parameter configuration

Detailed look into the following code commands:

.. code-block:: c

    typedef enum : NSInteger {
        GameCommunityThemWG = 0,        //WG skin
        GameCommunityThemPurple,        //Purple skin
        GameCommunityThemDark,          //Dark skin（Not configured, the default set of skin）
        GameCommunityThemLM,            ///Lords Mobile skin
        GameCommunityThemCC,            //Castle Clash skin
    } GameCommunityThemType;            //Five skin types

    @interface WeGamersSDKParams : NSObject
    @property(nonatomic, copy) NSString* sdkId;                     //WeGamers embedded community assigned community SDK number ID
    @property(nonatomic, copy) NSString* sessionKey;                //WeGamers embedded community assigned community sessionKey
    @property(nonatomic, copy) NSString* gameAccountId;             //Game account ID (account ID determined by the game itself)
    @property(nonatomic, copy) NSString* nickName;                  //Player’s nickname
    @property(nonatomic, assign) GameCommunityThemType skinType;    // Skin type
    @end

- Initialize interface

.. code-block:: c

    /**
    Game community initialization interface
    @param param parameter, please refer to WeGamersSDKParams
    @param showNotifyRedBlock Red dot notification callback “YES” means that there is a new message. “NO” means clear red dot display
    @param supportBlock Is the game community supported when returned to the current system environment
    */
    + (void)initGameCommunity:(WeGamersSDKParams*)param showCommunityRed:(void (^)(BOOL bShow))showNotifyRedBlock supportGameCommunity:(void (^)(BOOL bSupport))supportBlock;

Configure the parameters and call this initialization interface before the game opens the game community interface. showNotifyRedBlock is used to notify that the game UI community has new comments, notify about the red dot UI display, or hidden callbacks. 

- Detect game community notifications

.. code-block:: c

   /**In-game pop-up community notifications (call interface when pop-up is required)
   @param window Application main Window
   @param completionHandler Callback condition error nil means success, otherwise it means failure
   */
   + (void)checkGameCommunityNotice:(UIWindow *)window completionBlock:(void (^)(NSError * _Nullable error))completionHandler;

The game calls this interface where it needs to display the notification pop-up. It detects that there is a new notification message. The pop-up window displays the notification message to join the incoming window level. Click the pop-up window to enter the corresponding notification message.

- Open the game embedded community interface

.. code-block:: c

  /** Open game community page
  @return Open window result:
  1）Community home page view controller
  2） NSError object. Error code:
  -1，Indicates that the community home page view controller object creation failed
  -2，Indicates that the application main window not found
  -3，Indicates abnormal pop-up
  -4，Indicating that the parameter is filled in abnormally (may be empty)
  */
  + (GameCommunityEntryResult *)openGameCommunityHomePageAndwillExitLive:(void (^)(void))blockWillExit;

When the game taps on the community button, the interface is called. To close the community, use the callback block, blockWillExit.

- Prevent the game pop-up notification pop-up window to interrupt the game battle screen interface

.. code-block:: c

  /**
    Used to prevent the game from fighting when the checkGameCommunityNotice pop-up notification pop-up interrupts the game battle screen, the game manufacturer can call this interface when the game player interface is re-entered to prevent the notification window from interrupting the battle.Recalling the checkGameCommunityNotice battle state will clear
    Parameter description: bInComBat: YES enters the combat state, NO is the release of the combat state. Calling checkGameCommunityNotice again will automatically set NO.
 */
 + (void)setInComBat:(BOOL)bInComBat;

Engineering Code Change
=========================

In order to use the correct screen orientation, please refer to the following steps to call the relevant method to initialize and set up accordingly!

- Screen orientation supported by the initial setup: 

.. code-block:: c

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
        //Tell SDK program the supported screen orientation
        [GameCommunityEntry initAppOrientationMask:XXX];
    }

- Set the screen orientation supported by the app: used to support the game’s embedded community switch screen orientation. After closing the game community, the value will be changed to the initial screen orientation setting.

.. code-block:: c

    - (UIInterfaceOrientationMask)application:(UIApplication *)application supportedInterfaceOrientationsForWindow:(UIWindow *)window
    {//AppDelegate 的supportedInterfaceOrientationsForWindow Process callback interface
        return [GameCommunityEntry appOrientationMask];//Or call return UIInterfaceOrientationMaskAllButUpsideDown; call UIInterfaceOrientationMaskAllButUpsideDown game's own interface needs to handle its own horizontal and vertical screen state
    }
  





