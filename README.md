Ajax Request Demo
=================
[![Build Status](https://travis-ci.org/slugbyte/ajax-request-demo.svg?branch=master)](https://travis-ci.org/slugbyte/ajax-request-demo)  

**index.html**
``` html   
<!DOCTYPE html>
<html>
  <head>
    <title>Metiorites</title>
    <script src="js/jquery.min.js"></script>
    <script src="js/reqwest.min.js"></script>
    <link rel="stylesheet" type="text/css" href="css/style.css">
  </head>
  <body>
    <header></header>
    <main>
      <section>
        <h1>Them fall from space!</h1>
        <ul class="metiorite-image-list"></ul>
      </section>
    </main>
    <footer></footer>
    <script src="js/app.js"></script>
  </body>
</html>
```   

**css/style.css**
``` css   
* {
  margin: 0px; 
  padding: 0px;
}

body {
  background-color: #ff7a7a;
}

main {
  margin: 30px;
}

main section h1 {
  margin-bottom: 30px;
}

main section ul {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  padding: 0px;
}

li {
  background-color: #fff;
  border: solid 1px black;
  margin: 10px;
  padding: 10px;
  border-radius: 3px;
  list-style: none;
  width: 200px;
}

li:hover {
  background-color: rgba(101,237,237, 1);
}

main section h1, main section p {
  text-align: center;
}

```   

**js/app.js**
``` javascript   
'use strict';

// api globals
var appToken = 'RLf82kXaTSeOLmaES2qZ5Bkc8';
var metioriteLandingUrl = 'https://data.nasa.gov/resource/gh4g-9sfh.json';
var seattleCrime = 'https://data.seattle.gov/resource/3xqu-vnum.json';

// app globals
var metoriteData = [];

function Metiorite(name, mass, fall, year){
  this.name = name;
  this.mass = mass;
  this.fall = fall;
  this.year = year;
  metoriteData.push(this);
}

Metiorite.prototype.createLiEl = function(){
  var li = document.createElement('li');
  var section = document.createElement('section');
  li.appendChild(section);
  var h1 = document.createElement('h1');
  h1.textContent = this.name;
  section.appendChild(h1);
  var p = document.createElement('p');
  p.textContent = 'Mass: ' + this.mass + ', Year: ' + new Date(this.year).getFullYear();
  section.appendChild(p);
  return li;
};

function handleRequestFail(data, textSatus){
  dubugger;
}

function getDataFromUrl(url, data, handler){
  url += '?$$app_token=' + appToken;
  $.getJSON(url, data, handler).fail(function(xhr){
    alert( 'Sorry, there was a problem!' );
    console.log('Error Status Code: ' + xhr.status);
    console.log('Error Status Text: ' + xhr.statusText);
    console.dir(xhr);
  });

  //$.ajax({
    //url: url,
    //type: 'GET',
    //data: data,
    //dataType: 'json',
    //success: handler,
  //}).fail(function(xhr) {
    //alert( 'Sorry, there was a problem!' );
    //console.log('Error Status Code: ' + xhr.status);
    //console.log('Error Status Text: ' + xhr.statusText);
    //console.dir(xhr);
  //})
}

function render250Metiorites(offset){
  var metioriteImageListUL = document.getElementsByClassName('metiorite-image-list')[0];
  if (offset < metoriteData.length){
    for (var i = offset; i < metoriteData.length && i < (offset + 250); i++){
      var meatiorLi = metoriteData[i].createLiEl();
      metioriteImageListUL.appendChild(meatiorLi);
    }
  }
}

function handleAjaxRequest(data){
  if (Array.isArray(data)){
    for(var i = 0; i < data.length; i++){
      var item = data[i]
      new Metiorite(item.name, item.mass, item.fall, item.year);
    }
    render250Metiorites(0);
  }
}

var options = {
  '$limit': 5, 
  '$offset': 100,
};

getDataFromUrl(metioriteLandingUrl, options,  handleAjaxRequest);

//function handleSetateRequest(data){
  //console.dir(data);
  //debugger;
//}

//getDataFromUrl(seattleCrime, options,  handleSetateRequest);
```   

