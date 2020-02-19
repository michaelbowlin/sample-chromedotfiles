# Setup

Download and install the [Chrome dotfiles](https://github.com/diffsky/chromedotfiles) extension.

In Chrome go to `chrome://extensions` and make sure the *developer mode* checkbox is checked. Then load the extension from wherever you've saved it.

Inside the extension directory create a folder called `chromedotfiles` and put in the JavaScript file below.

Add the following code to `qch2o.caliberdirect.com.js`:

```javascript
(function() {
  const socket = new WebSocket('ws://127.0.0.1:8888/ws');
  const { body, head } = document;

  function createLink(url) {
    let link = document.createElement('link');
    link.type = 'text/css';
    link.rel = 'stylesheet';
    link.href = url;

    return link;
  }

  function createScript(url) {
    let script = document.createElement('script');
    script.type = 'text/javascript';
    script.src = url;

    return script;
  }

  let fonts = createLink('https://fonts.googleapis.com/css?family=Material+Icons|Fira+Sans:300,300i,500,500i|Open+Sans:400,400i,600,600i');
  let styles = createLink('http://127.0.0.1:8888/index.css');
  let script = createScript('http://127.0.0.1:8888/overrides.js');

  head.append(fonts);
  head.append(styles);
  body.append(script);

  function update() {
    head.removeChild(styles);
    head.append(styles);
  }

  socket.onmessage = function(msg) {
    if (msg.data === 'reload') {
      window.location.reload();
    } else if (msg.data === 'refreshcss') {
      update();
    }
  };

  const slug = location.href.split('/').pop().replace(/\.[^/.]+$/, '');

  if (body.classList) {
    body.classList.add(slug);
  } else {
    body.className += ' ' + slug;
  }
}.call(this));
```

If first time, run install:

```bash
$ npm install
```

Run the project:

```bash
$ npm start
```
