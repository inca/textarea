# Zen Area — textarea with inner power

_This is a work in progress. Stay tuned for 1.x.x releases!_

This tiny (~15Kb minified) framework-agnostic library
provides an extensible set of commands along with some fluent API.

Use them to train your textarea into powerful text processing warrior.

Add a few lines of code:

```js
new ZenArea(document.querySelector('textarea'))
  .command('Tab', 'indent')
  .command('Shift + Tab', 'outdent');
```

Congrats, you've just trained your textarea to indent selected lines
with <kbd>Tab</kbd> and remove indentation with <kbd>Shift</kbd>+<kbd>Tab</kbd>.

You can also bind commands to UI (buttons, menus, etc.) and easily create
your own commands.

[Download latest build](https://raw.githubusercontent.com/inca/zenarea/master/build/zenarea.js)

## Key Features

* Vanilla JavaScript, no dependencies
* Compatible with Browserify and RequireJS/AMD
* No custom/shadow elements — only textarea and you

## Install

### Browserify

We highly recommend building front-end with [npm](https://npmjs.org) and
[Browserify](http://browserify.org). Installation is trivial:

```
npm i zenarea
```

```js
var ZenArea = require('zenarea');
```

### AMD/RequireJS

Grab [latest build](https://raw.githubusercontent.com/inca/zenarea/master/build/zenarea.js)
into your project and require it (`ZenArea` class is exposed).

### Script tag

Grab [latest build](https://raw.githubusercontent.com/inca/zenarea/master/build/zenarea.js)
and include it to your page:

```html
<script src="/js/zenarea.js"></script>
```

The `ZenArea` class is now bound to `window`.

## Usage

This library exposes `ZenArea` constructor which accepts a **single** textarea Element:

```
var z = new ZenArea(document.querySelector('textarea'));
```

This instance holds all API methods described [below](#API).

### Binding commands to keyboard

Commands are simply [API methods](#API) defined on `ZenArea.prototype`.
 
You can bind any command to your keyboard like this:

```js
z.command('Ctrl + B', 'surround', ['*', '*', true]);
```

Pressing <kbd>Ctrl</kbd>+<kbd>B</kbd> is now equivalent to

```js
z.surround('*', '*', true)
```

See? It's also pretty easy to [add your own command](#Custom_commands).

By default ZenArea does not bind any command, it's up to you
to teach your textarea your favorite kung fu style. 

### Init / destroy

ZenArea attaches certain events listeners to textarea (e.g. keydown).
Use `z.destroy()` to remove them — this is especially useful in
Single Page Applications to prevent memory leaks.

Once ZenArea has been destroyed it is possible to return it back to its former
glory with `z.init()`.

### Custom commands 

Commands are simply methods defined on `ZenArea.prototype`.
You can add new command to all ZenArea instances like this:

```js
ZenArea.prototype.sayHello = function(name) {
  return this.insetText('Hello, ' + name);
}
```

Here's how you bind this command to <kbd>Ctrl</kbd>+<kbd>H</kbd>:

```
z.command('Ctrl + H', 'sayHello', ['World']);
```

## API

### Selection

* `getSelection()` returns current user selection like this:

    ```js
    {
      start: 10,
      end: 15,
      length: 5,
      value: 'World'
    }
    ```
    
* `setSelection(start[, end])`

* `selectAll()`

* `selectLines(start[, end])`

* `selectCurrentLines()` — expand selection in both directions up to line boundaries

* `selectLeft(predicateFn)` — expand selection to the left until `predicateFn(sel)`
  returns `true`

* `selectRight(predicateFn)`

* `expandSelection()` — expand selection: from caret to word, from word to line,
  from line to everything
  
### Manipulation

* `insertText(text[, preserveSelection])` — replace selected text with specified one;
  passing `true` as second argument will also select this text

* `indent([indentation])` — prepends each current line with specified
  indentation chars (two spaces by default), preserving original selection;
  if nothing is selected, just inserts indentation chars at caret position
   
* `outdent([indentation])` — removes specified indentation chars from current lines,
  preserving original selection

* `surround(prefix, suffix[, toggle])` — surrounds selected text with `prefix`
  and `suffix`; when the third argument is truthy it will detect already surrounded
  text and de-surround it (e.g. strip prefix and suffix instead)
  
### Search & Replace

* `search(regex[, startIndex])` — selects first match of `regex` starting
  at specified `startIndex`
* `searchNext(regex)` - like `search`, but starts at the end of current selection
* `replace(regex, replacement[, startIndex])` — replaces first match of `regex`
* `replaceNext(regex, replacement)` — like `replace`, but starts at the end of
  current selection
* `replaceAll(regex, replacement)` — replaces all matches of `regex` with `replacement`
  (the `g` flag on regex is mandatory, otherwise just replaces first occurrence)

## License

ISC / Boris Okunskiy

