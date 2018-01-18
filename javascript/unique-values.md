# Unique value from array with `Set` and spread operator

ES2015 Set() and ...rest operator can be used to discard duplicate values.

```javascript
const uniqueValues = (arr) => [...new Set(arr)];

uniqueValues([1, 2, 2, 3, 4, 4, 4, 5, 5, 5, 5, 6]);
// [1, 2, 3, 4, 5, 6]

uniqueValues(['Today', 'Today', 'I', 'I', 'I', 'Learned']);
// ['Today', 'I', 'Learned']
```

Source: <https://twitter.com/addyosmani/status/952805052086824960>