# country.proxynova.com

> https://country.proxynova.com/

This was inspired by one API service from Amazon that simply returns the IP address of the client that's making the request.

> http://checkip.amazonaws.com/

mentioned here:

> https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-sg.html#configuring-a-security-group

The goal then was to create something that's equally simple to use, 
but instead to return the country of origin for the connecting IP address 
which can then be used in many useful ways.

## Check Country as a Web Service

Features:

- No JSON to parse - simple plain text responses.
- No HTTP headers to decode - HTTP 200 everytime.
- No API keys/limits/throttling to deal with - unlimited usage.
- Works with both HTTP and HTTPS.

With this, you get a consistent, plain text response no matter what:

```shell
# curl http://country.proxynova.com -i
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Content-Length: 13

United States
```

In cases where invalid input is provided, an empty response with status code 200 is returned.

## Examples

```shell
curl http://country.proxynova.com
151.101.1.69
```

Will assume the IP address of the connecting client OR use custom IP provided.

```shell
curl http://country.proxynova.com/77.193.88.77
France
```

Can work with hostnames too:

```shell
curl http://country.proxynova.com/vk.com
Russia
```

## Check user's country of origin using PHP

```php
$client_ip = $_SERVER['REMOTE_ADDR'];
$country_name = file_get_contents("https://country.proxynova.com/{$client_ip}");

if($country_name == "Pakistan"){
    die("Pakistan is Blocked!");
}
```

## Check user's country using Javascript's Jquery

https://jsfiddle.net/jf6hqLm9/

```javascript
$.get("https://country.proxynova.com/", function(data){
    if(data == 'Pakistan'){
        alert('Pakistan is Blocked!');
    }
});
```

## Detect visitor's country using pure javascript

https://jsfiddle.net/bqug7v3x/

```javascript
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://country.proxynova.com');
xhr.onload = function(){
	document.getElementById('country').innerHTML = xhr.responseText;
};
xhr.send();
```

## List of Country Names

Some of the country names returned may not be what you expect them to be.
For example, Lithuania shows up as "Republic of Lithuania".

To see the exact list of country names used by MaxMind go here:  
[data/GeoLite2-Country-Locations-en.csv](https://github.com/Athlon1600/country.proxynova.com/blob/master/data/GeoLite2-Country-Locations-en.csv)


## GeoLite2 Free

All the data is from MaxMind.

https://dev.maxmind.com/geoip/geoip2/geolite2/
