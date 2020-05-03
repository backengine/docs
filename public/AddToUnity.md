# Add Backengine to your Unity project

### Prerequisites

* Install or update Unity Editor to its latest version or at least 2018.4 lts version.
* [Sign in to Backengine](https://backengine.net) (If you don't have an account, just [register](https://backengine.net/login) one, it's FREE).

If you don't already have an Unity project and just want to try out Backengine, you can download one of our [quickstart samples](https://backengine/samples).

### **STEP 1:** Create a Backengine App 

Before you can add Backengine to your Unity project, you need to create a Backengine App to connect to your Unity project.

#### Create Backengine App
1. In the Backengine homepage, click `Dashboard`.
2. Login to dashboard with your account credentials.
3. In the Backengine dashboard, click `Apps`, then click `Create An App` or ![Plus Button](/images/plusbutton.png) button.
4. Insert you App information.
5. Click `Add App`.

Backengine automatically provisions resources for your Backengine App. When the process completes, you'll be taken to the overview page for your Backengine App

### **STEP 2:** Add Backengine Unity SDK

You can add Backengine Unity SDK to your Unity project using the Unity Assets Store, or you can install the SDK manually.

#### Using the Unity Assets Store

* In your Unity Editor, select menu `Windows/Asset Store` to open Asset Store tab.
* In Asset Store page, search `Backengine SDK` and go into it.
* Click `Download` then `Import` to import Backengine Unity SDK to your Unity project.

#### Manual installation

* In the Backengine homepage, click [SDKs](https://backengine.net/sdks) menu.
* Click Unity SDK icon to download lastest `Backengine Unity SDK` package.
* In your open Unity project, navigate to Assets > Import Package > Custom Package.
* Select the `BE_Unity_SDK.unitypackage`file.
* In the Import Unity Package window, click `Import`.

#### Add App secret to your Unity project

* Back to your Backengine App overview page
* You can see an `App Secret` under your App Name
* Copy that `App Secret` string.
* In your Unity project, select `BackConfig` in `BackEngine/Resources` folder.
* Paste `App Secret` string into `App Secret` field.
