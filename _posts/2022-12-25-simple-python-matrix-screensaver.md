---
layout: post
permalink: /matrix-screensaver.html
date: 2022-12-25 20:26:34
tags: python matrix screensaver
title: "A simple python matrix screensaver"
image: https://da8y01.github.io/gh-blog/assets/SimplePythonMatrixScreensaver.png
description: "A simple python matrix screensaver."
---


<div style="text-align:center" markdown="1">
![SimplePythonMatrixScreensaver][SimplePythonMatrixScreensaver]{: width="60%"}
</div>


{% highlight python %}
import random
import time

LINE_LENGTH = 60
ONE_SEC = 1

# According to the https://www.asciitable.com/
FIRST_CHAR = 33
LAST_CHAR = 126

line = []
while True:
    for i in range(LINE_LENGTH):
        rnd_int = random.randint(FIRST_CHAR, LAST_CHAR)
        rnd_chr = chr(rnd_int)
        line.append(rnd_chr)

    print(' '.join(line))
    line = []
    time.sleep(ONE_SEC)
{% endhighlight %}


## References
* [https://www.python.org/][PythonLang]{: target="_blank"}
* [https://en.wikipedia.org/wiki/The_Matrix][TheMatrix]{: target="_blank"}


[PythonLang]: https://www.python.org/
[TheMatrix]: https://en.wikipedia.org/wiki/The_Matrix

[SimplePythonMatrixScreensaver]: {{ site.baseurl }}/assets/SimplePythonMatrixScreensaver.png "A simple python matrix screensaver"
