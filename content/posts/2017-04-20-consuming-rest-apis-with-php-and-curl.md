---
author: chrisbeeley
categories:
- Uncategorized
date: "2017-04-20T11:27:38Z"
title: Consuming REST APIs with PHP and CURL
---

I wasted such a lot of time on this that I must commit it to the internet on the off chance that it helps someone else in the same situation.

If you are using PHP to consume a RESTful API via CURL and you want to manipulate the data you get back it’s very important that you set CURLOPT\_RETURNTRANSFER to true. This allows you to collect the response from the server in a variable. If you don’t set this option it will just echo the return to the screen, which is obviously of no use whatsoever.

While I’m here I may as well mention as well that if you want json\_decode to return an array you need to use json\_decode($result, true); otherwise you get an object back. The final code I wrote looks like this:

```
$ch = curl_init();
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_URL, "http://YOUR_URL_HERE");
$result = curl_exec($ch);

curl_close($ch);

$result = json_decode($result, true); // giving true to json_decode returns array

```