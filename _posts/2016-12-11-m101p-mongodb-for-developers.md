---
layout: post
title: "M101P: MongoDB for Developers"
date: 2016-12-11 04:35:30
tags: mongo mongodb db nosql 10gen tengen course curso university education m101 m101p python py bottle
---



## Week 1: Introduction
Following are presented the contents and answers to the HomeWorks belonging to **Week 1 (Introduction)** for "*M101P: MongoDB for Developers*" course offered by [https://university.mongodb.com/][1]{: target="_blank"} started at 2014-04-14.


### HOMEWORK 1.1

### ANSWER HOMEWORK 1.1:


### HOMEWORK 1.2

### ANSWER HOMEWORK 1.2:
{% highlight python linenos %}
import pymongo
import sys


# Copyright 2013, 10gen, Inc.
# Author: Andrew Erlichson


# connnecto to the db on standard port
connection = pymongo.MongoClient("mongodb://localhost")



db = connection.m101                 # attach to db
collection = db.funnynumbers         # specify the colllection


magic = 0

try:
    iter = collection.find()
    for item in iter:
        if ((item['value'] % 3) == 0):
            magic = magic + item['value']

except:
    print "Error trying to read collection:" + sys.exc_info()[0]


print "The answer to Homework One, Problem 2 is " + str(int(magic))
{% endhighlight %}


### HOMEWORK 1.3

### ANSWER HOMEWORK 1.3:
{% highlight python linenos=table %}
import pymongo
import bottle
import sys


# Copyright 2012, 10gen, Inc.
# Author: Andrew Erlichson


@bottle.get("/hw1/<n>")
def get_hw1(n):

    # connnecto to the db on standard port
    connection = pymongo.MongoClient("mongodb://localhost")

    n = int(n)

    db = connection.m101                 # attach to db
    collection = db.funnynumbers         # specify the colllection


    magic = 0

    try:
        iter = collection.find({},limit=1, skip=n).sort('value', direction=1)
        for item in iter:
            return str(int(item['value'])) + "\n"
    except:
        print "Error trying to read collection:", sys.exc_info()[0]


bottle.debug(True)
bottle.run(host='localhost', port=8080)
{% endhighlight %}



## Week 2: CRUD
Following are presented the contents and answers to the HomeWorks belonging to **Week 2 (CRUD)** for "*M101P: MongoDB for Developers*" course offered by https://university.mongodb.com/ started at 2014-04-14.


### HOMEWORK 2.1

### ANSWER HOMEWORK 2.1:
{% highlight javascript linenos=table %}
use students
db.grades.find({"type":"exam", "score":{"$gte":65}}).sort({"score":1}).limit(1)
{% endhighlight %}

{% highlight text linenos=table %}
MongoDB shell version: 2.6.0
connecting to: test
switched to db students
{ "_id" : ObjectId("50906d7fa3c412bb040eb5cf"), "student_id" : 22, "type" : "exam", "score" : 65.02518811936324 }
bye
{% endhighlight %}


### HOMEWORK 2.2

### ANSWER HOMEWORK 2.2:
{% highlight text linenos=table %}
MongoDB shell version: 2.6.0
connecting to: students
Server has startup warnings:
2014-04-28T23:20:41.846-0500 [initandlisten]
2014-04-28T23:20:41.846-0500 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2014-04-28T23:20:41.846-0500 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2014-04-28T23:20:41.846-0500 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2014-04-28T23:20:41.846-0500 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2014-04-28T23:20:41.846-0500 [initandlisten]
> db.grades.count()
800
> db.grades.findOne()
{
	"_id" : ObjectId("50906d7fa3c412bb040eb577"),
	"student_id" : 0,
	"type" : "exam",
	"score" : 54.6535436362647
}
> db.grades.count()
600
> db.grades.findOne()
{
	"_id" : ObjectId("50906d7fa3c412bb040eb577"),
	"student_id" : 0,
	"type" : "exam",
	"score" : 54.6535436362647
}
> db.grades.find().sort({'score':-1}).skip(100).limit(1)
{ "_id" : ObjectId("50906d7fa3c412bb040eb709"), "student_id" : 100, "type" : "homework", "score" : 88.50425479139126 }
> db.grades.find({},{'student_id':1, 'type':1, 'score':1, '_id':0}).sort({'student_id':1, 'score':1, }).limit(5)
{ "student_id" : 0, "type" : "quiz", "score" : 31.95004496742112 }
{ "student_id" : 0, "type" : "exam", "score" : 54.6535436362647 }
{ "student_id" : 0, "type" : "homework", "score" : 63.98402553675503 }
{ "student_id" : 1, "type" : "homework", "score" : 44.31667452616328 }
{ "student_id" : 1, "type" : "exam", "score" : 74.20010837299897 }
> db.grades.aggregate({'$group':{'_id':'$student_id', 'average':{$avg:'$score'}}}, {'$sort':{'average':-1}}, {'$limit':1})
{ "_id" : 54, "average" : 96.19488173037341 }
>
{% endhighlight %}
![HW 2.2 1][m101p_hw22-1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 2.3

### ANSWER HOMEWORK 2.3:
Code snippet for `def validate_login(self, username, password):` method:
{% highlight python linenos=table %}
# Validates a user login. Returns user record or None
def validate_login(self, username, password):

    user = None
    try:
        # XXX HW 2.3 Students Work Here
        # you will need to retrieve right document from the users collection.
        user = self.users.find_one({'_id':username})
        # print "This space intentionally left blank."
    except:
        print "Unable to query database for user"

    if user is None:
        print "User not in database"
        return None

    salt = user['password'].split(',')[1]

    if user['password'] != self.make_pw_hash(password, salt):
        print "user password is not a match"
        return None

    # Looks good
    return user
{% endhighlight %}

Code snippet for `def add_user(self, username, password, email):` method:
{% highlight python linenos=table %}
# creates a new user in the users collection
def add_user(self, username, password, email):
    password_hash = self.make_pw_hash(password)

    user = {'_id': username, 'password': password_hash}
    if email != "":
        user['email'] = email

    try:
        # XXX HW 2.3 Students work here
        # You need to insert the user into the users collection.
        # Don't over think this one, it's a straight forward insert.
        self.users.insert(user)

        # print "This space intentionally left blank."

    except pymongo.errors.OperationFailure:
        print "oops, mongo error"
        return False
    except pymongo.errors.DuplicateKeyError as e:
        print "oops, username is already taken"
        return False

    return True
{% endhighlight %}
![HW 2.3 1][m101p_hw23-1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Week 3: Schema Design
Following are presented the contents and answers to the HomeWorks belonging to **Week 3 (Schema Design)** for "*M101P: MongoDB for Developers*" course offered by https://university.mongodb.com/ started at 2014-04-14.


### HOMEWORK 3.1
![HW 3.1 Submit][m101p_hw31_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 3.1:
![HW 3.1 MongoImport][m101p_hw31_MongoImport]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 3.1 DBStudentsCount][m101p_hw31_DBStudentsCount]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 3.1 Execution][m101p_hw31_Execution]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 3.1 MongoCheck][m101p_hw31_MongoCheck]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 3.2

### ANSWER HOMEWORK 3.2:
Code snippet for `def insert_entry(self, title, post, tags_array, author):` method:
{% highlight python linenos=table %}
# inserts the blog entry and returns a permalink for the entry
def insert_entry(self, title, post, tags_array, author):
    print "inserting blog entry", title, post

    # fix up the permalink to not include whitespace

    exp = re.compile('\W') # match anything not alphanumeric
    whitespace = re.compile('\s')
    temp_title = whitespace.sub("_",title)
    permalink = exp.sub('', temp_title)

    # Build a new post
    post = {"title": title,
            "author": author,
            "body": post,
            "permalink":permalink,
            "tags": tags_array,
            "comments": [],
            "date": datetime.datetime.utcnow()}

    # now insert the post
    try:
        # XXX HW 3.2 Work Here to insert the post
        # print "Inserting the post"
        self.posts.insert(post)
    except:
        print "Error inserting post"
        print "Unexpected error:", sys.exc_info()[0]

    return permalink
{% endhighlight %}

Code snippet for `def get_posts(self, num_posts):` method:
{% highlight python linenos=table %}
# returns an array of num_posts posts, reverse ordered
def get_posts(self, num_posts):

    cursor = []         # Placeholder so blog compiles before you make your changes

    # XXX HW 3.2 Work here to get the posts
    cursor = self.posts.find().sort([('date',pymongo.DESCENDING)]).limit(num_posts)

    l = []

    for post in cursor:
        post['date'] = post['date'].strftime("%A, %B %d %Y at %I:%M%p") # fix up date
        if 'tags' not in post:
            post['tags'] = [] # fill it in if its not there already
        if 'comments' not in post:
            post['comments'] = []

        l.append({'title':post['title'], 'body':post['body'], 'post_date':post['date'],
                  'permalink':post['permalink'],
                  'tags':post['tags'],
                  'author':post['author'],
                  'comments':post['comments']})

    return l
{% endhighlight %}

Code snippet for `def get_post_by_permalink(self, permalink):` method:
{% highlight python linenos=table %}
# find a post corresponding to a particular permalink
def get_post_by_permalink(self, permalink):

    post = None
    # XXX Work here to retrieve the specified post
    post = self.posts.find_one({'permalink':permalink})

    if post is not None:
        # fix up date
        post['date'] = post['date'].strftime("%A, %B %d %Y at %I:%M%p")

    return post
{% endhighlight %}

![HW 3.2 Validate][m101p_hw-32-33_Validate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 3.2 Submit][m101p_hw32_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 3.3

### ANSWER HOMEWORK 3.3:
Code snippet for `def add_comment(self, permalink, name, email, body):` method:
{% highlight python linenos=table %}
# add a comment to a particular blog post
def add_comment(self, permalink, name, email, body):

    comment = {'author': name, 'body': body}

    if (email != ""):
        comment['email'] = email

    try:
        last_error = {'n':-1}           # this is here so the code runs before you fix the next line
        # XXX HW 3.3 Work here to add the comment to the designated post
        last_error = self.posts.update({'permalink':permalink}, {'$push':{'comments':comment}}, multi=False)


        return last_error['n']          # return the number of documents updated

    except:
        print "Could not update the collection, error"
        print "Unexpected error:", sys.exc_info()[0]
        return 0
{% endhighlight %}

![HW 3.3 Submit][m101p_hw33_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Week 4: Performance
Following are presented the contents and answers to the HomeWorks belonging to **Week 4 (Performance)** for "*M101P: MongoDB for Developers*" course offered by https://university.mongodb.com/ started at 2014-04-14.


### HOMEWORK 4.1
![HW 4.1 Statement1][m101p_hw41_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.1 Statement2][m101p_hw41_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.1 Statement3][m101p_hw41_Statement3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 4.1:
A sample of defined indexes in the `db.products` collection:
{% highlight text linenos=table %}
> db.products.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"ns" : "store.products",
		"name" : "_id_"
	},
	{
		"v" : 1,
		"key" : {
			"sku" : 1
		},
                "unique" : true,
		"ns" : "store.products",
		"name" : "sku_1"
	},
	{
		"v" : 1,
		"key" : {
			"price" : -1
		},
		"ns" : "store.products",
		"name" : "price_-1"
	},
	{
		"v" : 1,
		"key" : {
			"description" : 1
		},
		"ns" : "store.products",
		"name" : "description_1"
	},
	{
		"v" : 1,
		"key" : {
			"category" : 1,
			"brand" : 1
		},
		"ns" : "store.products",
		"name" : "category_1_brand_1"
	},
	{
		"v" : 1,
		"key" : {
			"reviews.author" : 1
		},
		"ns" : "store.products",
		"name" : "reviews.author_1"
	}
{% endhighlight %}


### HOMEWORK 4.2
![HW 4.2 Statement1][m101p_hw42_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.2 Statement2][m101p_hw42_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 4.2:
An example output of `.explain()`:
{% highlight text linenos=table %}
db.tweets.find({"user.followers_count":{$gt:1000}}).sort({"created_at" : 1 }).limit(10).skip(5000).explain()
{
        "cursor" : "BtreeCursor created_at_-1 reverse",
        "isMultiKey" : false,
        "n" : 10,
        "nscannedObjects" : 46462,
        "nscanned" : 46462,
        "nscannedObjectsAllPlans" : 49763,
        "nscannedAllPlans" : 49763,
        "scanAndOrder" : false,
        "indexOnly" : false,
        "nYields" : 0,
        "nChunkSkips" : 0,
        "millis" : 205,
        "indexBounds" : {
                "created_at" : [
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
        "server" : "localhost.localdomain:27017"
}
{% endhighlight %}


### HOMEWORK 4.3
![HW 4.3 Statement1][m101p_hw43_Statement1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.3 Statement2][m101p_hw43_Statement2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.3 Statement3][m101p_hw43_Statement3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 4.3:
Inspect file system directory, and import exercise database to the mongo local service:
{% highlight text linenos=table %}
homework_4_3$ l
hw4-3/
homework_4_3$ cd hw4-3/
homework_4_3/hw4-3$ l
hw4-3/
homework_4_3/hw4-3$ cd hw4-3/
homework_4_3/hw4-3/hw4-3$ l
blogPostDAO.py*   blog.py*     sessionDAO.py*   userDAO.py*   validate.py*
blogPostDAO.pyc*  posts.json*  sessionDAO.pyc*  userDAO.pyc*  views/
homework_4_3/hw4-3/hw4-3$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d blog -c posts < posts.json
connected to: 127.0.0.1
2014-05-10T23:39:54.273-0500 			400	133/second
2014-05-10T23:39:57.109-0500 			800	133/second
2014-05-10T23:39:58.279-0500 check 9 1000
2014-05-10T23:39:58.280-0500 imported 1000 objects
homework_4_3/hw4-3/hw4-3$
{% endhighlight %}
![HW 4.3 InspectImport][m101p_hw43_InspectImport]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Inside mongo shell check the imported objects:
{% highlight text linenos=table %}
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
> use blog
switched to db blog
> show tables
posts
system.indexes
> db.posts.drop()
true
> show tables
system.indexes
> show tables
posts
system.indexes
>
{% endhighlight %}
![HW 4.3 MongoChecks][m101p_hw43_MongoChecks]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}

Inside mongo shell check and define the needed indexes:
{% highlight text linenos=table %}
> db.posts.getIndices()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "blog.posts"
	}
]
> get.posts.ensureIndice({date:-1})
2014-05-10T23:45:20.200-0500 ReferenceError: get is not defined
> db.posts.ensureIndice({date:-1})
2014-05-10T23:45:30.836-0500 TypeError: Property 'ensureIndice' of object blog.posts is not a function
> db.posts.ensureIndex({date:-1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}
> db.posts.getIndices()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "blog.posts"
	},
	{
		"v" : 1,
		"key" : {
			"date" : -1
		},
		"name" : "date_-1",
		"ns" : "blog.posts"
	}
]
> db.posts.ensureIndex({tags:1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 2,
	"numIndexesAfter" : 3,
	"ok" : 1
}
> db.posts.ensureIndex({permalink:1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 3,
	"numIndexesAfter" : 4,
	"ok" : 1
}
> db.posts.ensureIndex({tags:1,date:-1})
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 4,
	"numIndexesAfter" : 5,
	"ok" : 1
}
> db.posts.getIndices()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "blog.posts"
	},
	{
		"v" : 1,
		"key" : {
			"date" : -1
		},
		"name" : "date_-1",
		"ns" : "blog.posts"
	},
	{
		"v" : 1,
		"key" : {
			"tags" : 1
		},
		"name" : "tags_1",
		"ns" : "blog.posts"
	},
	{
		"v" : 1,
		"key" : {
			"permalink" : 1
		},
		"name" : "permalink_1",
		"ns" : "blog.posts"
	},
	{
		"v" : 1,
		"key" : {
			"tags" : 1,
			"date" : -1
		},
		"name" : "tags_1_date_-1",
		"ns" : "blog.posts"
	}
]
>
{% endhighlight %}
![HW 4.3 MongoShell1][m101p_hw43_MongoShell1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.3 MongoShell2][m101p_hw43_MongoShell2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.3 MongoShell3][m101p_hw43_MongoShell3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

In the terminal inspect the file system directory, import the exercise database to the local mongo daemon, run the blog application, and obtain the validation code for the homework:
{% highlight text linenos=table %}
homework_4_3$ l
hw4-3/
homework_4_3$ cd hw4-3/
homework_4_3/hw4-3$ l
hw4-3/
homework_4_3/hw4-3$ cd hw4-3/
homework_4_3/hw4-3/hw4-3$ l
blogPostDAO.py*   blog.py*     sessionDAO.py*   userDAO.py*   validate.py*
blogPostDAO.pyc*  posts.json*  sessionDAO.pyc*  userDAO.pyc*  views/
homework_4_3/hw4-3/hw4-3$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d blog -c posts < posts.json
connected to: 127.0.0.1
2014-05-10T23:39:54.273-0500 			400	133/second
2014-05-10T23:39:57.109-0500 			800	133/second
2014-05-10T23:39:58.279-0500 check 9 1000
2014-05-10T23:39:58.280-0500 imported 1000 objects
homework_4_3/hw4-3/hw4-3$ l
blogPostDAO.py*   blog.py*     sessionDAO.py*   userDAO.py*   validate.py*
blogPostDAO.pyc*  posts.json*  sessionDAO.pyc*  userDAO.pyc*  views/
homework_4_3/hw4-3/hw4-3$ python blog.py
Bottle v0.12.7 server starting up (using WSGIRefServer())...
Listening on http://localhost:8082/
Hit Ctrl-C to quit.

127.0.0.1 - - [10/May/2014 23:51:26] "GET / HTTP/1.1" 200 50742
127.0.0.1 - - [10/May/2014 23:51:26] "GET /favicon.ico HTTP/1.1" 404 742
about to query on permalink =  TLxrBfyxTZjqOKqxgnUP
127.0.0.1 - - [10/May/2014 23:51:29] "GET /post/TLxrBfyxTZjqOKqxgnUP HTTP/1.1" 200 26592
127.0.0.1 - - [10/May/2014 23:51:31] "GET /tag/pantyhose HTTP/1.1" 200 63658
127.0.0.1 - - [10/May/2014 23:51:33] "GET /tag/pail HTTP/1.1" 200 49097
^Chomework_4_3/hw4-3/hw4-3$
homework_4_3/hw4-3/hw4-3$ python validate.py
Welcome to the HW 4.3 Checker. My job is to make sure you added the indexes
that make the blog fast in the following three situations
	When showing the home page
	When fetching a particular post
	When showing all posts for a particular tag
Data looks like it is properly loaded into the posts collection
Home page is super fast. Nice job.

Blog retrieval by permalink is super fast. Nice job.

Blog retrieval by tag is super fast. Nice job.

Tests Passed for HW 4.3. Your HW 4.3 validation code is 893jfns29f728fn29f20f2
homework_4_3/hw4-3/hw4-3$
{% endhighlight %}
![HW 4.3 InspectImportRunValidate][m101p_hw43_InspectImportRunValidate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### HOMEWORK 4.4
![HW 4.4 Statement][m101p_hw44_Statement]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 4.4:
In the terminal import the `profile` exercise database to the local mongo service:
{% highlight text linenos=table %}
homework_4_4$ l
sysprofile.json
homework_4_4$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d m101 -c profile sysprofile.json
connected to: 127.0.0.1
2014-05-10T23:09:46.879-0500 check 9 1515
2014-05-10T23:09:46.902-0500 imported 1515 objects
homework_4_4$
{% endhighlight %}
![HW 4.4 MongoImport][m101p_hw44_MongoImport]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Inside mongo shell check imported objects:
{% highlight text linenos=table %}
$ /opt/mongodb-linux-i686-2.6.0/bin/mongo
MongoDB shell version: 2.6.0
connecting to: test
Server has startup warnings:
2014-05-10T23:07:16.028-0500 [initandlisten]
2014-05-10T23:07:16.028-0500 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2014-05-10T23:07:16.028-0500 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2014-05-10T23:07:16.028-0500 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2014-05-10T23:07:16.028-0500 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2014-05-10T23:07:16.029-0500 [initandlisten]
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
> use m101
switched to db m101
> show tables
funnynumbers
hw1
system.indexes
> show tables
funnynumbers
hw1
profile
system.indexes
> db.profile.count()
1515
> db.profile.findOne()
{
	"_id" : ObjectId("536ef80a3a951b395bd1b416"),
	"ts" : ISODate("2012-11-20T20:02:24.386Z"),
	"op" : "command",
	"ns" : "school2.$cmd",
	"command" : {
		"drop" : "student_grades"
	},
	"ntoreturn" : 1,
	"keyUpdates" : 0,
	"numYield" : 0,
	"lockStats" : {
		"timeLockedMicros" : {
			"r" : 0,
			"w" : 1481
		},
		"timeAcquiringMicros" : {
			"r" : 0,
			"w" : 4
		}
	},
	"responseLength" : 129,
	"millis" : 1,
	"client" : "127.0.0.1",
	"user" : ""
}
>
{% endhighlight %}
![HW 4.4 MongoChecks1][m101p_hw44_MongoChecks1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.4 MongoChecks2][m101p_hw44_MongoChecks2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


In the terminal show and execute the required query and save the output:

Code snippet for the mongo query to be executed:
{% highlight javascript linenos=table %}
use m101
query = {ns:"school2.students"}; null;
project = {ns:1, millis:1}; null;
sorting = {millis:-1}; null;
quantity = 3; null;
db.profile.find(query,project).sort(sorting).limit(quantity).pretty()
{% endhighlight %}

{% highlight text linenos=table %}
homework_4_4$ l
hw44_00.js  sysprofile.json
homework_4_4$ cat hw44_00.js
use m101
query = {ns:"school2.students"}; null;
project = {ns:1, millis:1}; null;
sorting = {millis:-1}; null;
quantity = 3; null;
db.profile.find(query,project).sort(sorting).limit(quantity).pretty()homework_4_4$
homework_4_4$ /opt/mongodb-linux-i686-2.6.0/bin/mongo < hw44_00.js
MongoDB shell version: 2.6.0
connecting to: test
switched to db m101
null
null
null
null
{
	"_id" : ObjectId("536ef80a3a951b395bd1b79b"),
	"ns" : "school2.students",
	"millis" : 15820
}
{
	"_id" : ObjectId("536ef80a3a951b395bd1b418"),
	"ns" : "school2.students",
	"millis" : 12560
}
{
	"_id" : ObjectId("536ef80a3a951b395bd1b57d"),
	"ns" : "school2.students",
	"millis" : 11084
}
bye
homework_4_4$ /opt/mongodb-linux-i686-2.6.0/bin/mongo < hw44_00.js > hw44_00.out
homework_4_4$ l
hw44_00.js  hw44_00.out  sysprofile.json
homework_4_4$ cat hw44_00.out
MongoDB shell version: 2.6.0
connecting to: test
switched to db m101
null
null
null
null
{
	"_id" : ObjectId("536ef80a3a951b395bd1b79b"),
	"ns" : "school2.students",
	"millis" : 15820
}
{
	"_id" : ObjectId("536ef80a3a951b395bd1b418"),
	"ns" : "school2.students",
	"millis" : 12560
}
{
	"_id" : ObjectId("536ef80a3a951b395bd1b57d"),
	"ns" : "school2.students",
	"millis" : 11084
}
bye
homework_4_4$
{% endhighlight %}

Output of the executed query:
{% highlight text linenos=table %}
MongoDB shell version: 2.6.0
connecting to: test
switched to db m101
null
null
null
null
{
	"_id" : ObjectId("536ef80a3a951b395bd1b79b"),
	"ns" : "school2.students",
	"millis" : 15820
}
{
	"_id" : ObjectId("536ef80a3a951b395bd1b418"),
	"ns" : "school2.students",
	"millis" : 12560
}
{
	"_id" : ObjectId("536ef80a3a951b395bd1b57d"),
	"ns" : "school2.students",
	"millis" : 11084
}
bye
{% endhighlight %}
![HW 4.4 Terminal1][m101p_hw44_Terminal1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 4.4 Terminal2][m101p_hw44_Terminal2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Week 5: Aggregation Framework
Following are presented the contents and answers to the HomeWorks belonging to **Week 5 (Aggregation Framework)** for "*M101P: MongoDB for Developers*" course offered by https://university.mongodb.com/ started at 2014-04-14.


### HOMEWORK 5.1

### ANSWER HOMEWORK 5.1:


### HOMEWORK 5.2

### ANSWER HOMEWORK 5.2:


### HOMEWORK 5.3

### ANSWER HOMEWORK 5.3:


### HOMEWORK 5.4

### ANSWER HOMEWORK 5.4:



## Week 6: Application Engineering
Following are presented the contents and answers to the HomeWorks belonging to **Week 6 (Application Engineering)** for "*M101P: MongoDB for Developers*" course offered by https://university.mongodb.com/ started at 2014-04-14.


### HOMEWORK 6.1
![HW 6.1 Submit][m101p_hw61_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 6.1:


### HOMEWORK 6.2
![HW 6.2 Submit][m101p_hw62_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 6.2:


### HOMEWORK 6.3
![HW 6.3 Submit][m101p_hw63_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 6.3:


### HOMEWORK 6.4
![HW 6.4 Submit1][m101p_hw64_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.4 Submit2][m101p_hw64_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.4 Submit3][m101p_hw64_Submit3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 6.4:


### HOMEWORK 6.5
![HW 6.5 Submit1][m101p_hw65_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.5 Submit2][m101p_hw65_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.5 Submit3][m101p_hw65_Submit3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### ANSWER HOMEWORK 6.5:
{% highlight text linenos=table %}
homework_6_5$ clear

homework_6_5$ l
1.log                      1.log.2014-05-25T06-06-49  2.log.2014-05-25T06-05-58  3.log.2014-05-25T06-04-55  init_sharded_env.sh*
1.log.2014-05-25T06-00-40  2.log                      2.log.2014-05-25T06-06-49  3.log.2014-05-25T06-05-58  validate.py
1.log.2014-05-25T06-04-54  2.log.2014-05-25T06-00-40  3.log                      3.log.2014-05-25T06-06-49
1.log.2014-05-25T06-05-57  2.log.2014-05-25T06-04-55  3.log.2014-05-25T06-00-40  hw65_Terminal.txt
homework_6_5$ cat init_sharded_env.sh
#!/usr/bin/env bash

# Andrew Erlichson
# 10gen
# script to start a sharded environment on localhost

# clean everything up
echo "killing mongod and mongos"
killall mongod
killall monogs
echo "removing data files"
rm -rf /data/rs*


# start a replica set and tell it that it will be a shord0
mkdir -p /data/rs1 /data/rs2 /data/rs3
mongod --replSet m101 --logpath "1.log" --dbpath /data/rs1 --port 27017 --oplogSize 64 --smallfiles --fork
mongod --replSet m101 --logpath "2.log" --dbpath /data/rs2 --port 27018 --oplogSize 64 --smallfiles --fork
mongod --replSet m101 --logpath "3.log" --dbpath /data/rs3 --port 27019 --oplogSize 64 --smallfiles --fork

sleep 5
# connect to one server and initiate the set
mongo --port 27017 << 'EOF'
config = { _id: "m101", members:[
          { _id : 0, host : "localhost:27017"},
          { _id : 1, host : "localhost:27018"},
          { _id : 2, host : "localhost:27019"} ]
};
rs.initiate(config)
rs.status()
EOF
homework_6_5$ ./init_sharded_env.sh
killing mongod and mongos
monogs: no process found
removing data files
2014-05-25T01:12:30.360-0500
2014-05-25T01:12:30.360-0500 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2014-05-25T01:12:30.360-0500
about to fork child process, waiting until server is ready for connections.
forked process: 8591
child process started successfully, parent exiting
2014-05-25T01:12:30.476-0500
2014-05-25T01:12:30.476-0500 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2014-05-25T01:12:30.476-0500
about to fork child process, waiting until server is ready for connections.
forked process: 8604
child process started successfully, parent exiting
2014-05-25T01:12:30.639-0500
2014-05-25T01:12:30.639-0500 warning: 32-bit servers don't have journaling enabled by default. Please use --journal if you want durability.
2014-05-25T01:12:30.639-0500
about to fork child process, waiting until server is ready for connections.
forked process: 8622
child process started successfully, parent exiting
MongoDB shell version: 2.6.1
connecting to: 127.0.0.1:27017/test
{
	"_id" : "m101",
	"members" : [
		{
			"_id" : 0,
			"host" : "localhost:27017"
		},
		{
			"_id" : 1,
			"host" : "localhost:27018"
		},
		{
			"_id" : 2,
			"host" : "localhost:27019"
		}
	]
}
{
	"info" : "Config now saved locally.  Should come online in about a minute.",
	"ok" : 1
}
{
	"startupStatus" : 6,
	"ok" : 0,
	"errmsg" : "Received replSetInitiate - should come online shortly."
}
bye
homework_6_5$ mongo
MongoDB shell version: 2.6.1
connecting to: test
Server has startup warnings:
2014-05-25T01:12:30.373-0500 [initandlisten]
2014-05-25T01:12:30.373-0500 [initandlisten] ** NOTE: This is a 32 bit MongoDB binary.
2014-05-25T01:12:30.373-0500 [initandlisten] **       32 bit builds are limited to less than 2GB of data (or less with --journal).
2014-05-25T01:12:30.373-0500 [initandlisten] **       Note that journaling defaults to off for 32 bit and is currently off.
2014-05-25T01:12:30.373-0500 [initandlisten] **       See http://dochub.mongodb.org/core/32bit
2014-05-25T01:12:30.373-0500 [initandlisten]
m101:PRIMARY> rs.status()
{
	"set" : "m101",
	"date" : ISODate("2014-05-25T06:13:03Z"),
	"myState" : 1,
	"members" : [
		{
			"_id" : 0,
			"name" : "localhost:27017",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 33,
			"optime" : Timestamp(1400998356, 1),
			"optimeDate" : ISODate("2014-05-25T06:12:36Z"),
			"electionTime" : Timestamp(1400998364, 1),
			"electionDate" : ISODate("2014-05-25T06:12:44Z"),
			"self" : true
		},
		{
			"_id" : 1,
			"name" : "localhost:27018",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 27,
			"optime" : Timestamp(1400998356, 1),
			"optimeDate" : ISODate("2014-05-25T06:12:36Z"),
			"lastHeartbeat" : ISODate("2014-05-25T06:13:02Z"),
			"lastHeartbeatRecv" : ISODate("2014-05-25T06:13:02Z"),
			"pingMs" : 0,
			"syncingTo" : "localhost:27017"
		},
		{
			"_id" : 2,
			"name" : "localhost:27019",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 27,
			"optime" : Timestamp(1400998356, 1),
			"optimeDate" : ISODate("2014-05-25T06:12:36Z"),
			"lastHeartbeat" : ISODate("2014-05-25T06:13:02Z"),
			"lastHeartbeatRecv" : ISODate("2014-05-25T06:13:02Z"),
			"pingMs" : 0,
			"syncingTo" : "localhost:27017"
		}
	],
	"ok" : 1
}
m101:PRIMARY>
bye
homework_6_5$ l
1.log                      1.log.2014-05-25T06-12-30  2.log.2014-05-25T06-06-49  3.log.2014-05-25T06-05-58  validate.py
1.log.2014-05-25T06-00-40  2.log                      2.log.2014-05-25T06-12-30  3.log.2014-05-25T06-06-49
1.log.2014-05-25T06-04-54  2.log.2014-05-25T06-00-40  3.log                      3.log.2014-05-25T06-12-30
1.log.2014-05-25T06-05-57  2.log.2014-05-25T06-04-55  3.log.2014-05-25T06-00-40  hw65_Terminal.txt
1.log.2014-05-25T06-06-49  2.log.2014-05-25T06-05-58  3.log.2014-05-25T06-04-55  init_sharded_env.sh*
homework_6_5$ python validate.py
Welcome to the HW 6.x replica Checker. My job is to make sure you started a replica set with three nodes
Looks good. Replica set with three nodes running
Tests Passed for HW 6.5. Your HW 6.5 validation code is kjvjkl3290mf0m20f2kjjv
homework_6_5$
{% endhighlight %}
![HW 6.5 Terminal1][m101p_hw65_Terminal1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.5 Terminal2][m101p_hw65_Terminal2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.5 Terminal3][m101p_hw65_Terminal3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![HW 6.5 Terminal4][m101p_hw65_Terminal4]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## Final Exam
Following are presented the contents and answers to the questions belonging to **Final Exam** for "*M101P: MongoDB for Developers*" course offered by https://university.mongodb.com/ started at 2014-04-14.


### Final: Question 1
![M101P Final Question 1 Submit1][m101p_final01_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 1 Submit2][m101p_final01_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 1. Answer:
In the terminal restore the exercise dump:
{% highlight text linenos=table %}
final_exam$ l
dump/  enron.zip
final_exam$ /opt/mongodb-linux-i686-2.6.0/bin/mongorestore dump/
connected to: 127.0.0.1
2014-06-01T23:29:39.905-0500 dump/enron/messages.bson
2014-06-01T23:29:39.905-0500 	going into namespace [enron.messages]
2014-06-01T23:29:42.124-0500 		Progress: 36910456/396236668	9%	(bytes)
2014-06-01T23:29:45.013-0500 		Progress: 77777873/396236668	19%	(bytes)
2014-06-01T23:29:48.013-0500 		Progress: 122868650/396236668	31%	(bytes)
2014-06-01T23:29:51.111-0500 		Progress: 168746175/396236668	42%	(bytes)
2014-06-01T23:29:54.130-0500 		Progress: 203620712/396236668	51%	(bytes)
2014-06-01T23:29:57.018-0500 		Progress: 259974419/396236668	65%	(bytes)
2014-06-01T23:30:00.025-0500 		Progress: 286585438/396236668	72%	(bytes)
2014-06-01T23:30:03.006-0500 		Progress: 341485812/396236668	86%	(bytes)
2014-06-01T23:30:06.068-0500 		Progress: 380244200/396236668	95%	(bytes)
120477 objects found
2014-06-01T23:30:07.346-0500 	Creating index: { key: { _id: 1 }, ns: "enron.messages", name: "_id_" }
final_exam$
{% endhighlight %}
![M101P Final Question 1 Terminal][m101p_final01_Terminal]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Inside the mongo shell inspect and execute the required query:
{% highlight text linenos=table %}
> show dbs
admin                         (empty)
blog                          0.078GB
final07                       0.078GB
local                         0.078GB
m101                          0.078GB
school                        0.078GB
test                          0.078GB
weather                       0.078GB
> show dbs
admin                         (empty)
blog                          0.078GB
enron                         0.953GB
final07                       0.078GB
local                         0.078GB
m101                          0.078GB
school                        0.078GB
test                          0.078GB
weather                       0.078GB
> use enron
switched to db enron
> show tables
messages
system.indexes
> db.messages.count()
120477
> db.messages.findOne()
{
	"_id" : ObjectId("4f16fc97d1e2d32371003f02"),
	"body" : "COURTYARD\n\nMESQUITE\n2300 HWY 67\nMESQUITE, TX  75150\ntel: 972-681-3300\nfax: 972-681-3324\n\nHotel Information: http://courtyard.com/DALCM\n\n\nARRIVAL CONFIRMATION:\n Confirmation Number:84029698\nGuests in Room: 2\nNAME: MR ERIC  BASS \nGuest Phone: 7138530977\nNumber of Rooms:1\nArrive: Oct 6 2001\nDepart: Oct 7 2001\nRoom Type: ROOM - QUALITY\nGuarantee Method:\n Credit card guarantee\nCANCELLATION PERMITTED-BEFORE 1800 DAY OF ARRIVAL\n\nRATE INFORMATION:\nRate(s) Quoted in: US DOLLAR\nArrival Date: Oct 6 2001\nRoom Rate: 62.10  per night. Plus tax when applicable\nRate Program: AAA AMERICAN AUTO ASSN\n\nSPECIAL REQUEST:\n NON-SMOKING ROOM, GUARANTEED\n   \n\n\nPLEASE DO NOT REPLY TO THIS EMAIL \nAny Inquiries Please call 1-800-321-2211 or your local\ninternational toll free number.\n \nConfirmation Sent: Mon Jul 30 18:19:39 2001\n\nLegal Disclaimer:\nThis confirmation notice has been transmitted to you by electronic\nmail for your convenience. Marriott's record of this confirmation\nnotice is the official record of this reservation. Subsequent\nalterations to this electronic message after its transmission\nwill be disregarded.\n\nMarriott is pleased to announce that High Speed Internet Access is\nbeing rolled out in all Marriott hotel brands around the world.\nTo learn more or to find out whether your hotel has the service\navailable, please visit Marriott.com.\n\nEarn points toward free vacations, or frequent flyer miles\nfor every stay you make!  Just provide your Marriott Rewards\nmembership number at check in.  Not yet a member?  Join for free at\nhttps://member.marriottrewards.com/Enrollments/enroll.asp?source=MCRE\n\n",
	"filename" : "2.",
	"headers" : {
		"Content-Transfer-Encoding" : "7bit",
		"Content-Type" : "text/plain; charset=us-ascii",
		"Date" : ISODate("2001-07-30T22:19:40Z"),
		"From" : "reservations@marriott.com",
		"Message-ID" : "<32788362.1075840323896.JavaMail.evans@thyme>",
		"Mime-Version" : "1.0",
		"Subject" : "84029698 Marriott  Reservation Confirmation Number",
		"To" : [
			"ebass@enron.com"
		],
		"X-FileName" : "eric bass 6-25-02.PST",
		"X-Folder" : "\\ExMerge - Bass, Eric\\Personal",
		"X-From" : "Reservations@Marriott.com",
		"X-Origin" : "BASS-E",
		"X-To" : "EBASS@ENRON.COM",
		"X-bcc" : "",
		"X-cc" : ""
	},
	"mailbox" : "bass-e",
	"subFolder" : "personal"
}
> db.messages.count({"headers.From":"andrew.fastow@enron.com", "headers.To":"john.lavorato@enron.com"})
1
> db.messages.count({"headers.From":"andrew.fastow@enron.com", "headers.To":"jeff.skilling@enron.com"})
3
>
{% endhighlight %}
![M101P Final Question 1 MongoShell1][m101p_final01_MongoShell1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 1 MongoShell2][m101p_final01_MongoShell2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 2
![M101P Final Question 2 Submit][m101p_final02_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 2. Answer:
Solution query:
{% highlight javascript linenos=table %}
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
{% endhighlight %}

In the terminal, show, execution and saving of the output of the script:
{% highlight text linenos=table %}
final_exam$ l
dump/  enron.zip  final02_00.js
final_exam$ cat final02_00.js
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
final_exam$ /opt/mongodb-linux-i686-2.6.0/bin/mongo < final02_00.js > final02_00.out
final_exam$ l
dump/  enron.zip  final02_00.js  final02_00.out
final_exam$ cat final02_00.out
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
final_exam$
{% endhighlight %}
![M101P Final Question 2 Terminal1][m101p_final02_Terminal1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 2 Terminal2][m101p_final02_Terminal2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Output of the execution of the solution query:
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


### Final: Question 3
![M101P Final Question 3 Submit1][m101p_final03_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 3 Submit2][m101p_final03_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 3. Answer:
Solution query:
{% highlight javascript linenos=table %}
// use enron
query = {"headers.Message-ID":"<8147308.1075851042335.JavaMail.evans@thyme>"}
project = {"_id":1, "headers":1}
change = {$push:{"headers.To":"mrpotatohead@10gen.com"}}
db.messages.update(query,change)
db.messages.findOne(query)
{% endhighlight %}


### Final: Question 4
![M101P Final Question 4 Submit1][m101p_final04_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 4 Submit2][m101p_final04_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 4. Answer:
In the terminal import the exercise database:
{% highlight text linenos=table %}
$ cd question_4/
question_4$ l
final4/  posts.json
question_4$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d blog -c posts posts.json
connected to: 127.0.0.1
2014-06-02T01:03:57.557-0500 		Progress: 31308170/34788312	89%
2014-06-02T01:03:57.557-0500 			900	300/second
2014-06-02T01:03:57.793-0500 check 9 1000
2014-06-02T01:03:57.796-0500 imported 1000 objects
question_4$ cd final4/
question_4/final4$ l
final4/  __MACOSX/
question_4/final4$ cd final4/
question_4/final4/final4$ l
blogPostDAO.py*  blog.py*  sessionDAO.py*  userDAO.py*  validate.py*  views/
question_4/final4/final4$ python blog.py
Bottle v0.12.7 server starting up (using WSGIRefServer())...
Listening on http://localhost:8082/
Hit Ctrl-C to quit.
{% endhighlight %}
![M101P Final Question 4 Terminal][m101p_final04_Terminal]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Inside the mongo shell, check for imported objects:
{% highlight text linenos=table %}
> show dbs
admin                         (empty)
blog                          0.078GB
enron                         0.953GB
final07                       0.078GB
local                         0.078GB
m101                          0.078GB
school                        0.078GB
test                          0.078GB
weather                       0.078GB
> use blog
switched to db blog
> show tables
posts
system.indexes
> db.posts.count()
1000
> db.posts.drop()
true
> show tables
system.indexes
> show tables
posts
system.indexes
> db.posts.count()
1000
>
{% endhighlight %}
![M101P Final Question 4 MongoShell][m101p_final04_MongoShell]{: width="50%" style="display:block; margin-left:auto; margin-right:auto"}

Code snippet for the `def increment_likes(self, permalink, comment_ordinal):` method:
{% highlight python linenos=table %}
# increments the number of likes on a particular comment. Returns the number of documented updated
def increment_likes(self, permalink, comment_ordinal):

    #
    # XXX Final exam
    # Work here. You need to update the num_likes value in the comment being liked
    #
    try:
        last_error = self.posts.update({'permalink': permalink}, {'$inc': {'comments.'+str(comment_ordinal)+'.num_likes': 1}}, upsert=False)
        return last_error['n']

    except:
        print "Could not update the collection, error"
        print "Unexpected error:", sys.exc_info()[0]
        return 0
{% endhighlight %}

In the terminal run the validation script to get the code:
{% highlight text linenos=table %}
question_4/final4/final4$ l
blogPostDAO.py*  blogPostDAO.pyc  blog.py*  sessionDAO.py*  sessionDAO.pyc  userDAO.py*  userDAO.pyc  validate.py*  views/
question_4/final4/final4$ python validate.py
Welcome to the M101 Final Exam, Question 4 Validation Checker
Trying to grab the blog home page at url and find the first post. http://localhost:8082/
Fount a post url:  /post/mxwnnnqaflufnqwlekfd
Trying to grab the number of likes for url  http://localhost:8082/post/mxwnnnqaflufnqwlekfd
Likes value  2
Clicking on Like link for post:  /post/mxwnnnqaflufnqwlekfd
Trying to grab the number of likes for url  http://localhost:8082/post/mxwnnnqaflufnqwlekfd
Likes value  3
Tests Passed for Final 4. Your validation code is 3f837hhg673ghd93hgf8
question_4/final4/final4$
{% endhighlight %}
![M101P Final Question 4 Validate][m101p_final04_Validate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 5
![M101P Final Question 5 Submit1][m101p_final05_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 5 Submit2][m101p_final05_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 5 Submit3][m101p_final05_Submit3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 5. Answer:
A model of the indexes of the collection:
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


### Final: Question 6
![M101P Final Question 6 Submit1][m101p_final06_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 6 Submit2][m101p_final06_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 6. Answer:


### Final: Question 7
![M101P Final Question 7 Submit1][m101p_final07_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 7 Submit2][m101p_final07_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 7. Answer:
In the terminal import the collections for the exercise:
{% highlight text linenos=table %}
question_7$ l
final7/
question_7$ cd final7/
question_7/final7$ l
final7/
question_7/final7$ cd final7/
question_7/final7/final7$ l
albums.json*  images.json*
question_7/final7/final7$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d final07 -c albums albums.json
connected to: 127.0.0.1
2014-06-02T01:42:42.637-0500 check 9 1000
2014-06-02T01:42:42.639-0500 imported 1000 objects
question_7/final7/final7$ /opt/mongodb-linux-i686-2.6.0/bin/mongoimport -d final07 -c images images.json
connected to: 127.0.0.1
2014-06-02T01:43:11.008-0500 		Progress: 5517195/9635092	57%
2014-06-02T01:43:11.009-0500 			57300	19100/second
2014-06-02T01:43:12.777-0500 check 9 100000
2014-06-02T01:43:13.344-0500 imported 100000 objects
question_7/final7/final7$ l
albums.json*  final07_00.py  images.json*
question_7/final7/final7$
{% endhighlight %}

Inside mongo shell, check objects, inspect collections, watch indexes, count documents for later verification:
{% highlight text linenos=table %}
> show dbs
admin                         (empty)
blog                          0.078GB
enron                         0.953GB
local                         0.078GB
m101                          0.078GB
school                        0.078GB
test                          0.078GB
weather                       0.078GB
> show dbs
admin                         (empty)
blog                          0.078GB
enron                         0.953GB
final07                       0.078GB
local                         0.078GB
m101                          0.078GB
school                        0.078GB
test                          0.078GB
weather                       0.078GB
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
> db.images.count({tags:"kittens"})
49932
>
{% endhighlight %}
![M101P Final Question 7 MongoShell1][m101p_final07_MongoShell1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 7 MongoShell2][m101p_final07_MongoShell2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 7 MongoShell3][m101p_final07_MongoShell3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 7 MongoShell4][m101p_final07_MongoShell4]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Code snippet of the script to execute:
{% highlight python linenos=table %}
# final07_00.py
# M101P (2014-04) Final Exam Question 07.
# 2014-06-02 02:08:50

import pymongo
import sys

# establish a connection to the database
# connection = pymongo.Connection("mongodb://localhost", safe=True)
connection = pymongo.MongoClient("mongodb://localhost")

# get a handle to the school database
db=connection.final07
albums = db.albums
images = db.images


def final07():
	try:
		removed = 0
		cursor_images = images.find()
		for image in cursor_images:
			image_id = image['_id']
			found = albums.find({'images':image_id}).count()
			if found == 0:
				images.remove({'_id':image_id})
				removed = removed+1
				print "["+str(removed)+"] Removed image: "+str(image_id)
	except:
		print "[ERROR] Unexpected error:", sys.exc_info()[0]


final07()
{% endhighlight %}

Tail of the script execution:
![M101P Final Question 7 Execution][m101p_final07_Execution]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

Mongo shell verifications after execution of script:
{% highlight text linenos=table %}
> db.images.count()
89737
> db.images.count({tags:"kittens"})
44822
>
{% endhighlight %}
![M101P Final Question 7 MongoShell5][m101p_final07_MongoShell5]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}


### Final: Question 8
![M101P Final Question 8 Submit][m101p_final08_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 8. Answer:


### Final: Question 9
![M101P Final Question 9 Submit][m101p_final09_Submit]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 9. Answer:


### Final: Question 10
![M101P Final Question 10 Submit1][m101p_final10_Submit1]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 10 Submit2][m101p_final10_Submit2]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P Final Question 10 Submit3][m101p_final10_Submit3]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}

### Final: Question 10. Answer:
The query to execute:
{% highlight javascript linenos=table %}
db.messages.find({'headers.Date':{'$gt': new Date(2001,3,1)}},{'headers.From':1, _id:0}).sort({'headers.From':1}).explain()
{% endhighlight %}

The `.explain()` output:
{% highlight text linenos=table %}
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
![M101P progress][m101p_progress]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



## CERTIFICATE
![M101P downloadcertificate][m101_downloadcertificate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}
![M101P certificate][m101p_certificate]{: width="75%" style="display:block; margin-left:auto; margin-right:auto"}



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

[m101p_hw22-1]: {{ site.baseurl }}/assets/m101p_hw22-1.png
[m101p_hw23-1]: {{ site.baseurl }}/assets/m101p_hw23-1.png
[m101p_hw31_MongoImport]: {{ site.baseurl }}/assets/m101p_hw31_MongoImport.png
[m101p_hw31_DBStudentsCount]: {{ site.baseurl }}/assets/m101p_hw31_DBStudentsCount.png
[m101p_hw31_Execution]: {{ site.baseurl }}/assets/m101p_hw31_Execution.png
[m101p_hw31_MongoCheck]: {{ site.baseurl }}/assets/m101p_hw31_MongoCheck.png
[m101p_hw31_Submit]: {{ site.baseurl }}/assets/m101p_hw31_Submit.png
[m101p_hw-32-33_Validate]: {{ site.baseurl }}/assets/m101p_hw-32-33_Validate.png
[m101p_hw32_Submit]: {{ site.baseurl }}/assets/m101p_hw32_Submit.png
[m101p_hw33_Submit]: {{ site.baseurl }}/assets/m101p_hw33_Submit.png
[m101p_hw41_Statement1]: {{ site.baseurl }}/assets/m101p_hw41_Statement1.png
[m101p_hw41_Statement2]: {{ site.baseurl }}/assets/m101p_hw41_Statement2.png
[m101p_hw41_Statement3]: {{ site.baseurl }}/assets/m101p_hw41_Statement3.png
[m101p_hw42_Statement1]: {{ site.baseurl }}/assets/m101p_hw42_Statement1.png
[m101p_hw42_Statement2]: {{ site.baseurl }}/assets/m101p_hw42_Statement2.png
[m101p_hw43_Statement1]: {{ site.baseurl }}/assets/m101p_hw43_Statement1.png
[m101p_hw43_Statement2]: {{ site.baseurl }}/assets/m101p_hw43_Statement2.png
[m101p_hw43_Statement3]: {{ site.baseurl }}/assets/m101p_hw43_Statement3.png
[m101p_hw43_InspectImport]: {{ site.baseurl }}/assets/m101p_hw43_InspectImport.png
[m101p_hw43_MongoChecks]: {{ site.baseurl }}/assets/m101p_hw43_MongoChecks.png
[m101p_hw43_MongoShell1]: {{ site.baseurl }}/assets/m101p_hw43_MongoShell1.png
[m101p_hw43_MongoShell2]: {{ site.baseurl }}/assets/m101p_hw43_MongoShell2.png
[m101p_hw43_MongoShell3]: {{ site.baseurl }}/assets/m101p_hw43_MongoShell3.png
[m101p_hw43_InspectImportRunValidate]: {{ site.baseurl }}/assets/m101p_hw43_InspectImportRunValidate.png
[m101p_hw44_Statement]: {{ site.baseurl }}/assets/m101p_hw44_Statement.png
[m101p_hw44_MongoImport]: {{ site.baseurl }}/assets/m101p_hw44_MongoImport.png
[m101p_hw44_MongoChecks1]: {{ site.baseurl }}/assets/m101p_hw44_MongoChecks1.png
[m101p_hw44_MongoChecks2]: {{ site.baseurl }}/assets/m101p_hw44_MongoChecks2.png
[m101p_hw44_Terminal1]: {{ site.baseurl }}/assets/m101p_hw44_Terminal1.png
[m101p_hw44_Terminal2]: {{ site.baseurl }}/assets/m101p_hw44_Terminal2.png
[m101p_hw61_Submit]: {{ site.baseurl }}/assets/m101p_hw61_Submit.png
[m101p_hw62_Submit]: {{ site.baseurl }}/assets/m101p_hw62_Submit.png
[m101p_hw63_Submit]: {{ site.baseurl }}/assets/m101p_hw63_Submit.png
[m101p_hw64_Submit1]: {{ site.baseurl }}/assets/m101p_hw64_Submit1.png
[m101p_hw64_Submit2]: {{ site.baseurl }}/assets/m101p_hw64_Submit2.png
[m101p_hw64_Submit3]: {{ site.baseurl }}/assets/m101p_hw64_Submit3.png
[m101p_hw65_Submit1]: {{ site.baseurl }}/assets/m101p_hw65_Submit1.png
[m101p_hw65_Submit2]: {{ site.baseurl }}/assets/m101p_hw65_Submit2.png
[m101p_hw65_Submit3]: {{ site.baseurl }}/assets/m101p_hw65_Submit3.png
[m101p_hw65_Terminal1]: {{ site.baseurl }}/assets/m101p_hw65_Terminal1.png
[m101p_hw65_Terminal2]: {{ site.baseurl }}/assets/m101p_hw65_Terminal2.png
[m101p_hw65_Terminal3]: {{ site.baseurl }}/assets/m101p_hw65_Terminal3.png
[m101p_hw65_Terminal4]: {{ site.baseurl }}/assets/m101p_hw65_Terminal4.png
[m101p_final01_MongoShell1]: {{ site.baseurl }}/assets/m101p_final01_MongoShell1.png
[m101p_final01_MongoShell2]: {{ site.baseurl }}/assets/m101p_final01_MongoShell2.png
[m101p_final01_Submit1]: {{ site.baseurl }}/assets/m101p_final01_Submit1.png
[m101p_final01_Submit2]: {{ site.baseurl }}/assets/m101p_final01_Submit2.png
[m101p_final01_Terminal]: {{ site.baseurl }}/assets/m101p_final01_Terminal.png
[m101p_final02_Submit]: {{ site.baseurl }}/assets/m101p_final02_Submit.png
[m101p_final02_Terminal1]: {{ site.baseurl }}/assets/m101p_final02_Terminal1.png
[m101p_final02_Terminal2]: {{ site.baseurl }}/assets/m101p_final02_Terminal2.png
[m101p_final03_Submit1]: {{ site.baseurl }}/assets/m101p_final03_Submit1.png
[m101p_final03_Submit2]: {{ site.baseurl }}/assets/m101p_final03_Submit2.png
[m101p_final04_MongoShell]: {{ site.baseurl }}/assets/m101p_final04_MongoShell.png
[m101p_final04_Submit1]: {{ site.baseurl }}/assets/m101p_final04_Submit1.png
[m101p_final04_Submit2]: {{ site.baseurl }}/assets/m101p_final04_Submit2.png
[m101p_final04_Terminal]: {{ site.baseurl }}/assets/m101p_final04_Terminal.png
[m101p_final04_Validate]: {{ site.baseurl }}/assets/m101p_final04_Validate.png
[m101p_final05_Submit1]: {{ site.baseurl }}/assets/m101p_final05_Submit1.png
[m101p_final05_Submit2]: {{ site.baseurl }}/assets/m101p_final05_Submit2.png
[m101p_final05_Submit3]: {{ site.baseurl }}/assets/m101p_final05_Submit3.png
[m101p_final06_Submit1]: {{ site.baseurl }}/assets/m101p_final06_Submit1.png
[m101p_final06_Submit2]: {{ site.baseurl }}/assets/m101p_final06_Submit2.png
[m101p_final07_Execution]: {{ site.baseurl }}/assets/m101p_final07_Execution.png
[m101p_final07_MongoShell1]: {{ site.baseurl }}/assets/m101p_final07_MongoShell1.png
[m101p_final07_MongoShell2]: {{ site.baseurl }}/assets/m101p_final07_MongoShell2.png
[m101p_final07_MongoShell3]: {{ site.baseurl }}/assets/m101p_final07_MongoShell3.png
[m101p_final07_MongoShell4]: {{ site.baseurl }}/assets/m101p_final07_MongoShell4.png
[m101p_final07_MongoShell5]: {{ site.baseurl }}/assets/m101p_final07_MongoShell5.png
[m101p_final07_Submit1]: {{ site.baseurl }}/assets/m101p_final07_Submit1.png
[m101p_final07_Submit2]: {{ site.baseurl }}/assets/m101p_final07_Submit2.png
[m101p_final08_Submit]: {{ site.baseurl }}/assets/m101p_final08_Submit.png
[m101p_final09_Submit]: {{ site.baseurl }}/assets/m101p_final09_Submit.png
[m101p_final10_Submit1]: {{ site.baseurl }}/assets/m101p_final10_Submit1.png
[m101p_final10_Submit2]: {{ site.baseurl }}/assets/m101p_final10_Submit2.png
[m101p_final10_Submit3]: {{ site.baseurl }}/assets/m101p_final10_Submit3.png
[m101p_progress]: {{ site.baseurl }}/assets/m101p_progress.png
[m101_downloadcertificate]: {{ site.baseurl }}/assets/m101_downloadcertificate.png
[m101p_certificate]: {{ site.baseurl }}/assets/m101p_certificate.png
