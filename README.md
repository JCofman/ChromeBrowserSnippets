# Chrome Browser Snippets / Firefox Scratchpad / Bookmarklets for Developer

This is a collection of usefull 🚀💪 / unuseful 🙈 Snippets for JS developer
that you can run on any page by using the Chrome snippets toolbar.

## 👩‍🎨 Turn on Designmode

you can turn on design mode to manipulate for example text or add an icon on a
page.

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

Add a JS Performance Monitor [More infos](https://github.com/mrdoob/stats.js/)

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

### 💩 Fartscroll

After running this snippet you can scroll through the page and your webpage will
fart. [More infos](https://github.com/theonion/fartscroll.js)

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

### 💅 Get Critical CSS

After running this snippet it prints out critical CSS.
[More infos](https://gist.github.com/PaulKinlan/6284142)

```
(function() {
  var CSSCriticalPath = function(w, d, opts) {
    var opt = opts || {};
    var css = {};
    var pushCSS = function(r) {
      if(!!css[r.selectorText] === false) css[r.selectorText] = {};
      var styles = r.style.cssText.split(/;(?![A-Za-z0-9])/);
      for(var i = 0; i < styles.length; i++) {
        if(!!styles[i] === false) continue;
        var pair = styles[i].split(": ");
        pair[0] = pair[0].trim();
        pair[1] = pair[1].trim();
        css[r.selectorText][pair[0]] = pair[1];
      }
    };

    var parseTree = function() {
      // Get a list of all the elements in the view.
      var height = w.innerHeight;
      var walker = d.createTreeWalker(d, NodeFilter.SHOW_ELEMENT, function(node) { return NodeFilter.FILTER_ACCEPT; }, true);

      while(walker.nextNode()) {
        var node = walker.currentNode;
        var rect = node.getBoundingClientRect();
        if(rect.top < height || opt.scanFullPage) {
          var rules = w.getMatchedCSSRules(node);
          if(!!rules) {
            for(var r = 0; r < rules.length; r++) {
              pushCSS(rules[r]);
            }
          }
        }
      }
    };

    this.generateCSS = function() {
      var finalCSS = "";
      for(var k in css) {
        finalCSS += k + " { ";
        for(var j in css[k]) {
          finalCSS += j + ": " + css[k][j] + "; ";
        }
        finalCSS += "}\n";
      }

      return finalCSS;
    };

    parseTree();
  };


  var cp = new CSSCriticalPath(window, document);
  var css = cp.generateCSS();

  console.log(css);
})();
```

## 🚅 Show Perfmap

This snippet generates a perfmap. [More infos](https://zeman.github.io/perfmap)

```
javascript:(function(){var el=document.createElement('script');el.src='https://zeman.github.io/perfmap/perfmap.js';document.body.appendChild(el);})();
```

## ⚒ Remove all CSS from current site

If the currently visited site uses jQuery you can easily run this snippet.
Otherwise run first [jQuery Snippet](#add-jquery) to inject jQuery to the
currently visited page and [More infos](https://zeman.github.io/perfmap)

```
javascript: (function() {
    $('[style]').removeAttr('style');
    $('link[type="text/css"]').remove();
    $('style').remove();
}
)();
```

## ⏰ Timings

```
(function(window) {
    "use strict";
    window.timing = window.timing || {
        getTimes: function(opts) {
            var performance = window.performance || window.webkitPerformance || window.msPerformance || window.mozPerformance;
            if (performance === undefined) {
                return false
            }
            var timing = performance.timing;
            var api = {};
            opts = opts || {};
            if (timing) {
                if (opts && !opts.simple) {
                    for (var k in timing) {
                        if (isNumeric(timing[k])) {
                            api[k] = parseFloat(timing[k])
                        }
                    }
                }
                if (api.firstPaint === undefined) {
                    var firstPaint = 0;
                    if (window.chrome && window.chrome.loadTimes) {
                        firstPaint = window.chrome.loadTimes().firstPaintTime * 1e3;
                        api.firstPaintTime = firstPaint - window.chrome.loadTimes().startLoadTime * 1e3
                    } else if (typeof window.performance.timing.msFirstPaint === "number") {
                        firstPaint = window.performance.timing.msFirstPaint;
                        api.firstPaintTime = firstPaint - window.performance.timing.navigationStart
                    }
                    if (opts && !opts.simple) {
                        api.firstPaint = firstPaint
                    }
                }
                api.loadTime = timing.loadEventEnd - timing.fetchStart;
                api.domReadyTime = timing.domComplete - timing.domInteractive;
                api.readyStart = timing.fetchStart - timing.navigationStart;
                api.redirectTime = timing.redirectEnd - timing.redirectStart;
                api.appcacheTime = timing.domainLookupStart - timing.fetchStart;
                api.unloadEventTime = timing.unloadEventEnd - timing.unloadEventStart;
                api.lookupDomainTime = timing.domainLookupEnd - timing.domainLookupStart;
                api.connectTime = timing.connectEnd - timing.connectStart;
                api.requestTime = timing.responseEnd - timing.requestStart;
                api.initDomTreeTime = timing.domInteractive - timing.responseEnd;
                api.loadEventTime = timing.loadEventEnd - timing.loadEventStart
            }
            return api
        },
        printTable: function(opts) {
            var table = {};
            var data = this.getTimes(opts) || {};
            Object.keys(data).sort().forEach(function(k) {
                table[k] = {
                    ms: data[k],
                    s: +(data[k] / 1e3).toFixed(2)
                }
            });
            console.table(table)
        },
        printSimpleTable: function() {
            this.printTable({
                simple: true
            })
        }
    };
    function isNumeric(n) {
        return !isNaN(parseFloat(n)) && isFinite(n)
    }
    if (typeof module !== "undefined" && module.exports) {
        module.exports = window.timing
    }
}
)(typeof window !== "undefined" ? window : {});
timing.printTable();
```

## 👓 Tota11y Accessibility

[More infos](khan.github.io/tota11y/tota11y/build/tota11y)

```
javascript: (function() {
    var tota11y = document.createElement('SCRIPT');
    tota11y.type = 'text/javascript';
    tota11y.src = '//khan.github.io/tota11y/tota11y/build/tota11y.min.js';
    document.getElementsByTagName('head')[0].appendChild(tota11y);
}
)();
```
