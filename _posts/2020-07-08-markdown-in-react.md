---
layout: post
title: 'Rendering Markdown with React'
date: 2020-07-08
categories: react markdown
---

Markdown (MD) is the Rich Text Format of developers. With it we can put on a screen our ideas in a visual way without having to rely on other, more complicated, markup languages.

I want to explore the process to insert markdown on React components and maybe go further each time.

## Limitations of Markdown

First, let's get this out of the way: Markdown limits itself to text and image related content. That means it's only capable of handling headings (`h1`...`h6`), text, lists, links and images. Any other elements like `main`, `section`, `div` is out of MD's scope.

## Simple rendering

Here, I want to render text like this:

```markdown
# Hello World!

This is Markdown.
```

And have it render like this in HTML:

```html
<h1>Hello World!</h1>
<p>This is Markdown.</p>
```

Fortunately, there's already a few libraries that can do this: [react-markdown](https://github.com/rexxars/react-markdown), [react-markdown-renderer](https://github.com/InsidersByte/react-markdown-renderer) and [react-mde](https://github.com/andrerpena/react-mde).

I will use react-markdown for this example.

```jsx
import React from 'react';
import ReactMarkdown from 'react-markdown';
import './styles.css';

const input = `
# Hello World!

This is Markdown.
`;

export default function App() {
  return (
    <div className='App'>
      <ReactMarkdown source={input} />
    </div>
  );
}
```

And sure enough, that works! Here is the result in [Codesandbox](https://codesandbox.io/s/react-markdown-simple-demo-9tbb8?file=/src/App.js).

That was easy. Now for something a little more complicated.

## Rendering from a file

Is it as easy as importing the MD file from our JSX file?
It is as easy as importing the MD file from our JSX file!

Using react-markdown, for its simplicity, again.

```jsx
import React from 'react';
import ReactMarkdown from 'react-markdown';
import HelloWorld from './hello-world.md';
import './styles.css';

export default function App() {
  return (
    <div className='App'>
      <ReactMarkdown source={HelloWorld} />
    </div>
  );
}
```

Here is the demo in [Codesandbox](https://codesandbox.io/s/react-markdown-from-file-demo-hni2h) again. It is even neater than the previous example.
Now, something even more difficult.

## Rendering multiple files

Finally, I want to read multiple files at the same time and output their render as before.

In addition, I want to be able to output each file on it's own with a link.

This is done with a router. Fortunately, a [react-router](https://github.com/ReactTraining/react-router) exists! Even more so, I'm able to achieve what I wanted just reading at the first example in the page, which is excellent news.

Also, Node has a module called `fs` that allows to read the [file system](https://nodejs.org/api/fs.html), in this case our app environment. And it's as easy as a couple of lines.

Assuming I have put my file on a a `/post` folder, I can execute this code.

```jsx
const path = './src/posts/'; // path for the MD files folder within our project
const fs = require('fs');
const fileList = fs.readdirSync('./src/posts'); // creates an array of filenames
```

Then, I want to read the data contents, so I create a function for this.

```jsx
function getData(file) {
  const data = fs.readFileSync(file, 'utf8');
  return data ? data : null;
}
```

This function takes a file and returns its content as a string. I have to declare encoding (`"utf-8"`) or else I get an array of number, which is the characters values in ASCII.

Now I can create my app.

```jsx
export default function App() {
  return (
    <div className='App'>
      <Router>
        <ul>
          {fileList.map((post) => (
            <li>
              <Link to={`/${post}`}>{post}</Link>
            </li>
          ))}
        </ul>

        <Switch>
          {fileList.map((post) => (
            <Route path={`/${post}`}>
              <ReactMarkdown source={getData(`${path}${post}`)} />
            </Route>
          ))}
        </Switch>
      </Router>
    </div>
  );
}
```

I have surround all my code on the `<Router>` component for it to work and create separate `.map` functions for the link list and the actual routing. The `<Switch>` component takes care of rendering the text formatted from markdown. I can't do it in one fucntion because it seems that the `<Switch>` can't have the links inside of it.

The result, in [Codesandbox](https://codesandbox.io/s/react-markdown-multiple-files-mdhry), is a bit hacky since it uses the file name as the link text.

But I'm satisfied that I could get the result I was looking for with relative ease.
All I'm left with is to explore how to read certain lines from a file to extract the title and fix the link text.

I will do that in a later post.
