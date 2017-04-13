Unwired LocationAPI Android Library
===================================

 

Introduction
------------

The Unwired Labs' LocationAPI locates all sorts of devices based on cell towers,
WiFi APs and IP addresses, anywhere in the world. LocationAPI works both indoors
& outdoors, when GPS isn't available or preferred (high battery drain).

The new Android library is a drop-in replacement to Google Play's Geolocation
services and offers the following benefits:

-   **Offline location** - now you can ask for the device's location and get a
    location in milliseconds even if the device has no or slow internet access.

-   **Higher accuracy** - Unwired's Geolocation triangulation & trilateration
    algorithms are *far superior* to Google's 'nearest-cell' based locations.
    This means no more locations in the sea and also, locations that are much
    closer to the device's real location.

-   **One-time permission request** - Unlike the Play Service library, the
    Unwired library works even when location is off, so your app can locate
    during events / emergencies, improving User Experience while ensuring that
    your location logic does not break. *Note that you must ensure the end user
    is aware of this feature and consents to its usage.*

-   \<coming-soon\> **HTTP callbacks to your server** - We can send the device's
    location along with some custom tags / data to your server straight from the
    library or our servers with end-to-end encryption. This saves you the
    trouble of having to build & maintain secure device \<\> server
    communications.

-   \<coming-soon\> **Offline street addresses, Location updates, Geofencing,
    Context-data model**

Using the LocationAPI Library
-----------------------------

**tldr;**

We've created an open-source demo application that uses the [LocationAPI Library
here](https://github.com/unwiredlabs/location-library-demo).

### Requirements

**Library file:** The library which you must include in your project is
[available for download as a .aar file
here.](https://github.com/unwiredlabs/location-library-docs/releases/download/0.9/locationapi-release_0.9.0.aar.zip)

**Developer token:** You will also need a free developer token, which you can
get by [signing up here](https://unwiredlabs.com/trial).

**Android requirement:** The library requires an Android device with a minimum
API Level of 10 (Gingerbread 2.3.3). It has been tested to be working on devices
from API Level 10 through API Level 23 (Marshmallow 6.0).

### Step-by-step Usage Instructions

If you prefer step-by-step instructions, here's a quick getting started guide:

-   Include the '.aar' file in your app; Android Studio users can follow [this
    guide](http://stackoverflow.com/a/24894387/1094271).

-   Add import statements to your class

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
import com.unwiredlabs.locationapi.Location.LocationAdapter;
import com.unwiredlabs.locationapi.Location.UnwiredLocationListener;
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   To get an instant location, call the `LocationAdapter.getLastLocation()` by
    specifying the accuracy level of the location you'd like and voila, you have
    it! Be aware that this is function is designed to be called synchronously
    and works exactly like Google's `getLastLocation()` where it might not be
    the absolute latest location (recently cached, etc) or match the accuracy
    level you specified. However, it is easy to call and great for applications
    where the developer prefers speed *versus* accuracy.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
//1. Initialize LocationAdapter class with Context & your developer Token
final LocationAdapter locationAdapter = new LocationAdapter(this,"your_api_token");

//2. Request for the last location
lastLocation = LocationAdapter.getLastLocation();
if (lastLocation != null) {
    latitudeText.setText(String.valueOf(lastLocation.getLatitude()));
    longitudeText.setText(String.valueOf(lastLocation.getLongitude()));
}
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-   If you'd like to get a fresh location or need it returned asynchronously,
    you can call the (slightly more complex) `LocationAdapter.getLocation()`.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
//1. Initialize LocationAdapter class with Context & your developer Token
final LocationAdapter locationAdapter = new LocationAdapter(this,"your_api_token");

//2. Set the desired PRIORITY / Accuracy level
locationAdapter.setPriority(LocationAdapter.PRIORITY_BALANCED_POWER_ACCURACY);

//3. Initialize the location listener
UnwiredLocationListener unwiredLocationListener = new UnwiredLocationListener() {
    @Override
    public void onLocationChanged(Location location) {
        if (location != null) {
            latitudeText.setText(String.valueOf(location.getLatitude()));
            longitudeText.setText(String.valueOf(location.getLongitude()));
        }
    }
    
};

//4. Request location update
locationAdapter.getLocation(unwiredLocationListener);

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~



### Types of Priorities / Accuracy levels

`LocationAdapter.setPriority()` - This method sets the priority or desired
accuracy level for all subsequent Location requests. The library will use this
definition to decide which location sources to use. The default priority is
`PRIORITY_BALANCED_POWER_ACCURACY`

The following values are supported:

`PRIORITY_BALANCED_POWER_ACCURACY` - Use this setting to request location
precision to within a city block, which is an accuracy of approximately 50
meters. This is considered a coarse level of accuracy, and is likely to consume
less power. With this setting, the location services are likely to use WiFi and
cell tower positioning. Note, however, that the choice of location provider
depends on many other factors, such as which sources are available.

`PRIORITY_HIGH_ACCURACY` - Use this setting to request the most precise location
possible. With this setting, the location services are more likely to use GPS to
determine the location. In the event that GPS isn't available (no satellite fix,
no permissions, etc), a less accurate but immediately available source of
location is used.

`PRIORITY_LOW_POWER` - Use this setting to request locality-level precision,
which is an accuracy of approximately 3 kilometres. This is considered a coarse
level of accuracy, and is likely to consume less power. With this setting, the
location services are likely to use just cell tower positioning.

`PRIORITY_NO_POWER` - Use this setting if you need negligible impact on power
consumption, but want to receive location updates when available. With this
setting, offline location lookups - lookups that are powered by Unwired's
Offline Location feature - and Android's passive locations updates - lookups
triggered by other apps - will be returned.

### Permissions

The accompanying .aar lists the permissions required by the library, which are
the following:

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
<!-- used to communicate with Unwired Labs' servers -->
<uses-permission android:name="android.permission.INTERNET" />

<!-- used to obtain WiFi IDs -->
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

<!-- used to obtain cell tower ID -->
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_UPDATES" />

<!-- used to access GPS location, for HIGH_ACCURACY priorities -->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

<!-- used to run background service that makes offline location work -->
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Android 6.0 (Marshmallow) note:**

The library fully supports the requirements specified by Android 6.0 and will
show a popup when you first request the user's location.

Further Help
------------

[Reach out to us](https://unwiredlabs.com/contact) for any further questions or
comments.

Documentation To-Dos
--------------------

-   Detailed method index

-   Return values & types
