
This lab contains a [DOM-based cross-site scripting](https://portswigger.net/web-security/cross-site-scripting/dom-based) vulnerability on the home page. It uses jQuery's `$()` selector function to auto-scroll to a given post, whose title is passed via the `location.hash` property.

To solve the lab, deliver an exploit to the victim that calls the `print()` function in their browser.






Here portswigger's website is telling us this:

Another potential sink to look out for is jQuery's `$()` selector function, which can be used to inject malicious objects into the DOM.

jQuery used to be extremely popular, and a classic DOM XSS vulnerability was caused by websites using this selector in conjunction with the `location.hash` source for animations or auto-scrolling to a particular element on the page. This behavior was often implemented using a vulnerable `hashchange` event handler, similar to the following:

`$(window).on('hashchange', function() { var element = $(location.hash); element[0].scrollIntoView(); });`

As the `hash` is user controllable, an attacker could use this to inject an XSS vector into the `$()` selector sink. More recent versions of jQuery have patched this particular vulnerability by preventing you from injecting HTML into a selector when the input begins with a hash character (`#`). However, you may still find vulnerable code in the wild.

To actually exploit this classic vulnerability, you'll need to find a way to trigger a `hashchange` event without user interaction. One of the simplest ways of doing this is to deliver your exploit via an `iframe`:

`<iframe src="https://vulnerablesite.com/#" onload="this.src+='<img src=1 onerror=alert(1)>'">`

In this example, the `src` attribute points to the vulnerable page with an empty hash value. When the `iframe` is loaded, an XSS vector is appended to the hash, causing the `hashchange` event to fire.

#### Note

Even newer versions of jQuery can still be vulnerable via the `$()` selector sink, provided you have full control over its input from a source that doesn't require a `#` prefix.



This are the solutions

1. Notice the vulnerable code on the home page using Burp or the browser's DevTools.
2. From the lab banner, open the exploit server.
3. In the **Body** section, add the following malicious `iframe`:
    
   `<iframe src="https://0ad80043031d158681626bca008f0001.web-security-academy.net/#" onload="this.src+='<img src=1 onerror=print()>'">`
4. Store the exploit, then click **View exploit** to confirm that the `print()` function is called.
5. Go back to the exploit server and click **Deliver to victim** to solve the lab.





