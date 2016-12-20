---
layout: post
title: "M101JS: MongoDB for Node.js Developers"
date: 2016-12-08 23:45:30
tags: mongo mongodb db nosql 10gen tengen course curso university education m101 m101js node.js nodejs node express swig
---



## Week 1: Introduction
Following are presented the contents and answers to the HomeWorks belonging to **Week 1 (Introduction)** for "*M101JS: MongoDB for Node.js Developers*" course offered by [https://university.mongodb.com/][1]{: target="_blank"} started at 2014-03-24.


### HOMEWORK 1.1
![HW 1.1 Submit][m101js_hw11_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 1.1:
Simply it was about verifying the correct MongoDB installation, example data import and basic query execution over those imported data.

First, `mongorestore`:
{% highlight text linenos=table %}
$ /opt/mongodb-linux-i686-2.6.0/bin/mongorestore
connected to: 127.0.0.1
2014-05-07T07:49:22.176-0500 dump/m101/hw1_1.bson
2014-05-07T07:49:22.176-0500 	going into namespace [m101.hw1_1]
1 objects found
2014-05-07T07:49:22.178-0500 	Creating index: { key: { _id: 1 }, ns: "m101.hw1_1", name: "_id_" }
$
{% endhighlight %}
![HW 1.1 MongoRestore][m101js_hw11_MongoRestore]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Then, some MongoShell checks about the restoration and the result answer:
{% highlight text linenos=table %}
MongoDB shell version: 2.6.0
connecting to: test
Server has startup warnings:
2014-05-07T07:48:32.639-0500 [initandlisten]
2014-05-07T07:48:32.639-0500 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2014-05-07T07:48:32.639-0500 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2014-05-07T07:48:32.639-0500 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2014-05-07T07:48:32.639-0500 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2014-05-07T07:48:32.640-0500 [initandlisten]
> show dbs
admin    (empty)
blog     0.078GB
enron    0.953GB
final7   0.078GB
local    0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> show dbs
admin    (empty)
blog     0.078GB
enron    0.953GB
final7   0.078GB
local    0.078GB
m101     0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> use m101
switched to db m101
> show tables
hw1_1
system.indexes
> db.hw1_1.count()
1
> db.hw1_1.findOne()
{
	"_id" : ObjectId("51e4524ef3651c651a42331c"),
	"answer" : "Hello from MongoDB!"
}
>
{% endhighlight %}
![HW 1.1 MongoChecks][m101js_hw11_MongoChecks]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 1.2

### ANSWER HOMEWORK 1.2:
![HW 1.2 Submit][m101js_hw12_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 1.3

### ANSWER HOMEWORK 1.3:
![HW 1.3 Submit][m101js_hw13_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Week 2: CRUD
Following are presented the contents and answers to the HomeWorks belonging to **Week 2 (CRUD)** for "*M101JS: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


### HOMEWORK 2.1
![HW 2.1 Submit][m101js_hw21_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 2.1:
![HW 2.1 MongoshellChecks][m101js_hw21_MongoshellChecks]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight javascript linenos=table %}
use weather
query = {"Wind Direction":{$gt:180, $lt:360}}
project = {_id:false, State:true}
sorting = {Temperature:1}
db.data.find(query,project).sort(sorting).limit(5)
{% endhighlight %}

{% highlight javascript linenos=table %}
MongoDB shell version: 2.6.0
connecting to: test
switched to db weather
{ "Wind Direction" : { "$gt" : 180, "$lt" : 360 } }
{ "_id" : false, "State" : true }
{ "Temperature" : 1 }
{ "State" : "New Mexico" }
{ "State" : "New Mexico" }
{ "State" : "Vermont" }
{ "State" : "Vermont" }
{ "State" : "Vermont" }
bye
{% endhighlight %}

![HW 2.1 ImportMongoOutput][m101js_hw21_ImportMongoOutput]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 2.2
![HW 2.2 Submit][m101js_hw22_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 2.2:
![HW 2.2 Output][m101js_hw22_Output]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 2.2 MongoProc][m101js_hw22_MongoProc]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 2.3
![HW 2.3 Submit][m101js_hw23_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 2.3:
Code snippet for `this.addUser = function(username, password, email, callback)` function:
{% highlight javascript linenos=table %}
this.addUser = function(username, password, email, callback) {
    "use strict";

    // Generate password hash
    var salt = bcrypt.genSaltSync();
    var password_hash = bcrypt.hashSync(password, salt);

    // Create user document
    var user = {'_id': username, 'password': password_hash};

    // Add email if set
    if (email != "") {
        user['email'] = email;
    }

    // TODO: hw2.3
    users.insert(user, function (err, result) {
        "use strict";

        if (!err) {
            console.log("Inserted new user");
            return callback(null, result[0]);
        }

        return callback(err, null);
    });
}
{% endhighlight %}

Code snippet for `this.validateLogin = function(username, password, callback)` function:
{% highlight javascript linenos=table %}
this.validateLogin = function(username, password, callback) {
    "use strict";

    // Callback to pass to MongoDB that validates a user document
    function validateUserDoc(err, user) {
        "use strict";

        if (err) return callback(err, null);

        if (user) {
            if (bcrypt.compareSync(password, user.password)) {
                callback(null, user);
            }
            else {
                var invalid_password_error = new Error("Invalid password");
                // Set an extra field so we can distinguish this from a db error
                invalid_password_error.invalid_password = true;
                callback(invalid_password_error, null);
            }
        }
        else {
            var no_such_user_error = new Error("User: " + user + " does not exist");
            // Set an extra field so we can distinguish this from a db error
            no_such_user_error.no_such_user = true;
            callback(no_such_user_error, null);
        }
    }

    // TODO: hw2.3
    users.findOne({ '_id' : username }, validateUserDoc);
}
{% endhighlight %}

![HW 2.3 Terminal][m101js_hw23_Terminal]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 2.3 MongoProc][m101js_hw23_MongoProc]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Week 3: Schema Design
Following are presented the contents and answers to the HomeWorks belonging to **Week 3 (Schema Design)** for "*M101JS: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


### HOMEWORK 3.1
![HW 3.1 Submit][m101js_hw31_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 3.1:
![HW 3.1 Import][m101js_hw31_Import]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 3.1 MongoChecks][m101js_hw31_MongoChecks]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 3.1 Output][m101js_hw31_Output]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 3.1 MongoVerify][m101js_hw31_MongoVerify]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 3.2

### ANSWER HOMEWORK 3.2:
Code snippet for `this.insertEntry = function (title, body, tags, author, callback)` function:
{% highlight javascript linenos=table %}
this.insertEntry = function (title, body, tags, author, callback) {
    "use strict";
    console.log("inserting blog entry" + title + body);

    // fix up the permalink to not include whitespace
    var permalink = title.replace( /\s/g, '_' );
    permalink = permalink.replace( /\W/g, '' );

    // Build a new post
    var post = {"title": title,
            "author": author,
            "body": body,
            "permalink":permalink,
            "tags": tags,
            "comments": [],
            "date": new Date()}

    // now insert the post
    // hw3.2 TODO
    posts.insert(post, function (err, result) {
        "use strict";

        if (err) return callback(err, null);

        console.log("Inserted new post");
        callback(err, permalink);
    });
}
{% endhighlight %}


### HOMEWORK 3.3

### ANSWER HOMEWORK 3.3:
Code snippet for `this.addComment = function(permalink, name, email, body, callback)` function:
{% highlight javascript linenos=table %}
this.addComment = function(permalink, name, email, body, callback) {
    "use strict";

    var comment = {'author': name, 'body': body}

    if (email != "") {
        comment['email'] = email
    }

    // hw3.3 TODO
    posts.update({'permalink': permalink}, {'$push': {'comments': comment}}, function(err, numModified) {
        "use strict";

        if (err) return callback(err, null);

        callback(err, numModified);
    });
}
{% endhighlight %}



## Week 4: Performance
Following are presented the contents and answers to the HomeWorks belonging to **Week 4 (Performance)** for "*M101JS: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


### HOMEWORK 4.1

### ANSWER HOMEWORK 4.1:


### HOMEWORK 4.2

### ANSWER HOMEWORK 4.2:


### HOMEWORK 4.3

### ANSWER HOMEWORK 4.3:


### HOMEWORK 4.4

### ANSWER HOMEWORK 4.4:



## Week 5: Aggregation Framework
Following are presented the contents and answers to the HomeWorks belonging to **Week 5 (Aggregation Framework)** for "*M101JS: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


### HOMEWORK 5.1

### ANSWER HOMEWORK 5.1:
{% highlight javascript linenos=table %}
db.posts.aggregate([
	{"$unwind":"$comments"},
	{"$group":{
		"_id":"$comments.author",
		"count":{"$sum":1}
	}},
	{"$sort":{
		"count":-1
	}},
	{"$limit":1}
])
{% endhighlight %}


### HOMEWORK 5.2

### ANSWER HOMEWORK 5.2:
{% highlight javascript linenos=table %}
db.zips.aggregate([
	{"$match":{
		"state":{"$in":["CA","NY"]}
	}},
	{"$group":{
		"_id":{
			"city":"$city",
			"state":"$state"
		},
		"pop":{"$sum":"$pop"}
	}},
	{"$match":{
		"pop":{"$gt":25000}
	}},
	{"$group":{
		"_id":null,
		"avg_requested":{"$avg":"$pop"}
	}}
])
{% endhighlight %}


### HOMEWORK 5.3

### ANSWER HOMEWORK 5.3:
{% highlight javascript linenos=table %}
db.grades.aggregate([
	{"$unwind":"$scores"},
	{"$match":{
		"scores.type":{"$ne":"quiz"}
	}},
	{"$group":{
		"_id":{
			"class_id":"$class_id",
			"student_id":"$student_id"
		},
		"avg_class_student":{"$avg":"$scores.score"}
	}},
	{"$group":{
		"_id":"$_id.class_id",
		"avg_class":{"$avg":"$avg_class_student"}
	}},
	{"$sort":{
		"avg_class":-1
	}},
	{"$limit":1}
])
{% endhighlight %}


### HOMEWORK 5.4

### ANSWER HOMEWORK 5.4:
{% highlight javascript linenos=table %}
use test
db.zips.aggregate([
	{"$match":{
		"city":{"$regex":/^\d/}
	}},
	{"$group":{
		"_id":null,
		"pop_rural":{"$sum":"$pop"}
	}}
])
{% endhighlight %}



## Week 6: Application Engineering
Following are presented the contents and answers to the HomeWorks belonging to **Week 6 (Application Engineering)** for "*M101JS: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


### HOMEWORK 6.1

### ANSWER HOMEWORK 6.1:
![HW 6.1 Submit][m101js_hw61_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 6.2

### ANSWER HOMEWORK 6.2:
![HW 6.2 Submit][m101js_hw62_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 6.3

### ANSWER HOMEWORK 6.3:
![HW 6.3 Submit][m101js_hw63_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 6.4

### ANSWER HOMEWORK 6.4:
![HW 6.4 Submit][m101js_hw64_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 6.5
![HW 6.5 Submit][m101js_hw65_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 6.5:
![HW 6.5 Bash][m101js_hw65_Bash]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.5 Mongo][m101js_hw65_Mongo]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.5 Validate][m101js_hw65_Validate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Final Exam
Following are presented the contents and answers to the questions belonging to **Final Exam** for "*M101JS: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


### Final: Question 1
![M101JS Final Question 1 Statement][m101js_final01_Statement]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 1. Answer:
{% highlight text linenos=table %}
$ l
enron.zip
$ unzip enron.zip
Archive:  enron.zip
   creating: dump/enron/
  inflating: dump/enron/messages.bson  
  inflating: dump/enron/messages.metadata.json  
$ l
dump/  enron.zip
$ /opt/mongodb-linux-i686-2.6.0/bin/mongorestore
connected to: 127.0.0.1
2014-05-08T14:20:31.377-0500 dump/enron/messages.bson
2014-05-08T14:20:31.377-0500 	going into namespace [enron.messages]
2014-05-08T14:20:34.690-0500 		Progress: 67557346/396236668	17%	(bytes)
2014-05-08T14:20:37.006-0500 		Progress: 106126234/396236668	26%	(bytes)
2014-05-08T14:20:40.255-0500 		Progress: 139318935/396236668	35%	(bytes)
2014-05-08T14:20:43.108-0500 		Progress: 188432810/396236668	47%	(bytes)
2014-05-08T14:20:46.008-0500 		Progress: 227400576/396236668	57%	(bytes)
2014-05-08T14:20:49.030-0500 		Progress: 282543056/396236668	71%	(bytes)
2014-05-08T14:20:52.128-0500 		Progress: 332774078/396236668	83%	(bytes)
2014-05-08T14:20:55.102-0500 		Progress: 372492093/396236668	94%	(bytes)
120477 objects found
2014-05-08T14:20:57.348-0500 	Creating index: { key: { _id: 1 }, ns: "enron.messages", name: "_id_" }
$ l
dump/  enron.zip
$
{% endhighlight %}
![M101JS Final Question 1 UnzipRestore][m101js_final01_UnzipRestore]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
> show dbs
admin    (empty)
blog     0.078GB
final7   0.078GB
local    0.078GB
m101     0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> show dbs
admin    (empty)
blog     0.078GB
enron    0.953GB
final7   0.078GB
local    0.078GB
m101     0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> use enron
switched to db enron
> show tables
messages
system.indexes
> db.messages.count()
120477
>
{% endhighlight %}
![M101JS Final Question 1 MongoChecks][m101js_final01_MongoChecks]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
$ l
dump/  enron.zip  final01_00.js
$ cat final01_00.js
use enron
query = {"headers.From":"andrew.fastow@enron.com", "headers.To":"john.lavorato@enron.com"}
db.messages.find(query).count()
$ /opt/mongodb-linux-i686-2.6.0/bin/mongo < final01_00.js
MongoDB shell version: 2.6.0
connecting to: test
switched to db enron
{
	"headers.From" : "andrew.fastow@enron.com",
	"headers.To" : "john.lavorato@enron.com"
}
1
bye
{% endhighlight %}
![M101JS Final Question 1 MongoQuery][m101js_final01_MongoQuery]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 2
![M101JS Final Question 2 Statement][m101js_final02_Statement]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 2. Answer:
Query to be executed:
{% highlight javascript linenos=table %}
use enron

project = {_id:1, headers:1, body:0}

unwind_original_to = {$unwind:"$headers.To"}

group_by_idfrom_then_addtoset = {$group:{
	_id: {oid:"$_id", from:"$headers.From"},
	to_filtered: {$addToSet:"$headers.To"}
}}

unwind_filtered_to = {$unwind:"$to_filtered"}

group_by_fromto = {$group:{
	_id: {from:"$_id.from", to:"$to_filtered"},
	count: {$sum:1}
}}

sort_count_desc = {$sort: {count:-1}}

limit = {$limit:5}

db.messages.aggregate(
	project,
	unwind_original_to,
	group_by_idfrom_then_addtoset,
	unwind_filtered_to,
	group_by_fromto,
	sort_count_desc,
	limit
)
{% endhighlight %}

Output of the previous query:
{% highlight text linenos=table %}
MongoDB shell version: 2.6.0
connecting to: test
switched to db enron
null
null
null
null
null
null
null
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "jeff.dasovich@enron.com" }, "count" : 750 }
{ "_id" : { "from" : "soblander@carrfut.com", "to" : "soblander@carrfut.com" }, "count" : 679 }
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "james.steffes@enron.com" }, "count" : 646 }
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "richard.shapiro@enron.com" }, "count" : 616 }
{ "_id" : { "from" : "evelyn.metoyer@enron.com", "to" : "kate.symes@enron.com" }, "count" : 567 }
bye
{% endhighlight %}

