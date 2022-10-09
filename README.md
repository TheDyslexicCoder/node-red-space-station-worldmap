# node-red-space-station-worldmap
I am using Node-RED's WorldMap to locate the International Space Station &amp; Chinese Space Station—Tiangong via N2YO.com API!

## Minimum Viable Product, or MVP
<img width="1360" alt="Tracking the ISS   CSS on Node-Red" src="https://user-images.githubusercontent.com/65976215/194736822-1c7bd1a3-e39a-4087-9dfd-e04cac966c20.png">

## Prerequisites:

* Create a free account with [N2YO.COM](https://www.n2yo.com/login/) to access the APIs for the International Space Station & Chinese Space Station—Tiangong.
* Have access to [Node-Red Terminal](https://nodered.org/docs/getting-started/).
* Install [Node-Red Worldmap](https://flows.nodered.org/node/node-red-contrib-web-worldmap).
* Willingness to shoot for the Stars~ :rocket:

## Table of Contents:
1. NY2O Account Setup
2. Building Out the Node-RED Canvas
3. Deploying the ISS & CSS Tracker flow

## Documentation:
### Procedure:
#### Part 1: NY2O Account Setup
1. Start by accessing N2YO.com.
2. Click register for a new account.

https://user-images.githubusercontent.com/65976215/194736261-f95ce6ac-5e59-41a6-bf99-dba1c6dfda6d.mp4

3. Create an account with your username & password. We will need them in the upcoming section.
4. Click 'Submit' when done.

https://user-images.githubusercontent.com/65976215/194736298-7d2890e0-17e0-44aa-bf48-d384d6f61020.mp4

5. Relogin to N2YO.com, and enter the username & password you created.
![ReLogin to N2YO com](https://user-images.githubusercontent.com/65976215/194736426-c2cf943d-dc52-4276-bb9e-2a9a7181023e.png)
6. To check our profile, look towards the lower right corner of the screen labeled 'Your Current Location.' Click 'Change your location.'
![CurrentLocation](https://user-images.githubusercontent.com/65976215/194736445-fe208460-f7f7-478f-be64-001042c03b31.png)
7. On our Profile Screen, locate our observer coordinates.

https://user-images.githubusercontent.com/65976215/194736361-fbeccaba-9c8d-438d-b694-7d349e630cef.mp4

8. Next, generate our API key by clicking the 'Generate your API license key' button.

https://user-images.githubusercontent.com/65976215/194736378-3036589f-19ab-4c36-a50e-3024efa43f7b.mp4

9. Great! Let's compile the two API Request URLs for the International Space Station & Tiangong Space Station with our observable values.
<details><summary>Click to view my observable values from NY2O.com</summary>
<p>

| NY2O.com Variables  | My observable values |
| :---:   |     :---:   |
| **NORAD ID for ISS** | 25544  |
| **NORAD ID for CSS** | 25544  |
| **Latitude**  | 37.84  |
| **Longitude**  | -122.29  |
| **API License Key** | W2P49E-CYAT4A-PL4YAB-4TSZ  |
</p>
</details>    
    
   - *Below copy the Sample URL & substitute the variables inside the Sample URL for your values from N2YO.com:*

<details><summary>Click to view the sample API URL for the International Space Station & the Chinese Space Station—Tiangong </summary>
<p>

  ```
  SAMPLE URL: https://api.n2yo.com/rest/v1/satellite/positions/NORADID/Lat/Lon/0/2/&apiKey={yourAPIKey}
  ```
</p>
</details>
    
- 
    - Translate your code to:

<details><summary>Click to view the final API URL for the International Space Station & the Chinese Space Station—Tiangong</summary>
<p>

  ```
International Space Station API URL: https://api.n2yo.com/rest/v1/satellite/positions/25544/37.84/-122.29/0/2/&apiKey=W2P49E-CYAT4A-PL4YAB-4TSZ
Chinese Space Station—Tiangong API URL: https://api.n2yo.com/rest/v1/satellite/positions/48274/37.84/-122.29/0/2/&apiKey=W2P49E-CYAT4A-PL4YAB-4TSZ

  ```
</p>
</details>
10. Awesome! You should now have your two unique API URLs! Keep those URLs on the back burner, and we will come back to them later in the project!

#### Part 2: Building Out the Node-RED Canvas
1. Open Node-RED on the Raspberry Pi terminal.
<details><summary>Click to view the steps for launching Node-RED on the Raspberry Pi terminal.</summary>
<p>

```
 node-red start
```
</p>
</details>

2. Access Node-RED through the server URL and navigate the Installation Palette in the User Settings to install the WorldMap node.

https://user-images.githubusercontent.com/65976215/194736515-9cf7b2d4-bc7f-4a86-9a53-aa2ada14d0f6.mp4

<details><summary>Click to view the steps for installing the WorldMap in the Installation Palette.</summary>
<p>

```
 Menu > Manage Palette > Install > Type in WorldMap
```
</p>
</details>

- *If needed, click install and follow the steps to complete the installation process.*
  - Now that we have installed the Worldmap node, we can start building the flow!

3. We will begin by renaming the canvas from Flow 1 to ISS & CSS Tracker.

https://user-images.githubusercontent.com/65976215/194736550-c026a7b5-a050-4bbf-a166-6d49e7e89e85.mp4

4. Next, drag the Inject node into our canvas and adjust the following configurations:

https://user-images.githubusercontent.com/65976215/194736570-c9904786-3259-4f60-8fd8-29d806304a9e.mp4

<details><summary>Click to view the steps for configuring the Inject node.</summary>
<p>

```
Inject node > Properties > Select Inject Once after 0.1
seconds, then > Repeat Interval every 32 seconds > Done
```
</p>
</details>

- Perfect! Remember the two custom URLs you made for the ISS & CSS? It’s time to grab them, as we will use them in our HTTP request node!
5. To create the International Space Station HTTP Request node, drag an HTTP request node into our canvas and follow configurations steps:

https://user-images.githubusercontent.com/65976215/194736606-e5faa912-5e00-4479-89e3-97b2fdb72fcc.mp4

<details><summary>Click to view the steps for configuring the International Space Station HTTP Request node.</summary>
<p>

```
http request node > Properties > 
URL: https://api.n2yo.com/rest/v1/satellite/positions/25544/37.84/-122.29/0/2/&apiKey=W2P49E-CYAT4A-PL4YAB-4TSZ 
> Return: a parsed JSON object > Name: N2YO Rest API (ISS) > Done
```
</p>
</details>

6. We will add another HTTP request node for the Chinese Space Station and follow the configurations steps:

https://user-images.githubusercontent.com/65976215/194736648-1b67b2ea-28cc-4a04-8e77-4ba821590087.mp4

<details><summary>Click to view the steps for configuring the Chinese Space Station HTTP Request node.</summary>
<p>

```
http request node > Properties > 
URL: https://api.n2yo.com/rest/v1/satellite/positions/25544/37.84/-122.29/0/2/&apiKey=W2P49E-CYAT4A-PL4YAB-4TSZ 
> Return: a parsed JSON object > Name: N2YO Rest API (ISS) > Done
```
</p>
</details>

7. Next, we will drag the Function node to the International Space Station's path. Double-click the Function node, and type in the following JavaScript code, which will extract the JSON data from the HTTP Request node.

https://user-images.githubusercontent.com/65976215/194736692-e6185fac-f22d-4607-9336-ef77bf2809c1.mp4

<details><summary>Click to view the JavaScript code for ISS Position. </summary>
<p>

```
var iss = "International Space Station";
var lat = msg.payload.positions[0].satlatitude;
var lon = msg.payload.positions[0].satlongitude;
msg.payload = {
"name": iss,
"lat" : lat,
"lon": lon,
};
return msg;
``` 
</p>
</details>

8. We’ll repeat the step for the CSS.

https://user-images.githubusercontent.com/65976215/194736718-6366ecf3-f326-4b52-8c97-edef4d6a407c.mp4

<details><summary>Click to view the JavaScript code for CSS Position. </summary>
<p>

```
var css = "Chinese Space Station";
var lat = msg.payload.positions[0].satlatitude;
var lon = msg.payload.positions[0].satlongitude;
msg.payload = {
"name": css,
"lat" : lat,
"lon": lon,
};
return msg;
``` 
</p>
</details>

9. Excellent, we are almost done! We’ll create the attributes to show the ISS on the WorldMap with the code below:

https://user-images.githubusercontent.com/65976215/194736733-5e52f0cb-7651-4114-8561-918eef6eeb33.mp4

<details><summary>Click to view the JavaScript code for ISS Attributes. </summary>
<p>

```
var icon = "iss";
msg.payload = {
"name": msg.payload.name,
"lat": msg.payload.lat,
"lon":  msg.payload.lon,
"icon": icon,
"iconColor":"#ebbe56",
};
return msg;
``` 
</p>
</details>

10. We’ll recreate the attributes for the CSS with the code below:

https://user-images.githubusercontent.com/65976215/194736744-1eed4a17-b934-4b9e-a290-7640bbb4bb55.mp4

<details><summary>Click to view the JavaScript code for CSS Attributes.</summary>
<p>

```
var icon = "iss";
msg.payload = {
"name": msg.payload.name,
"lat": msg.payload.lat,
"lon":  msg.payload.lon,
"icon": icon,
"iconColor":"red",
};
return msg;
``` 
</p>
</details>

11. Add a Debug node to the end of each flow path and edit the configurations:

https://user-images.githubusercontent.com/65976215/194736756-cd928681-4aea-4476-a7ef-c7a44ee1f3dc.mp4

<details><summary>Click to view the steps for configuring the Debug node.</summary>
<p>

```
debug node > Properties > Output:complete msg object > Done
``` 
</p>
</details>

12. Drag the WorldMap node and link the two functions node to it!

https://user-images.githubusercontent.com/65976215/194736216-2016b8b8-f247-4e64-9294-696d0527feac.mp4

13. Finally, click the WorldMap node on the info tab, and right-click “here” to open the map on a new tab.
<img width="1276" alt="Opening Worldmap on Node-Red" src="https://user-images.githubusercontent.com/65976215/194736797-353edaea-22dc-4200-b77d-4a2b9fb40c81.png">

#### Part 3: Deploying the ISS & CSS Tracker flow

1. Click Deploy our flow, and click the inject node to view the Space Stations on Worldmap on the other tab!

https://user-images.githubusercontent.com/65976215/194736184-41e01689-11a2-4c24-9c7a-470ebd7f9e18.mp4


### End of Procedure.

🎉 Way to Go! 🎉 We’ve created a flow to view the relative positions of the International Space Station & Tiangong Space Station in real-time using an open-source API from N2YO.com!

Feel free to copy the full code and add your twist to it! :sparkles:	


