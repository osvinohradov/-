# HOS CLOUD API 101 Example	 	 	
## HOW TO USE ARTOFUS HOS CLOUD API

### 1.Setup local NodeJs server

Run following commands to start local server:
```bash
 npm init
 npm i request --save
 npm i express --save
 npm i eventsource --save
 node server.js
 ```

Install NodeJS if needed:
```bash
curl --tlsv1 -sL https://deb.nodesource.com/setup_7.x | sudo -E bash -
apt-get install -y nodejs
```

Go to http://localhost:3001 to check HOS CLOUD test Stream

### 2. Login user

Connect module require to the javascript file. And, on the received object, call method POST by passing the first argument route to authorization server, second argument is an object with the setting with user_data, that will be transferred? And third parameter a callback function that takes two parameters, object with error and response object. In the body of function take response object and call property body. For example:

```javascript
var request = require('request');
// =============== Login user ===============
var HOS_MANAGEMENT = 'http://hos-management-api.test.gbksoft.net/api/v1.0/login';
var user_data = {
    username: "admin2@artofus.com",
    password: "admin2",
    requestAuthToken: true
};
request.post(HOS_MANAGEMENT, {
    json: user_data
}, function(err, res){
    console.log(res.body);
});
// Response from hos management looks like:
{ code: 200,
  status: 'success',
  message: 'User info',
  result:
   { token: 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjVhZDYzMDM1LTFlMDUtNDVlOC1iZDEyLTNkNTgyMWU2ODhlYSIsImlhdCI6MTUxOTEzODY0MX0.O8WFIy2Ev5Bgm_z5Jp0xWzelarPLndxXUfu-cExl3HY',
     user:
      { type: 'admin',
        updated: '2018-02-12T09:43:48.357Z',
        validFrom: '2017-06-18T16:24:00.872Z',
        authId: 'auth0|5a27be654aa13269653ce412',
        validUntil: '2017-09-18T16:24:00.872Z',
        status: 'Active',
        surname: 'Simple',
        firstName: 'Admin2',
        description: 'Admin description',
        role: '40b4a10e-26db-4d7d-a5ce-6933ef5a2659',
        name: 'Admin2 Simple',
        username: 'admin2@artofus.com',
        id: '5ad63035-1e05-45e8-bd12-3d5821e688ea',
        collectionName: 'admins' },
     allRoles:
      { type: 'admin',
        updated: '2018-02-12T09:43:48.352Z',
        rights: [Object],
        name: 'Admin2',
        adminId: '9222c6c2-6a34-4009-8e57-989cdff5834c',
        id: '40b4a10e-26db-4d7d-a5ce-6933ef5a2659',
        collectionName: 'roles' },
     auth0_token: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik5FUXdNa0pDT1RJNVJUUTRNRVJCTmtORE5EZ3dNVVpHTWpjMFFrWkVOa013UlVRMFFqVkJPUSJ9.eyJpc3MiOiJodHRwczovL2FydG9mdXMuZXUuYXV0aDAuY29tLyIsInN1YiI6ImF1dGgwfDVhMjdiZTY1NGFhMTMyNjk2NTNjZTQxMiIsImF1ZCI6Imh0dHBzOi8vYXJ0b2Z1cy5ldS5hdXRoMC5jb20vYXBpL3YyLyIsImlhdCI6MTUxOTEzODY0MSwiZXhwIjoxNTU4ODgyNjQxLCJhenAiOiJBRnZxRkJVS2ZYQkZadEhXV2ZXNTBQTmZKOWtYMEp3ZSIsInNjb3BlIjoicmVhZDpjdXJyZW50X3VzZXIgdXBkYXRlOmN1cnJlbnRfdXNlcl9tZXRhZGF0YSBkZWxldGU6Y3VycmVudF91c2VyX21ldGFkYXRhIGNyZWF0ZTpjdXJyZW50X3VzZXJfbWV0YWRhdGEgY3JlYXRlOmN1cnJlbnRfdXNlcl9kZXZpY2VfY3JlZGVudGlhbHMgZGVsZXRlOmN1cnJlbnRfdXNlcl9kZXZpY2VfY3JlZGVudGlhbHMgdXBkYXRlOmN1cnJlbnRfdXNlcl9pZGVudGl0aWVzIiwiZ3R5IjoicGFzc3dvcmQifQ.qtfOUhPtH3TcFOy-7PQ_VuEY20z576V888BnpQuz7-Mgm-s7rApm5yl4NQjsE8pmFJVwI_cjrulRIhD78ZEjpsv5xV_3L7tNdaE8OQlQE61nspm9ut85NMYjGxguh9mEklmxRgL5hKRFhI5IB9JQ0Xve8TtR1Z_sDcjjcTiAiiddizwJTLr4yBrtbcJSFtndGqXx0syLbEdpxLzKsUI12D9QBp_Fps8Lmlj64xERTW1ehLEu7DGUUZJP09uLQ0EZFcIMfLGR5WQf23hsr-aGNql8rSbvjs7w4OzPJe2v8wfQSDHtKM1xgT85iRyPgoD9mB8qJkxX6F8MIWk4Wx_gIw' } }
```
### 3. Get stream with images

