Twilio Client for iOS SDK
= 

Getting Started
=

1.  Download and install Xcode 4.3 or higher.  
	1.  Install the iOS 4.3 Simulator and Command Line Tools (Preferences->Downloads->Components).
	1.  Install PackageMaker in /Applications (Xcode Menu->Open Developer Tools->More Developer Tools...->Auxiliary Tools for Xcode)
1.  clone this repo
1.  <code>cd twilio-ios-sdk; git submodule init; git submodule update</code> to pull in submodules.
1.  fork the twilio/markdown repo, then clone it.
	- Web versions of the API docs and quickstart live here
1.  fork the twilio/twilio-client-sdks repo, then clone it (if you're not the tech lead for Client), or just clone the repo (if you're the tech lead)
	- This repo is where the final builds of the SDK get placed for loading into production static web servers
1.  <code>cd sdk/openssl</code>
1.  <code>./build.sh</code>
1.  <code>open sdk/TwilioSDK/TwilioSDK.xcodeproj</code>
1.  Compile target "TwilioSDK"
1.  Explore the example apps, or the internal apps.
	- Note that the example apps are designed to be compiled and run from the final formal build of the SDK, and will not run from their locations in the source tree.  The internal apps can be compiled from their source director locations.
1.  Profit.


Documentation
=

