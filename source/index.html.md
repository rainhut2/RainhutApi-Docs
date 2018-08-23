---
title: Rainhut API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='mailto:lani@rainhut.com'>Email us for a key.</a>

includes:
  - errors

search: true
---

# Introduction

Rainhut API.

Welcome to the Rainhut API! You can use our API to access Rainhut API endpoints, which you can use to create digital media: books, cards, flyers, and more.

We have language bindings in JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

> To authorize, use this code:


```javascript
const rainhutApi = require('rainhutApi');
let api = rainhutApi('privateKey', 'publicKey')
```


Rainhut uses private and public token keys to allow access to the API. You can register by contacting us at <a href='mailto: lani@rainhut.com'>rainhut.</a>



<aside class="notice">
You must replace <code>'privateKey', 'publicKey'</code> with your personal private and public keys.

Checkout the javascript api for example on how to authenticate your own app.
</aside>

# Rainhut Setup

For every call you will need to pass a setup object.


Parameter | Default | Description
--------- | ------- | -----------
paddingLeft | 0 | Will set padding on page left
paddingRight | 0 | Will set padding on page right
paddingTop | 0 | Will set padding on page top
paddingBottom | 0 | Will set padding on page bottom
headerHeight | 50 | Header height
footerHeight | 50 | Footer height
columnSpacing | 10 | How much space between columns
entrySpacing | 10 | How much space between each item
width | 1000 | Document width. Including padding
height | 800 | Document height. Including padding
titleStyle | defaultStyle | Style for entry title
contentStyle | defaultStyle | Style for entry content
footerStyle | defaultStyle | Style for footer
headerStyle | defaultStyle | Style for header
dateStyle | defaultStyle | Style for date
maxFontSize | 50 | Max font size to increase

Styles

Parameter | Default | Description
--------- | ------- | -----------
textColor | 0 | Will set padding on page left
fontFamily | 0 | Will set padding on page right
size | 1.0 | Font size on relative scale set to 18px. Font can increase up to given maxFontSize
weight | normal | Font weight. Can be normal, italic, bold, bold italic

> Setup Example:

```
   "setup":{  
      "paddingLeft":50,
      "paddingRight":50,
      "paddingTop":50,
      "paddingBottom":50,
      "headerHeight":50,
      "footerHeight":50,
      "columnSpacing":10,
      "entrySpacing":5,
      "width":1000,
      "height":800,
      "maxFontSize":50,
      "titleStyle":{  
         "textColor":"#708090",
         "fontFamily":"Roboto",
         "size":1,
         "weight":"bold"
      },
      "contentStyle":{  
         "textColor":"#696969",
         "fontFamily":"Roboto",
         "size":0.8,
         "weight":"normal"
      },
      "footerStyle":{  
         "textColor":"#000000",
         "fontFamily":"Roboto",
         "size":1,
         "weight":"bold italic"
      },
      "dateStyle":{  
         "textColor":"#A9A9A9",
         "fontFamily":"Roboto",
         "size":0.6,
         "weight":"Light"
      },
      "headerStyle":{  
         "textColor":"#000000",
         "fontFamily":"LucidaSans",
         "size":1,
         "weight":"normal"
      }
      "fitImagesRule":"few",
      "fitTextRule":"many",
      "cropImage":false,
      "backgroundImage":"",
      "rotateAmount":7,
      "borderColor":"#e2e2e2",
      "borderWidth":4
   }

```

# API Endpoints

## Create Book

### HTTP Post

`Post https://artapi2.rainhut.com/api/create2`

### Body Parameters

Parameter | Default | Description
--------- | ------- | -----------
setup | defaultSetup | See Setup Params
entries | [] | Entries object

Entry

Parameter | Default | Description
--------- | ------- | -----------
imageWidth | 0 | Image width, Int
imageHeight | 0 | Image height, Int
title | '' | Title for entry
content | '' | Content for entry
date | '' | Date for entry. UTC format
footer | '' | Footer 
header | '' | Header string
isSingleEntry | false | Will force this to be the only entry on a page.

### Description 

Passing entries and setup will return a book with different layouts.

<aside class="success">
  This is the start for creating a book.
</aside>


### Returns 

Parameter | Type | Description
--------- | ------- | -----------
bookId | String | unique id for generated book
pages | Array of page | returns an array of each page

Page 

Parameter | Type | Description
--------- | ------- | -----------
layout | String | current layout
layoutPreview | String | url to image
otherLayoutOptions | Array of String | other options including current option. Sorted best to least (if applicable)
entries | Array of entry | Each entry for the current page

Entry extra parameters returned.

Parameter | Type | Description
--------- | ------- | -----------
imageFirst | Bool | Does image layout first. Might not be applicable for given layout.
rotateImage | Int | Rotates image by degrees can be negative or positive



```javascript
const rainhutApi = require('rainhutApi');
let api = rainhutApi('privateKey', 'publicKey')
let book = api.kittens.create({});
```

> The above command returns JSON structured like this:

```json
{
  "bookId": "7007ac62-456e-43c5-afc7-751546c5fce4",
  "webhook": {"url": "http://localhost:3002/noexists"},
  "pages": [
    {
      "layout": "2_a",
      "otherLayoutOptions": [
        "2_a",
        "2_d",
        "2_f",
        "2_g"
      ],
      "entries": [
        {
          "header": "Header",
          "question": "",
          "answer": "Hey there...",
          "image": "imageurlhere",
          "imageWidth": 993,
          "imageHeight": 1355,
          "date": "2018-02-01 05:55:56 UTC",
          "footer": "5 Months",
          "isHeader": false,
          "textCountEntry": 342,
          "textCountEntryPercentage": 1,
          "weight": 0,
          "cropImage": false,
          "imageFirst": false,
          "rotateImage": 0,
          "hasQuestionHeader": true
        }
      ],
      "pageId": "de8da3d1-cba0-493b-b9bd-edea2ec68e2a",
      "sampleImage2": "urltoimage"
    }],
    "setup": {}
}
```


## Update Book

### HTTP Post

`Post https://artapi2.rainhut.com/api/update2`

### Body Parameters

Simple pass the book object returned from create2. 

Remove any pages that you are not updating. Replace the page(s) that you passed with the pages that are returned.

## Upload Book

### HTTP Post

`Post https://artapi2.rainhut.com/api/upload2`

### Body Parameters

Simple pass the book object returned from create2 or the modified update2 book


Extra parameter. 

### Webhook

Parameter | Type | Description
--------- | ------- | -----------
webhook | Object | Callback for the status of the upload of each page

Webhook object
Parameter | Type | Description
--------- | ------- | -----------
url | String | Your callback url
headers | Object | Any headers you want to pass back ie. 
{headers: {authentication: 'test'}}