Step one. Create a directory named example, which will contain the source files, and go to it in the terminal. Then create two files index.html and server.js. Then, using the terminal, install the necessary packages, namely the request module, for executing queries to remote resources, express for creating a server and eventsource for creating an SSE connection.

Step two. Open the server.js file and write the code in Listing 1.
Listing 1

```javascript
var request = require('request');
var express = require('express');
var EventSource = require('eventsource');

var HOS_MANAGEMENT = 'http://hos-management-api.dev.gbksoft.net/api/v1.0/login';
var MOU_IP = 'https://mouth.aw-stage.cloud.artofushos.com/api/v2/stream';
// object contains data for login in hos management
var user_data = {
   username: 'admin2@artofus.com',
   password: 'admin2',
   requestAuthToken: true
};
// setup application port
var port = process.env.port || 3000;
// create express application
var app = express();
// set handler on route /stream for get sse connection on FE/
app.get('/stream', function (req, res) {
   var promise = new Promise((resolve, reject) => {
       // The request to hos management for login user and get auth0_token
       request.post(HOS_MANAGEMENT, { json: user_data }, function (err, res) {
           if (err) reject(err);
           resolve(res.body.result['auth0_token']);
       });
   });
   // if promise have status resolve, we call function, and pass auth0_token by first argument
   promise.then((auth_token) => {
       // object with configuration for MOU_IP
       var options = {
           headers: { Authorization: `Bearer ${auth_token}` },
           https: { rejectUnauthorized: false }
       };
       var evt = new EventSource(MOU_IP, options);

       // set handler on onopen event.
       evt.onopen = function () {
           console.log('Connection established');
           // set response header for SSE conection
           res.writeHead(200, {
               'Content-Type': 'text/event-stream',
               'Cache-Control': 'no-cache, no-store, must-revalidate',
               'Connection': 'keep-alive'
           });
       };
       // set handler on message event. The callback function received msg object that contains response data from HOS_MANAGEMENT.
       evt.on('message', function (msg) {
           var data = JSON.parse(msg.data);
           // Check, if data has user picture id, we request this picture from HOS_FACE
           if (data.sensors[0].observs[0].pic) {
               request.get(`http://54.76.135.162:8080/api/v1/face/get?id=${data.sensors[0].observs[0].pic}`, function (err, response, body) {
                   if (err) console.log('Error: ', err);
                   // If data received successfull, transfer data to frontend
                   res.write('data: ' + body + '\n\n');
               });
           };
       });
       // Set handler on error event
       evt.on('error', function (msg) {
           console.log('Error: ', msg);
           res.send('Error response');
       });
   });
});
// set handler for route /index. handler return file index.html
app.get('/index', function (req, res) {
   res.sendFile(`${__dirname}/index.html`);
});
// start listening port
app.listen(port, function () {
   console.log('Server started on port', port);
});
```

Step three. Open the index.html file and put the code from Listing 2 into it.
Listing 2

```html
<html lang="en">
<head>
   <meta charset="UTF-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <meta http-equiv="X-UA-Compatible" content="ie=edge">
   <title>Document</title>
   <style>
       img {
           width: 100px;
           margin: 10px;
       }

       table {
           width: 900px;
           margin: 0 auto;
       }

       table td {
           width: 30%;
           word-wrap: break-word;
       }

       table td:nth-child(2) {
           text-align: center;
       }

       pre {
           display: block;
           margin: 5px;
           max-width: 400px;
           word-break: break-all;
           white-space: pre-wrap;
       }
   </style>
</head>

<body>
   <div id="container">
       <table border="1">
           <thead>
               <th>PersonID</th>
               <th>Image</th>
               <th>MOU data</th>
           </thead>
       </table>
   </div>
   <script>     
       (function () {
           // find the tag table
           var table = document.querySelector('table');
           // create new EventSource object for connecting to server.
           // If request are success, we receive data and add new row to table with data from server
           var evt = new EventSource('/stream');
           var img = null;
           var row = null;
           // set handler onmessage event.
           evt.onmessage = function (msg) {
               msg = JSON.parse(msg.data);

               img = new Image();
               img.src = `data:image/jpg;base64, ${msg.image}`;

               var tr = document.createElement('tr');
               var personIdCell = document.createElement('td');
               var imageCell = document.createElement('td');
               var messageCell = document.createElement('td');
               personIdCell.innerText = msg.metadata.person_id;
               imageCell.appendChild(img);
               msg.image = "base 64";
               messageCell.innerHTML = `<pre>${JSON.stringify(msg)}</pre>`;

               tr.appendChild(personIdCell)
               tr.appendChild(imageCell);
               tr.appendChild(messageCell);
               table.appendChild(tr);
           };
           evt.onerror = function (msg) {
               console.log(msg);
           };
       })()
   </script>
</body>
</html>
```

Step four. Run the file server.js using the platform nodejs.

Step five. Open the browser and enter the address where the server is running in the address bar. (localhost:3001/index)

Step six. Enjoy!



