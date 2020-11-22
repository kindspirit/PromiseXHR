# PromiseXHR
Lightweight XMLHttpRequest library that uses JavaScript's native Promises and works on old browsers 

Only 1.2 Kilobytes!

Works on IE6!

Old browsers make use of https://github.com/stefanpenner/es6-promise/ which loads via:

    IF (!self.Promise) {
        document.write("<script src=https://cdn.jsdelivr.net/npm/es6-promise@4/dist/es6-promise.auto.min.js><\/script>"))
    }
    
This polyfill does not meet the standards of the specification but I haven't found a better one.

<h1>How to use:</h1>

To support old browsers like Internet Explorer, put the following at the BOTTOM of your HTML just before &lt;/body&gt;.

    <script src="promisexhr.min.js"></script>

Do not use async or defer attributes. This is because of the syncronous document.write call in browsers not supporting native Promises.

Usage:

    promiseXHR(xhr (XMLHttpRequest) [, data (object | string) [, timeout (number)]])

    promiseXHR(url (string) [, POST data (object | string) [, timeout (number)]])

    promiseXHR({url: URL (string), method: request method (string), headers: headers (object)} [, data (object | string) [, timeout (number)]])

Any parameter can be omitted. The request method defaults to GET unless the data parameter is provided in which case it defaults to POST. URL defaults to the current page (location.pathname+location.search).

The data parameter can be a string, an object containing name-value pairs (which will be urlencoded and converted to a string), or anything supported by <a href="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/send">xhr.send()</a>.

The headers parameter if provided must be an object containing name-value pairs containing only US-ASCII characters. Header keys are converted to lowercase.

The timeout parameter must be a number containing the number of milliseconds before the promise is rejected and xhr.abort() is called. The default is 0 (no timeout).

Returns fulfilled Promise containing xhr object when xhr.readyState===4. if xhr.statusText==="" that means the request failed. Otherwise xhr.status and xhr.statusText will contain the HTTP response code and text respectively. The promise is only rejected if there is a timeout (with object {message:"timeout"}).

If you do not pass your own XMLHttpRequest object, the following header is added:

    x-requested-with: XMLHttpRequest
    
If you do not pass your own XMLHttpRequest object, the request method is POST, and a FormData object is not passed as the data parameter, the following header is added:

    content-type: application/x-www-form-urlencoded

This example will submit a POST request with a timeout of 30000 miliseconds:

    promiseXHR("contact.php", {FromName: "john smith", FromEmail: "johnsmith@gmail.com", subject: "Contact Form", body: message_body, "cc[]":["carboncopy1@gmail.com","carboncopy2@gmail.com","carboncopy3@gmail.com"]}, 30000).then(function(xhr) {
        if (xhr.status==200) {
            alert("Thank you for contacting us!")
        }
        else {
            return Promise.reject({message: xhr.status+" "+xhr.statusText});
        }
    })["catch"](function(error) {// Catch is a reserved word in Internet Explorer and must be in quotes
        alert("Request failed for the following reason: "+error.message);
    });
