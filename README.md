# IBM IoT Connected Vehicle Insights - Mobility Starter Application server

The Fleet Management Starter Application for the IBM® IoT Connected Vehicle Insights SaaS offering demonstrates how quickly you can build a smart car sharing automotive solution on IBM Cloud.

## Overview

The starter app provides a mobile app for customers to quickly find and hire a car without human intervention from a car-hire company. The server component provides features that can help car hire companies manage a pool of cars and to get insights into customer usage and requirements that can also help to attract and retain customers.

## Server component

The server component provides the back-end car sharing and system monitoring services. Car-hire companies can use the server component features to do the following car-hire operations tasks:

- Monitor the location of all cars in the hire pool on a GIS map
- View detailed information about each vehicle and its current status
- View and manage customer information
- Evaluate and score customer driving behavior

### Mobile app

Customers who want to hire a car can use the Mobility Starter Application mobile app to search for available cars that are nearby, reserve a car, unlock the car, and then start driving the car. While you drive the car, the service tracks your location and also records your driving behavior. When you reach your driving destination, you can view information about each trip that you took in the car and you can also view your driving behavior score. You can also see how this new type of service generates vehicle data that helps automotive companies attract and retain customers.

For more information about the mobile app for either iOS or Android devices, see [Deploying the mobile app](#deploy_mobile_app).

The Mobility Starter Application server demonstrates how quickly you can build a car sharing solution on IBM Cloud by using the following services:

   * [IBM Watson IoT Platform](https://console.ng.bluemix.net/catalog/services/internet-of-things-platform/)
   * [Cloudant NoSQL DB](https://console.ng.bluemix.net/catalog/services/cloudant-nosql-db/)
   * [Weather Company Data for IBM Cloud](https://console.ng.bluemix.net/catalog/services/weather-company-data-for-ibm-bluemix/)
   * [Push Notifications](https://console.ng.bluemix.net/catalog/services/push-notifications/)

## Deploying the Mobility Starter Application server on IBM Cloud

You can automatically deploy an instance of the Mobility Starter Application server on IBM Cloud by clicking [![Deploy to IBM Cloud](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/ibm-watson-iot/iota-starter-server.git). You can also deploy the app manually. Note that you have to do the service activation step mentioned below after the automatic or manual deployment.

To manually deploy your own instance of the Mobility Starter Application server on IBM Cloud, complete all of the following steps:

1. Register an account on IBM Cloud or use an existing valid account.

2. Download and install the [Cloud-foundry CLI][cloud_foundry_url] tool.

3. Clone the Mobility Starter Application app to your local environment by using the following Terminal command:

  ```
  git clone https://github.com/ibm-watson-iot/iota-starter-server.git
  ```
4. Change to the directory that you just created.

5. Edit the `manifest.yml` file and change the values of `<name>` and `<host>` to something unique.

  ```
  applications:
         :
    disk_quota: 1024M
    host: iot-automotive-starter
    name: IoT-Automotive-Starter
    path: .
    instances: 1
    memory: 640M
         :
  ```
  The host value is used to generate your application URL, which is in the following syntax:
  `<host>.mybluemix.net`.

6. By using the command line tool, connect to IBM Cloud and log in when prompted.

  ```
  $ cf api https://api.ng.bluemix.net
  $ cf login
  ```

7. Create an instance of Watson IoT Platform, Cloudant NoSQL DB, and Weather Company Data services on IBM Cloud.

  ```
  $ cf create-service iotf-service iotf-service-free IoTPlatform
  $ cf create-service cloudantNoSQLDB Lite MobilityDB
  $ cf create-service weatherinsights Free-v2 WeatherInsights
  ```

9. Push the app to IBM Cloud. As you will need to complete further steps when the app is deployed, be sure to use the **--no-start** argument by using the following push command:

  ```
  $ cf push --no-start
  ```
  **Important:** When you run the `push` command, you must include the `--no-start` option as you must complete further steps manually before you start the app.

### Configuring IBM IoT Connected Vehicle Insights endpoints and credentials
To connect to your IBM IoT Connected Vehicle Insights service instance, the starter app reads the REST API endpoints and credentials from an environment variable on IBM Cloud. Configure the environment variables as follows:

1. Open the [IBM Cloud dashboard][bluemix_dashboard_url] in your browser.
1. Open the IBM IoT Connected Vehicle Insights service.
1. Select **Runtime** tab at the left navigation bar.
1. Click **Environment variables**.
1. Add the following environment variable:

```
USER_PROVIDED_VCAP_SERVICES = <endpoint definition in JSON format>
```
Define the endpoints in the following JSON format. Note that you must remove any line breaks that are at the end of each line to set to the environment value on IBM Cloud.
```
{
    "iotforautomotive": [
        {
            "credentials": {
                "api": <IBM IoT Connected Vehicle Insights endpoint>,
                "username": <user name>,
                "password": <passsword>
            }
        }
    ]
}
```
To obtain the correct values, contact your system administrator.

Key | Description of Value
----- | -----
api | A URL to call the REST API for IBM IoT Connected Vehicle Insights, which must end with a forward slash character (/)
username | User name for accessing the Vehicle Data Hub (VDH) and other IBM IoT Connected Vehicle Insights endpoints
password | Password for accessing the VDH and other IBM IoT Connected Vehicle Insights endpoints


You now have your very own instance of the IBM IoT Connected Vehicle Insights - Mobility Starter Application on IBM Cloud. To run the car sharing Mobility Starter Application you need to install the mobile app and connect it to your server instance.

## <a name="deploy_mobile_app"></a>Deploying the mobile app

After deploying the server component, to simulate and run the starter experience, you need to install a mobile app. The mobile app is available for both iOS and Android devices.

- To access the mobile app source code and deployment instructions for iOS, see [iota-starter-carsharing](https://github.com/ibm-watson-iot/iota-starter-carsharing).
- To access the mobile app source code and deployment instructions for Android, see [iota-starter-carsharing-android](https://github.com/ibm-watson-iot/iota-starter-carsharing-android).

To play a demo of the app, see the [Starter Experience home page](https://iot-automotive-starter.mybluemix.net).


### (Optional) Set up the Push Notifications service

You can enable push notifications when the weather at the drop off time of your car reservation becomes bad. Complete the following steps to enable push notifications:

1. Start a browser and open the [IBM Cloud dashboard][bluemix_dashboard_url].

2. Open the app that you created.

3. Click **ADD A SERVICE OR API**.

4. From the catalog, select **Push Notifications**.

5. Click **CREATE**.

6. Open the push notifications service that you just created.

7. Click **Setup Push**.

8. Register your Apple Push Notification Service certificate as outlined in [Configuring credentials for Apple Push Notifications](https://console.ng.bluemix.net/docs/services/mobilepush/t_push_provider_ios.html).


### (Optional) Setting up the Mobile Client Access service
You cannot utilize Mobile Client Access for Mobility Starter Application because Mobile Client Access service doesn't support custom authentication any more.

## Running the app

- To start your Mobility Starter Application instance, open the [IBM Cloud dashboard][bluemix_dashboard_url] and start the app.

Congratulations! You are ready to use your own instance of the IBM IoT Connected Vehicle Insights Starter app. Open `http://<host>.mybluemix.net/monitoring/#/settings` in your browser and follow the instructions in the mobile client connection section to connect your mobile app to the IBM IoT Connected Vehicle Insights - Mobility Starter Application.

## Connecting your own devices to the app

When you start your mobile app, you might see some simulated cars plotted at the current location on the map. The simulated cars are  generated automatically by the simulation engine that is provided by the app. Instead of using the simulated cars, you can connect and register your own physical devices to the app by using the following instructions.

### Disabling the simulation engine

Your physical devices can coexist with the simulated cars in the app. You can disable the simulated cars if you prefer.

1. Go to your [IBM Cloud dashboard][bluemix_dashboard_url] and open the app.

2. Stop the app.

3. Click **_Environment Variables_** in the left bar.

4. Click **USER-DEFINED**.

5. Insert `DISABLE_DEMO_CAR_DEVICES=true` and save the change.

6. Restart the app.

### Registering your device

To display your device on a map on the mobile app, register your device by completing the following instructions:

1. Start your browser and open `http://<host>.mybluemix.net/admin/ui/device`.

2. Enter your credentials and click **_OK_**. By default, the credentials are ADMIN/ADMIN.

3. If you see simulated cars in the list, you can delete them.  A device type for simulated cars is __ConnectedCarDevice__.

4. Click **_Create_**.

5. Complete the required fields on the form and then click **_Create_**.

  Specify a unique ID that identifies your device, for example, a MAC address. If you do not enter a device ID, an 8-digit ID will be generated automatically.

6. Go back to the list page and confirm that your car name and photo displays in the list.

Credentials for vehicle management page are set to ADMIN/ADMIN by default. However, they can be changed by setting the following environment variables:
```
ADMIN_USER=<administrator name>
ADMIN_PASSWORD=<administrator password>
```

To create trip data, your connected sensor device must publish car probe data to Watson IoT Platform. Car probe data includes information about the driving trip, for example, location and speed. If your device is registered successfully, it is added to Watson IoT Platform with the device type __UserOwnedCarDevice__.

You can also add your device to Watson IoT Platform directly. For more information, see the following resources:
 * [Quickstart](https://quickstart.internetofthings.ibmcloud.com/)
 * [How to Register Devices in IBM Watson IoT Platform](https://developer.ibm.com/recipes/tutorials/how-to-register-devices-in-ibm-iot-foundation/)


### Connecting your device to Watson IoT Platform

When your device is registered to your Watson IoT Platform instance, your device can connect to Watson IoT Platform and send car probe data by using the credentials that are assigned to your device. Lightweight API commands are provided with the app.

To simplify configuration and retrieve the credentials, you can use the following API command:
```
GET /user/device/credentials/:deviceId
```
**Notes:**
- To call the API from your device, you must disable the Mobile Client Access service.
- The **deviceId** value is the ID that is assigned to the device. If you did not register your device by using the above step, the API will register your device automatically with the specified ID and returns the credentials for the device. In this case, the default car values, for example name, photo, and car model, are assigned automatically. The default name is __User owned car__.

### Modifying car details

To modify car information, complete the following steps:

1. From your browser, open `http://<host>.mybluemix.net/admin/ui/device`.

2. Enter the credentials when prompted and then click **_OK_**. By default, the credentials are ADMIN/ADMIN.

3. Click your device ID to open the car details page.

4. Modify the required values and then click **_Update_**.

5. Go back to the list page and confirm that your car name and photo were updated correctly.

### Data format

Your device must publish car probe data to the Watson IoT Platform in the following data format.

```
{
  "d": {
    "lat": <latitude in double>,
    "lng": <longitude in double>,
    "trip_id": "<trip id in string>",
    "speed": <vehicle speed in double (km/h)>
  }
}
```
The `lat` and `lng` values are required to plot and display the location of your car on a map in the mobile app. Set the `lat` and `lng` values to match your device location. When you record your trip route after you reserve the car, the `trip_id` and `speed` values are also required. The value of `trip_id` must match the value that you set during the reservation.

### Create trip data with your device

To create trip data for your device, complete the following steps:

1. Connect your device to Watson IoT Platform.

2. Start sending the latest location data from the device. Keep sending the location data at short intervals.

3. Start the mobile app and show the current location on a map. Note that the device must be located at the same location.

4. Select your car and reserve it on the mobile app.

5. Unlock the car on the mobile app.

6. Generate unique trip id on the device and send it along with real-time location and speed data at short intervals.

7. Start driving with the device.

8. Stop driving. Stop sending the trip_id from the device.

9. Lock the car and complete the reservation on your mobile app.

You can see your trip route on Trips tab and driving behavior on Profile tab. It may take some minutes to analyze your trip data.

### Using the mobile app to create trip data

If you reserve a simulated car and unlock it on the mobile app, the simulated car starts running automatically around the location until the reservation is completed. You can see the simulated trip route and associated driving behavior on the mobile app after you complete the reservation.

The mobile app also provides capabilities to send car probe data. Therefore, instead of your own sensor device, you can use the mobile app to record your trip data easily without requiring the complex device settings that are described above. Select the _Analyze My Driving_ option at the top page of the mobile app. The mobile app registers itself to the Watson IoT Platform and starts sending car probe data. And, by driving around with your iOS or Android mobile device, data from your mobile app are recorded and analyzed in the app.

## Implementation

The following diagram shows the components that are used by the car sharing Mobility Starter Application and the flows that exist between the components.

![Components and flows](docs/components_and_flow.png)

* **Reservation** handles a request related to a reservation. The [routes/user/reservation.js](routes/user/reservation.js) component contains  the implementation.
* **Car Control** gets a request for controlling a car device, for example, to lock or unlock a car. It sends a command to the target car through Watson IoT Platform. The [routes/user/reservation.js](routes/user/reservation.js) component contains implementation for /carControl end point.
* **Driver Profile** handles a request to access a driver's behavior by using IBM IoT Connected Vehicle Insights SaaS offering Driver Behavior service. The [routes/user/insights.js](routes/user/insights.js) component defines the end point and the  [driverInsights/analyze.js](driverInsights/analyze.js) component contains the implementation.
* **Driving Analysis** gets events containing probe data from registered cars through Watson IoT Platform. It then sends the probe data to the IBM IoT Connected Vehicle Insights SaaS offering Context Mapping service to get the corrected location and sends the corrected location to the IBM IoT Connected Vehicle Insights Driver Behaivor service to get the driver's behavior. The [driverInsights/probe.js](driverInsights/probe.js) component is the entry point to explore the implementation. It also stores the probe data to Cloudant database "trip_route" that is used to retrieve a trip route. For more information, see the  [driverInsights/tripRoutes.js](driverInsights/tripRoutes.js) component.
* **Monitoring System** provides a dashboard showing real-time information about cars and reservations, and information about driver's behaviors extracted from probe data by IBM IoT Connected Vehicle Insights SaaS offering Driver Behavior service. The Map page pulls car status and reservation information via HTTP requests, and also receives car probe events relayed by IoT Platform via Web Socket connection. The page aggregates those informations to visualize the real-time state of cars on a map. The Users page shows statistics of driving behaviors associated to users' reservations. The server side implementation periodically, every two hours by default, extracts driving behavior information from the __Reservation__ and the __Driver Profile__ components and caches them to a database. The page gets the statistics from the database via HTTP requests. The implementation includes static files in [public/monitoring](public/monitoring) folder and server-side implementations in [routes/monitoring](routes/monitoring) folder which provides the end points and data synchronization.

* **Simulated Driving Data** is preset driving data under the [devicesSimulation/data](devicesSimulation/data) folder. The data is passed to Driving Analysis directly in [devicesSimulation/simulationImporter.js](devicesSimulation/simulationImporter.js) that is called in [_app.js](_app.js).
* **Cars (Simulator)** simulates different types of car behaviors. The implementation is under the  [devicesSimulationEngine](devicesSimulationEngine) folder.

## Reporting defects
To report a defect with the IBM IoT Connected Vehicle Insights - Mobility Starter Application server component, go to the [Issues](https://github.com/ibm-watson-iot/iota-starter-server/issues)) section.

## Troubleshooting
To debug problems, check the IBM Cloud app logs. To view the logs, run the following command from the Cloud Foundry CLI:

  ```
  $ cf logs <application-name> --recent
  ```
For more detailed information on troubleshooting your application, see the [Troubleshooting section](https://www.ng.bluemix.net/docs/troubleshoot/tr.html) in the IBM Cloud documentation.

## Privacy Notice

The IBM IoT Connected Vehicle Insights Starter app includes code to track deployments to [IBM Cloud](https://www.bluemix.net/) and other Cloud Foundry platforms. During each deployment, the following information is sent to a [Deployment Tracker](https://github.com/cloudant-labs/deployment-tracker) service:

* Node.js package version
* Node.js repository URL
* Application Name (`application_name`)
* Application GUID (`application_id)`
* Application instance index number (`instance_index`)
* Space ID (`space_id`) or OS username
* Application Version (`application_version`)
* Application URIs (`application_uris`)
* Cloud Foundry API (`cf_api`)
* Labels of bound services
* Number of instances for each bound service and associated plan information
* Metadata in the repository.yaml file

This data is collected from the `package.json` and `repository.yaml` file in the sample application and the `VCAP_APPLICATION` and `VCAP_SERVICES` environment variables in IBM Cloud and other Cloud Foundry platforms. This data is used by IBM to track metrics around deployments of sample applications to IBM Cloud to measure the usefulness of our examples, so that we can continuously improve the content we offer to you. Only deployments of sample applications that include code to ping the Deployment Tracker service will be tracked.

### Disabling Deployment Tracking

You can disable the Deployment Tracker service by removing `require("metrics-tracker-client").track();` from the beginning of the `app.js` server file.

## Questions, comments or suggestions
For your questions, comments or suggestions to us, visit [IBM IoT Connected Vehicle Insights Application community site] (https://www.ibm.com/developerworks/community/groups/service/html/communitystart?communityUuid=3b06ca1c-fd7c-4a59-a888-e5e3a8384091).

## Useful links
[IBM Cloud](https://bluemix.net/)
[IBM Cloud Documentation](https://www.ng.bluemix.net/docs/)
[IBM Cloud Developers Community](http://developer.ibm.com/bluemix)
[IBM Watson Internet of Things](http://www.ibm.com/internet-of-things/)
[IBM Watson IoT Platform](http://www.ibm.com/internet-of-things/iot-solutions/watson-iot-platform/)
[IBM Watson IoT Platform Developers Community](https://developer.ibm.com/iotplatform/)

[bluemix_dashboard_url]: https://console.ng.bluemix.net/dashboard/
[bluemix_signup_url]: https://console.ng.bluemix.net/registration/
[cloud_foundry_url]: https://github.com/cloudfoundry/cli
[mobile_app_readme]: https://github.com/ibm-watson-iot/iota-starter-carsharing/blob/master/README.md
