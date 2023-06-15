---
author: chrisbeeley
categories:
- Uncategorized
date: "2015-06-12T09:32:01Z"
guid: http://chrisbeeley.net/?p=740
id: 740
title: Convert pdf to text on Ubuntu
url: /?p=740
---

We had rather an ugly scanned pdf of a very lovely poem over on our [feedback website](http://feedback.nottinghamshirehealthcare.nhs.uk/content/wonderful-poem-about-nurses-care-and-compassion-mother-and-baby-unit) so I thought I would try to post the text from it using Optical Character Recognition using [tesseract](https://code.google.com/p/tesseract-ocr/).

On Ubuntu start with:

``

sudo apt-get install tesseract-ocr imagemagick

You’ll need to convert the pdf to an image file:

``

convert -density 600 input.pdf output.tif

and then output to output.txt like this:

``

tesseract myscan.png out

Do note that if you have problems where an empty text file is returned (as I did) this could be because the margins around the image are too large- crop it down (not too tightly) and it should work nicely (although I would say it returned “l” a lot when in natural English that is obviously going to be an “I”, seems kind of obvious to a user but there must be some technical problem there, I suppose).