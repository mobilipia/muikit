# MuiKit

MuiKit (short for Mobile UI Kit) is a collection of iPhone code that aims to make programming on iPhone simpler, easier and more rewarding and effective.

That's mostly it. It's underdocumented and overhyped. It's all under a new-BSD-style license. Enjoy it.

## Embedding MuiKit in your own projects…

… is made needlessly complicated by a number of Xcode stupidites — *especially* if you want things to work both on device and on simulator. Sigh.

The steps are as follows:

 - **Before you start, set up the ∞labs build tools.**
   * Clone the repository at [http://github.com/millenomi/infinitelabs-build-tools/](http://github.com/millenomi/infinitelabs-build-tools/);
   * Set it up in Xcode by going into **Xcode &gt; Preferences &gt; Source Trees** and adding a new tree with the following data:
     - setting name: `INFINITELABS_TOOLS`
     - display name: "∞labs build tools" (or anything descriptive that you like)
     - path: *the full path to the repository clone above.*
 - Check out the source in a directory and make sure it builds.
 - Set up an interproject dependency between your new project and `MuiKit.xcodeproj`'s `MuiKit` target:
   * Drag `MuiKit.xcodeproj` into your project.
   * Select your target and choose File > Get Info.
   * Add the target named just `MuiKit` to the dependencies list (by clicking "+" under the list in the top part of the General pane of the window). *(There are other targets that start with MuiKit, eg `MuiKit (Resources)` — ignore them, they're built as part of the target you just set a dependency on. They're "implementation details" if you will :D)*
   * Keep the Get Info window open, because you need it to…
 - Set header search paths to include the headers in `{MuiKit directory}/Build/Headers`, and linker options to include Objective-C categories:
   * In the Get Info window from the previous step, switch to the Build pane.
   * Look for the "Header Search Paths" setting.
   * Add the following path to the setting `{MuiKit's source directory}/Build/Headers`, non recursive.
   * Look for the "Other Linker Flags" setting.
   * Add the `-ObjC` flag to the end of the setting.
 - Add resources and libraries to the application target:
   * Go back to the project window.
   * Locate `MuiKit.xcodeproj` and expand it with the arrow on its right.
   * Drag `libMuiKit.a` to your application target's Link With Libraries phase.
   * Drag `MuiKit.bundle` to your application target's Copy Resources phase.

… aaaand you're set. If you use MuiKit, **you will get a CodeSign build error if you choose a "Device" SDK from the Xcode pop-up** as the resources bundle is built. This is normal and as of 3.1.3 unavoidable. To build for the device instead, change your project's base SDK in the project's Get Info window to what you need, then **use the "Project Setting" item from the pop-up instead** — this will respect the overrides that MuiKit has to apply to the build system in order to avoid the error. The "Simulator" SDK works fine and does not trigger the error. (As an added bonus, MuiKit will be built for the simulator and used correctly as you would expect.)

To use any header from MuiKit, use:

	#import <MuiKit/MuiKit.h>

or similarly for individual `.h`s.
