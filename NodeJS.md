**Contents of the Node JS**
  1. [Introduction](#introduction)
  2. [NPM](#npm)
  3. [Modules](#modules)
  4. [HTTP Module](#http-module)
  5. [File System Module](#file-system-module)
  6. [URL Module](#url-module)
  7. [Formidable Module - To upload file](#formidable-module---to-upload-file)
  8. [Nodemailer Module - Send an Email](#nodemailer-module---send-an-email)
  9. [Events](#events)
***  
## Introduction
* Node.js allows you to run JavaScript on the server environment
* Node.js runs single-threaded, non-blocking, callback function, asynchronously programming, which is very memory efficient.
* Node.js eliminates the waiting, and simply continues with the next request.
  * Here is how PHP or ASP handles a file request -
    ```
    + Sends the task to the computer's file system.
    + Waits while the file system opens and reads the file.
    + Returns the content to the client.
    + Ready to handle the next request.
    ```
  * Here is how Node.js handles a file request -
    ```
    + Sends the task to the computer's file system.
    + Ready to handle the next request.
    + When the file system has opened and read the file, the server returns the content to the client.
    ```
* What is a Node.js File?
  - Node.js files must be initiated on the server before having any effect
  - Node.js files contain tasks that will be executed on certain events
  - A typical event is someone trying to access a port on the server
  - Node.js files have extension `.js`
    
## NPM
* **Package** and **modules** are used interchangeably
* NPM is a package manager for Node.js packages/modules. Modules are JavaScript libraries you can include in your project
* NPM creates a folder named `node_modules`, where the package will be placed. All packages you install in the future will be placed in this folder.

## Modules
* **Definition:** A set of functions (JavaScript Library) you want to include in your application.
* **Include Module:** To include modules, use `require("module_name")`
* **Custom Module:** 
  ```node
  // Creates a module that returns a date and time object
  exports.myDateTime = function () {
    return Date();
  };
  // exports keyword - Use it to make properties and methods available outside the module file.
  // Include Custom Module - 
  var custMod = require('./myfirstmodule');  // './' means that the module is located in the same folder as the Node.js file.
  .....
  console.log(custMod.myDateTime());
  ```
   
## HTTP Module
* **Include HTTP Module:** `var http = require('http');`
* `createServer()`: Creates an HTTP server that listens to server ports and gives a response back to the client
* **HTTP Header Example:** If the response from the HTTP server is supposed to be displayed as HTML, you should include an HTTP header with the correct content type
  ```node
  var http = require('http');
  http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});  
    // The first argument is the status code, 200 means that all is OK, 
    // The second argument is an object containing the response headers.
    res.write('Hello World!');
    res.write(req.url);  // url property holds the part of the url that comes after the domain name
    res.end();
  }).listen(8080);
  ```      

## File System Module
  * `var fs = require('fs');`
  * fs module allows you to work with the file system on your computer. System behaves as a File Server
  * `readFile()`: used to read files on your computer.
  * `open()`: takes a **flag** as the second argument, if the flag is **w** for **writing**, the specified file is opened for writing. If the file does not exist, an empty file is created
  * `appendFile()`: appends specified content to a file. If the file does not exist, the file will be created
  * `writeFile()`: replaces the specified file and content if it exists. If the file does not exist, a new file, containing the specified content, will be created
  * `unlink()`: deletes specified file
  * `rename()`: renames specified file
  * **Upload File:** You can also use Node.js to upload files to your computer

## URL Module
  * `var url = require('url');`
  * `parse()`: returns a URL object from the web address
    ```node
    var url = require('url');
    var adr = 'http://localhost:8080/default.htm?year=2017&month=february';
    var adrObj = url.parse(adr, true);
    console.log(adrObj.host); //returns 'localhost:8080'
    console.log(adrObj.pathname); //returns '/default.htm'
    console.log(adrObj.search); //returns '?year=2017&month=february'
    var queryObj = adrObj.query; //returns an object: { year: 2017, month: 'february' }
    console.log(queryObj.month); //returns 'february'
    ```

## Formidable Module - To upload file
   ```node
    var http = require('http');
    var formidable = require('formidable');
    var fs = require('fs');
    http.createServer(function (req, res) {
        if (req.url == '/fileupload') {
            var form = new formidable.IncomingForm();
            form.parse(req, function (err, fields, files) {
                var oldpath = files.filetoupload.path;
                var newpath = 'C:/Users/Your Name/' + files.filetoupload.name;
                fs.rename(oldpath, newpath, function (err) {
                    if (err) throw err;
                    res.write('File uploaded and moved!');
                    res.end();
                });
            });
        } else {
            res.writeHead(200, {'Content-Type': 'text/html'});
            res.write('<form action="fileupload" method="post" enctype="multipart/form-data">');
            res.write('<input type="file" name="filetoupload"><br>');
            res.write('<input type="submit">');
            res.write('</form>');
            return res.end();
        }
    }).listen(8080);
   ```

## Nodemailer Module - Send an Email

## Events
  * Every action on a computer is an event. Like when a connection is made or a file is opened
  * Node.js has a built-in module, called "Events", where you can create-, fire-, and listen for- your own events
  * All event properties and methods are an instance of an EventEmitter object -
    ```node
    var events = require('events');
    var eventEmitter = new events.EventEmitter();      
    // Create an event handler
    var myEventHandler = function () {  
        console.log('I hear a scream!');
    }
    // Assign the event handler to an event. Listen to the 'scream' event 
    eventEmitter.on('scream', myEventHandler);    
    // Fire the 'scream' event
    eventEmitter.emit('scream');  
    ```
