---
title:        Cache Length During Loops
level:        intermediate
---

In a for loop, don't access the length property of an array every time; cache
it beforehand.

```
var myLength = myArray.length;

for (var i = 0; i < myLength; i++) {
  // do stuff
}
```
