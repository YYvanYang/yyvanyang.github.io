---
layout: default
---

### js

```js

//custom shape
class OneNote extends mojs.CustomShape {
  getShape () { return `<path d="M18.709
	...
"/>`; }
}
mojs.addShape( 'oneNote', OneNote ); 

const note1 = new mojs.ShapeSwirl({
  shape:    'oneNote',
  ...
});

```

### css

```css

.hello-world {
  width: 100%;
}

```

### html

```html

<h1 id="code-c-on-a-mac-with-visual-studio-code-and-net-core">Code C# on a Mac with Visual Studio Code and .NET Core</h1>

```

