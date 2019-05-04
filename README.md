# TinyJS

Tiny (very hacky) Javascript in HTML pages.

## Snake

A very mini game of *Snake* in just 2<sup>8</sup>=256 bytes. Use arrow keys, eat the food, avoid your tail! Note that the level wraps-around at the edges.

  * [Play Snake Mini](https://danielgjackson.github.io/tinyjs/mini.html) - use arrow keys.

> ```<body onkeydown=k^=(z=event.which^k)&1?z:0 onload="setInterval('c.width=288;b=b.includes(n=n+[s,31,-s,1][k&3]&991)?[f]:[n,...b];for(i of [f,...b])c.getContext`2d`.fillRect(i%s*9,~~(i/s)*9,8,8);n^f?b.pop():f=~f*89&991',b=[n=f=k=s=64])"><button><canvas id=c>```

I've discovered a (terrible) whitespace hack of using *Vertical Tab* characters instead of spaces: Javascript still sees these as spaces, but HTML doesn't stop processing an unquoted attribute.  I've used this to make a newer-limit-Tweetable 279-byte URL version of the game:

> ```data:text/html,<body%20onkeydown=k^=(z=event.which^k)&1?z:0%20onload=setInterval('c.width=288;b=b.includes(n=n+[s,31,-s,1][k&3]&991)?[f]:[n,...b];for(i%0bof%0b[f,...b])c.getContext`2d`.fillRect(i%s*9,~~(i/s)*9,8,8);n^f?b.pop():f=~f*89&991',b=[n=f=k=s=64])><button><canvas%20id=c>```

I've managed to make the HTML version even smaller (just 226 bytes!), but this sacrifices quite a bit of playability.

> ```<body onkeyup=c.k=c.k?event.which:setInterval('c.width^=0;b=b.includes(n=n+[s,31,-s,1][c.k&3]&991)?[n]:[n,...b];for(iof[f,...b])c.getContext`2d`.fillRect(i%s*8,i/s<<3,7,7);n^f?b.pop():f=~f*89&991',b=[n=f=s=64])><canvas id=c>```

  * [Play Snake Micro](https://danielgjackson.github.io/tinyjs/micro.html) - use arrow keys.


## Tron

A *Tron*-like game in just 182 bytes.

> ```<body onkeyup=k=[s,-1,-s,1][event.which%4] onload=s=300;p=22650;k=0;b=c.getContext`2d`;setInterval('0<p%s&p<s*s/2&(b[p]^=1)?b.fillRect(p%s,p/s,1,1,p+=k):k=0',9)><button><canvas id=c>```

* [Play Tron](https://danielgjackson.github.io/tinyjs/tron.html) - use arrow keys.


## Etch-a-sketch

A *tiny* *Etch-a-sketch* in 124 bytes.

> ```<body onkeydown=c.getContext`2d`.fillRect((c.p=[s=300,-1,-s,1][event.which&3]+c.p||22650)%s,c.p/s,1,1)><button><canvas id=c>```

  * [Play Etch-a-sketch](https://danielgjackson.github.io/tinyjs/etch.html) - use arrow keys.

By removing the `<button>` (whose default formatting provides a nice background), it can be made into a `data:` URL of only 135 characters -- still just under the (old) tweet limit:

> ```data:text/html,<body%20onkeydown=c.getContext`2d`.fillRect((c.p=[s=300,-1,-s,1][event.which&3]+c.p||22650)%s,c.p/s,1,1)><canvas%20id=c>```
