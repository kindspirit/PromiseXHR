# PromiseXHR
Lightweight library that combines XMLHttpRequest with JavaScript's native Promises

Lightweight! Only 1.2 Kilobytes. (1233 bytes minified)

Tested and working in IE 6! IE 6 and other old browsers make use of https://github.com/taylorhakes/promise-polyfill which loads via:

self.Promise||document.write("<script src=https://cdn.jsdelivr.net/npm/promise-polyfill@8/dist/polyfill.min.js><\/script>"))

In other words the external script load happens syncronously (which is bad) but only in the event that the Promise object isn't already defined (which is good since it is defined in all modern browsers excluding Internet Explorer.)

Usage:

Pass your own XMLHttpRequest object:
XHR(XMLHttpRequest [, data (object | string) [, timeout (number)]])


XHR(URL (string) [, data (object | string) [, timeout (number)]])

XHR({url: URL (string), method: request method (string), headers: headers (object)} [, data (object | string) [, timeout (number)]])



Returns a Promise, fulfilled with xhr on network response. Rejected on error or timeout with reason "error" or "timeout" respectively.

Unless you pass your own XMLHttpRequest object, adds header:

    X-Requested-With: XMLHttpRequest
    
Unless you pass your own XMLHttpRequest object, or you use a request method other than POST, or you pass a FormData as your data object, adds header:

    Content-Type: application/x-www-form-urlencoded
