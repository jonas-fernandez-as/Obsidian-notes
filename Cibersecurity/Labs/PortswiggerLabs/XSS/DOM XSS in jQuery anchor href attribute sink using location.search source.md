
This lab contains a [DOM-based cross-site scripting](https://portswigger.net/web-security/cross-site-scripting/dom-based) vulnerability in the submit feedback page. It uses the jQuery library's `$` selector function to find an anchor element, and changes its `href` attribute using data from `location.search`.

To solve this lab, make the "back" link alert `document.cookie`.



In the real world , we need firs to see if the jqery library is working on the site. This article may be very helpfull


https://francescricart.com/como-comprobar-si-una-pagina-ha-cargado-la-libreria-jquery/


We can see a portion where potswigger is telling us this

### Sources and sinks in third-party dependencies

Modern web applications are typically built using a number of third-party libraries and frameworks, which often provide additional functions and capabilities for developers. It's important to remember that some of these are also potential sources and sinks for DOM XSS.

#### DOM XSS in jQuery

If a JavaScript library such as jQuery is being used, look out for sinks that can alter DOM elements on the page. For instance, jQuery's `attr()` function can change the attributes of DOM elements. If data is read from a user-controlled source like the URL, then passed to the `attr()` function, then it may be possible to manipulate the value sent to cause XSS. For example, here we have some JavaScript that changes an anchor element's `href` attribute using data from the URL:

`$(function() { $('#backLink').attr("href",(new URLSearchParams(window.location.search)).get('returnUrl')); });`

You can exploit this by modifying the URL so that the `location.search` source contains a malicious JavaScript URL. After the page's JavaScript applies this malicious URL to the back link's `href`, clicking on the back link will execute it:

`?returnUrl=javascript:alert(document.domain)`


so, basically on the url you must change the parameter by this one :
```/feedback?returnPath=javascript:alert(document.cookie)``` and you will solve the lab when you click on back.



