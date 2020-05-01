# Vanilla JS Single Page App

Contents 
- Overview
- Documentation
- Resources / Contact Info

## Overview

I wanted to create a very simple file + data combo for a single page app for some of my photos. I decided to try and do everything in a single html file as a challenge using a single JSON file to handle the photography data. I also wanted to demonstrate a simple use of the Fetch API. _Note: that means for this to work you have to upload the files to a server_.

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

## Documentation

###### Fetch API

**lines 395-408 of index.html**
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

How the `fetch` function works is it accepts two arguments, the url of the resource you are trying to get and an object that describes how you're going to get it. The object is where you can declare things like whether the request will use the `GET` method, what the headers will be, etc.

You can then chain several functions after the initial fetch that describes what you'll do with the response data. In this example, I'm taking the response and converting it into readable data. Read about the json() method [here](https://developer.mozilla.org/en-US/docs/Web/API/Body/json) to get some more documentation about that.

I then pass the data on to my function `displayPics(data)`, which converts the data into html.

**lines 217-275 of index.html**
```
let displayPics = (data) => {

		let photos = data.photos;

		photos = shuffle(photos);

		let main = document.querySelector('main');
		let aside = document.querySelector('aside');

		offset_array = [];

		for(var i = 0; i < photos.length; i++) {

			let article = createArticle(photos[i], i);

			main.appendChild(article);

			let article_indicator = createArticleIndicator(i, article.offsetTop);

			offset_array.push(article.offsetTop);

			aside.appendChild(article_indicator);

		}

		window.addEventListener('scroll', function() {


			for (var i = 0; i < offset_array.length; i++) {

				let circle = document.querySelector('.circle-' + i);

				if (window.pageYOffset > offset_array[i] && window.pageYOffset < offset_array[i + 1] && i + 1 != offset_array.length) {

					circle.setAttribute('fill', 'rgb(200,200,200)');

				} else {

					circle.setAttribute('fill', 'rgb(245,245,245)')

				}

				if (window.pageYOffset > offset_array[i] && i == offset_array.length - 1) {

					circle.setAttribute('fill', 'rgb(200,200,200)')

				}

				if (window.pageYOffset == 0 && i == 0) {

					circle.setAttribute('fill', 'rgb(200,200,200)')

				}

			}

		})

	}
```
