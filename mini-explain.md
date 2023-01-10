# Snake Mini Explanation

This is the full 249 bytes of the [Snake Mini](https://danielgjackson.github.io/tinyjs/#snake) HTML page ([playable](https://danielgjackson.github.io/tinyjs/mini.html)):

```html
<body/onload="setInterval('c.width=288;[f,...b=[n,...b.includes(n=n+[s,31,-s,1][k&3]&991)?[]:b]].map(i=>c.getContext`2d`.fillRect(i%s*9,(i>>6)*9,8,8));n^f?b.pop():f=~f*89&991',b=[n=f=k=s=64])"onkeydown=k^=(z=event.which^k)&1&&z><button><canvas/id=c>
```

This page explains how this mess works!


## Locations

The locations (of snake body parts and food) are represented by a single number with the bit pattern:

> **YYYY**0**XXXXX**

The lower bits representing the horizontal position (0-31), and the upper bits representing the vertical position (0-15).  The `0` bit between allows the position to overflow from a delta update, yet be wrapped back to the other side with a single bit-mask:

> **1111**0**11111**

This has the decimal value `991`, and is used in the code to ensure locations wrap and remain within the playfield.

The span of the playfield (`s` below) is `64` as this is the difference in the location index when incrementing the vertical position.


## HTML Document

The HTML document is as follows (with `$LOAD` and `$KEYDOWN` as placeholders):

```html
<body/onload="$LOAD"onkeydown=$KEYDOWN><button><canvas/id=c>
```

...which is parsed to have the structure:

```html
<body onload="$LOAD" onkeydown="$KEYDOWN">
  <button>
    <canvas id="c">
```

To avoid whitespace so that it can be used directly as a `data:` URL without escaping, the source uses `/` to separate the first attribute from the element name, and chains the second `onkeydown` attribute directly after the closing `"` (this also saves a byte).  

The `onload` handler is the shortest way to run the script (as `<script>` blocks require a closing tag).  The double quotes are required for this attribute as the greater-than `>` character is used in the body of the script.  

The `onkeydown` handler is a short way to add the required event listener.

The `<button>` element is a short way to provide a border and background to the canvas playing field.

The `<canvas>` element allows drawing the game state.  The default height (150) is used.  The `id` of `c` is directly available to the script as a global.


## Startup

The `$LOAD` code is as follows (with `$UPDATE` as a placeholder):

```javascript
setInterval('$UPDATE',b=[n=f=k=s=64])
```

This causes the `$UPDATE` code to be repeatedly called to move the game state forwards.

The second parameter is the interval, and this is used to initialize multiple variables at once, all to `64`:

* `n` the **n**ext head position
* `f` the **f**ood position
* `k` the direction from the **k**eyboard
* `s` the horizontal **s**pan of the playfield

...and, to an array containing `64`:

* `b` an array containing the position of each part of the **b**ody of the snake.

Only `s` is required to be `64`, the others benefit from a shorter overall length if initialized to the same value.

When `setInterval()` parses the second parameter as a number, the array is first converted to a string, and `[64].toString()` is `64`, so the update code is called every 64 milliseconds.


## Update

The `$UPDATE` code is repeatedly called:

```javascript
c.width=288;[f,...b=[n,...b.includes(n=n+[s,31,-s,1][k&3]&991)?[]:b]].map(i=>c.getContext`2d`.fillRect(i%s*9,(i>>6)*9,8,8));n^f?b.pop():f=~f*89&991
```

This is explained below in three parts.


### Part 1: Clear canvas

```javascript
c.width=288;
```

This accesses the canvas through its `id` as the global variable `c`, and sets its width to `288` (32 columns of 9 pixels wide).  Directly setting the width forces the canvas to be cleared before each rendering step.


### Part 2: Render canvas

The canvas is updated by drawing a block for each required location (with `$LOCATIONS` as a placeholder):

```javascript
$LOCATIONS.map(i=>c.getContext`2d`.fillRect(i%s*9,(i>>6)*9,8,8));
```

For each location `i`, the canvas is accessed through its `id` as the global variable `c`, template literals are abused to call the `getContext()` function with the first parameter set to `2d` (saving a couple of bytes by not requiring the brackets), and the `fillRect` function is called.  The rectangle is drawn with 8x8 pixel dimensions, and the coordinates are calculated as follows:

  * x: `i%s*9` - The location index modulo the span (64), the bottom six bits of the location (the top bit of which will always be zero).  Scaled to a 9 pixel spacing (one pixel of padding between columns).

  * y: `(i>>6)*9` - The location index right-shifted by 6, the top bits of the location.  Scaled to a 9 pixel spacing (one pixel of padding between rows).

`$LOCATIONS` is an array of all of the blocks to render from the game state  (with `$REMAINING_BODY` as a placeholder):

```javascript
[f,...b=[n,...$REMAINING_BODY]]
```

This is the `f` food location, and the contents of the `b` updated body location.  The body location is first updated to the `n` next head location and the `$REMAINING_BODY` with:

1. (With `$NEW_LOCATION` as a placeholder):

    ```javascript
    b.includes(n=$NEW_LOCATION)
    ```

    Check whether the `b` previous snake body parts already includes the next head location `n` (i.e. the snake has run into itself).  The result of this decides which arm of the tertiary expression is evaluated.
    
    The next head location `$NEW_LOCATION` is calculated as:

    ```javascript
    n+[s,31,-s,1][k&3]&991
    ```

    The previous location `n` is updated based on the (`&3` masked) lower two bits of the the `k` direction from the keyboard, used as an index (0-3) into delta location values for down (`s` span), left (adding 31 is, after the masking, effectively -1 with wrap-around), up (negative `s` span), right (1).  The next location is bit-masked with `991` to ensure it wraps around and remains within the playfield (see above for details).


2. ```javascript
    ?[]
    ```

    If the body parts already included the next location, the snake has crashed into itself, and the `$REMAINING_BODY` evaluates to nothing else (the snake will reset to only contain the head).  


3. ```javascript
    :b
    ```

    If the body parts did not include the new location, the `$REMAINING_BODY` evaluates to the remainder of the `b` body parts.  The result is that the `n` next head position is prepended to the body parts, growing them by one segment (for now, this will be trimmed if the food is not eaten).


### Part 3: Update snake and food

```javascript
n^f?b.pop():f=~f*89&991
```

This deals with consuming, or not consuming, food on this update.

1. ```javascript
    n^f
    ```
    The `n` head position is XORed with the `f` food position.  If they are different, the result is non-zero, and the food is not eaten.  If they are the same, the result is zero, and the food is eaten.

2. ```javascript
    ?b.pop()
    ```
    This first part of the ternary operator is evaluated if the food is not eaten.  The `b` body parts array is `pop`ped to remove the oldest segment of the snake, to prevent it growing.

3. ```javascript
    :f=~f*89&991
    ```
    This second part of the ternary operator is evaluated if the food is eaten.  The snake will grow as the oldest body part is not trimmed under this condition.  The `f` food is repositioned to be at an (extremely poor!) psuedorandom location: the current location is `~` bitwise negated then multiplied by 89.  The new location is bit-masked with `991` to ensure it wraps around and remains within the playfield (see above for details).


## Key Handler

The `$KEYDOWN` handler is called whenever a key down event occurs:

```javascript
k^=(z=event.which^k)&1&&z
```

The `event` variable is present, and `event.which` is the scan code of the key that was pressed.  For the arrow keys, the codes are:

* left: 37
* up: 38
* right: 39
* down: 40

`k` is updated to reflect the snake direction from the key pressed.  

This could be accomplished with just `k=event.which`, but the added complication in the code is to help prevent backtracking into the snake's own body (although this can still be defeated if an off-axis key is pressed first within the same time step).

Note that the four scan codes alternate between horizontal and vertical directions, and so the least significant bit depends on the axis: left and right have the least significant bit set, while up and down have the least significant bit cleared.  

The `z` variable is set to the scan code XORed with the current key value.  If the result of this has the least-significant bit set, it is on a different axis to the current movement.  The logical `&&` *and* is shortcircuiting so that, only when the key is in a different axis, the value is used, otherwise this expression evaluates to `false`.  The previous key value is XOR-assigned `^=` with the result, which sets it to the new key value if different (XORing `k` reverses the original XOR with the scan code), or otherwise leaves it the same (`false` causes a no-operation of an XOR with `0`).
