This lab contains a [DOM-based cross-site scripting](https://portswigger.net/web-security/cross-site-scripting/dom-based) vulnerability in the search query tracking functionality. It uses the JavaScript `document.write` function, which writes data out to the page. The `document.write` function is called with data from `location.search`, which you can control using the website URL.

To solve this lab, perform a [cross-site scripting](https://portswigger.net/web-security/cross-site-scripting) attack that calls the `alert` function.




The first step on this lab on the search box I put random numbers

![[Screenshot_2023-09-18_13_41_48.png]]

ON the description  we can see this 

```
src="/resources/images/tracker.gif?searchTerms=asdwe3e123"
```

Now, the idea is put on the search box  ```"><svg onload=alert(1)>```
![[Screenshot_2023-09-18_13_47_10.png]]
And we solve the lab, because the alert is successfully inserted on the document, inside of a label svg.





# DOM XSS in innerHTML sink using source location.search #

This lab contains a [DOM-based cross-site scripting](https://portswigger.net/web-security/cross-site-scripting/dom-based) vulnerability in the search blog functionality. It uses an `innerHTML` assignment, which changes the HTML contents of a `div` element, using data from `location.search`.

To solve this lab, perform a [cross-site scripting](https://portswigger.net/web-security/cross-site-scripting) attack that calls the `alert` function.

You must apply the same solution than the lab that we did recently on the top.