- [Twilio Client Quick Start](http://www.twilio.com/docs/client/ios)
- Web API Docs
	- [TCDevice](http://www.twilio.com/docs/client/ios/TCDevice)
	- [TCConnection](http://www.twilio.com/docs/client/ios/TCConnection)
	- [TCDeviceDelegate](http://www.twilio.com/docs/client/ios/TCDeviceDelegate)
	- [TCConnectionDelegate](http://www.twilio.com/docs/client/ios/TCConnectionDelegate)
	- [TCPresenceEvent](http://www.twilio.com/docs/client/ios/TCPresenceEvent)
- [Twilio Client iOS Architecture](https://docs.google.com/a/twilio.com/document/d/17mtyJ5h2vZrQ4UgHUsHy97XlVhZBtjcU35e2qEvHQCI/edit)


Library Dependencies
=

- PJSIP
- OpenSSL
- AsyncSocket
- GTM (Google Tools)
- SBJSON (can be removed if the library only supports iOS 5 and above)


Tool Dependencies
=

- [appledoc](https://github.com/tomaz/appledoc) - generates Xcode-style documentation from the public Objective-C headers

Compatibility and Team Devices
=

Twilio Client for iOS works on devices running iOS 4.2 and higher.

To ensure compatibility of the SDK with iOS 4.2 and higher, the Xcode target for TwilioSDK has an "iOS Deployment Target" of 4.2.  In addition, code should be compiled and run either in an iOS 4 simulator (installed as a separate option with Xcode 4.3 and higher), or on an iOS 4 device.  Jeffiel's iPad (1st) generation still runs iOS 4.3 as of May 9, 2012.

Keep in mind that downgrading devices is a pain in the ass, if not impossible in some cases.  Doing so requires maintaining copies of the SHSH keys on your device for each release you want to downgrade to.  Only upgrade a device when absolutely necessary, and don't upgrade all devices at the same time to the same version of iOS.
 
iOS 5.1 compiling and device support requires OS X Lion and Xcode 4.3+.


Useful Dev Tools
=

- [classdump](http://www.codethecode.com/projects/class-dump/)
- [wireshark](http://www.wireshark.org/download.html)
- [MarkdownLive](https://github.com/rentzsch/markdownlive)
- [GitX(L)](http://gitx.laullon.com/)


Releases
=

Preparing Documentation
-

Documentation in the SDK comes in a few formats:

- Xcode documentation compiled from the public header files in the sdk/TwilioSDK/public/include directory (they're symlinks to keep things easier in the source tree).  This is packaged into the Xcode-style documentation using the tools/appledoc tool, and rolled up into an Installer.app module using PackageMaker.  The installer will drop docs into the correct location for them to be show up in the Organizer->Documentation pane within Xcode.
- PDF copies of the Getting Started Guide and Quick Starts are deposited into the .tar.gz bundle.  These PDFs are generated by going to their Google Docs equivalents [here](https://docs.google.com/a/twilio.com/document/d/1YHrTx2_zgpwDC80ggRqzhohclfcFaw64ayqGJFqO4xY/edit?hl=en_US) and [here](https://docs.google.com/a/twilio.com/document/d/1Rjoxl-EPX-Gvk9BNMQMpr3w2OVsKTkQ4sHoJuAVAmTg/edit?authkey=CJX5_ogP&hl=en_US), respectively, updating the contents, then printing to PDF, then committing those PDFs to the source tree.
- HTML versions of the API documentation and the Quick Start are in the markdown repository (markdown/docs/client/ios and ... I have no idea where the Quick Start lives, check with the Web team and update this doc please).  The API docs are extracted from the generated Xcode documentation in build/com.twilio.TwilioClient.docset/Contents/Resources/Documents and massaged to work in the format necessary to display on our website.  It's unfortunately a hand-rolled process at the moment.


Preparing a Build
-

1.  If needed, update the version number in `sdk/sdk-version.txt`.  This
    should just be the version stem, such as "1.2.0" or "1.2.0_rc1".
    The build script will automatically add a git revision tag, and (for
    debug builds) a date/time tag.
1.  From the SDK root, run `./tools/mksdk.sh'.  Build products will be
    in the 'build/' subdir.
1.  If new assets are added at the root level of the SDK:
    1.  Open the build/TwilioClient folder in Finder.  Make sure "View->As Icons" is checked in the Finder.
    1.  Clean up the display ordering within the window.
    1.  `cp build/TwilioClient/.DS_Store tools/DS_STORE` and check into
        git
    1.  Re-run the `mksdk.sh` script, this time passing `--no-clean
        --no-openssl` (to make the build go much faster).
1.  To upload to the mobiledev machine, run `./tools/upload-sdk.sh
    build`.
1.  If distributing (internally or externally), please also tag the
    release using `git tag $(./tools/print-cur-version.sh)`.

Testing the Build
-

1.  Perform a '<code>file libTwilioClient.a</code>' in the build/TwilioClient folder and make sure all 3 architectures are present (i386, armv6, armv7) on the static library.
1.  Copy the .tar.gz archive to a random spot on your hard drive, then compile and run either one of the quick starts or one of the examples.  This ensures we don't have any full paths from one of the development boxes or build machines as a dependency.
1.  Run the documentation installer to put the Xcode documents in the correct place, and verify that the latest versions show up in the Organizer->Documentation pane (may require a relaunch of Xcode).


Deployment
-

1.  Copy the .tar.gz file into your copy of the twilio-client-sdks repo and deposit it in the ios folder.
1.  Add/commit/push the file to your remote repo, then submit a pull request to the team lead to accept the changes in your fork.
1.  Notify the web team that the software's ready to go. 
1.  When the web static nodes have been deployed, verify that the links to the files work on each realm.  Links to the binaries are of a form <code>http://static.{realm}.twilio.com/sdk/ios/{file}.tar.gz</code>
	- The binary downloaded should open up, and you should be able to compile and run the provided examples. 
1.  Provide links to marketing and/or beta coordinator(s) as appropriate
1.  Finally, drop a tag on the twilio-ios-sdk git repo to mark the release.


TODOs
=
- set up openssl as a submodule of the tree, a la pjproject
- set up the iOS SDK to use git hash tag for packaging & build number, like the Android TC builds
- set up the build process to inject the build number into the code, rather than manually updating
- standardize on the naming scheme of the zip file.  should this include the major/minor version as well as build num?  probably.  should match android.
- convert the Google Docs + PDF versions of Getting Started and Quick Start guides into markdown.   Figure out if they should only be on the web, or if copies should also live with the SDK bundle as generated HTML.
- add simple tests to the build script to verify assets are present, the library contains all 3 architectures, etc.
- unit and functional tests, code coverage.  set up jenkins on the Mac build machine for this.
- make the Web API doc production scripted and easier to update.
