# Introduction
**GUIDE: Using simple CSS and Javascript to create animated scenes**

<img src="http://thedanzorlabs.com/api-preview/preview.jpg" />

**DISCLAIMER:** There are much better ways to create the same desired experience that also perform better. Such as using WebGL (Three.JS) and HTML5 Canvas.
This small guide is to show you how CSS can be used a little outside of the box to create some nice scenic web experiences.

All the material inside this repo is free to use, including the example in its full form.

I did this for fun, so i do not restrict myself to my usual coding standards. I did use some approaches that i wouldn't in production code and hope you won't use them either. But for hacking around and just building something cool, go for it ;)

**Brief Introduction:**
CSS should in my honest opinion be used purely for styling, animating and enriching a user experience when used on a live production page. Transforming elements on the page should be done with as much precision as possible (do not use transform: all; for example) to ensure the optimal rendering experience.
Take advantage of translate, transitions, and the goodies that come with CSS3 to give a good user experience without flooding the user.
CSS should not used to create fully animated scenes, as it is one of the least performant ways of approaching it. However it is still fun to do!

I personally love creating interesting scenes or experiences while also using methods that may be a little unorthodox. CSS can allow you to create some interesting user-experiences very quickly, it has a lot of tools to transform and reuse materials while giving them different behaviors.

SASS or other CSS pre-processing tools can also make the maintenance of the code easier and smaller. However for now i wish to use native CSS and maybe later add SASS/SCSS to further optimize the code-base.

For those who are a little new to CSS, HTML and JS i will try to simplify my explanations and leave as many code comments as possible.

I use simplistic JS to create this scene, I use it to create dom elements for me instead of manually maintaining it myself.
However i advise 110% to **never** user .innerHTML if your interacting with the DOM in your javascript. This is a very brute-force way of manipulating the DOM Tree and the page.

