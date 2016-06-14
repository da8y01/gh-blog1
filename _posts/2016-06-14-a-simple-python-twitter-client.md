---
layout: post
title:  "A simple python twitter client"
date:   2016-06-14 14:52:44
tags: python py twitter client dev development programming code tw
---



## Install the gem
Proceed to install the required library with `pip install python-twitter` as indicated at [https://github.com/bear/python-twitter][1]{: target="_blank"}.


## Sample code
{% highlight python linenos %}
import twitter
import sys
import time
import datetime


try:
  api = twitter.Api(
    consumer_key='THE_CONSUMER_KEY',
    consumer_secret='THE_CONSUMER_SECRET',
    access_token_key='THE_ACCESS_TOKEN_KEY',
    access_token_secret='THE_ACCESS_TOKEN_SECRET'
  )

  lines = [line.rstrip('\n') for line in open(sys.argv[1]) if line != '\n']
  for line in lines:
    api.PostUpdates(line)
    print "[*] At %s tweeted: %s" % (datetime.datetime.now().isoformat(), line)
    time.sleep(60*20) # this is for delay the post updates tweets for 20 minutes
except Exception, e:
  print e
{% endhighlight %}


## References
* [https://github.com/bear/python-twitter][1]{: target="_blank"}



[1]: https://github.com/bear/python-twitter
