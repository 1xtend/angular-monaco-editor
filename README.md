# Monaco Editor for Angular 17+

This library wraps the well-known `monaco-editor` for easy use in Angular 17+. Feel free to contribute. Let's make it better!

`It only supports Angular 17 and above versions.`

## ⚡ Setup

### Install required dependencies

```
npm i monaco-editor @1xtend/ng-monaco-editor
```

### Import monaco-editor assets

Your application must have access to `monaco-editor` assets to work properly. To provide this access, you need to make the following changes to the `angular.json` file.

```typescript
{
  "assets": [
    "src/favicon.ico",
    "src/assets",
    {
      "glob": "**/*",
      "input": "node_modules/monaco-editor/min",
      "output": "assets/monaco-editor/min"
    }
  ],
}
```

### Provide config

You need to define and provide a `NG_MONACO_EDITOR_CONFIG` in your `app.config.ts`.

```typescript
const config: NgMonacoEditorConfig = {
  /**
   * Optional.
   *
   * Base URL to monaco-editor assets via AMD. By default it's "/assets", but you can change the path to assets in previous step.
   */
  baseUrl: "/lib/assets",

  /**
   * Optional.
   *
   * Provide editor default options.
   */
  defaultOptions: {
    theme: "vs-dark",
    fontSize: 20,
  },

  /**
   * Optional.
   *
   * This function is called when monaco is loaded.
   */
  onMonacoLoad: () => {
    /**
     * You can use it to set various settings.
     */
    monaco.languages.typescript.typescriptDefaults.setDiagnosticsOptions({
      noSyntaxValidation: true,
      noSemanticValidation: true,
    });

    console.log("monaco has been loaded successfully!");
  },

  /**
   * Optional.
   *
   * The interval for auto re-layout. Defaul value is 100ms.
   */
  autoLayoutInterval: 500,
};

export const appConfig: ApplicationConfig = {
  providers: [
    {
      provide: NG_MONACO_EDITOR_CONFIG,
      useValue: config,
    },
  ],
};
```

## ✨ Usage

For both `ng-monaco-editor` and `ng-monaco-diff-editor` you need to create options. You can use `NgEditorOptions` interface.

```typescript
@Component({
  selector: "my-editors",
  standalone: true,
  imports: [MonacoEditorComponent, MonacoDiffEditorComponent],
})
export class MyEditor {
  options: NgEditorOptions = {
    language: "typescript",
    theme: "vs-dark",
  };

  diffOptions: NgEditorOptions = {
    language: "html",
  };
}
```

```html
<ng-monaco-editor [options]="options"></ng-monaco-editor>
```

```html
<ng-monaco-diff-editor [options]="diffOptions"></ng-monaco-diff-editor>
```

You can use both `Reactive` and `Template Driven` forms to control editors value.

### Reactive forms example:

```typescript
@Component({
  selector: "my-editor",
  standalone: true,
  imports: [MonacoEditorComponent, ReactiveFormsModule],
})
export class MyEditor {
  options: NgEditorOptions = {
    language: "typescript",
    theme: "vs-dark",
  };
  valueControl = new FormControl("const age: number = 100");
}
```

```html
<ng-monaco-editor [options]="options" [formControl]="valueControl"></ng-monaco-editor>
```

### Template Driven forms example:

```typescript
@Component({
  selector: "my-editor",
  standalone: true,
  imports: [MonacoEditorComponent, FormsModule],
})
export class MyEditor {
  options: NgEditorOptions = {
    language: "typescript",
    theme: "vs-dark",
  };
  value: string = "const age: number = 100";
}
```

```html
<ng-monaco-editor [options]="options" [(ngModel)]="value"></ng-monaco-editor>
```

Additionally `ng-monaco-diff-editor` has `originalValue` input to display its original value.

```typescript
@Component({
  selector: "my-editor",
  standalone: true,
  imports: [MonacoDiffEditorComponent, FormsModule],
})
export class MyEditor {
  options: NgEditorOptions = {
    language: "typescript",
    theme: "vs-dark",
  };
  value: string = "const age: number = 100";
  originalValue: string = "const age: number = 99";
}
```

```html
<ng-monaco-diff-editor [options]="options" [(ngModel)]="value" [originalValue]="originalValue"></ng-monaco-diff-editor>
```

## 🎨 Styling

Both `ng-monaco-editor` and `ng-monaco-diff-editor` have the same `ng-monaco-editor-wrapper` class. You can style component by using `::ng-deep`.

```html
<ng-monaco-editor [options]="options" class="my-editor"></ng-monaco-editor>
```

```css
.my-editor ::ng-deep .ng-monaco-editor-wrapper {
  display: block;
  width: 50%;
  border: 1px solid red;
}
```

Also, editors have their own classes like `ng-monaco-editor` and `ng-monaco-diff-editor`

## 🔗 Useful links

- [Monaco Editor Playground](https://microsoft.github.io/monaco-editor/playground.html)
- [Editor Options](https://microsoft.github.io/monaco-editor/typedoc/variables/editor.EditorOptions.html)

## 📃 License

[MIT](https://github.com/1xtend/ng-monaco-editor/blob/master/license) @[1xtend](https://github.com/1xtend)
