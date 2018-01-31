# Component Interaction

Demo: https://stackblitz.com/edit/hostbinding-hostlistener-input

## Parent to child: `@Input`

use `@Input` properties to get data from the parent component

### Detecting changes in input

`ngOnChanges` is a lifecycle hook that is called when any data-bound property of a component changes.

```typescript
import {SimpleChange} from '@angular/core';

ngOnChanges(change: SimpleChange) {
    // do something
}
```

### When angular doesn't detect changes

Angular's change detection mechanism only checks for changes in object references, so if the object properties have been modified but the object reference remains the same, then it won't be detected as a change.


