# Vanilla JS Single Page App

Contents 
- Overview
- Javascript Documentation
- CSS Documentation
- Resources / Contact Info

## Overview

I wanted to create a very simple file + data combo for a single page app for some of my photos. I decided to try and do everything in a single html file as a challenge using a single JSON file to handle the photography data. I also wanted to demonstrate a simple use of the Fetch API. 

_Note: that means for this to work you have to upload the files to a server_.

The CSS is declared in a `<style>` element between the `<head>` and the `<body>` element. The Javascript is declared after the `<footer>` element and before the closing `</body>` element.

The data I'm using is simple. It's just an array of image objects that contain the attributes for each image.

**data.json**
```
{
	"photos": [
		{
			"id":0,
			"title":"$SPY +0.69",
			"filepath":"/photos/spy69.png",
			"date":"April 5 2020",
			"price":"69"
		},
    ...
}
```

The app takes the data and creates the html using a series of functions in vanilla javascript.

## Javascript Documentation

**lines 238-251 of index.html**
```
	fetch('./data.json', {
		method: 'GET',
		mode: 'cors',
		credentials: 'same-origin',
		headers: {
			'Content-Type': 'application/json'
		}
	})
  		.then((response) => {
    	return response.json();
  	})
  		.then((data) => {
    	displayPics(data);
  	});
```

This is the function that retreives the JSON file. It uses the Fetch API which allows you to chain promises together. Read more about the Fetch API [here](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API).

There is no reason why I chose to use the Fetch API over requiring the JSON file, since it's a local file, other than to show a simple use of it.

The `fetch` function accepts two arguments, the url of the resource you are trying to get and an object that describes how you're going to get it. The object is where you can declare things like whether the request will use the `GET` method, what the headers will be, etc.

You can then chain several functions after the initial fetch that describes what you'll do with the response data. In this example, I'm taking the response and converting it into readable data. Read about the json() method [here](https://developer.mozilla.org/en-US/docs/Web/API/Body/json) to get some more documentation about that.

I then pass the data on to my function `displayPics(data)`, which converts the data into html.

**lines 179-193 of index.html**
```
	let displayPics = (data) => {

		let photos = data.photos;

		let main = document.querySelector('main');

		for(var i = 0; i < photos.length; i++) {

			let article = createArticle(photos[i], i);

			main.appendChild(article);

		}

	}

```

The `displayPics` function gets the `<main>` element from the document and adds an `<article>` element to it for each photo. The `<article>` element is created by the `createArticle` function.

**lines 195-214 of index.html**
```
	let createArticle = (photo, i) => {

		let article = document.createElement('article');
		let image = getImage(photo.filepath, photo.title);
		let date = getDate(photo.date);


		article.appendChild(image);
		article.appendChild(date);

		switch (i % 2) {
			case 0: 
				article.className = 'article-type-1';
				break;
			case 1:
				article.className = 'article-type-2';
				break;
		}

		return article;
	}
```

The `createArticle` function uses the `createElement` method to create an `<article>` element. The image and the date are appended to the `<article>` elements and are created by the `getImage` and `getDate` functions. 

The `<article>` element that is created gets a classname of `article-type-1` or `article-type-2` depending on if is index in the photos array is even or odd. Those classes will determine the article's alignement.

**lines 216-236 on index.html**
```
	let getImage = (src, alt) => {

		let image = document.createElement('img');

		image.src = src;

		image.alt = alt;

		return image;
	}

	let getDate = (date) => {

		let date_element = document.createElement('h3');

		date_element.innerHTML = date;

		date_element.className = "desktop-display-only";

		return date_element;
	}
```

## CSS Documentation

Because this app is simple and I don't plan on extending it beyond what it is, the CSS is also simple and relies on element style declarations instead of class names, ids, or any other attributes. 

This has two benefits: it's easier and more semantic to read imo and it's perfect for a lazy developer.

I use `flexbox` to lay out this app. Learning how to use `flex` is a game changer for CSS and practically makes your positioning problems disappear. Check out the [documentation](https://www.w3schools.com/css/css3_flexbox.asp) to learn how to use it.

This approach also has many downsides if you tried this style within a team setting or on a more complicated app or website where the styles would need to scale.

CSS naming conventions are really interesting to me, especially as a freelancer who hasn't had to work with other developers, just designers. If you're in the same boat, check out this [article](https://css-tricks.com/bem-101/) on a popular naming convention to get yourself thinking about CSS in a scalable way. 

## Resources / Contact Info



