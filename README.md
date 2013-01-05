#CollectionFS
Is a simple way of handling files on the web in the Meteor environment

CollectionFS is a mix of both Meteor.Collection and GridFS mongoDB.
Using Meteor and gridFS priciples we get:
	* Security handling
	* Handling sharing
	* Restrictions eg. only allow certain content types, fields, users etc.
	* Reactive data, the collectionFS's methods should all be reactive
	* Abillity for the user to resume upload after connection loss, browser crash or cola in keyboard
	* At the moment files are loaded into the client as Blob universal way of handling large binary data

Sorry:
	* This is made as ```Make it work, make it fast```, well it should work
	* No test suite - any good ones for Meteor?
	* No smart packages - dont know how, yet
	* Current code contains relics in form of logs and timers used in the example ```statistics``` and for debuggin
	* I'm new to node.js, Meteor and github - this is my first code ever in Meteor

Decisions:
	* Initially I thought about using localStorage, but the limited size in the sandbox didn't make sense
	* Really wanted to make the Meteor serve files directly via url handling, getting the benefit of server+browser caching - but
	* Deviating the gridFS spec to make the code work and faster

Future:
	* Handlebar helpers? ```{{fileProgress}}```, ```{{fileInQue}}```, ```{{{fileAsURL}}}```, ```{{{fileURL _id}}}``` etc.
	* Maybe in time have the option to serve files directly from Meteor via url ```{{{fileAsURL}}}```- leaving caching to the server and browser?
	* Server side handling image size etc. not supported, but would be a must have if to be used in realworld apps
	* It doesn't use gridFS, but could be compatible with gridFS and other databases in time
	* Only one can upload at the moment, but really multiple instances and users could be supported ```(TODO in code)```
	* When code hot deploy the que halts, not sure how to address this, maybe a listener on connection status?
	* Deviates from gridFS by using chunkSize = 1024 (gridFS = 256?) - less transport bagage
	* Deviates frim gridFS by using files.len istead of files.length (as in gridFS, using .length creates error in Meteor?)
	* Speed, it sends data via Meteor.apply, this lags big time, therefore multiple workers are spawned to compensate
	* Current version is set to autosubscribe, this needs to be addressed in future, eg. ```subscribeFS()``` and ```publishFS()```

#How?
This couldn't get any simpler:
	1. Include the file ```CollectionFS.js```, contains both server and clien code
