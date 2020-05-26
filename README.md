# quark

Quark takes a folder with HTML/CSS/script/image resources and produces monolithic executable ready to run “as it is”:

![](http://quark.sciter.com/wp-content/uploads/2020/05/schema.png)

[Quark is an application compiled by itself.](http://quark.sciter.com/wp-content/uploads/2020/05/quark-ui.png)

Application produced by the Quark is a bundle that contains Sciter Engine and custom resources that describe UI and logic of the application. Therefore minimal size of resulting binary is **5+ mb** (not compressed) that is comparable with minimal “hello world” applications using other popular UI libraries and frameworks:

* Qt – 5+ mb, but if your application needs to render HTML (QtWebKit) then the size will be around 30mb;
* Electron – 50+ mb folder with a lot of stuff inside, 200+ mb (at least) of RAM and 2 processes (at least);
* wxWidgets (linked statically) 3+ mb at least.

## Sciter Engine Runtime

So what is inside of those 5mb? What functionality is available out of the box?

* 
  * **Graphics**:  GPU Accelerated Graphic backends (DirectX, OpenGL) on all platforms. Supports as retained as immediate (a la ImGUI) rendering  modes.
  * **HTML**: parser and DOM that supports as all HTML5 constructs as extended set of input elements including:
    * `<include src="..." media="...">` – client side includes to allow assembling HTML from multiple files;
    * `<htmlarea>` – native implementation of HTML WYSIWYG editor;
    * `<plaintext>` – multiline plaintext editor with native support of syntax highlighting;
    * `<frameset>` – can contain arbitrary content (not just <frame>s) – essentially that is so called split-view available out of the box.
    * `<frame type=pager>` – customizable print preview and print modules;
    * `<select type="tree">` and other practical variations of standard  HTML input elements;
    * `<popup>`, `<menu.context>` and `<menu.popup>` – elements – out-of-canvas DOM element rendering;
    * extra HTML attributes to support desktop UI cases including decoration of windows, dialog and message boxes.
  * **CSS** – supports full implementation of CSS 2.1 plus some useful CSS3 modules:
    * transitions and animations;
    * [flexes and grids](https://terrainformatica.com/w3/flex-layout/flex-vs-flexbox.htm), with different syntax more suitable for UI though, but, still, functionality is there;
    * CSS syntax extended by SaSS alike constructs `@const`, `@mixin`, `@set` and CSS variables.
    * [CSS resident vector images](https://sciter.com/lightweight-inline-vector-images-in-sciter/) – no need for separate FontAwesome and similar icon fonts.
  * **Script** –  Sciter uses its own JS derived script engine with syntax extended to better suit UI needs:
    * extended set of types on top of 4 basic JS value types: Integer, Float, Duration, Length, Tuple and so on;
    * real `class`es and `namespace`s, symbols;
    * `generator`, `await`, `async` and `event` syntax constructs and promises;
    * built-in data persistence – **NoSQL database** (a la MongoDB) integrated into the language;
    * Sciter contains and uses **libUV** for asynchronous I/O so pretty much all functionality of NodeJS is available: file I/O, file and directory monitors, Process execution objects with stdio/stderr/stdin hooks.
    * [Reactor and SSX](https://sciter.com/developers/sciter-docs/reactor-and-ssx/) – Sciter contains native implementation of ReactJS and JSX built-in into script compiler.
  * [Desktop UI and **windowing**](https://sciter.com/html-window/):
    * `view.window(html)`, `view.dialog(html)` and `view.msgbox(html)` functions – creation of native desktop windows and real modal dialogs.
    * `view.trayIcon()` – built-in support of tray icons;
    * `view.windowBlurbehind` – support of acrylic and behind-window blending on Windows and Macs;
    * `view.windowFrame` – standard and custom windows frames including layered/non-rectangular windows;

## Addons and native extensions

If the above functionality is not enough then Quark (Sciter’s runtime in fact) supports loading of custom modules from custom DLL’s (.dll,.dylib,.so). You can design and use:

* Native script classes  and namespaces, for example SQLite can be used from plug-in dll;
* Native custom DOM elements and input elements (a.k.a. native behaviors): DLL-resident DOM element implementations can handle events and do native painting using  [sciter::graphics API](https://github.com/c-smile/sciter-sdk/blob/master/include/sciter-x-graphics.hpp#L332) ;
* Native DOM elements wrapping of existing HWND, NSView, GtkWidget based components

