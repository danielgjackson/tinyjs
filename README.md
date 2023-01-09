# TinyJS

Tiny (very hacky) Javascript in HTML pages.  This is *code golf* and therefore an example of what not to do in the real world.

## Snake

A mini game of *Snake* in 250 bytes. Use arrow keys, eat the food, avoid your tail! Note that the level wraps-around at the edges.

  * [Play Snake Mini](https://danielgjackson.github.io/tinyjs/mini.html) - use arrow keys.

> ```<body/onload="setInterval('c.width=288;[f,...b=b.includes(n=n+[s,31,-s,1][k&3]&991)?[f]:[n,...b]].map(i=>c.getContext`2d`.fillRect(i%s*9,(i>>6)*9,8,8));n^f?b.pop():f=~f*89&991',b=[n=f=k=s=64])"onkeydown=k^=(z=event.which^k)&1&&z><button><canvas/id=c>```

See [Snake Mini Explanation](mini-explain.md) for a description of how the code works.

As it uses a forward-slash as a separator between the element name attributes, this avoids having to URL-encode the whitespace.  I've used this to make a 265-byte URL version of the game:

> ```data:text/html,<body/onload="setInterval('c.width=288;[f,...b=b.includes(n=n+[s,31,-s,1][k&3]&991)?[f]:[n,...b]].map(i=>c.getContext`2d`.fillRect(i%s*9,(i>>6)*9,8,8));n^f?b.pop():f=~f*89&991',b=[n=f=k=s=64])"onkeydown=k^=(z=event.which^k)&1&&z><button><canvas/id=c>```

As the above uses `map()` and an arrow function `=>`, the angle-bracket means the attribute must be quoted.  A `for-of` could be used instead, but the whitespace would also usually force the attribute to be quoted.  I've discovered a (terrible?) whitespace hack of using *Vertical Tab* characters instead of spaces: Javascript still sees these as whitespace, but HTML doesn't stop processing an unquoted attribute.  Using this, and several other compromises, I've managed to make the HTML version even smaller (just 224 bytes!), but this sacrifices quite a bit of playability.

> ```<body/onkeyup=c.k=c.k?event.which:setInterval('c.width^=0;for(iof[f,...b=b.includes(n=n+[s,31,-s,1][c.k&3]&991)?[n]:[n,...b]])c.getContext`2d`.fillRect(i%s*8,i/s<<3,7,7);n^f?b.pop():f=~f*89&991',b=[n=f=s=64])><canvas/id=c>```

  * [Play Snake Micro](https://danielgjackson.github.io/tinyjs/micro.html) - use arrow keys.


When converting this to a `data:` URL, the whitespace hack requires URL-encoding, so reverting to `.map()` saves characters overall (240 bytes):

> ```data:text/html,<body/onkeyup="c.k=c.k?event.which:setInterval('c.width^=0;[f,...b=b.includes(n=n+[s,31,-s,1][c.k&3]&991)?[n]:[n,...b]].map(i=>c.getContext`2d`.fillRect(i%s*8,i/s<<3,7,7));n^f?b.pop():f=~f*89&991',b=[n=f=s=64])"><canvas/id=c>```


## Tron

A *Tron*-like game in just 172 bytes.

> ```<body/onkeyup=k=[s=300,-1,-s,1,c.z=c.z||setInterval('0<p%s&p<s*s/2&(c[p+=k]^=1)?c.getContext`2d`.fillRect(p%s,p/s,1,1):k=0',9,p=22650)][event.which%4]><button><canvas/id=c>```

* [Play Tron](https://danielgjackson.github.io/tinyjs/tron.html) - use arrow keys.

<!--
data:text/html,<body/onkeyup=k=[s=300,-1,-s,1,c.z=c.z||setInterval('0<p%s&p<s*s/2&(c[p+=k]^=1)?c.getContext`2d`.fillRect(p%s,p/s,1,1):k=0',9,p=22650)][event.which%4]><button><canvas/id=c>
-->

## Etch-a-sketch

A tiny *Etch-a-sketch* in 124 bytes.

> ```<body/onkeydown=c.getContext`2d`.fillRect((c.p=[s=300,-1,-s,1][event.which&3]+c.p||22650)%s,c.p/s,1,1)><button><canvas/id=c>```

  * [Play Etch-a-sketch](https://danielgjackson.github.io/tinyjs/etch.html) - use arrow keys.

This can be made into a `data:` URL of only 139 characters:

> ```data:text/html,<body/onkeydown=c.getContext`2d`.fillRect((c.p=[s=300,-1,-s,1][event.which&3]+c.p||22650)%s,c.p/s,1,1)><button><canvas/id=c>```


## Prime Numbers

Display prime numbers (inefficiently!) in 44 bytes, reduced from [this Reddit post](https://www.reddit.com/r/javascript/comments/gqoxwh):

> ```for(i=j=0;;)i%j--||(j||console.log(i),j=i++)```

...and as a function returning the first `n` prime numbers, 62 bytes:

> ```n=>eval('for(p=[],i=j=0;n;)i%j--||(j||p.push(i)|n--,j=i++);p')```

<!--
Non-`eval()` version (63 bytes):

> ```n=>{for(p=[],i=j=0;n;)i%j--||(j||p.push(i)|n--,j=i++);return p}```
-->
