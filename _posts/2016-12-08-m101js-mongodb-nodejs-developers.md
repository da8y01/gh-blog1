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
{% highlight text linenos %}
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
{% highlight text linenos %}
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
Following are presented the contents and answers to the HomeWorks belonging to **Week 2 (CRUD)** for "*M101J: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


### HOMEWORK 2.1
![HW 2.1 Submit][m101js_hw21_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 2.1:
![HW 2.1 MongoshellChecks][m101js_hw21_MongoshellChecks]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight javascript linenos %}
use weather
query = {"Wind Direction":{$gt:180, $lt:360}}
project = {_id:false, State:true}
sorting = {Temperature:1}
db.data.find(query,project).sort(sorting).limit(5)
{% endhighlight %}

{% highlight javascript linenos %}
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
{% highlight javascript linenos %}
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
{% highlight javascript linenos %}
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
Following are presented the contents and answers to the HomeWorks belonging to **Week 3 (Schema Design)** for "*M101J: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


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
{% highlight javascript linenos %}
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
{% highlight javascript linenos %}
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
Following are presented the contents and answers to the HomeWorks belonging to **Week 4 (Performance)** for "*M101J: MongoDB for Java Developers*" course offered by https://university.mongodb.com/ started at 2014-03-17.


### HOMEWORK 4.1

### ANSWER HOMEWORK 4.1:
![HW 4.1][hw41]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 4.2

### ANSWER HOMEWORK 4.2:
![HW 4.2][hw42]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 4.3

### ANSWER HOMEWORK 4.3:
![HW 4.3][hw43]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 4.4

### ANSWER HOMEWORK 4.4:
![HW 4.4][hw44]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Week 5: Aggregation Framework
Following are presented the contents and answers to the HomeWorks belonging to **Week 5 (Aggregation Framework)** for "*M101J: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-17.


### HOMEWORK 5.1

### ANSWER HOMEWORK 5.1:
{% highlight javascript linenos %}
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
{% highlight javascript linenos %}
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
{% highlight javascript linenos %}
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
{% highlight javascript linenos %}
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
Following are presented the contents and answers to the HomeWorks belonging to **Week 6 (Application Engineering)** for "*M101J: MongoDB for Node.js Developers*" course offered by https://university.mongodb.com/ started at 2014-03-24.


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
Following are presented the contents and answers to the questions belonging to **Final Exam** for "*M101J: MongoDB for Java Developers*" course offered by https://university.mongodb.com/ started at 2014-03-17.


### Final: Question 1
![M101J Final Question 1][m101j_finalq01]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 1. Answer:
{% highlight text linenos %}
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

![M101J Final Question 1 Answer 1][m101j_finalq01_answer1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos %}
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

![M101J Final Question 1 Answer 2][m101j_finalq01_answer2]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight text linenos %}
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
$ l
dump/  enron.zip  final01_00.js
$ cat final01_00.js
use enron
query = {"headers.From":"andrew.fastow@enron.com", "headers.To":"jeff.skilling@enron.com"}
db.messages.find(query).count()
$ /opt/mongodb-linux-i686-2.6.0/bin/mongo < final01_00.js
MongoDB shell version: 2.6.0
connecting to: test
switched to db enron
{
	"headers.From" : "andrew.fastow@enron.com",
	"headers.To" : "jeff.skilling@enron.com"
}
3
bye
$
{% endhighlight %}

