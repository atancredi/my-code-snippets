# Javascript operators

## Non-null assertion operator (!)
The ! after tells TypeScript that the property is definitely not null or undefined.
This is useful when TypeScript thinks a value might be null or undefined, but you as a developer are certain that it won't be at runtime.

## Optional chaining operator (?)
The ? means optional chaining.
If the checked property is null or undefined the entire expression returns undefined instead of throwing an error.