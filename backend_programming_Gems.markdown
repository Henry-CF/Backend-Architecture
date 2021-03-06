# Backend Programming Gems

By: Brian Wigginton

# Table of Contents

1. Stacks
1. HTTP
	1. Request Types
	1. HTTP Headers
	1. Response Codes
1. Code
	1. SCRs
	1. Frameworks
1. Security
	a. Storing User Data
		a. PII
		a. Passwords
1. Performance
	a. Caching
1. Architecture
	1. Servers
1. Databases
1. Monitoring
	1. Testing
	1. Logging
1. Deployments

# Stacks

Asa  developer you need to know the stack you're working on. The two big ones are:

1. **LAMP**: Linux Apache MySQL PHP|Python
2. **WINS**: Windows IIS .NET SQLServer

# HTTP Protocol

As a developer you should understand the HTTP protocol, here's an example HTTP Request:

Response:

	GET / HTTP/1.1
	Host: getconnected.southwestwifi.com
	Connection: keep-alive
	User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/535.19 (KHTML, like Gecko) 	Chrome/18.0.1025.168 Safari/535.19
	Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
	Accept-Encoding: gzip,deflate,sdch
	Accept-Language: en-US,en;q=0.8
	Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.3
	Cookie: s_sq=%5B%5BB%5D%5D; __utma=114654737.1980819.1336079312.1336079312.1336081502.2; __utmb=114654737.2.10.1336081502; __utmc=114654737; __utmz=114654737.1336079312.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none); s_pers=%20s_getnr%3D1336081529628-New%7C1399153529628%3B%20s_nrgvo%3DNew%7C1399153529630%3B; s_sess=%20s_cc%3Dtrue%3B%20s_sq%3D%3B



Request:

	HTTP/1.1 302 Found
	Date: Thu, 03 May 2012 21:49:38 GMT
	Server: Apache/2.2.19 (Unix) DAV/2 mod_ssl/2.2.19 OpenSSL/1.0.0d mod_wsgi/3.3 Python/2.7.2
	Location: http://getconnected.southwestwifi.com/getpage?page=home
	Content-Length: 395
	Connection: close
	Content-Type: text/html; charset=iso-8859-1

	<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
	<html><head>
	<title>302 Found</title>
	</head><body>
	<h1>Found</h1>
	<p>The document has moved <a href="http://getconnected.southwestwifi.com/getpage?page=home">here</a>.</p>
	<hr>
	<address>Apache/2.2.19 (Unix) DAV/2 mod_ssl/2.2.19 OpenSSL/1.0.0d mod_wsgi/3.3 Python/2.7.2 	Server at getconnected.southwestwifi.com Port 80</address>
	</body></html>

Now usually your framework or web server will handle all this, but you need to know how all this works.

## HTTP Request Methods

There's only a few HTTP methods that are included in the HTTP protocol.

You're probably familiar with the two biggies. GET and POST, as these are most commonly used when creating forms on the Web.

1. GET
1. POST
1. PUT
1. DELETE
1. HEAD
1. UPDATE
1. C_____

### GET

GET requests are the most common. You're basically asking the server to GET you some content.

### HEAD

HEAD requests are the same as GET requests, except the server will only send you the HTTP HEADERS, and omit the response BODY. It's a great way to check the server and not waste a bunch of bandwidth. 

### POST

POST request are again similiar to GET requests except you can send a lot more data to the server. You've proabbly seen these when working with forms, and needing to send large amounts of data to the server, for example, sending a picture or file upload.


## HTTP Status Codes

There's TONS of HTTP status codes to describe how things went when you requested the page. I'm sure you've seen a 404 Not Found page before. 404 means Not Found. Status Codes are a common language to describe what happened when the server tried to respond to your request. Ona  good day you should always get 200, which means everything wen't great, here's your content. when you go to google.com and the search bar is shown the server is basically saying, 200! (hey everything went fine, here's your search bar.)

When developing an app you may come across instances where the users asks for something, but you encountered an issue and couldn't give them that page. Let's say on an ecommerce site, the user asked to see their account info, but they wern't logged in. You could respond with a status code 403 (which means unathorized in server speak, and then show an unauthorized page you've developer) There's HTTP Status codes for just about every state you could come up with. For example, if you're developing an API and the API Limit has been hit, you could respond with an HTTP 429 status code. Browser caching also leveverages status codes, that's what the 300 series is for. 400 is for authorizating, 500 is usually for server errors. When you get bad data from a request, you could respond with a 5__, which means hey brwoser you sent me invalid data, please come again.

HTTP status codes are a language to communicate between a client and a server. If you're a fonrtend developer, and need a JSON servive to be created, have your backend developer use Status Codes to communicate how the request went. Trust me, doing this will make it easier to know what happened in the long term, instead of creating you're own status code langauge.

# Security
## Storing User Data
### PII

Sensitive user data should be encrypted with a tested crypto algorithm such as AES.

### Passwords - Hashing, Salting

Passwords stored as plain text is SIN. Think about it, if someone were to get a hold of the datastore your passwords live in, they could easily read through the data and login as one of your users. 

As an attacker there's a couple things I would look for in your database, and one of this is user credentials for a couple of reasons. First, I can now login to your app as any user on your site. and Second, I can probably alos login to another site with the same credentials. People often use the same passwords for different sites. FREE PRON!

To make this harder for the hoxors, you should do a couple things to lock down passwords. First, use a hashing algorithm instead of storing the password in plaintext. `bcrypt` comes highly recommended. Second, use a salt with the hash, so a hacker can't use a rainbow table.

Hashing is great an all, but hashes are very vulnerable to rainbow tables. Rainbow tables can be found online, and even downlaoded from the internet. It's basically a giant databases of popular words that have been hashed. So the attacker doesn't need to bruteforce anything, just grab the hash, and perform a lookup in the Rainbow table. h4k3d.

With the use of a SALT, 

# Performance

## GZIP

Just gzip everything you can, it'll make everything so much smaller. Most servers come with this capability out of the box, just be sure to enable it!

## Browser Caching

Browser caching is going to GREATLY increase page speed for consecutive visits from a user. When the user first visits your site, they're going to need to download the page content as well as all img, css, js assets that come with the page. If you're site uses a global CSS file, you only want the user to have to download that file one time. You can accomplish this with browser caching.

Browser caching relies on HTTP HEADERs.

# Monitoring

## Logging

Logging should be used to capture and log issues within your application architecture. For instance, a failed login attempt might not be worth noting, but several consecutive may be a warning of something fishy going on. 

# Deployments

Deployments should never be a big deal! Also, if you're paying by the deployment, you're doing it wrong. Deployments should be fast, easy, and not a problem to role back.