![M101J Final Question 1 Answer 3][m101j_finalq01_answer3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 2
![M101J Final Question 2][m101j_finalq02]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 2. Answer:
{% highlight javascript linenos %}
use enron
db.messages.aggregate([
	{$unwind:"$headers.To"},
	{$group:{
		_id:{id:"$_id",from:"$headers.From"},
		tos:{$addToSet:"$headers.To"}
	}},
	{$unwind:"$tos"},
	{$group:{
		_id:{
			from:"$_id.from",
			to:"$tos"
		},
		count:{$sum:1}
	}},
	{$sort:{
		count:-1
	}},
	{$limit:5}
])
{% endhighlight %}

{% highlight javascript linenos %}
MongoDB shell version: 2.6.0
connecting to: test
switched to db enron
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "jeff.dasovich@enron.com" }, "count" : 750 }
{ "_id" : { "from" : "soblander@carrfut.com", "to" : "soblander@carrfut.com" }, "count" : 679 }
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "james.steffes@enron.com" }, "count" : 646 }
{ "_id" : { "from" : "susan.mara@enron.com", "to" : "richard.shapiro@enron.com" }, "count" : 616 }
{ "_id" : { "from" : "evelyn.metoyer@enron.com", "to" : "kate.symes@enron.com" }, "count" : 567 }
bye
{% endhighlight %}

![M101J Final Question 2 Answer][m101j_finalq02_answer]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 3
![M101J Final Question 3][m101j_finalq03]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 3. Answer:
{% highlight javascript linenos %}
// use enron
query = {"headers.Message-ID":"<8147308.1075851042335.JavaMail.evans@thyme>"}
change = {$push:{"headers.To":"mrpotatohead@10gen.com"}}
db.messages.update(query,change)
db.messages.findOne(query)
{% endhighlight %}

![M101J Final Question 3 Answer][m101j_finalq03_answer]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 4
![M101J Final Question 4][m101j_finalq04]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 4. Answer:
![M101J Final Question 4 Answer 1][m101j_finalq04_answer1_import]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

![M101J Final Question 4 Answer 2][m101j_finalq04_answer2_count]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}

Code snippet for `public void likePost(final String permalink, final int ordinal)` method:
{% highlight java linenos %}
public void likePost(final String permalink, final int ordinal) {

// XXX Final Exam, Please work here
// Add code to increment the num_likes for the 'ordinal' comment
// that was clicked on.
// provided you use num_likes as your key name, no other changes should be required
// alternatively, you can use whatever you like but will need to make a couple of other
// changes to templates and post retrieval code.
    postsCollection.update(new BasicDBObject("permalink",permalink), new BasicDBObject("$inc", new BasicDBObject("comments."+ordinal+".num_likes",1)));
}
{% endhighlight %}

![M101J Final Question 4 Answer 3][m101j_finalq04_answer3_validate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 5

### Final: Question 5. Answer:
![M101J Final Question 5 Answer][m101j_finalq05]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 6

### Final: Question 6. Answer:
![M101J Final Question 6 Answer][m101j_finalq06]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 7
![M101J Final Question 7][m101j_finalq07]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 7. Answer:
![M101J Final Question 7 Answer 1][m101j_finalq07_answer1_win_import]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101J Final Question 7 Answer 2][m101j_finalq07_answer2_win_check]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101J Final Question 7 Answer 3][m101j_finalq07_answer3_win_indexes]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

{% highlight javascript linenos %}
use final7
counter = 1
cursor_images = db.images.find(); null;
while (cursor_images.hasNext()) {
	current_image = cursor_images.next()
	if (! db.albums.find({images:current_image._id}).hasNext()) {
		print('['+counter+++'] Orphan image found in images collection to remove:\n'+JSON.stringify(db.images.findOne(current_image))+'\n')
		db.images.remove(current_image)
	}
}
{% endhighlight %}