Execution on the terminal:
{% highlight text linenos=table %}
$ l
dump/  enron.zip  final01_00.js  final02_00.js  final02_01.js
$ cat final02_00.js
use enron

project = {$project: {_id:1, headers:1}}; null;

unwind_original_to = {$unwind:"$headers.To"}; null;

group_by_idfrom_then_addtoset = {$group:{
	_id: {oid:"$_id", from:"$headers.From"},
	to_filtered: {$addToSet:"$headers.To"}
}}; null;

unwind_filtered_to = {$unwind:"$to_filtered"}; null;

group_by_fromto = {$group:{
	_id: {from:"$_id.from", to:"$to_filtered"},
	count: {$sum:1}
}}; null;

sort_count_desc = {$sort: {count:-1}}; null;

limit = {$limit:5}; null;

db.messages.aggregate(
	project,
	unwind_original_to,
	group_by_idfrom_then_addtoset,
	unwind_filtered_to,
	group_by_fromto,
	sort_count_desc,
	limit
)
$ /opt/mongodb-linux-i686-2.6.0/bin/mongo < final02_00.js
MongoDB shell version: 2.6.0
connecting to: test
switched to db enron
null
null
null
null
null
null
null
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "jeff.dasovich@enron.com" }, "count" : 750 }
{ "_id" : { "from" : "soblander@carrfut.com", "to" : "soblander@carrfut.com" }, "count" : 679 }
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "james.steffes@enron.com" }, "count" : 646 }
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "richard.shapiro@enron.com" }, "count" : 616 }
{ "_id" : { "from" : "evelyn.metoyer@enron.com", "to" : "kate.symes@enron.com" }, "count" : 567 }
bye
$ /opt/mongodb-linux-i686-2.6.0/bin/mongo < final02_00.js > final02_00.out
$
{% endhighlight %}
![M101JS Final Question 2 Terminal][m101js_final02_Terminal]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 3
![M101JS Final Question 3 Statement 1][m101js_final03_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 3 Statement 2][m101js_final03_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 3. Answer:
{% highlight text linenos=table %}
> db.messages.find({"headers.Message-ID":"<8147308.1075851042335.JavaMail.evans@thyme>"}).count()
1
> db.messages.findOne({"headers.Message-ID":"<8147308.1075851042335.JavaMail.evans@thyme>"}, {body:0})
{
	"_id" : ObjectId("4f16fd52d1e2d32371039e44"),
	"filename" : "1646.",
	"headers" : {
		"Content-Transfer-Encoding" : "7bit",
		"Content-Type" : "text/plain; charset=us-ascii",
		"Date" : ISODate("2001-07-13T14:47:00Z"),
		"From" : "donna.fulton@enron.com",
		"Message-ID" : "<8147308.1075851042335.JavaMail.evans@thyme>",
		"Mime-Version" : "1.0",
		"Subject" : "RTO Orders - Grid South, SE Trans, SPP and Entergy",
		"To" : [
			"steven.kean@enron.com",
			"richard.shapiro@enron.com",
			"james.steffes@enron.com",
			"christi.nicolay@enron.com",
			"sarah.novosel@enron.com",
			"ray.alvarez@enron.com",
			"sscott3@enron.com",
			"joe.connor@enron.com",
			"dan.staines@enron.com",
			"steve.montovano@enron.com",
			"kevin.presto@enron.com",
			"rogers.herndon@enron.com",
			"mike.carson@enron.com",
			"john.forney@enron.com",
			"laura.podurgiel@enron.com",
			"gretchen.lotz@enron.com",
			"juan.hernandez@enron.com",
			"miguel.garcia@enron.com",
			"rudy.acevedo@enron.com",
			"heather.kroll@enron.com",
			"david.fairley@enron.com",
			"elizabeth.johnston@enron.com",
			"bill.rust@enron.com",
			"edward.baughman@enron.com",
			"terri.clynes@enron.com",
			"oscar.dalton@enron.com",
			"doug.sewell@enron.com",
			"larry.valderrama@enron.com",
			"nick.politis@enron.com",
			"fletcher.sturm@enron.com",
			"chris.dorland@enron.com",
			"jeff.king@enron.com",
			"john.kinser@enron.com",
			"matt.lorenz@enron.com",
			"patrick.hansen@enron.com",
			"lloyd.will@enron.com",
			"dduaran@enron.com",
			"john.lavorato@enron.com",
			"louise.kitchen@enron.com",
			"greg.whalley@enron.com"
		],
		"X-FileName" : "skean.nsf",
		"X-Folder" : "\\Steven_Kean_Oct2001_2\\Notes Folders\\Attachments",
		"X-From" : "Donna Fulton",
		"X-Origin" : "KEAN-S",
		"X-To" : "Steven J Kean, Richard Shapiro, James D Steffes, Christi L Nicolay, Sarah Novosel, Ray Alvarez, sscott3@enron.com, Joe Connor, Dan Staines, Steve Montovano, Kevin M Presto, Rogers Herndon, Mike Carson, John M Forney, Laura Podurgiel, Gretchen Lotz, Juan Hernandez, Miguel L Garcia, Rudy Acevedo, Heather Kroll, David Fairley, Elizabeth Johnston, Bill Rust, Edward D Baughman, Terri Clynes, Oscar Dalton, Doug Sewell, Larry Valderrama, Nick Politis, Fletcher J Sturm, Chris Dorland, Jeff King, John Kinser, Matt Lorenz, Patrick Hansen, Lloyd Will, dduaran@enron.com, John J Lavorato, Louise Kitchen, Greg Whalley",
		"X-bcc" : "",
		"X-cc" : ""
	},
	"mailbox" : "kean-s",
	"subFolder" : "attachments"
}
> db.messages.update({"headers.Message-ID":"<8147308.1075851042335.JavaMail.evans@thyme>"}, {$addToSet:{"headers.To":"mrpotatohead@mongodb.com"}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.messages.findOne({"headers.Message-ID":"<8147308.1075851042335.JavaMail.evans@thyme>"}, {body:0})
{
	"_id" : ObjectId("4f16fd52d1e2d32371039e44"),
	"filename" : "1646.",
	"headers" : {
		"Content-Transfer-Encoding" : "7bit",
		"Content-Type" : "text/plain; charset=us-ascii",
		"Date" : ISODate("2001-07-13T14:47:00Z"),
		"From" : "donna.fulton@enron.com",
		"Message-ID" : "<8147308.1075851042335.JavaMail.evans@thyme>",
		"Mime-Version" : "1.0",
		"Subject" : "RTO Orders - Grid South, SE Trans, SPP and Entergy",
		"To" : [
			"steven.kean@enron.com",
			"richard.shapiro@enron.com",
			"james.steffes@enron.com",
			"christi.nicolay@enron.com",
			"sarah.novosel@enron.com",
			"ray.alvarez@enron.com",
			"sscott3@enron.com",
			"joe.connor@enron.com",
			"dan.staines@enron.com",
			"steve.montovano@enron.com",
			"kevin.presto@enron.com",
			"rogers.herndon@enron.com",
			"mike.carson@enron.com",
			"john.forney@enron.com",
			"laura.podurgiel@enron.com",
			"gretchen.lotz@enron.com",
			"juan.hernandez@enron.com",
			"miguel.garcia@enron.com",
			"rudy.acevedo@enron.com",
			"heather.kroll@enron.com",
			"david.fairley@enron.com",
			"elizabeth.johnston@enron.com",
			"bill.rust@enron.com",
			"edward.baughman@enron.com",
			"terri.clynes@enron.com",
			"oscar.dalton@enron.com",
			"doug.sewell@enron.com",
			"larry.valderrama@enron.com",
			"nick.politis@enron.com",
			"fletcher.sturm@enron.com",
			"chris.dorland@enron.com",
			"jeff.king@enron.com",
			"john.kinser@enron.com",
			"matt.lorenz@enron.com",
			"patrick.hansen@enron.com",
			"lloyd.will@enron.com",
			"dduaran@enron.com",
			"john.lavorato@enron.com",
			"louise.kitchen@enron.com",
			"greg.whalley@enron.com",
			"mrpotatohead@mongodb.com"
		],
		"X-FileName" : "skean.nsf",
		"X-Folder" : "\\Steven_Kean_Oct2001_2\\Notes Folders\\Attachments",
		"X-From" : "Donna Fulton",
		"X-Origin" : "KEAN-S",
		"X-To" : "Steven J Kean, Richard Shapiro, James D Steffes, Christi L Nicolay, Sarah Novosel, Ray Alvarez, sscott3@enron.com, Joe Connor, Dan Staines, Steve Montovano, Kevin M Presto, Rogers Herndon, Mike Carson, John M Forney, Laura Podurgiel, Gretchen Lotz, Juan Hernandez, Miguel L Garcia, Rudy Acevedo, Heather Kroll, David Fairley, Elizabeth Johnston, Bill Rust, Edward D Baughman, Terri Clynes, Oscar Dalton, Doug Sewell, Larry Valderrama, Nick Politis, Fletcher J Sturm, Chris Dorland, Jeff King, John Kinser, Matt Lorenz, Patrick Hansen, Lloyd Will, dduaran@enron.com, John J Lavorato, Louise Kitchen, Greg Whalley",
		"X-bcc" : "",
		"X-cc" : ""
	},
	"mailbox" : "kean-s",
	"subFolder" : "attachments"
}
>
{% endhighlight %}

![M101JS Final Question 3 MongoShell1][m101js_final03_MongoShell1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 3 MongoShell2][m101js_final03_MongoShell2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 3 MongoShell3][m101js_final03_MongoShell3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 3 MongoShell4][m101js_final03_MongoShell4]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
$ l
final03_00.js  final3-validate.js*  package.json*
$ nodejs final3-validate.js

module.js:340
    throw err;
          ^
Error: Cannot find module 'mongodb'
    at Function.Module._resolveFilename (module.js:338:15)
    at Function.Module._load (module.js:280:25)
    at Module.require (module.js:364:17)
    at require (module.js:380:17)
    at Object.<anonymous> (final3-validate.js:1:5982)
    at Module._compile (module.js:456:26)
    at Object.Module._extensions..js (module.js:474:10)
    at Module.load (module.js:356:32)
    at Function.Module._load (module.js:312:12)
    at Function.Module.runMain (module.js:497:10)
$ npm install
npm WARN package.json question-3@0.0.0 No README.md file found!
npm http GET https://registry.npmjs.org/mongodb
npm http GET https://registry.npmjs.org/commander
npm http 200 https://registry.npmjs.org/commander
npm http 200 https://registry.npmjs.org/mongodb
npm http GET https://registry.npmjs.org/bson/0.2.5
npm http GET https://registry.npmjs.org/kerberos/0.0.3
npm http 304 https://registry.npmjs.org/bson/0.2.5
npm http 304 https://registry.npmjs.org/kerberos/0.0.3

> kerberos@0.0.3 install node_modules/mongodb/node_modules/kerberos
> (node-gyp rebuild 2> builderror.log) || (exit 0)


> bson@0.2.5 install node_modules/mongodb/node_modules/bson
> (node-gyp rebuild 2> builderror.log) || (exit 0)

make: Entering directory `node_modules/mongodb/node_modules/kerberos/build'
make: Entering directory `node_modules/mongodb/node_modules/bson/build'
  CXX(target) Release/obj.target/bson/ext/bson.o
  SOLINK_MODULE(target) Release/obj.target/kerberos.node
  SOLINK_MODULE(target) Release/obj.target/kerberos.node: Finished
  COPY Release/kerberos.node
make: Leaving directory `node_modules/mongodb/node_modules/kerberos/build'
  SOLINK_MODULE(target) Release/obj.target/bson.node
  SOLINK_MODULE(target) Release/obj.target/bson.node: Finished
  COPY Release/bson.node
make: Leaving directory `node_modules/mongodb/node_modules/bson/build'
commander@2.0.0 node_modules/commander

mongodb@1.3.23 node_modules/mongodb
├── kerberos@0.0.3
└── bson@0.2.5
$ l
final03_00.js  final3-validate.js*  node_modules/  package.json*
$ nodejs final3-validate.js
Welcome to the Final Exam Q3 Checker. My job is to make sure you correctly updated the document
Final Exam Q3 Validated successfully!
Your validation code is: vOnRg05kwcqyEFSve96R
$
{% endhighlight %}

![M101JS Final Question 3 Validate1][m101js_final03_Validate1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 3 Validate2][m101js_final03_Validate2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 4
![M101JS Final Question 4 Statement1][m101js_final04_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 4 Statement2][m101js_final04_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 4. Answer:
{% highlight text linenos=table %}
$ l
final4/  posts.json
question_4$ cd final4/l
-bash: cd: final4/l: No such file or directory
question_4$ cd final4/
question_4/final4$ l
Final4/  __MACOSX/
question_4/final4$ cd Final4/
question_4/final4/Final4$ l
blog/  validate/
question_4/final4/Final4$ cd validate/
question_4/final4/Final4/validate$ l
final4-validate.js*  package.json*
question_4/final4/Final4/validate$ cd ../blog/
question_4/final4/Final4/blog$ l
app.js*  package.json*  posts.js*  README.md*  routes/  sessions.js*  users.js*  views/
question_4/final4/Final4/blog$ cd ../../
question_4/final4$ l
Final4/  __MACOSX/
question_4/final4$ cd ..
question_4$ l
final4/  posts.json
question_4$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d blog -c posts posts.json
connected to: 127.0.0.1
2014-05-08T15:44:37.195-0500 		Progress: 34788312/34788312	100%
2014-05-08T15:44:37.195-0500 			1000	333/second
2014-05-08T15:44:37.195-0500 check 9 1000
2014-05-08T15:44:37.196-0500 imported 1000 objects
question_4$
{% endhighlight %}
![M101JS Final Question 4 InspectImport][m101js_final04_InspectImport]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
> show dbs
admin    (empty)
enron    0.953GB
final7   0.078GB
local    0.078GB
m101     0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> show dbs
admin    (empty)
blog     0.078GB
enron    0.953GB
final7   0.078GB
local    0.078GB
m101     0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> use blog
switched to db blog
> show tables
posts
system.indexes
> db.posts.count()
1000
>
{% endhighlight %}
![M101JS Final Question 4 MongoChecks][m101js_final04_MongoChecks]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
blog$ nodejs app.js
Express server listening on port 3000
Found 10 posts
{% endhighlight %}
![M101JS Final Question 4 RunBlog][m101js_final04_RunBlog]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
validate$ l
final4-validate.js*  node_modules/  package.json*
validate$ nodejs final4-validate.js
Trying to fetch blog homepage for url http://localhost:3000/
Trying to grab the number of likes for url http://localhost:3000/post/mxwnnnqaflufnqwlekfd
Trying to increment the number of likes for post: /post/mxwnnnqaflufnqwlekfd
Trying to grab the number of likes for url http://localhost:3000/post/mxwnnnqaflufnqwlekfd
Successfully clicked like
Blog validated successfully!
Your validation code is: VQ3jedFjG5VmElLTYKqS
validate$
{% endhighlight %}
![M101JS Final Question 4 Validate][m101js_final04_Validate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 5
![M101JS Final Question 5 Statement1][m101js_final05_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 5 Statement2][m101js_final05_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 5 Statement3][m101js_final05_Statement3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"ns" : "test.fubar",
		"name" : "_id_"
	},
	{
		"v" : 1,
		"key" : {
			"a" : 1,
			"b" : 1
		},
		"ns" : "test.fubar",
		"name" : "a_1_b_1"
	},
	{
		"v" : 1,
		"key" : {
			"a" : 1,
			"c" : 1
		},
		"ns" : "test.fubar",
		"name" : "a_1_c_1"
	},
	{
		"v" : 1,
		"key" : {
			"c" : 1
		},
		"ns" : "test.fubar",
		"name" : "c_1"
	},
	{
		"v" : 1,
		"key" : {
			"a" : 1,
			"b" : 1,
			"c" : -1
		},
		"ns" : "test.fubar",
		"name" : "a_1_b_1_c_-1"
	}
]
{% endhighlight %}

{% highlight text linenos=table %}
db.fubar.find({'a':{'$lt':10000}, 'b':{'$gt': 5000}}, {'a':1, 'c':1}).sort({'c':-1})
{% endhighlight %}

### Final: Question 5. Answer:


### Final: Question 6
![M101JS Final Question 6 Statement1][m101js_final06_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 6 Statement2][m101js_final06_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
{
	"_id" : ObjectId("50c598f582094fb5f92efb96"),
	"first_name" : "John",
	"last_name" : "Doe",
	"date_of_admission" : ISODate("2010-02-21T05:00:00Z"),
	"residence_hall" : "Fairweather",
	"has_car" : true,
	"student_id" : "2348023902",
	"current_classes" : [
		"His343",
		"Math234",
		"Phy123",
		"Art232"
	]
}
{% endhighlight %}

### Final: Question 6. Answer:


### Final: Question 7
![M101JS Final Question 7 Statement1][m101js_final07_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 7 Statement2][m101js_final07_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 7. Answer:
{% highlight text linenos=table %}
question_7/final7/final7$ l
albums.json*  images.json*
question_7/final7/final7$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d final07 -c albums albums.json
connected to: 127.0.0.1
2014-05-09T22:04:49.534-0500 check 9 1000
2014-05-09T22:04:49.535-0500 imported 1000 objects
question_7/final7/final7$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d final07 -c images images.json
connected to: 127.0.0.1
2014-05-09T22:05:03.003-0500 		Progress: 7098364/9635092	73%
2014-05-09T22:05:03.004-0500 			73700	24566/second
2014-05-09T22:05:03.915-0500 check 9 100000
2014-05-09T22:05:04.562-0500 imported 100000 objects
question_7/final7/final7$
{% endhighlight %}
![M101JS Final Question 7 Statement2][m101js_final07_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
> show dbs
admin    (empty)
blog     0.078GB
enron    0.953GB
local    0.078GB
m101     0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> show dbs
admin    (empty)
blog     0.078GB
enron    0.953GB
final07  0.078GB
local    0.078GB
m101     0.078GB
school   0.078GB
test     0.078GB
weather  0.078GB
> use final07
switched to db final07
> show tables
albums
images
system.indexes
> db.albums.count()
1000
> db.images.count()
100000
> db.albums.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "final07.albums"
	}
]
> db.images.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "final07.images"
	}
]
> db.albums.findOne()
{
	"_id" : 0,
	"images" : [
		2433,
		2753,
		2983,
		6510,
		11375,
		12974,
		15344,
		16952,
		19722,
		23077,
		24772,
		31401,
		32579,
		32939,
		33434,
		36328,
		39247,
		39892,
		40597,
		45675,
		46147,
		46225,
		48406,
		49947,
		55361,
		57420,
		60101,
		62423,
		64640,
		65000,
		67203,
		68064,
		75918,
		80196,
		80642,
		82848,
		83837,
		84460,
		86419,
		87089,
		88595,
		88904,
		89308,
		91989,
		92411,
		98135,
		98548,
		99334
	]
}
> db.images.findOne()
{
	"_id" : 0,
	"height" : 480,
	"width" : 640,
	"tags" : [
		"dogs",
		"work"
	]
}
> db.albums.ensureIndex({images:1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}
> db.images.ensureIndex({tags:1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}
> db.albums.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "final07.albums"
	},
	{
		"v" : 1,
		"key" : {
			"images" : 1
		},
		"name" : "images_1",
		"ns" : "final07.albums"
	}
]
> db.images.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "final07.images"
	},
	{
		"v" : 1,
		"key" : {
			"tags" : 1
		},
		"name" : "tags_1",
		"ns" : "final07.images"
	}
]
> db.images.find({tags:'kittens'}).count()
49932
>
{% endhighlight %}

![M101J Final Question 7 MongoChecks1][m101js_final07_MongoChecks1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101J Final Question 7 MongoChecks2][m101js_final07_MongoChecks2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101J Final Question 7 MongoChecks3][m101js_final07_MongoChecks3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101J Final Question 7 MongoChecks4][m101js_final07_MongoChecks4]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos=table %}
> db.albums.findOne()
{
	"_id" : 67
	"images" : [
		4745,
		7651,
		15247,
		17517,
		17853,
		20529,
		22640,
		27299,
		27997,
		32930,
		35591,
		48969,
		52901,
		57320,
		96342,
		99705
	]
}

> db.images.findOne()
{ "_id" : 99705, "height" : 480, "width" : 640, "tags" : [ "dogs", "kittens", "work" ] }
{% endhighlight %}

![M101JS Final Question 7 Terminal][m101js_final07_Terminal]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 8
![M101JS Final Question 8 Statement][m101js_final08_Statement]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 8. Answer:


### Final: Question 9
![M101JS Final Question 9 Statement][m101js_final09_Statement]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 9. Answer:


### Final: Question 10
![M101JS Final Question 10 Statement1][m101js_final10_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS Final Question 10 Statement2][m101js_final10_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 10. Answer:
{% highlight javascript linenos=table %}
db.messages.find({'headers.Date':{'$gt': new Date(2001,3,1)}},{'headers.From':1, _id:0}).sort({'headers.From':1}).explain()
{% endhighlight %}

{% highlight javascript linenos=table %}
{
	"cursor" : "BtreeCursor headers.From_1",
	"isMultiKey" : false,
	"n" : 83057,
	"nscannedObjects" : 120477,
	"nscanned" : 120477,
	"nscannedObjectsAllPlans" : 120581,
	"nscannedAllPlans" : 120581,
	"scanAndOrder" : false,
	"indexOnly" : false,
	"nYields" : 0,
	"nChunkSkips" : 0,
	"millis" : 250,
	"indexBounds" : {
		"headers.From" : [
			[
				{
					"$minElement" : 1
				},
				{
					"$maxElement" : 1
				}
			]
		]
	},
	"server" : "Andrews-iMac.local:27017"
}
{% endhighlight %}



## PROGRESS
![M101JS progress][m101js_progress]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## CERTIFICATE
![M101JS downloadcertificate][m101_downloadcertificate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101JS certificate][m101js_certificate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## REFERENCES
* [https://university.mongodb.com/][1]{: target="_blank"}
* [http://api.mongodb.org/][2]{: target="_blank"}
* [http://www.mongodb.com/][3]{: target="_blank"}
* [http://hao-deng.blogspot.com/search/label/Mongodb][4]{: target="_blank"}
* [http://ankurthetechie.blogspot.com/2013/09/final-exam-answers-with-explanation-for.html][5]{: target="_blank"}
* [http://techinote.com/mongodb-final-exam/][6]{: target="_blank"}

[1]: https://university.mongodb.com/
[2]: http://api.mongodb.org/
[3]: http://www.mongodb.com/
[4]: http://hao-deng.blogspot.com/search/label/Mongodb
[5]: http://ankurthetechie.blogspot.com/2013/09/final-exam-answers-with-explanation-for.html
[6]: http://techinote.com/mongodb-final-exam/

[m101js_hw11_Submit]: {{ site.baseurl }}/assets/m101js_hw11_Submit.png
[m101js_hw11_MongoRestore]: {{ site.baseurl }}/assets/m101js_hw11_MongoRestore.png
[m101js_hw11_MongoChecks]: {{ site.baseurl }}/assets/m101js_hw11_MongoChecks.png
[m101js_hw12_Submit]: {{ site.baseurl }}/assets/m101js_hw12_Submit.png
[m101js_hw13_Submit]: {{ site.baseurl }}/assets/m101js_hw13_Submit.png
[m101js_hw21_Submit]: {{ site.baseurl }}/assets/m101js_hw21_Submit.png
[m101js_hw21_ImportMongoOutput]: {{ site.baseurl }}/assets/m101js_hw21_ImportMongoOutput.png
[m101js_hw21_MongoshellChecks]: {{ site.baseurl }}/assets/m101js_hw21_MongoshellChecks.png
[m101js_hw22_Submit]: {{ site.baseurl }}/assets/m101js_hw22_Submit.png
[m101js_hw22_Output]: {{ site.baseurl }}/assets/m101js_hw22_Output.png
[m101js_hw22_MongoProc]: {{ site.baseurl }}/assets/m101js_hw22_MongoProc.png
[m101js_hw23_Submit]: {{ site.baseurl }}/assets/m101js_hw23_Submit.png
[m101js_hw23_Terminal]: {{ site.baseurl }}/assets/m101js_hw23_Terminal.png
[m101js_hw23_MongoProc]: {{ site.baseurl }}/assets/m101js_hw23_MongoProc.png
[m101js_hw31_Submit]: {{ site.baseurl }}/assets/m101js_hw31_Submit.png
[m101js_hw31_Import]: {{ site.baseurl }}/assets/m101js_hw31_Import.png
[m101js_hw31_MongoChecks]: {{ site.baseurl }}/assets/m101js_hw31_MongoChecks.png
[m101js_hw31_Output]: {{ site.baseurl }}/assets/m101js_hw31_Output.png
[m101js_hw31_MongoVerify]: {{ site.baseurl }}/assets/m101js_hw31_MongoVerify.png
[m101js_hw61_Submit]: {{ site.baseurl }}/assets/m101js_hw61_Submit.png
[m101js_hw62_Submit]: {{ site.baseurl }}/assets/m101js_hw62_Submit.png
[m101js_hw63_Submit]: {{ site.baseurl }}/assets/m101js_hw63_Submit.png
[m101js_hw64_Submit]: {{ site.baseurl }}/assets/m101js_hw64_Submit.png
[m101js_hw65_Submit]: {{ site.baseurl }}/assets/m101js_hw65_Submit.png
[m101js_hw65_Bash]: {{ site.baseurl }}/assets/m101js_hw65_Bash.png
[m101js_hw65_Mongo]: {{ site.baseurl }}/assets/m101js_hw65_Mongo.png
[m101js_hw65_Validate]: {{ site.baseurl }}/assets/m101js_hw65_Validate.png
[m101js_final01_Statement]: {{ site.baseurl }}/assets/m101js_final01_Statement.png
[m101js_final01_UnzipRestore]: {{ site.baseurl }}/assets/m101js_final01_UnzipRestore.png
[m101js_final01_MongoChecks]: {{ site.baseurl }}/assets/m101js_final01_MongoChecks.png
[m101js_final01_MongoQuery]: {{ site.baseurl }}/assets/m101js_final01_MongoQuery.png
[m101js_final02_Statement]: {{ site.baseurl }}/assets/m101js_final02_Statement.png
[m101js_final02_Terminal]: {{ site.baseurl }}/assets/m101js_final02_Terminal.png
[m101js_final03_Statement1]: {{ site.baseurl }}/assets/m101js_final03_Statement1.png
[m101js_final03_Statement2]: {{ site.baseurl }}/assets/m101js_final03_Statement2.png
[m101js_final03_MongoShell1]: {{ site.baseurl }}/assets/m101js_final03_MongoShell1.png
[m101js_final03_MongoShell2]: {{ site.baseurl }}/assets/m101js_final03_MongoShell2.png
[m101js_final03_MongoShell3]: {{ site.baseurl }}/assets/m101js_final03_MongoShell3.png
[m101js_final03_MongoShell4]: {{ site.baseurl }}/assets/m101js_final03_MongoShell4.png
[m101js_final03_Validate1]: {{ site.baseurl }}/assets/m101js_final03_Validate1.png
[m101js_final03_Validate2]: {{ site.baseurl }}/assets/m101js_final03_Validate2.png
[m101js_final04_Statement1]: {{ site.baseurl }}/assets/m101js_final04_Statement1.png
[m101js_final04_Statement2]: {{ site.baseurl }}/assets/m101js_final04_Statement2.png
[m101js_final04_InspectImport]: {{ site.baseurl }}/assets/m101js_final04_InspectImport.png
[m101js_final04_MongoChecks]: {{ site.baseurl }}/assets/m101js_final04_MongoChecks.png
[m101js_final04_RunBlog]: {{ site.baseurl }}/assets/m101js_final04_RunBlog.png
[m101js_final04_Validate]: {{ site.baseurl }}/assets/m101js_final04_Validate.png
[m101js_final05_Statement1]: {{ site.baseurl }}/assets/m101js_final05_Statement1.png
[m101js_final05_Statement2]: {{ site.baseurl }}/assets/m101js_final05_Statement2.png
[m101js_final05_Statement3]: {{ site.baseurl }}/assets/m101js_final05_Statement3.png
[m101js_final06_Statement1]: {{ site.baseurl }}/assets/m101js_final06_Statement1.png
[m101js_final06_Statement2]: {{ site.baseurl }}/assets/m101js_final06_Statement2.png
[m101js_final07_Statement1]: {{ site.baseurl }}/assets/m101js_final07_Statement1.png
[m101js_final07_Statement2]: {{ site.baseurl }}/assets/m101js_final07_Statement2.png
[m101js_final07_MongoChecks1]: {{ site.baseurl }}/assets/m101js_final07_MongoChecks1.png
[m101js_final07_MongoChecks2]: {{ site.baseurl }}/assets/m101js_final07_MongoChecks2.png
[m101js_final07_MongoChecks3]: {{ site.baseurl }}/assets/m101js_final07_MongoChecks3.png
[m101js_final07_MongoChecks4]: {{ site.baseurl }}/assets/m101js_final07_MongoChecks4.png
[m101js_final07_Terminal]: {{ site.baseurl }}/assets/m101js_final07_Terminal.png
[m101js_final08_Statement]: {{ site.baseurl }}/assets/m101js_final08_Statement.png
[m101js_final09_Statement]: {{ site.baseurl }}/assets/m101js_final09_Statement.png
[m101js_final10_Statement1]: {{ site.baseurl }}/assets/m101js_final10_Statement1.png
[m101js_final10_Statement2]: {{ site.baseurl }}/assets/m101js_final10_Statement2.png
[m101js_progress]: {{ site.baseurl }}/assets/m101js_progress.png
[m101_downloadcertificate]: {{ site.baseurl }}/assets/m101_downloadcertificate.png
[m101js_certificate]: {{ site.baseurl }}/assets/m101js_certificate.png
