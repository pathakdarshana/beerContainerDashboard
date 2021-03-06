# Beer Container Dashboard
Dashboards for delivery trucks to monitor temperature of beer containers.
![Screenshot of Dashboard](beerDashboard.png?raw=true "Title")
## Purpose of App
To show driver current temperature of beer containers and alert if temperature falls out of range

## Steps to execute
 - Required Node version: **8.9.4**
 ### Run application
```
git clone https://github.com/pathakdarshana/beerContainerDashboard.git
cd beerContainerDashboard
npm install
npm start
```
### Run tests
```
git clone https://github.com/pathakdarshana/beerContainerDashboard.git
cd beerContainerDashboard
npm install
npm test
```
## Assumption
We assume that:
  - One container has only one type of beer.

## Logic
### Initialization
  - It a process of loading of containers in truck with a set temperature.
  - The process is done through `src/initContainer.js`. It loads data from `src/mockData/beerContainer.json` and creates various instances of container.
  - Sample data for initialization process:
  ```
  {
     "Container1" : {
      "initialTemperature" : 5,
      "beerType" : "beer1"
    },
    "Container2" : {
      "initialTemperature" : 5,
      "beerType" : "beer2"
    }
   }
  ```
### Temperature sensor
  - As there is no real source for getting current temperature from container's temperature sensor so temeperature data  is generated randomly using a function.
  - **Range configuration:** To make the logic dynamic temperature randomizer generates temperature in a range configurable through `config.json`.  
  - The function is performed by `src/temperatureSensor.js`.
  
### Monitoring
  - The temperature given by the sensor of a container is compared against the desirable range of beer stored in it.
  - If current temperature is within the desirable range status is set to `Normal`.
  - If current temperature is out of the desirable range status is set to `Alert!`.
  - The desirable range of temperature of all beers is maintained in file : `src/mockData/beer.json`
  - Sample data for beer temperature range:
  ```
  {
    "beer1":{
      "temperatureRange":{
        "lower": 4,
        "upper": 6
      }
    }
  }
  ```
### Dashboard
  - The dashboard is shown in the `terminal` only, it has no web page or UI.
  - Current temperature of containers and status `Normal or Alert!` is displayed for all containers in a tabular format.
  - It has an `event emitter` in it which is triggred if the temperature of container goes out of range and notifies. 
  - **Refresh Interval:** 
    - Dashboard is refreshed after every 5 seconds.
    - It can be configured through `config.json`
  - **Colour coding:**
    - `Normal` -> Blue
    - `Alert!` -> Bold and Red
### Note: 
  - To closely simulate the behaviour of a real temperature Sensor, a condition has been put on the time period when `temperature Sensor` generates new temperature.
  - `checkInterval` and `dashboardRefreshInterval` in `config.json` should follow conditions given below:
    - `checkInterval` is a multiple of `dashboardRefreshInterval`.
    - Interval should be `integer`.
    - **Example:**
    ```
     dashboardRefreshInterval   checkInterval
     -------------------------   -------------
          5                         10         
          2                          8
          5                         15
    ```
    
### Possible Improvements
  - **User interface :**
      - Customizable color, fonts.
      - Interactive.
      - Responsive
  - **Notification system:**
      - Adding acknowledgement system.
      - Making it audible.
      - Escalation system can be added.
  - Decouple logic and UI by making API.
  - Add capability to monitor remotely.
  - Integration with real temperature sensor data.
  
