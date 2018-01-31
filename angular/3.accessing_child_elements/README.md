# Accessing Child elements

There are 2 types of children that a component could have,

1. `ViewChild` / `ViewChildren` - elements used within the template
2. `ContentChild` / `ContentChildren` - elements indirectly used within the template (using Content Projection, etc)

## `ViewChild` & `ViewChildren`

Demo: https://stackblitz.com/edit/ttt-view-child-vbmwxf

**child.component.ts**

```typescript
@Component({
  selector: 'app-child',
  template: '<p><b>Child: </b>{{value}}</p>',
})
export class ChildComponent {
  public value: string;
}
```

**app.component.html**

```html
<app-child></app-child>
```

**app.component.ts**

```typescript
export class AppComponent implements AfterViewInit {
    @ViewChild(ChildComponent) child: ChildComponent;
    public ngAfterViewInit(): void {
        this.child.value = "something";
    }
}

```

When `ViewChild` is provided a class selector, the first instance of the selector is selected from the DOM

```html
<app-child></app-child>
<app-child></app-child>
```

```typescript
@ViewChild(ChildComponent) child: ChildComponent; // first child is selected
```

To get all the elements, use `ViewChildren`

```typescript
@ViewChildren(ChildComponent) children: ChildComponent[]; // all children
```


## `ContentChild` & `ContentChildren`

Content children are the elements which are dynamically added to the template using methods like content projection.

You can use `ContentChild` similar to `ViewChild` except that, you won't be able to refer to instances using the `#ref` notation

The angular lifecycle hook `AfterContentInit: ngAfterContentInit()` could be used to know about the initialization status of content children.