## Attribute selectors
[att] Represents an element with the att attribute, whatever the value of the attribute.

[att=val] Represents an element with the att attribute whose value is exactly "val".

[att~=val] Represents an element with the att attribute whose value is a whitespace-separated list of words, one of which is exactly "val". If "val" contains whitespace, it will never represent anything (since the words are separated by spaces). Also if "val" is the empty string, it will never represent anything.

[att^=val] Represents an element with the att attribute whose value begins with the prefix "val". If "val" is the empty string then the selector does not represent anything.

[att$=val] Represents an element with the att attribute whose value ends with the suffix "val". If "val" is the empty string then the selector does not represent anything.

[att*=val] Represents an element with the att attribute whose value contains at least one instance of the substring "val". If "val" is the empty string then the selector does not represent anything.