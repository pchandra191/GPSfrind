# GPSfrind
Android Application based on the GPS integration, named "GPS Friend Finder"

Introduction
The App “GPS based Friend Finder” is a GPS service based application which would help us in locating the exact geo-position of people depending upon their current location/whereabouts. Geo-position would be displayed on the map-view on our android set and display functioning can analogue to the current usage of Google Maps. Some Key points about the App: All users’ locations would be retrieved from an online database so as to centrally control the permissions for viewing. For restricting user access, user authentication would be supported. Periodic refreshing has to be present so that each time the geo–location changes or after a fixed interval of time the values in database should be updated. All devices would be having a unique ID (UID) and this would be used for searching for the user. The app would have additional support in terms of
o Street View
o Getting Address from the Map
o Locating Multiple users (support for Multiple Pin Points)
o Zooming in / Zooming Out
o Application User Data Manipulation (password)
In our application, we have used Map Views as supported by Google APIs 10 or higher which would allow the use of app in devices starting from Gingerbread itself. We have used an Apache Server with PHP & MySQL support for remote database use. The data transaction from or to the database occurs with the help of PHP scripts and in the form of JSON objects. The android end of the app handles this JSON objects through HTTP clients. Onboard compass & map controllers are enabled. Locations are extracted from the device with the help of the GPS module available. A form of passive GPS use, the device decides on the best content with the information available from different providers. On touching the overlay on the map, options are asked ranging from extracting address to locating any other user on the same view.




	

Problem Statement
“”
A wide range of services that rely on users’ location information have been conceived, although the social networking applications are not yet mature. The main point is to remember that location is simply a useful bit of data that can be used to filter access to many types of geographical information services (GIS). There are numerous ways to exploit location to provide more relevant information, or derive new services. It can be particularly powerful when combined with other user profile information to offer personalized and location sensitive responses to users. Many governments are moving to require cellular operators to develop the capability to automatically identify subscribers’ locations in the event of an emergency. This data would then be forwarded to the appropriate public safety answering point (PSAP) to coordinate the dispatch of emergency personnel. We know what our government is not capable of delivering you the services at the time of emergency. What if you can locate someone nearby to for help in case of emergency? Think!!

















Objective:
To build an android application by importing gis repositories that locates friends nearby.
Methodology
1.
2
3
We need to focus on these things while creating an android application:
Activities
An activity represents a single screen with a user interface. For example, an gis app might have one activity that shows a map, another activity to locate a topography detail, and another activity for going through gps responses. Although the activities work together to form a cohesive user experience in the gis app, each one is independent of the others. As such, a different app can start any one of these activities (if the email app allows it). For example, a camera app can start the activity in the gis app that captures detailed images of a particular topography, in order for the user to share a picture.
Services
A service is a component that runs in the background to perform long-running operations or to perform work for remote processes. A service does not provide a user interface. For example, a service might download gis maps in the background while the user is in a different app, or it might fetch data over the network without blocking user interaction with an activity. Another component, such as an activity, can start the service and let it run or bind to it in order to interact with it.
Content providers
A content provider manages a shared set of app data. You can store the data in the file system, an SQLite database, on the web, or any other persistent storage location your app can access. Through the content provider, other apps can query or even modify the data (if the content provider allows it). For example, the Android system provides a content provider that manages the gis user's contact information. As such, any app with the proper permissions can query part of the content provider (such as ContactsContract.Data) to read and write information about a particular person.
Content providers are also useful for reading and writing data that is private to your app and not shared. For example, the Note Pad sample app uses a content provider to save notes.
Broadcast receivers
A broadcast receiver is a component that responds to system-wide broadcast announcements. Many broadcasts originate from the system—for example, a broadcast announcing that the screen has turned off, the battery is low, or a picture was captured. Apps can also initiate broadcasts—for example, to let other apps know that some data has been downloaded to the device and is available for them to use. Although broadcast receivers don't display a user interface, they may create a status bar notification to alert the user when a broadcast event occurs. More commonly, though, a broadcast receiver is just a "gateway" to other components and is intended to do a very minimal amount of work. For instance, it might initiate a service to perform some work based on the event.
 


The development of application is divided in four primary phases:
1.	Building a User Interface.
2.	Geological topography import via ArcGIS.
3.	Base algorithm to detect if anyone has logged in.
4.	Integration of an online database server to save login details.



Phase 1:
Building a User Interface
This can be achieved either by visual studio if you are working on windows or by android studio if you are concerned running application on an android platform. So let’s focus on what we are working on! It is an android application that we are developing right? In order to achieve that we need the following packages preinstalled on our system:
1.	Android Standard Development Kit
The Android SDK tools compile your code—along with any data and resource files—into an APK: an Android package, which is an archive file with an .apk suffix.
2.	Android Studio
You can always go for a visual application interface designer to create radio buttons, drop down menus, pop up notifications and many more.
 
Phase 2:
Geological topography import via Arcgis.
Import Gradle project into Android Studio
This is where we start to turn our project into an ArcGIS for Android project.

•	Right Click your project and select Open Module Settings
•	Click the + sign above SDK Location and select Import Existing Project then click Next
•	Navigate to the folder where you cloned the arcgis-android-api-lib-module repo and select the arcgis-android-v10.2.3 folder which contains the library module. Do not import the entire project, just the library module e.g. /[path-to-repo]/arcgis-android-api-lib-module/arcgis-android-v10.2.3 and click OK then Finish to import the library module.
For the above import we need to have arcgis repositories cloned to our disk via gradle.

Now we have to add details to our imported maps. We opt working on the following:
•	display a scale bar
•	display a map navigator
•	display a map legend
•	edit feature attributes
•	edit feature attachments
•	display pop-ups
•	display a slider for time-aware layers
•	display an overview map





Phase 3:
Source code to detect the location of someone who logged in.
private void initMyLocation() {
final MyLocationOverlay overlay = new MyLocationOverlay(this, map);
overlay.enableMyLocation();
overlay.enableCompass(); // does not work in emulator
overlay.runOnFirstFix(new Runnable() {
public void run() {
// Zoom in to current location
controller.setZoom(24);
controller.animateTo(overlay.getMyLocation());
}
});
map.getOverlays().add(overlay);
}

@Override
public void onLocationChanged(Location location) {
if (Location != null) {
lat = Location.getLatitude();
lon = Location.getLongitude();
GeoPoint New_geopoint = new GeoPoint((int) (lat * 1e6),
(int) (lon * 1e6));
controller.animateTo(New_geopoint);

}
Phase 4:
Integration of an online database server to save login details.
Our GPS-Tracker sends the location (latitude and longitude) and the ID of the device to the REST-Service, which runs on Bluemix. The REST-Service writes that information into the DB2, which runs on Bluemix too.
Our REST-Service has a web interface on which we can receive the location of a device according to its device ID. The communication between the application and the REST-Service is carried out by HTTP. The REST-Service uses SQL to interact with the DB2. The graphic below visualizes the simplified concept.
 










System Requirements:
To build a gis application for android we need:
	Windows	OS X/macOS	Linux
OS version	Microsoft Windows 10/8/7 (32- or 64-bit)	Mac OS X 10.8.5 or higher, up to 10.11.6 (El Capitan) or 10.12 (Sierra)	GNOME or KDE desktop
Main Memory	2 GB RAM minimum, 8 GB RAM recommended
Disk space	500 MB disk space for Android Studio, at least 1.5 GB for Android SDK, emulator system images, and caches
Java version	Java Development Kit (JDK) 8
Screen resolution	1280x800 minimum screen resolution

