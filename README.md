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
