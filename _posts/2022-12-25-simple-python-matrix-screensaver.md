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

line = []
while True:
    for i in range(70):
        rnd_int = random.randint(33, 126)
        rnd_chr = chr(rnd_int)
        line.append(rnd_chr)

    print(' '.join(line))
    line = []
    time.sleep(1)
{% endhighlight %}


## References
* [https://www.python.org/][PythonLang]{: target="_blank"}
* [https://en.wikipedia.org/wiki/The_Matrix][TheMatrix]{: target="_blank"}


[PythonLang]: https://www.python.org/
[TheMatrix]: https://en.wikipedia.org/wiki/The_Matrix


[SimplePythonMatrixScreensaver]: {{ site.baseurl }}/assets/SimplePythonMatrixScreensaver.png "A simple python matrix screensaver"
