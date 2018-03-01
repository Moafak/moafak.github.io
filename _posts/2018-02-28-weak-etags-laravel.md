---
layout: post
title: Weak Etags Middleware in Laravel 5
excerpt_separator: <!--more-->
---

## Intro
When building REST APIs you may find yourself hitting some endpoint(s) frequently to check if the resource at that endpoint has changed or not.  
In such cases you might consider saving bandwidth, and that can be achieved by using etags.  
In this blog post I’m going to first explain etags and how to use them, then will explain the reason behind the need to write a weak etag middleware for Laravel 5.

<!--more-->

## What is etag
As the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) say it:
>The ETag HTTP response header is an identifier for a specific version of a resource. It allows caches to be more efficient, and saves bandwidth, as a web server does not need to send a full response if the content has not changed.

So basically, etag means entity tag, and it is an http header that the server sends with the response, its value represents the entity (the resource) that is sent in the response.  
The etag is usually a hash of the contents sent in the http response body.

## How does etag work?
Here is an example you can run in Postman  
[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/3efe7ef7a024354b2726)  
When you first make a call to your API, you will get a response `200 OK`, and an http header `etag` with a value, the body will hold the value of the response (check the body tab on postman).

![200 OK](/public/images/weak-etags-laravel/200.png)

The next time you call the same endpoint, you send an http header called `If-None-Match` which holds the value of the `etag` received previously, and that means: I want to get the resource at this endpoint, but only if its representation is different from `If-None-Match` value.  
So if the resource has not changed, you will get a `304 Not Modified` response, which means that the resource at the given endpoint has not changed, and the body of the response will be empty.  

![304 Not Modified](/public/images/weak-etags-laravel/304.png)

## Nginx, gzip, and etag
I had a setup where we there was a Laravel app served by nginx, the problem was that we were receiving weak etags in the response, but when we send the weak etag in `If-None-Match` we didn’t get a `304`, we still got `200` responses.  
Apparently this was due to the `Accept-Encoding: gzip` http header, i.e. when we send the header we get weak etags because nginx makes the etag weak when the gzip header is there.  
[This](https://trac.nginx.org/nginx/ticket/377#comment:14) confirms that nginx was actually downgrading strong etags (set by the app) to weak etags, and when we sent back the weak etag, the app didn’t recognize it and `200` response was sent again.  
To work around this, I used a middleware that sets and compares weak etags on the app level, the weak etag middleware is inspired by the [etag middleware](https://github.com/matthewbdaly/laravel-etag-middleware) written by [Matthew Daly](https://matthewdaly.co.uk/blog/2015/06/14/setting-etags-in-laravel-5/), the only difference is that we are setting and comparing weak etags instead of strong ones.  
This worked and we got `304` responses from the endpoint.  

## The weak etag middleware 
```php
<?php

namespace App\Http\Middleware;

use Closure;

/**
 * Weak ETag middleware.
 */
class WeakEtagMiddleware
{
    /**
     * Implement weak Etag support.
     * inspired by https://github.com/matthewbdaly/laravel-etag-middleware
     *
     * @param \Illuminate\Http\Request $request The HTTP request.
     * @param \Closure                 $next    Closure for the response.
     *
     * @return mixed
     */
    public function handle(Request $request, Closure $next)
    {
        // Get response
        $response = $next($request);

        // If this was a GET request...
        if ($request->isMethod('get')) {
            // Generate Etag
            $etag = sha1($response->getContent());
            
            $requestEtag = str_replace('W/"', '', $request->getETags());    //beginning of string
            $requestEtag = str_replace('"', '', $requestEtag);              //end of string

            // Check to see if Etag has changed
            if ($requestEtag && $requestEtag[0] == $etag) {
                $response->setNotModified();
            }

            // Set Weak Etag
            $response->setEtag($etag, true);
        }

        // Send response
        return $response;
    }
}
```

To use this with Laravel 5, save it as `app/Http/Middleware/WeakETagMiddleware.php`.  
Then add the following to the `$middleware` array in `app/Http/Kernel.php`:
`'App\Http\Middleware\WeakETagMiddleware',`


## Conclusion
Hope this helps, since this problem occurred for many apps working behind nginx and was asked in different ways on [StakOverFlow](https://stackoverflow.com/search?page=2&tab=Relevance&q=nginx%20etag%20gzip).

[https://stackoverflow.com/questions/44772829/nginx-express-etag-caching-doesnt-work](https://stackoverflow.com/questions/44772829/nginx-express-etag-caching-doesnt-work)  
[https://stackoverflow.com/questions/39365856/nginx-http-caching-with-etags](https://stackoverflow.com/questions/39365856/nginx-http-caching-with-etags)  
[https://stackoverflow.com/questions/18693718/weak-etags-in-rails](https://stackoverflow.com/questions/18693718/weak-etags-in-rails)   
[https://masa331.github.io/2016/01/06/roda-etag-caching-gotcha.html](https://masa331.github.io/2016/01/06/roda-etag-caching-gotcha.html)  

### Another workaround in nginx configuration files
The following stackoverflow answers say that this can be solved in nginx configuration files  
[https://stackoverflow.com/questions/43397480/caching-with-rails-app-and-chrome-browser/43601615#43601615](https://stackoverflow.com/questions/43397480/caching-with-rails-app-and-chrome-browser/43601615#43601615)  
[https://stackoverflow.com/questions/33415865/getting-no-304-response-in-chrome-safari-but-via-curl/33419418#33419418](https://stackoverflow.com/questions/33415865/getting-no-304-response-in-chrome-safari-but-via-curl/33419418#33419418)  
I haven't this soltuion though, but you might find it worth trying.


