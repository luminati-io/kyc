# CPP API

#### check/update user API
Get current user status

```curl
curl "http://brightdata.com/api/ext_cust?email=johnsmith@google.com" -H "Authorization: Bearer XXX" -H "Content-type: application/json"
```

OK:
```js
{"status":"approved"}
```

Error:
```js
{"error":"user doesn't exist"}
```

Update user

```curl
curl -X PUT "http://brightdata.com/api/ext_cust" -H "Authorization: Bearer XXX" -H "Content-type: application/json" -d "{\"first_name\":\"John\",\"last_name\":\"Smith\",\"ip\":\"92.255.93.162\",\"email\":\"johnsmith@google.com\",\"country\":\"RU\"}"
```

OK:
```json
{"token":"bbb88c03f432b337223dd0924c362ae5ea4648903990612894ef084de216367a"}
```

Error:
```js
{"status":"suspisious_ip"}
```

#### Open iframe

After getting token open iframe (can be in modal dialog): `https://kyc.brightdata.com/#<token>`

e.g.:
https://kyc.brightdata.com/#98a718b94043a2c7f6da7d435f310c9b5b8f6ce921b22dae09a7ffa0ede04e85


#### Iframe example
```html
<script src="https://kyc.brightdata.com/www/kyc/pub/cpp.js"></script>
<h2>KYC</h2>
<div id=kyc style="width: 700px; height: 300px" />
<script>
window.cpp_kyc('#kyc', '98a718b94043a2c7f6da7d435f310c9b5b8f6ce921b22dae09a7ffa0ede04e85', m=>{
  let {type} = m;
  switch (type) {
    case 'verification_result':
	if (m.approved); // approved actions
	else // show reason of reject
	   console.log('Decline reason: '+m.error);
	break;
    case 'contact_us':
	let {error} = m;
	break;
    case 'app_wrapper_size':
	// resize event;
	let {width, height} = m.size;
	break;
    default: console.log('_____default', type, m); break;
  };
});
<script>
```


#### cpp_kyc including cpp.js
```js
function cpp_kyc(selector, token, cb){
    let container = document.querySelector(selector);
    el = document.createElement('iframe');
    el.setAttribute('src', 'https://kyc.brightdata.com/#'+token);
    el.setAttribute('allow', 'camera');
    el.setAttribute('frameborder', 0);
    el.style.overflow = 'hidden';
    el.style.width = '100%';
    el.style.height = '100%';
    container.appendChild(el);
    window.addEventListener("message", m=>{
	    if (m.origin!="https://kyc.brightdata.com")
		    return;
      if (cb)
        cb(m.data);
    });
}
```
