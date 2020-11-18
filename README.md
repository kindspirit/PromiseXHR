# PromiseXHR
Lightweight library that combines XMLHttpRequest with JavaScript's native Promises

Lightweight! Only 1.2 Kilobytes.

Works on IE6!

Old browsers make use of https://github.com/taylorhakes/promise-polyfill which loads via:

    IF (!self.Promise) {
        document.write("<script src=https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js><\/script>"))
    }

<h1>How to use:</h1>

To support old browsers like Internet Explorer, put the following at the BOTTOM of your HTML just before &lt;/body&gt;.

    <script src="promisexhr.min.js"></script>

Do not use async or defer attributes. This is because of the syncronous document.write call in browsers not supporting native Promises.

Usage:

    XHR(xhr [, data (object | string) [, timeout (number)]])

    XHR(url (string) [, POST data (object | string) [, timeout (number)]])

    XHR({url: URL (string), method: request method (string), headers: headers (object literal)} [, data (object | string) [, timeout (number)]])

Any parameter can be omitted. The request method defaults to GET unless the data parameter is provided in which case it defaults to POST. URL defaults to the current page (location.pathname+location.search).

The data parameter can be a string, an object literal containing name-value pairs (which will be converted to a string), or anything supported by <a href="https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/send">xhr.send()</a>.

Returns fulfilled Promise containing xhr object when xhr.readyState==4 (xhr.status<1 or >599 indicate network error). Rejected on timeout with object {message:"timeout"}.

If you do not pass your own XMLHttpRequest object, the following header is added:

    X-Requested-With: XMLHttpRequest
    
If you do not pass your own XMLHttpRequest object, the request method is POST, and a FormData object is not passed as the data parameter, the following header is added:

    Content-Type: application/x-www-form-urlencoded

This example will submit a POST request with a timeout of 30000 miliseconds:

    XHR("contact.php", {FromName: "john smith", FromEmail: "johnsmith@gmail.com", subject: "Contact Form", body: message_body}, 30000).then(function(xhr) {
        if (xhr.status==200) {
            alert("Thank you for contacting us!")
        }
        else {
            return Promise.reject({message: xhr.status+" "+xhr.statusText});
        }
    }).catch(function(reason) {
        alert("Request failed for the following reason: "+reason.message);
    });

