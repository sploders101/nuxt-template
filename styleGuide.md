# Style Guide
This is a style guide for all the things that tslint doesn't know how to check for. Any edits must follow this guide to ensure readability, terseness & git-friendliness.






## Documentation
Try to use JSDoc whenever possible. Verbosity is not required, but the default parameters inserted by VSCode when using the JSDoc snippet should be filled in. Typescript will handle type documentation, so you don't need to worry about that. In fact, please don't because it is just more to visually parse.

### Note on commas
Despite whether you think it is necessary or not, please always use the oxford (or serial) comma. At worst, it's an extra button press, and at best, it can clarify between multiple (potentially drastically different) possible meanings. As a rule of thumb, if you have to think about whether or not it's necessary, it is. This will save a lot of headache in the future. Due to this rule, it is safe to assume meanings based on its absence.



## Imports
Imports must follow one of these three forms.

### Default import
Importing the default export
```typescript
import defaultImport from "module";
```

### Single import
Importing a single non-default export. If, however, it is likely that you will add more imports later, use the `Multiple Imports` syntax
```typescript
import { onlyImport } from "module";
```
#### Rationale
This is shorter than the long version, which can result in considerably shorter import sections, and git can usually pick up on this change.

User1:
```diff
-import { onlyImport } from "module";
+import {
+	onlyImport,
+	import2,
+} from "module";
```

User2:
```diff
-import { onlyImport } from "module";
+import {
+	onlyImport,
+	import3,
+} from "module";
```

Merged:
Lines 1, 2, and 4 are matched between both revisions, leaving only import2 and import3 in question. If a merge conflict should arise, it is a simple concat, because order does not matter.
```diff
-import { onlyImport } from "module";
+import {
+	onlyImport,
+	import2,
+	import3,
+} from "module";
```
**or**
```diff
-import { onlyImport } from "module";
+import {
+	onlyImport,
+	import3,
+	import2,
+} from "module";
```

### Multiple imports
Importing multiple non-default exports
```typescript
import {
	import1,
	import2,
} from "module";
```

### Default import with other imports
If you need to import the default import along with any other exported variable, you **must** use the "multiple imports" method to ensure git-friendliness
```typescript
import defaultImport, {
	anyOtherImport,
} from "module";
```



## Boolean logic
If boolean logic is not trivial and unlikely to change, you **must** follow this format, as it has the potential to make the code more readable, and greatly improves git-friendliness.

### AND
```typescript
const varWithAnd = (true
	&& condition1
	&& condition2
	&& more
);
```

### OR
```typescript
const varWithOr = (false
	|| condition1
	|| condition2
	|| more
);
```

### Complex example
This is a complex example that combines the former two concepts into one acceptable statement.

* Possible conditions for `varWithComplexLogic === true`
	* **These** variables **must** be true
		* requiredCondition1
		* requiredCondition2
	* **These** variables **must** be true
		* requiredCondition3
		* requiredCondition4

```typescript
const varWithComplexLogic = (false
	|| (true
		&& requiredCondition1
		&& requiredCondition2
	)
	|| (true
		&& requiredCondition3
		&& requiredCondition4
	)
);
```



## Ternary Operator
```typescript
const conditionalVar = condition ?
	valueIfConditionIsTrue :
	valueIfConditionIsFalse;
```

### Rationale
The syntax of ternary operators does not allow for git-friendly formatting

Formatters will often try to format code like this:
```typescript
const conditionalVar = condition
	? valueIfConditionIsTrue
	: valueIfConditionIsFalse
```

Upon first glance, this may look nicer, but it does not flow well when reading. when I see it, I see `const conditionalVar = condition`, and then the next lines give me a brief hesitation while I take a moment to realize that this is a ternary statement.

Using **this** method, the `?` and `:` hint to the reader that the statement is incomplete, and therefore, context will be provided on the next line. It is a visual cue to interperet all three lines as one statement