**Requirements:**
* A little knowledge of CSS3, HTML and Javascript is needed.
* No web server is needed, but a CSS3 compliant browser is required for the full desired affect. (http://caniuse.com/#search=css3)

## Guide: Lets begin
### Setting up the canvas!
By canvas i am refering to an artists canvas and not a HTML5 canvas tag. So for this guide we will use a standard HTML5 page.

```
<!doctype html>

<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Guide: CSS and JS to create scenes</title>
  <meta name="description" content="A CSS Water scene behind an application">
  <meta name="author" content="Scott Jones">

  <link rel="stylesheet" href="css/styles.css">
  <link href='https://fonts.googleapis.com/css?family=Roboto:400,300,100,500,700,900&subset=latin,latin-ext' rel='stylesheet' type='text/css'>
</head>
<body>

	<script>
	</script>
</body>
</html>

```

If you are familar with HTML or responsive developement you will notice i do not have any mobile META tags inside my HEAD. At this time i am not too fussed with a mobile experience, although it will work fine on most modern devices I feel it is out-of-scope for this small guide.

I have linked to our stylesheet which will be in **./css/** folder. called **styles.css**.

The javascript will be inside my index.html page itself at trhe bottom, but feel free to split it into it's own file if you prefer. I am loading it at bottom of my document so it knows of all elements inside my page, it also performs visually better this way depending on the size / complexity of the script when it comes to loading the page initially.

### Setting up the Stylesheet!

```
/* CSS RESET */
body, html { padding:0px; margin:0px; background:#111111; }
div, p, h1, h2, h3, h4, h5, a { box-sizing: border-box; display:block; padding:0px; margin:0px; }

/* Basic Font styling */
body {
  font-family: 'Roboto', sans-serif;
  color:#8a8888;
  text-shadow:1px 1px 0px #000;
}
```

Our basic stylesheet contains a basic CSS reset for the elements i plan to use for this project. CSS resets is a common practice of changing the default state we want our elements to have on our page. For example BODY tags usually contain paddings / margin values and i personally always remove those in my CSS.

Elements have initial/default states that were practical when the world wide web was first created, but in recent times many developers / designers have preferred different default states and the web has come a long way in this regard. Please refer a little to this article on CSS resets to build your own opinion (http://sixrevisions.com/css/should-you-reset-your-css/)

### Setting up the materials for animating!

In this repo you will find 2 background images, 1 rain image and 1 lightning image. For this project that is all we need and i will show you 3 methods of creating animations with these materials.

You can optimize this by using sprite-maps and SVGs but for now we will stick to some basic formats. Optimization isn't a priority for me in this scene, it should be cool to look at, even if it's a little large to download initially.

In this step i created my main background (scene) image, that is the main backdrop for my scene. I have a copy of this background but with some features highlighted (Fel lava, clouds etc). This background copy we will animate in using the Opacity property that css provides to create glow effects.

The rain is an image with multiple rain drops which already contain some desired blur effects already. We are going to use this image multiple times and have it animate at different start and end locations to simulate rain. As an extra bonus i also add in some easing, to show how you can simulate the rain landing on a surface like glass.  

The lightning is one image which we will transform (flip), fade in and out (to flash) and adjust the dimensions so we can have multiple different lightning strikes while just using one image.

### Lets create the basic foundation for our scene

```
<body>
	<div class="main_background">
	  <div class="inner_background">

	  </div>
	</div>

	<div class="rain_placeholder">

	</div>

	<div class="content_wrapper">
	  <div class="content_wrapper_inner">

	  </div>
	</div>
</body>
```

We have 1 layer for our main background, 1 layer for our rain and 1 layer for our application / website content.

**CSS:**

```
/* Background animation */

/* Our scene should fill at least this resolution*/
.main_background,
.inner_background {
	width:100%;
	min-height:1600px;

	overflow:hidden;
}

/* Main background (static) */
.main_background {
	background:url('../img/background.jpg') top center no-repeat;
}

/* Inner Background opacity states */
@keyframes background {
	0%   { opacity:0.2; }
	25%  { opacity:1; }
	30%  { opacity:0.4; }
	60%  { opacity:1; }
	80%  { opacity:0.4; }
	95%  { opacity:1; }
	100% { opacity:0.2; }
}

/* Inner Background */
.inner_background {
  background:url('../img/bg2.jpg') top center no-repeat;

  opacity:0;

  animation-name: background;
  animation-duration: 6s;
  animation-iteration-count: infinite;
}
```

We now have our background animating in and out, i personally went for a 6 second repeating animation that changes the opacity at certain points. This is done to minick the fel glow from Warcraft.

**NOTE:** We're currently animating a large amount of real-estate using 2 large images, this is one of the first performance hits we will see. What we could do is split this into slightly smaller animations and that may result in a smoother frame-rate for some low-performance devices.
However this requires us to overlay a few more images and get there position perfect. Later on we do some responsive support so i went with one big image for this example.
Depending on the graphic device and driver version, screen tearing and flickering happens with this approach.

### Add some lightning

We are going to add our lightning image 1 or multiple times to give us some lightning storm effects.

	<div class="rain_placeholder">
		<img src="img/ligthning1.png" class="lightning" />
		<img src="img/ligthning1.png" class="lightning2 delayed1" />
	</div>

**CSS:**

```
/* Delay timers */
.rain_placeholder img.delayed1 { animation-delay: 1s; }
.rain_placeholder img.delayed2 { animation-delay: 2s; }
.rain_placeholder img.delayed3 { animation-delay: 3s; }
.rain_placeholder img.delayed4 { animation-delay: 2.5s; }
.rain_placeholder img.delayed5 { animation-delay: 1.5s; }
.rain_placeholder img.delayed6 { animation-delay: 4s; }
.rain_placeholder img.delayed7 { animation-delay: 0.5s; }

/* Lightning */
@keyframes lightning {
	0%   { opacity:0; }
	89%  { opacity:0; }
	90%  { opacity:1; }
	100% { opacity:0; }
}

.lightning,
.lightning2 {
  position: absolute;
  top: 0px;
  left:50%;

  animation-name: lightning;
  animation-duration: 1s;
}

.lightning2 {
  top: 200px;
  left: 24%;

  /* Scale the image */
  width: 150px;

  /* flip the image */
  -moz-transform: scaleX(-1);
  -o-transform: scaleX(-1);
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
  filter: FlipH;
  -ms-filter: "FlipH";
}
```

We have now added our Lightning!
In this example we have 2 lightning strikes. One will have a delay (i created a template of different delays i would like to use for various elements) as well as transforming one of the lightning strikes to make it position and face differently to the first lightning strike.

### Add our content

Lets start adding our content to the page, this allows us to see what part of the background is covered in content and how things may look in the end.

```
	  <h1 class="application_title"> Warcraft Raid Profile </h1>
	  <div class="version_num"> v0.0.1 </div>
	  <div class="username"> Username </div>

	  <div class="videoHolder section_pad">
		<div>
		  <video width="100%" height="100%" autoplay muted loop>
			<source src="video.mp4" type="video/mp4">
			Your browser does not support the video tag.
		  </video>
		</div>
	  </div>
```

```
	  <div class="content_section section_pad">
		<h3> About Username: </h3>
		<p> Use this section to add a description about yourself; as a person and as a raider. Talk about your past experiences with raiding and what sort of role and utility you can bring to the raid.  <br /> <br />

The description should be short enough to keep people interested, but long enough that you can tell as much needed information as possible for your application / profile. Feel free to add specific links to logs as well as talk about what sort of raiding envioment best suits you.  </p>
	  </div>

	  <div class="section_split"> </div>

	  <div class="content_section section_pad">
		<h3> Live Information: </h3>
		<div class="item_level"> Ilvl 740 </div>

		<div class="content_container">
		  <div class="content_row"> <h4> The Nighthold: </h4> 13/13 Normal | 13/13 Heroic | 13/13 Mythic </div>
		  <div class="content_row"> <h4> Emerald Nightmare: </h4> 13/13 Normal | 13/13 Heroic | 13/13 Mythic </div>
		  <div class="content_row"> <h4> Hellfire Citadel: </h4> 13/13 Normal | 13/13 Heroic | 13/13 Mythic </div>
		  <div class="content_row"> <h4> Blackrock Foundery: </h4> 13/13 Normal | 13/13 Heroic | 13/13 Mythic </div>
		  <div class="content_row"> <h4> Highmaul: </h4> 13/13 Normal | 13/13 Heroic | 13/13 Mythic </div>
		</div>

	  </div>

	  <div class="section_split"> </div>

	  <div class="content_section footer_block">
		World of Warcraft raid profile is currently a small beta application, Data displayed may be out-of-date and come from numerous sources.
		<a href="#"> WoW Raid profile is made by Scott Jones and open sourced </a>
	  </div>
```

**CSS:**

```
/* Content Positioning */
.content_wrapper {
  position: absolute;
  top:0px;
  left:0px;
  width:100%;
  height:100%;
}

.content_wrapper_inner {
  width:1000px;
  margin:0 auto;
  padding: 200px 0 20px 0;

  position: relative;
}

.section_pad { padding: 0px 184px; }

/* Video sections */
.videoHolder { margin:80px 0px 20px; }

.videoHolder div {
  background: #0f0f0f;
  border-bottom: 1px solid rgba(255,255,255,0.25);
  box-shadow: inset 3px 3px 5px rgba(0,0,0,0.4);
  height: 363px;
  border-radius: 8px;
  padding: 7px;
}

.videoHolder video { border-radius: 5px; }

/* Color*/
.application_title,
.version_num,
.username,
.content_section h3,
.content_row h4,
.item_level {
	color:#fff;
}

/* Header elements */
.application_title {
  font-size: 70px;
  font-weight: 300;
  text-align: center;
}

.version_num {
  position: absolute;
  top: 214px;
  right: 186px;

  font-size: 12px;
  font-weight: 200;
}

.username {
  text-align: right;
  padding-right: 186px;

  font-weight: 200;
  font-size: 35px;
  line-height: 10px;
}

/* Section split CSS */
.section_split {
  margin:60px 100px;
  border-bottom: 1px solid rgba(255,255,255,0.1);
}

/* Content section */
.content_section { font-size:13px; position:relative; }
.content_section h3 {
  font-weight:100;
  font-size:38px;
  padding-bottom: 10px;
}

.item_level {
  position: absolute;
  top: 10px;
  right: 200px;
  font-size: 30px;
}

.content_row {
  padding:20px 20px 20px;
  border-bottom: 1px solid rgba(255,255,255,0.1);
  position: relative;
}

.content_row:first-child { margin-top:20px; }
.content_row:last-child { border:none; }
.content_row:nth-child(even) { background:rgba(0,0,0,0.35); }

.content_row h4 {
  font-size: 24px;
  font-weight:100;
}

.content_container { padding-bottom:40px; }

/* Footer Block */
.footer_block { text-align:center; padding-bottom: 40px; }
.footer_block a { display:block; text-align:right; padding:10px 100px 0 0; }
```

The content is now positioned ontop of our scenes backdrop, we have different sections displaying different content. Added a featured video section and some generic copyright footer.

### Add our rain
Saving the best part till last, let us now create some rain!

**JS:**

```
  <script>
	// Get the container that we render our rain too
	var rainholder = document.querySelector('.rain_placeholder');
	var MAX_RAIN_LAYERS = 8;

	// Generate a number of rain elements
	// Change 8 to the number of rain layers you want.
	//
	// Each layer contains 8 rain images coming down at different locations (set in css) and follow a similar direction.
	// With 8 rain layers we hope to create a consistant rain effect
	//
	// InnerHTML is a nasty way of rendering items, however i didn't feel like writing a create element method to
	// create my rain images and assign the correct css classes to them. Then using appendChild to add them cleanly.
	// I will try to add this at a later time :)
	for (var i = 0; i < MAX_RAIN_LAYERS; i++) {
	  rainholder.innerHTML += '<img src="img/rain.png" class="rain1 delayed' + i +'" />' +
	  '<img src="img/rain.png" class="rain2 delayed' + i +'" />' +
	  '<img src="img/rain.png" class="rain3 delayed' + i +'" />' +
	  '<img src="img/rain.png" class="rain4 delayed' + i +'" />' +
	  '<img src="img/rain.png" class="rain5 delayed' + i +'" />' +
	  '<img src="img/rain.png" class="rain6 delayed' + i +'" />' +
	  '<img src="img/rain.png" class="rain8 delayed' + i +'" />' +
	  '<img src="img/rain.png" class="rain7 delayed' + i +'" />';
	}
  </script>
```

**CSS:**

```
/* Rain Default state*/
.rain_placeholder img {
  opacity: 0;
  position: absolute;
  top:0px;
  right:0px;

  animation-duration: 4s;
  animation-iteration-count: infinite;
  animation-timing-function: ease-in;
}

@keyframes rain1 {
	0%   { top:-400px; right:1000px; opacity:0.3; }
	96%  { top:900px; right:1200px;  opacity:0.2; }
	100% { top: 940px; right:1200px; opacity:0; }
}

@keyframes rain2 {
	0%   { top:-900px; right:700px; opacity:0.4; }
	80%  { top:900px; right:980px;  opacity:0.2; }
	100% { top: 940px; right:980px; opacity:0; }
}

@keyframes rain3 {
	0%   { top:-900px; right:1200px; opacity:0.5;}
	80%  { top:900px; right:1680px;  opacity:0.2;}
	100% { top: 940px; right:1680px; opacity:0;}
}

@keyframes rain4 {
	0%   { top:-400px; right:950px; opacity:0.3; }
	80%  { top:900px; right:1010px;  opacity:0.1; }
	100% { top: 940px; right:1010px; opacity:0; }
}

@keyframes rain5 {
	0%   { top:-700px; right:1850px; opacity:0.5; }
	80%  { top:900px; right:1930px;  opacity:0.2; }
	100% { top: 940px; right:1930px; opacity:0; }
}

@keyframes rain6 {
	0%   { top:-600px; right:1400px; opacity:0.5; }
	80%  { top:900px; right:1490px;  opacity:0.2; }
	100% { top: 940px; right:1490px; opacity:0; }
}

@keyframes rain7 {
	0%   { top:-600px; right:100px; opacity:0.4; }
	80%  { top:900px; right:190px;  opacity:0.2; }
	100% { top: 940px; right:190px; opacity:0; }
}

@keyframes rain8 {
	0%   { top:-600px; right:430px; opacity:0.4; }
	80%  { top:900px; right:540px;  opacity:0.2; }
	100% { top: 940px; right:540px; opacity:0; }
}

/* Rain Animations */
.rain1 { animation-name: rain1; }
.rain2 { animation-name: rain2; }
.rain3 { animation-name: rain3; }
.rain4 { animation-name: rain4; }
.rain5 { animation-name: rain5; }
.rain6 { animation-name: rain6; }
.rain7 { animation-name: rain7; }
.rain8 { animation-name: rain8; }
```

So we now have a script that will generate the rain images for us, assign the delays to ensure they do not overlap and have the CSS assign the animation positions.

Run this in the browser and you should have some nice rain affects going. Depending on the scene / materials you will certainly want to tweak the values.

### Ending
In the repo you will find some extra CSS which is used for some extra styling and some added responsive features for the background.

If you have any questions or concerns? please feel free to contact me.
Did i make any mistakes or wish to improve this guide? please feel free to send a pull request.

I hope this helps or inspires in some way :D

Regards,
Scott
