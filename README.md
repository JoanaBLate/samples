# Fixing WhatsApp link for Linux desktop


#### Disclaimer
This article was written in December/2020. It may be obsolete now.


### According to WhatsApp 
The right link to WhatsApp in your web page has an href attribute like this:

https://wa.me/XXXXXXXXXXX?text=Hello

You must replace XXXXXXXXXXX by the full phone number in international format. Omit any zeros, brackets, or dashes when adding the phone number in international format.


### The Problem
This method is OK when the user is at smartphone or Windows desktop. And is broken when the user is at a Linux desktop. Clicking on the WhatsApp link (in case of Linux desktop) makes the browser ask your authorization to access some application. Many people would not allow it. Even when you allow it, things don't work (never worked for me).

And what people think? "WhatsApp is broken!"? No. They think your webpage is poorly done, maybe was hacked.


WhatsApp In Desktop
In case you don't know, it is possible, convenient and VERY easy to access your WhatsApp at the browser in desktop.
I use this way to send and receive files easily.


### The Solution
The solution is use one href attribute when the user's device is smartphone and another one when it is desktop. This is also good for Windows desktop users because they jump the offer to install the WhatsApp application, which is not necessary.

# href model for smartphone
https://api.whatsapp.com/send?phone=XXXXXXXXXXX&text=Hello

# href model for desktop
https://web.whatsapp.com/send?phone=XXXXXXXXXXX&text=Hello

Of course, the web page needs a way to know if it is in a smartphone or a desktop. I use the function below but I suppose there is a better method somewhere.

```
function isMobileDevice() {
    if (typeof window.orientation != "undefined") { return true }
    if (navigator.userAgent.indexOf("IEMobile") != -1) { return true }
    return false
}
```

Now we need a function that searches links to WhatsApp in the Html code and edit them according to the device. The classic way to do this is create a special class for these links. You set the phone number and the message in Html, so the JavaScript code is always the same, very portable.

```
function adjustWhatsAppLinks() {
    var href = "https://"
    href += isMobileDevice() ? "api" : "web"
    href += ".whatsapp.com/send?" 
    
    var anchors = document.getElementsByClassName("whatsapp-link")
    for (var a of anchors) { a.href = href + a.href }
}
```

And a simple link in Html would be like this:
```
<a class="whatsapp-link" href="phone=XXXXXXXXXXX&text=Hello">WhatsApp</a>
```