![M101J Final Question 7 Answer 4][m101j_finalq07_answer4_win_output1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101J Final Question 7 Answer 5][m101j_finalq07_answer5_win_output2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 8
![M101J Final Question 8][m101j_finalq08]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 8. Answer:
![M101J Final Question 8 Answer][m101j_finalq08_answer]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 9

### Final: Question 9. Answer:
![M101J Final Question 9][m101j_finalq09]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 10

### Final: Question 10. Answer:
![M101J Final Question 8][m101j_finalq10]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## PROGRESS
![M101JS progress][m101js_progress]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## CERTIFICATE
![M101JS downloadcertificate][m101js_downloadcertificate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
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
[m101js_progress]: {{ site.baseurl }}/assets/m101js_progress.png
[m101js_downloadcertificate]: {{ site.baseurl }}/assets/m101js_downloadcertificate.png
[m101js_certificate]: {{ site.baseurl }}/assets/m101js_certificate.png

[hw11_MongoRestore]: {{ site.baseurl }}/assets/m101j_hw11_MongoRestore.png
[hw11_MongoChecks]: {{ site.baseurl }}/assets/m101j_hw11_MongoChecks.png
[hw13_Terminal1]: {{ site.baseurl }}/assets/m101j_hw13_Terminal1.png
[hw13_Terminal2]: {{ site.baseurl }}/assets/m101j_hw13_Terminal2.png
[hw14_Terminal]: {{ site.baseurl }}/assets/m101j_hw14_Terminal.png
[hw14_Browser]: {{ site.baseurl }}/assets/m101j_hw14_Browser.png
[m101j_finalq01]: {{ site.baseurl }}/assets/m101j_finalq01.png
[m101j_finalq01_answer1]: {{ site.baseurl }}/assets/m101j_finalq01_answer1.png
[m101j_finalq01_answer2]: {{ site.baseurl }}/assets/m101j_finalq01_answer2.png
[m101j_finalq01_answer3]: {{ site.baseurl }}/assets/m101j_finalq01_answer3.png
[m101j_finalq02]: {{ site.baseurl }}/assets/m101j_finalq02.png
[m101j_finalq02_answer]: {{ site.baseurl }}/assets/m101j_finalq02_answer.png
[m101j_finalq03]: {{ site.baseurl }}/assets/m101j_finalq03.png
[m101j_finalq03_answer]: {{ site.baseurl }}/assets/m101j_finalq03_answer.png
[m101j_finalq04]: {{ site.baseurl }}/assets/m101j_finalq04.png
[m101j_finalq04_answer1_import]: {{ site.baseurl }}/assets/m101j_finalq04_answer1_import.png
[m101j_finalq04_answer2_count]: {{ site.baseurl }}/assets/m101j_finalq04_answer2_count.png
[m101j_finalq04_answer3_validate]: {{ site.baseurl }}/assets/m101j_finalq04_answer3_validate.png
[m101j_finalq05]: {{ site.baseurl }}/assets/m101j_finalq05.png
[m101j_finalq05_answer]: {{ site.baseurl }}/assets/m101j_finalq05_answer.png
[m101j_finalq06]: {{ site.baseurl }}/assets/m101j_finalq06.png
[m101j_finalq06_answer]: {{ site.baseurl }}/assets/m101j_finalq06_answer.png
[m101j_finalq07]: {{ site.baseurl }}/assets/m101j_finalq07.png
[m101j_finalq07_answer1_win_import]: {{ site.baseurl }}/assets/m101j_finalq07_answer1_win_import.png
[m101j_finalq07_answer2_win_check]: {{ site.baseurl }}/assets/m101j_finalq07_answer2_win_check.png
[m101j_finalq07_answer3_win_indexes]: {{ site.baseurl }}/assets/m101j_finalq07_answer3_win_indexes.png
[m101j_finalq07_answer4_win_output1]: {{ site.baseurl }}/assets/m101j_finalq07_answer4_win_output1.png
[m101j_finalq07_answer5_win_output2]: {{ site.baseurl }}/assets/m101j_finalq07_answer5_win_output2.png
[m101j_finalq08]: {{ site.baseurl }}/assets/m101j_finalq08.png
[m101j_finalq08_answer]: {{ site.baseurl }}/assets/m101j_finalq08_answer.png
[m101j_finalq09]: {{ site.baseurl }}/assets/m101j_finalq09.png
[m101j_finalq10]: {{ site.baseurl }}/assets/m101j_finalq10.png
