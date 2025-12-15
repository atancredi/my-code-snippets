# Javascript operators

## Non-null assertion operator (!)
The ! after tells TypeScript that the property is definitely not null or undefined.
This is useful when TypeScript thinks a value might be null or undefined, but you as a developer are certain that it won't be at runtime.

## Optional chaining operator (?)
The ? means optional chaining.
If the checked property is null or undefined the entire expression returns undefined instead of throwing an error.

## Spread operator (...)
Expands an iterable into a specified receiver.

#### 1. Copy all or part of an existing array or object into another array or object
```js
const numbersOne = [1, 2, 3];
const numbersTwo = [4, 5, 6];
const numbersCombined = [...numbersOne, ...numbersTwo];
```
#### 2. Assign the first and second items from numbers to variables and put the rest in an array
```js
const numbers = [1, 2, 3, 4, 5, 6];
const [one, two, ...rest] = numbers;
```

#### 3. Update properties of an object
```js
const myVehicle = {
  brand: 'Ford',
  model: 'Mustang',
  color: 'red'
}

const updateMyVehicle = {
  type: 'car',
  year: 2021, 
  color: 'yellow'
}

const myUpdatedVehicle = {...myVehicle, ...updateMyVehicle}
```

#### 4. Expand an object with new properties
```js
let d = {
	"1": "aa",
	"2": "bb"
}

let d_extended = {...d, "3":"cc"}
// {"1":"aa","2":"bb","3":"cc"}
```

## Nullish coalescing operator (??=)
The Nullish Coalescing Assignment operator (??=) assigns the value on the right to the variable on the left only if the left variable is currently null or undefined. Otherwise, the variable keeps its existing value.

Think of it as a shortcut for: `variable = variable ?? value;`

If `variable` is `null` or `undefined`, it assigns `value` to `variable`.

If `value` has any other value (including false, 0, or an empty string ""), it remains unchanged.
