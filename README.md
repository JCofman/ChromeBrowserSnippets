# Chrome Browser Snippets / Bookmarklets for Developer

This is a collection of usefull 🚀💪 / unuseful 🙈 Snippets for JS developer that you can run on any page by using the Chrome snippets toolbar.

## Turn on Designmode

you can turn on design mode to manipulate for example text or add an icon on a page.

`document.designMode = 'on';`

## Add jQuery

```
javascript: (function(e, s) {
    e.src = s;
    e.onload = function() {
        jQuery.noConflict();
        console.log('jQuery injected');
    };
    document.head.appendChild(e);
})(document.createElement('script'), '//code.jquery.com/jquery-latest.min.js')
```

## Add FPS meter

```
javascript: (function() {
    var script = document.createElement('script');
    script.onload = function() {
        var stats = new Stats();
        document.body.appendChild(stats.dom);
        requestAnimationFrame(function loop() {
            stats.update();
            requestAnimationFrame(loop)
        });
    }
    ;
    script.src = '//rawgit.com/mrdoob/stats.js/master/build/stats.min.js';
    document.head.appendChild(script);
}
)()

``` 

### Fartscroll

After running this snippet you can scroll through the page and your webpage will fart. (https://github.com/theonion/fartscroll.js)

```
javascript: (function() {

    var script = document.createElement('script');
    
    script.onload = function() {
        fartscroll(); 
    };

    script.src = 'http://code.onion.com/fartscroll.js';
    document.head.appendChild(script);
}
)()
```
