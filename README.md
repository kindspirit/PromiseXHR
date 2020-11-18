# PromiseXHR
Lightweight library that combines XMLHttpRequest with JavaScript's native Promises

Lightweight! Only 1.2 Kilobytes.

Works on IE6!

Old browsers make use of https://github.com/taylorhakes/promise-polyfill which loads via:

    IF (!self.Promise) {
        document.write("<script src=https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js><\/script>"))
    }

In other words the external script load happens syncronously (which is bad) but only in the event that the Promise object isn't already defined (which is good since it is defined in all modern browsers excluding Internet Explorer.)

Usage:

    XHR(XMLHttpRequest [, data (object | string) [, timeout (number)]])

    XHR(URL (string) [, POST data (object | string) [, timeout (number)]])

    XHR({url: URL (string), method: request method (string), headers: headers (object)} [, data (object | string) [, timeout (number)]])

Any parameter can be omitted. The request method defaults to GET unless data is provided in which case it defaults to POST. URL defaults to the current page (location.pathname+location.search).

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


How to use. Put the following at the BOTTOM of your HTML just before &lt;/body&gt;. (Must be loaded syncronously. The async and defer attributes will crash old browsers. Don't use them.)

    <script src="promisexhr.min.js"></script>
