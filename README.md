# perfect-logger
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fheysulo%2Fperfect-logger.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fheysulo%2Fperfect-logger?ref=badge_shield)

The Perfect-Logger library exported as Node.js modules. With Perfect-Logger you can improve your application's logs


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fheysulo%2Fperfect-logger.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fheysulo%2Fperfect-logger?ref=badge_large)
=======

## Highlights

- Easy to use
- Configurable
- Write log events to file
- Automatic log switching to avoid huge log files
- Custom log codes
- Database callback support
- Colored terminal output

## Installation
Using npm:
```
npm i perfect-logger
````

In Node.JS
```javascript
// Load the perfect-logger module
let logger = require('perfect-logger');

// Configure Settings
logger.setLogDirectory("./ApplicationLogs");
logger.setLogFileName("AppLog");

// Initialize
logger.initialize();

// Use
logger.info("This is an information");
logger.warn("This is a warning");
logger.crit("This is a critical message");
```
Sample Terminal Output
```
2018/08/05 | 04:12:21 | INFO | This is an information
2018/08/05 | 04:12:21 | WARN | This is a warning
2018/08/05 | 04:12:21 | CRIT | This is a critical message
```
Sample Log File
```
******************************************************************************************
**
** NodeJS Logging System
** Application Version          : 1.0
** Node-Logger Version          : 1.0.0.0
** Application Start Time (UTC) : 04:12:21 2018/08/05
**
** Copyright 2018 Team whileLOOP
**
******************************************************************************************
**
** Log Number               : #1
** Continuing from Log File : None
** Log Start Time (UTC)     : 04:12:21 2018/08/05
**
******************************************************************************************
2018/08/05 | 04:12:21 | INFO | This is an information
2018/08/05 | 04:12:21 | WARN | This is a warning
2018/08/05 | 04:12:21 | CRIT | This is a critical message
```
# Configurations
## Time and Date
You can use your own time and date functions or you can use the inbuilt time and date functions which is default. Your custom functions should return a string. To ensure the log file quality, make sure that your functions return the time and date string at a constant length.

How to use your own time and date functions
```javascript
function time() {
    return "time";
}

function date() {
    return "date"
}

logger.setTimeFunction(time);
logger.setDateFunction(date);
```
In case if you decided to use the inbuilt time and date functions you canset the timezone like this. Default is UTC.
You can find a list of TimeZones here : https://timezonedb.com/time-zones
```
logger.setTimeZone("Asia/Colombo");
```

## Logging to File
By default this module will write all events to the log file. 
Configure where you want your log files to be written. Default location is `logs` folder in the current directory.
```javascript
logger.setLogDirectory("./NodeLogs");
```
The log file will be in following syntax.
```
<LogFileName>.<TimeStamp>.log
```
You can set the LogFileName with the following function
```javascript
logger.setLogFileName("MyApplicationLog");
```
Every log file has it's own banner which includes the application Name, Version and Copyright banner. It's required to provide these information to the logger module.
```javascript
logger.setApplicationInfo({
    name: "Facebook",
    banner: "Copyright 2018 Facebook",
    version: "1.0"
});
```
Output Banner for above configuration
```
******************************************************************************************
**
** Facebook Logging System
** Application Version          : 1.0
** Node-Logger Version          : 1.0.0.0
** Application Start Time (UTC) : 10:09:54 2018/08/05
**
** Copyright 2018 Facebook
**
******************************************************************************************
```
By setting a maximum log file size will allow you to switch to new log file when the file size limit is reached. This is usefull to split log files. You must provide the size limit in **Bytes**.
```javascript
logger.setMaximumLogSize(10000);
```
# Custom Log Codes
By default `INFO`, `WARN`, `CRIT` and `DEBG` methods are available for logging. You can create your own logging codes.
This should be done before calling the `logger.initialize()` function.
```javascript
// Creation
/**
 * @param alias - Name of the status code
 * @param code - Status Code
 * @param writeToDatabaseValue - Write to database by default
 * @param color - Color of the text (Optional) Ex: logger.colors.red or "red"
 */
logger.addStatusCode("flag", "FLAG", false);
logger.addStatusCode("socketEvent", "SOCK", false, "red");

// Usage
logger.flag("RESTART Flag received");
logger.socketEvent("Socket client connected");
```
Output
```
2018/08/05 | 10:23:01 | FLAG | RESTART Flag received
2018/08/05 | 10:23:01 | SOCK | Socket client connected
```
Available colors :
`black`, `red`, `green`, `yellow`, `blue`, `magenta`, `cyan`, `white`

# Database Callback
You can atatch a function which can be fired upon log event which can be used to write events to the database. By default `WARN` and `CRIT` events will fire the database callback. Attaching a database object as the 3rd parameter will always fire the database callback.

```javascript
function databaseCallback(dbObject){
    database.query('INSERT INTO `log` VALUES(?,?,?,?,?)',
            [dbObject.date, dbObject.time, dbObject.code, dbObject.message, JSON.stringyfy(dbObject.details)],
            function (err,payload) {
                if(err){
                    console.log("ERROR!");
                }
            });
}

logger.setDatabaseCallback(databaseCallback);
```

# Help and Support
For any queries drop an email to sulochana.456@live.com
