Tumblr app
==================================================

New Tumblr class js which use Tumblr API V1 and extend methods. Include infinite scroll, related posts and tag methods.

Instanciate the class
--------------------------------------

```javascript
var myTumblr = new TB({
	url							: 'http://mame.tumblr.com' //Optional
	limitPostInJSON				: 250,
	sessionStorage				: true,
	postSelector				: '.post',
	//Pass user:password@ in url to remove the popin authentification (No IE)
	privateBlog: {
		activate 				: false,
		password				: ''
	},
	infiniteScroll: {
		activate 				: true,
		nearBottom				: 200,
		targetPost				: '#content',
		appendMethod			: 'normal'
	}
});
```

###Session storage
Caching JSON data in browser storage. You can choose the limit of posts to cache in sessionStorage (250 by default). Activate this functionnality in production, not in development.
```javascript
sessionStorage	: true
limitPostInJSON	: 250
```

###Infinite scroll
Infinite scroll require total page parameter (send by Tumblr). Paste this line in your HTML before this js class. The name of the var is totalPages.

```html
<script>var totalPages = {TotalPages};</script>
```

You can disable infinite scroll in home page


```javascript
myTumblr.params.infiniteScroll.activate = false
```

Infinite scroll is compatible with grid plugin : Isotope, Masonry and Freetile. Change the value of appendMethod params
```javascript
appendMethod    : 'normal'
appendMethod    : 'isotope'
appendMethod    : 'masonry'
appendMethod    : 'freetile'
```

Infinite scroll can run with on or two container (odd/even compatibility) This functionnality is only compatible with normal appendMethod.
```javascript
targetPost      : '#content'
targetPost      : ['#content-left', '#content-right']
```

You can choose the distance (in pixel) near bottom of the page to trigger the infinite scroll method
```javascript
nearBottom      : 200
```

Access to JSON data

```javascript
myTumblr.attachEvent( myTumblr.events.JSON_COMPLETE, function(){
  var data = this.data;
});
```

## Methods

###Push content of the first page in the dom
Note : infinite scroll is available only in home page.

- **`target`** : target to push new content

```javascript
myTumblr.getFirstPushHome('#grid-home');
```

### Get related posts

**Require attach to JSON_COMPLETE event**. The function return a list of posts in object. There is a limit with related posts, only for X first post (X = limitPostInJSON) because getRelatedPosts() use getTagsPost() and sessionStorage.

- **`limit`** : number of related posts
- **`listTags`** : optional parameter, you can pass your own tag in array else the script get all tags of current post
- **`ignoreTag`** : optional parameter, you can ignore a tag in the list (first or last)

```javascript
myTumblr.attachEvent( myTumblr.events.JSON_COMPLETE, function(){
  var relateds = myTumblr.getRelatedPosts({ limit: 3 });
});
```

### Get all tags

**Require attach to JSON_COMPLETE event**. Get an array of all tags in the blog depending `limitPostJSON` param

```javascript
var allTags = myTumblr.getAllTags();
```

Get an array of all tags to specific post id

```javascript
var allTagsPost = smyTumblr.getAllTags('51726887298');
```

### Get tag of current page tagged

```javascript
var currentTag = myTumblr.getTagPage();
```

### Get tag of current or specific post

If the function pass no param, this return an array of all tags of current post (on post page). If you pass an id post in param, this return an array of all tags of specific post.
```javascript
var currentTag = myTumblr.getTagPage();
```


### Get id post of current post page

```javascript
var currentIdPost = myTumblr.getIdPostPage();
```

### Check the page

Return the name of the current page : home, post or tagged

```javascript
var currentPage = myTumblr.checkPage();
```

## Events

```javascript
myTumblr.events.ON_SCROLL
myTumblr.events.JSON_COMPLETE
myTumblr.events.START_LOADING
myTumblr.events.END_LOADING
myTumblr.events.END_PAGE
myTumblr.events.IMAGES_LOADED //Need jQuery.imagesLoaded.js
myTumblr.events.POST_APPEND
myTumblr.events.NO_RELATED
myTumblr.events.HAS_RELATED
```

`myTumblr.events.JSON_COMPLETE` is require for these methods :
- getRelatedPosts()
- getAllTags()

### Attach an event

```javascript
myTumblr.attachEvent( myTumblr.events.JSON_COMPLETE, function(){} );
```

### Detach an event

```javascript
myTumblr.detachEvent( myTumblr.events.JSON_COMPLETE );
```
