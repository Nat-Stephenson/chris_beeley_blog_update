---
author: chrisbeeley
categories:
- Uncategorized
date: "2014-06-20T21:03:47Z"
guid: http://chrisbeeley.net/?p=657
id: 657
title: Make your own Twitter ticker using PHP, CSS3, and JavaScript
url: /?p=657
---

I made a little Twitter ticker for work the other day, which will fetch posts with a particular hashtag and then animate them marquee style across the screen, I thought I may as well share it here. This version will fetch the last 100 tweets to the #rstats hashtag, remove the retweeets, and then show up to 50 animated across the screen.

All of the Twitter authentication PHP magic is stolen from [here](http://140dev.com/twitter-api-programming-blog/twitter-api-ebook-single-user-twitter-oauth-programming/).

I’m afraid the usual health warnings apply, I have no special talents in HTML, CSS, JavaScript, or PHP, I am learning all three, so this code merely works- it is not necessarily the best way of doing it.

In the first chunk of HTML we pull in the jQuery library and include a JavaScript function.

```
<pre class="brush: xml; title: ; notranslate" title="">


<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">

<head>
<title>Invest to lead</title>
  <meta http-equiv="content-type" content="text/html;charset=utf-8" />
  <meta name="generator" content="Geany 1.23.1" />
  <script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>

<script type="text/javascript">

```

The JavaScript function ensures that the animation runs at a constant speed regardless of how long the text is. Because the CSS3 animation takes a number of seconds as a parameter for how long the animation should run for, and because we have no way of knowing how long the text will be in advance, the JavaScript looks at the text at runtime and then scales the number of seconds the animation runs for by the length of the text. I then use jQuery to add it to the CSS instruction in the head of the html.

Much of the code is borrowed from [here](http://www.jonathan-petitcolas.com/2013/05/06/simulate-marquee-tag-in-css-and-javascript.html), I’ve fiddled with it slightly to make it do something slightly different.

The last instruction just tells the browser to reload when the animation finishes, which will restart the animation of course but also fetch new tweets.

```
<pre class="brush: jscript; title: ; notranslate" title="">
		
function getStringWidth(str) {

  var span = document.createElement("span"); 
  span.innerText = str;
  span.style.visibility = "hidden";

  var body = document.getElementsByTagName("body")[0];
  body.appendChild(span);
  var textWidth = span.offsetWidth;
  body.removeChild(span);

  return textWidth;
}

$(document).ready(function(){
  var ele = document.getElementById("thisone").innerHTML.length;
	
  $('head').append("<style>.marquee p {-moz-animation: marquee " + (ele / 5) + "s linear;}</style>")
  $('head').append("<style>.marquee p {-ms-animation: marquee " + (ele / 5) + "s linear;}</style>")
  $('head').append("<style>.marquee p {-webkit-animation: marquee " + (ele / 5) + "s linear;}</style>")
  $('head').append("<style>.marquee p {animation: marquee " + (ele / 5) + "s linear;}</style>")
	
var myBox = $('#thep');
	
myBox.one('webkitAnimationEnd oanimationend msAnimationEnd animationend',   
  function(e) {
    
    location.reload();

  });
	
}) 

```

```
<pre class="brush: xml; title: ; notranslate" title="">
</script>
<style>

```

The next part is the non-dynamic CSS, not much to say about this, again stolen from [here](http://www.jonathan-petitcolas.com/2013/05/06/simulate-marquee-tag-in-css-and-javascript.html)

```
<pre class="brush: css; title: ; notranslate" title="">
.marquee {
    white-space: nowrap;
	overflow: hidden;
}

.marquee p {
  position: absolute;
  bottom: 50px;
  font-size: 72px;

  -moz-transform:translateX(2%);
  -webkit-transform:translateX(2%);
  -ms-transform:translateX(2%);
  transform:translateX(2%);
	
}

  /* Make it move */
	
  @-moz-keyframes marquee {
    0%   { -moz-transform:translateX(2%) }
    100% { -moz-transform:translateX(-100%) }
  }
	
  @-ms-keyframes marquee {
    0%   { -ms-transform:translateX(2%) }
    100% { -ms-transform:translateX(-100%) }
  }
	
  @-webkit-keyframes marquee {
    0%   { -webkit-transform:translateX(2%) }
    100% { -webkit-transform:translateX(-100%) }
  }
	
  @keyframes marquee {
    0%   { transform:translateX(2%) }
    100% { transform:translateX(-100%) }
  }

```

I’ve included the next couple of lines of HTML for completeness.

```
<pre class="brush: xml; title: ; notranslate" title="">
	
</style>

</head>

<body>
	
<div id = "thisone" class="marquee">

```

This next part is the bit that fetches the tweets using PHP. It’s pretty simple if you follow the guide I link to at the top of the post. The only difficulty I had was parsing what comes back from the API, you can convert from JSON (as below) and you will end up with loads of nested associative arrays. It’s just a case of picking your way through by using print\_r().

As you can see in my code below most of the interesting stuff lives in $response\_data\[“statuses”\] (or maybe it all does, I never quite got to the bottom of it to be honest) and I check to see if the array key ‘retweeted\_status’ exists within that array and (if it doesn’t, i.e. it’s not a retweet) then I print $response\_data\[“statuses”\]\[“user”\]\[“name”\] and $response\_data\[“statuses”\]\[“text”\]

```
<pre class="brush: php; title: ; notranslate" title="">
	
<?php
 
require 'app_tokens.php';
require 'tmhOAuth.php';

$connection = new tmhOAuth(array(
  'consumer_key'    => $consumer_key,
  'consumer_secret' => $consumer_secret,
  'user_token'      => $user_token,
  'user_secret'     => $user_secret
));

// Get tweets
$connection->request('GET', $connection->url('1.1/search/tweets.json'), array(
  'q' => '#rstats',
  'count' => 100
));

// Get the HTTP response code for the API request
$response_code = $connection->response['code'];

// Convert the JSON response into an array
$response_data = json_decode($connection->response['response'],true);

// A response code of 200 is a success
if ($response_code <> 200) {
  print "Error: $response_code\n";
}

$counttweets = 0;

echo "<p id = 'thep'>";

foreach ($response_data["statuses"] as $result) {
	
	if (!array_key_exists('retweeted_status', $result))
	{
		echo $result["user"]["name"] . ": ";
		echo $result["text"]." *** ";
		if($counttweets++ > 50) break;
	}
}
echo "This twitter feed brought to you by Chris Beeley enterprises</p>";

?>

```

Last chunk of HTML for completeness.

```
<pre class="brush: xml; title: ; notranslate" title="">

</div>

</body>

</html>

```

And that’s it! It occurs to me that it would be very easy to add a GET function and allow searching of any hashtag by placing it in the URL. If I ever need to do this or get bored and decide to have a go I’ll add it to this post. There’s a demo of the feed [here](http://chrisbeeley.net/website/twitterapidemo/rstats.php).

</body></html>