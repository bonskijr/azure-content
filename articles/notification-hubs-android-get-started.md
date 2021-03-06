<properties linkid="develop-notificationhubs-tutorials-get-started-android" urlDisplayName="Get Started" pageTitle="Get Started with Windows Azure Notification Hubs" metaKeywords="" description="Learn how to use Windows Azure Notification Hubs to push notifications." metaCanonical="" services="notification-hubs" documentationCenter="Mobile" title="Get started with Notification Hubs" authors="ricksal"  solutions="" writer="ricksal" manager="dwrede" editor=""  />
# Get started with Notification Hubs

<div class="dev-center-tutorial-selector sublanding"><a href="/en-us/manage/services/notification-hubs/getting-started-windows-dotnet" title="Windows Store C#">Windows Store C#</a><a href="/en-us/manage/services/notification-hubs/get-started-notification-hubs-wp8" title="Windows Phone">Windows Phone</a><a href="/en-us/manage/services/notification-hubs/get-started-notification-hubs-ios" title="iOS">iOS</a><a href="/en-us/manage/services/notification-hubs/get-started-notification-hubs-android" title="Android" class="current">Android</a><a href="/en-us/manage/services/notification-hubs/getting-started-xamarin-ios" title="Xamarin.iOS">Xamarin.iOS</a><a href="/en-us/manage/services/notification-hubs/getting-started-xamarin-android" title="Xamarin.Android">Xamarin.Android</a></div>

This topic shows you how to use Windows Azure Notification Hubs to send push notifications to an Android application. 
In this tutorial, you create a blank Android app that receives push notifications using Google Cloud Messaging (GCM). When complete, you will be able to broadcast push notifications to all the devices running your app using your notification hub.

The tutorial walks you through these basic steps to enable push notifications:

