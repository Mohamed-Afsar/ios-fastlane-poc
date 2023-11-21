Fastlane iOS App Deployment
===========================

This repository serves as a Proof of Concept (POC) demonstrating the automation of building, signing, and deploying an iOS/Mac application using Fastlane tools. By following this POC, you can deploy both new and existing apps to TestFlight without directly interacting with the App Store Connect and Apple Developer portal.

System Requirements
-------------------

Ensure that your MacOS system has the following installed:

-   Ruby
-   Fastlane
-   dotenv
-   Xcode command line tools

Tip: Use Homebrew for installation if needed.
#### Example using Homebrew
`brew install fastlane`

Setup
-----

1.  Run `bundle init` in the project's root folder to create a Gemfile.

2.  Add the following line to the Gemfile:
 `gem "fastlane", "2.217.0"`
3. Run `bundle install` in the command line to install the required Gems. If there are issues with detecting the `fastlane` gem, consider installing it via Homebrew:
`brew install fastlane`

4. Execute `fastlane init` in the command line and follow the instructions to set up Fastlane. After setup, you should see the `fastlane` folder, `Appfile`, and `Fastfile` inside the project.

5. Create `.env`, `.env.ios`, `.env.mac`, and `.env.secret` files inside the `fastlane` folder. The latter, `.env.secret`, will be ignored from commits. Ensure you create your own `.env.secret` for experimentation.

    Expected Key-Value pairs in `.env.secret`:
 `FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD=<YourDevAppleIdAppSpecificPassword>`

Creating an App in App Store Connect
------------------------------------

1.  Create the `create_app` lane to make Fastlane create an app instance in App Store Connect using the `sync_code_signing` action.

2.  Run the following command to create the app instance:
 `fastlane create_app`

    Note: Ensure you have the necessary environmental variables in the `.env`.  You may need to input your App Store Connect account password during the first invocation.

Syncing Signing Certificates and Updating Signing Settings
----------------------------------------------------------

1.  Create the `sign` lane to make Fastlane create the certificate and profile in the developer portal, storing them in a private GitHub certificates repository.
2.  Add code to modify the project's code signing settings.
3.  You have to load the `.env.ios` before all of the iOS lanes. To do the same, use the dotenv tool and add the same to the Gemfile. 
`gem "dotenv", "2.8.1"`
4.  To have this dependency resolved you need to run the `bundle install` in the command line.
5.  Finally, run the following command to set up signing:
 `fastlane ios sign`

    Note: Ensure you have the necessary environmental variables in the `.env` and `.env.ios`. You may need to sign in to your GitHub account during the first invocation.

Building the Project
--------------------

1.  Create the `build` lane to invoke Fastlane's `build_ios_app` action, implicitly invoking the `sign` lane.
2.  An issue with this version of fastlane failed to build the apps with Multiplatform support. So we had to remove the iPad and MacOS support. You may have to do the same if you are trying this out in a different project.
3.  Run the following command to build the project:
`fastlane ios build`

    Note: Ensure you have the necessary environmental variables in the `.env.ios`.

Releasing the Project
---------------------

1.  Create the `release` lane to invoke Fastlane's `upload_to_app_store` action, implicitly invoking the `build` lane.

2.  Run the following command to release the project:
 `fastlane ios release`

    Note: Ensure you have the necessary environmental variables in the `.env` and `.env.ios` files. Additionally, add the Apple ID application-specific password to the `.env.secret` file. A dummy app icon is required in the asset catalog for the release.

Mac App Support
---------------

While this project primarily focuses on iOS, we have included the necessary Mac lanes and environmental variables for demonstration and reference purposes.

Git Actions
-----------

The `release` lane includes routine Git actions for seamless integration with the deployment process.
