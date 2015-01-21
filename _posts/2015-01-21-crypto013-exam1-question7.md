---
layout: post
title:  "crypto-013, exam 1, question 7"
date:   2015-01-21 16:24:15
tags: crypto crypto013 crypto-013 exam1 exam question1 question python
---



## Question 7
Suppose you are told that the one time pad encryption of the message "attack at dawn" is 6c73d5240a948c86981bc294814d (the plaintext letters are encoded as 8-bit ASCII and the given ciphertext is written in hex). What would be the one time pad encryption of the message "attack at dusk" under the same OTP key?


### Code
{% highlight python linenos %}
pt = 'attack at dawn'
print 'plain text: '+pt
pt_hex = pt.encode('hex')
print 'plain text (hexa): '+pt_hex+', length: '+str(len(pt_hex))
pt1 = 'attack at dusk'
print 'plain text modified: '+pt1
pt1_hex = pt1.encode('hex')
print 'plain text modified (hexa): '+pt1_hex+', length: '+str(len(pt1_hex))

ct = '6c73d5240a948c86981bc294814d'
print 'ciphered text (hexa): '+ct+', length: '+str(len(ct))

pt_hex_xor_ct = hex(int(pt_hex, 16) ^ int(ct, 16))
key_hex = '0'+pt_hex_xor_ct[2:-1]
print 'key (hexa): '+key_hex+', length: '+str(len(key_hex))

ct_1 = hex(int(key_hex, 16) ^ int(pt1_hex, 16))
ct_1_fmt = ct_1[2:-1]
print 'ciphered text modified (hexa): '+ct_1_fmt+', length: '+str(len(ct_1_fmt))
{% endhighlight %}


## References
* [https://class.coursera.org/crypto-013][1]{: target="_blank"}
* [https://www.google.com/][2]{: target="_blank"}



[1]: https://class.coursera.org/crypto-013
[2]: https://www.google.com/