* [Enable Google Cloud Messaging](#register)
* [Configure your Notification Hub](#configure-hub)
* [Connecting your app to the Notification Hub](#connecting-app)
* [Testing your app](#run-app)
* [Send notifications from your back-end](#send)

This tutorial demonstrates the simple broadcast scenario using Notification Hubs. Be sure to follow along with the next tutorial to see how to use notification hubs to address specific users and groups of devices. 

This tutorial requires the Android SDK (it is assumed you will be using Eclipse), which you can download from <a href="http://go.microsoft.com/fwlink/?LinkId=389797">here</a>.


Completing this tutorial is a prerequisite for all other notification hub tutorials for Android apps. 

<div class="dev-callout"><strong>Note</strong> <p>To complete this tutorial, you must have an active Windows Azure account. If you don't have an account, you can create a free trial account in just a couple of minutes. For details, see <a href="http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fwww.windowsazure.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Windows Azure Free Trial</a>.</p></div>

##<a id="register"></a>Enable Google Cloud Messaging

<p></p>

<div class="dev-callout"><b>Note</b>
<p>To complete the procedure in this topic, you must have a Google account that has a verified email address. To create a new Google account, go to <a href="http://go.microsoft.com/fwlink/p/?LinkId=268302" target="_blank">accounts.google.com</a>.</p>
</div> 

1. Navigate to the <a href="http://cloud.google.com/console" target="_blank">Google Cloud Console</a> web site, sign-in with your Google account credentials, and then click **CREATE PROJECT**.

   	![][1]   

	<div class="dev-callout"><b>Note</b>
	<p>When you already have an existing project, you are directed to the <strong>Dashboard</strong> page after login. To create a new project from the Dashboard, expand <strong>API Project</strong>, click <strong>Create...</strong> under <strong>Other projects</strong>, then enter a project name and click <strong>Create project</strong>.</p>
    </div>

2. Enter a project name, accept the terms of service, and click **Create**. Carry out the requested SMS Verification, and click **Create** again.

3. Make a note of the project number in the **Dashboard** section. 

	Later in the tutorial you set this value as the PROJECT_ID variable in the client.

3. In the left column, click **APIs & auth**, then scoll down and click the toggle to enable **Google Cloud Messaging for Android** and accept the terms of service. 

	![][5]

4. Click **Credentials**, and then click **CREATE NEW KEY** 

   	![][2]

5. In **Create a new key**, click **Server key**. In the next window click **Create**.

   	![][3]

6. Make a note of the **API key** value.

   	![][4] 

Next, you will use this API key value to enable your notification hub to authenticate with GCM and send push notifications on behalf of your application.

##<a id="configure-hub"></a>>Configure your Notification Hub

1. Log on to the [Windows Azure Management Portal], and then click **+NEW** at the bottom of the screen.

2. Click on **App Services**, then **Service Bus**, then **Notification Hub**, then **Quick Create**.

   	![][7]

3. Type a name for your notification hub, select your desired region, and then click **Create a new Notification Hub**.

   	![][8]

4. Click the namespace you just created (usually ***notification hub name*-ns**), and then click **Configure** at the top.

   	![][9]

5. Click the **Notification Hubs** tab at the top, and then click on the notification hub you just created.

   	![][10]

6. Click the **Configure** tab at the top, enter the **API Key** value you obtained in the previous section, and then click **Save**.

   	![][11]

7. Select the **Dashboard** tab at the top, then click **View Connection String**. Take note of the two connection strings.

<!--   	![][12] -->

Your notification hub is now configured to work with GCM, and you have the connection strings to register your app and send push notifications.

##><a id="connecting-app"></a>Connecting your app to the Notification Hub

1. In Eclipse ADT, create a new Android project (File, New, Android Application).

   	![][13]

2. Ensure that the **Minimum Required SDK** is set to *API 8: Android 2.2 (Froyo)*, and that the next two SDK entries are set to the latest available version. Choose Next, and follow the wizard, making sure **Create activity** is selected to create a blank activity. Accept the default Launcher Icon on the next box, and click **Finish** in the last box.

   	![][14]

3. Open the Android SDK Manager by clicking **Window** from the top toolbar of Eclipse. Under the latest version of the Android SDK, choose **Google APIs**. Scroll down to **Extras** and choose **Google Play Services**, as shown below. Click **Install Packages**. Note the SDK path, for use in the following step. Restart Eclipse.

   	![][15]


4. Install the Google Play Services SDK in your project. In Eclipse, click **File**, then **Import**. Select **Android**, then **Existing Android Code into Workspace**, and click **Next**. Click **Browse**, navigate to the Android SDK path (usually in a folder named `adt-bundle-windows-x86_64`), then go to the `\extras\google\google_play_services\libproject` subfolder, and there select the google-play-services-lib folder, and click **OK**. Check the **Copy projects into workspace** checkbox, and then click **Finish**.

	![][29]

5. Next you must reference the Google Play Services SDK library that you just imported, from your project. Follow the instructions at [Referencing a library project].

6. Download the Notification Hubs Android SDK from <a href="https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409">here</a>. Extract the .zip file and copy the file notificationhubs\notification-hubs-0.1.jar to the \libs directory of your project in the Package Explorer.

7. Right-click on the project in the Package Explorer, and click **Properties**. Then click **Android** in the left-hand pane. Check the **Google APIs** target for the highest version of the SDK. Click **OK**.

   	![][16]

	Now, set up the application to obtain a *registrationId* from GCM, and use it to register the app instance to the notification hub.

8. In the AndroidManifest.xml file, add the following line just below the <uses-sdk/> element. Make sure to replace `<your package>` with the package you selected for your app in step 1 (`com.yourCompany.wams_notificationhubs` in this example).

        <uses-permission android:name="android.permission.INTERNET"/>
		<uses-permission android:name="android.permission.GET_ACCOUNTS"/>
		<uses-permission android:name="android.permission.WAKE_LOCK"/>
		<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

		<permission android:name="<your package>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
		<uses-permission android:name="<your package>.permission.C2D_MESSAGE"/>

9. In the **MainActivity** class, add the following statements.

	import android.os.AsyncTask;

	import com.google.android.gms.gcm.*;

	import com.microsoft.windowsazure.messaging.*;

10. Add the following private members at the top of the class.

	<div class="dev-callout"><b>Note</b>
    <p>Make sure to set the SENDER_ID to the Project Number you obtained earlier.</p>
    </div> 

		private String SENDER_ID = "<your project number>";
		private GoogleCloudMessaging gcm;
		private NotificationHub hub;

11. In the **OnCreate** method add the following code, and make sure to replace the placeholders with your connection string with listen access obtained in the previous section, and the name of your notification hub that appears at the top of the page in Windows Azure for your hub (**not** the full url).

		gcm = GoogleCloudMessaging.getInstance(this);
        
		String connectionString = "<your listen access connection string>";
		hub = new NotificationHub("<your notification hub name>", connectionString, this);
		
		registerWithNotificationHubs();

12. In MainActivity.java, create the following method:

		@SuppressWarnings("unchecked")
		private void registerWithNotificationHubs() {
		   new AsyncTask() {
		      @Override
		      protected Object doInBackground(Object... params) {
		         try {
		            String regid = gcm.register(SENDER_ID);
		            hub.register(regid);
		         } catch (Exception e) {
		            return e;
		         }
		         return null;
		     }
		   }.execute(null, null, null);
		}

13. Because Android does not display notifications, you must write your own receiver. In **AndroidManifest.xml**, add the following element inside the `<application/>` element.

	<div class="dev-callout"><b>Note</b>
    <p>Replace the placeholder with your package name.</p>
    </div> 

		<receiver
		    android:name=".MyBroadcastReceiver"
		    android:permission="com.google.android.c2dm.permission.SEND" >
		   <intent-filter>
		      <action android:name="com.google.android.c2dm.intent.RECEIVE" />
		      <category android:name="<your package name>" />
		   </intent-filter>
		</receiver>

14. Create a new class (right-click your app package in Package Explorer and click **New**, then click **Class**). Name the class **MyBroadcastReceiver**, derived from **android.content.BroadcastReceiver**.

   	![][17]

15. Add the following code to **MyBroadcastReceiver**, and resolve any errors by hovering over it and choosing the option to add the appropriate **import** statements.

		public static final int NOTIFICATION_ID = 1;
		private NotificationManager mNotificationManager;
		NotificationCompat.Builder builder;
		Context ctx;
		
		@Override
		public void onReceive(Context context, Intent intent) {
		GoogleCloudMessaging gcm = GoogleCloudMessaging.getInstance(context);
		        ctx = context;
		        String messageType = gcm.getMessageType(intent);
		        if (GoogleCloudMessaging.MESSAGE_TYPE_SEND_ERROR.equals(messageType)) {
		            sendNotification("Send error: " + intent.getExtras().toString());
		        } else if (GoogleCloudMessaging.MESSAGE_TYPE_DELETED.equals(messageType)) {
		            sendNotification("Deleted messages on server: " + 
		                    intent.getExtras().toString());
		        } else {
		            sendNotification("Received: " + intent.getExtras().toString());
		        }
		        setResultCode(Activity.RESULT_OK);
		}
		
		private void sendNotification(String msg) {
		mNotificationManager = (NotificationManager)
		              ctx.getSystemService(Context.NOTIFICATION_SERVICE);
		      
		      PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
		          new Intent(ctx, MainActivity.class), 0);
		      
		      NotificationCompat.Builder mBuilder =
		          new NotificationCompat.Builder(ctx)
		          .setSmallIcon(R.drawable.ic_launcher)
		          .setContentTitle("Notification Hub Demo")
		          .setStyle(new NotificationCompat.BigTextStyle()
		                     .bigText(msg))
		          .setContentText(msg);
		      
		     mBuilder.setContentIntent(contentIntent);
		     mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
		}


##<a name="run-app"></a>Testing your app

You can test this app with an actual Android phone, or with the emulator. When you run it in the emulator, make sure that you use an Android Virtual Device (AVD) that supports Google APIs.

1. From **Window**, click **Android Virtual Device Manager**, select your device, and then click **Edit**.

   	![][18]

2. Select **Google APIs** in **Target**, then click **OK**.

   	![][19]

3. On the top toolbar, click **Run**, and then select your app. This starts the emulator and run the app.

4. The app retrieves the *registrationId* from GCM and registers with the Notification Hub.

	<div class="dev-callout"><b>Note</b>
    <p>In order to receive push notifications, you must set up a Google account on your Android Virtual Device (in the emulator, navigate to <strong>Settings</strong> and click <strong>Add Account</strong>). Also, make sure that the emulator is connected to the Internet.</p>
    </div> 

##<a name="send"></a>Send notification from your back-end

You can send notifications using Notification Hubs from any back-end using our <a href="http://msdn.microsoft.com/en-us/library/windowsazure/dn223264.aspx">REST interface</a>. In this tutorial we will send notifications with a .NET console app, and with a Mobile Service using a node script.

To send notifications using a .NET app:

1. Create a new Visual C# console application: 

   	![][20]

2. Add a reference to the Windows Azure Service Bus SDK with the <a href="http://nuget.org/packages/WindowsAzure.ServiceBus/">WindowsAzure.ServiceBus NuGet package</a>. In the Visual Studio main menu, click **Tools**, then **Library Package Manager**, then **Package Manager Console**. Then, in the console window type:

        Install-Package WindowsAzure.ServiceBus

    and press Enter.

2. Open the file Program.cs and add the following using statement:

        using Microsoft.ServiceBus.Notifications;

3. In your `Program` class add the following method:

        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"msg\":\"Hello from Windows Azure!\"}}");
        }

4. Then add the following lines in your Main method:

         SendNotificationAsync();
		 Console.ReadLine();

5. Press the F5 key to run the app. You should receive a toast notification.

   	![][21]

To send a notification using a Mobile Service, follow [Get started with Mobile Services], then:

1. Log on to the [Windows Azure Management Portal], and select your Mobile Service.

2. Select the tab **Scheduler** on the top.

   	![][22]

3. Create a new scheduled job, insert a name, and select **On demand**.

   	![][23]

4. When the job is created, click the job name. Then click the tab **Script** in the top bar.

5. Insert the following script inside your scheduler function. Make sure to replace the placeholders with your notification hub name and the connection string for *DefaultFullSharedAccessSignature* that you obtained earlier. Click **Save**.

        var azure = require('azure');
		var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
		notificationHubService.gcm.send(null,'{"data":{"msg" : "Hello from Mobile Services!"}}',
    	  function (error)
    	  {
        	if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
	    );

6. Click **Run Once** on the bottom bar. You should receive a toast notification.

## <a name="next-steps"> </a>Next steps

In this simple example you broadcast notifications to all your Android devices. In order to target specific users refer to the tutorial [Use Notification Hubs to push notifications to users], while if you want to segment your users by interest groups you can read [Use Notification Hubs to send breaking news]. Learn more about how to use Notification Hubs in [Notification Hubs Guidance] and on the [Notification Hubs How-To for Android].


<!-- Images. -->
[1]: ./media/notification-hubs-android-get-started/mobile-services-google-new-project.png
[2]: ./media/notification-hubs-android-get-started/mobile-services-google-create-server-key.png
[3]: ./media/notification-hubs-android-get-started/mobile-services-google-create-server-key2.png
[4]: ./media/notification-hubs-android-get-started/mobile-services-google-create-server-key3.png
[5]: ./media/notification-hubs-android-get-started/mobile-services-google-enable-GCM.png

[7]: ./media/notification-hubs-android-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-android-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-android-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-android-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-android-get-started/notification-hub-configure-android.png
[12]: ./media/notification-hubs-android-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app.png
[14]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app2.png
[15]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app4.png
[16]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app5.png
[17]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app6.png

[18]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/notification-hubs-android-get-started/notification-hub-scheduler2.png
[29]: ./media/mobile-services-android-get-started-push/mobile-eclipse-import-Play-library.png

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /en-us/develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /en-us/develop/mobile/tutorials/get-started-with-data-android
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-android
[Get started with push notifications]: /en-us/develop/mobile/tutorials/get-started-with-push-android
[Push notifications to app users]: /en-us/develop/mobile/tutorials/push-notifications-to-users-android
[Authorize users with scripts]: /en-us/develop/mobile/tutorials/authorize-users-in-scripts-android
[JavaScript and HTML]: /en-us/develop/mobile/tutorials/get-started-with-push-js
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Windows Azure Management Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/en-us/library/jj927170.aspx
[Notification Hubs How-To for Android]: http://msdn.microsoft.com/en-us/library/dn282661.aspx

[Use Notification Hubs to push notifications to users]: /en-us/manage/services/notification-hubs/notify-users-aspnet
[Use Notification Hubs to send breaking news]: /en-us/manage/services/notification-hubs/breaking-news-dotnet

