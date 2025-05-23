<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="robots" content="noindex">

<title>Start</title>

<script>
  var config = {
    commands: [
      { key: 'a', name: 'Amazon', url: 'https://www.amazon.com', search: '/s/?field-keywords=' },
      { key: 'd', name: 'Drive', url: 'https://drive.google.com/drive', search: '/search?q=' },
      { key: 'e', name: 'Egghead', url: 'https://egghead.io', search: '/search?q=' },
      { key: 'f', name: 'Facebook', url: 'https://www.facebook.com', search: '/search/top/?q=' },
      { key: 'g', name: 'GitHub', url: 'https://github.com', search: '/search?q=' },
      { key: 'h', name: 'Hacker News', url: 'https://hn.algolia.com', search: '/?query=' },
      { key: 'i', name: 'Inbox', url: 'https://inbox.google.com', search: '/search/' },
      { key: 'I', name: 'Instagram', url: 'https://www.instagram.com' },
      { key: 'k', name: 'Keep', url: 'https://keep.google.com', search: '/#search/text=' },
      { key: 'p', name: 'Product Hunt', url: 'https://www.producthunt.com', search: '/search?q=' },
      { key: 'r', name: 'Reddit', url: 'https://www.reddit.com', search: '/search?q=' },
      { key: 's', name: 'SoundCloud', url: 'https://soundcloud.com', search: '/search?q=' },
      { key: 't', name: 'Twitter', url: 'https://twitter.com', search: '/search?q=' },
      { key: 'u', name: 'Unsplash', url: 'https://unsplash.com', search: '/search?keyword=' },
      { key: 'y', name: 'YouTube', url: 'https://www.youtube.com', search: '/weather?search_query=' },
      { key: 'Y', name: 'YTS', url: 'https://yts.ag', search: '/browse-movies/' },
      { key: '8', name: '0.0.0.0:8080', url: 'http://0.0.0.0:8080' }
    ],

    // if none of the keys are matched, this is triggered
    // for Duck Duck Go use: https://duckduckgo.com/?q=
    defaultCommand: 'https://www.google.com/search?q=',

    // the delimiter between the key and your search query
    // e.g. to search GitHub for potatoes you'd type "g:potatoes"
    searchDelimiter: ':'
  };
</script>

<script>WebFontConfig={google:{families:['Lato:300:latin']}};</script>
<script src="https://ajax.googleapis.com/ajax/libs/webfont/1/webfont.js" async></script>

<style type="text/css">
  body {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: #303641;
    margin: 0;
    padding: 0;
    color: #C0C5CE;
    font-family: 'Lato', sans-serif;
    font-weight: 300;
  }

  main {
    position: absolute;
    top: 50%;
    right: 0;
    left: 0;
    width: 90%;
    max-width: 310px;
    margin: 0 auto;
    transform: translateY(-100px);
    text-align: center;
  }

  time {
    display: block;
    margin-bottom: 20px;
    font-size: 5rem;
    letter-spacing: 6px;
  }

  input,
  input:focus {
    margin: 0;
    border: 0;
    outline: 0;
    -webkit-appearance: none;
    -moz-appearance: none;
  }

  input {
    box-sizing: border-box;
    width: 100%;
    padding: 12px;
    transition: 0.5s;
    border-radius: 2px;
    background-color: #4F5B66;
    color: #fff;
    font-family: 'Lato', sans-serif;
    font-size: 1.1rem;
  }

  aside {
    position: fixed;
    box-sizing: border-box;
    left: 0;
    width: 100%;
    max-width: 190px;
    height: 100%;
    padding: 15px 0 0;
    transition: transform 700ms;
    transform: translateX(-205px);
    background-color: #fff;
    box-shadow: 0 0 15px 0 rgba(0, 0, 0, 0.2);
    overflow: auto;
    z-index: 1;
  }

  aside[data-toggled='true'] {
    transform: translateX(0);
  }

  h1 {
    margin: 0 25px 15px;
    line-height: 1rem;
  }

  ul {
    margin: 0 0 15px;
    padding: 0;
  }

  li {
    list-style: none;
  }

  a {
    display: block;
    padding: 0 25px;
    color: #222;
    line-height: 1.7rem;
    text-decoration: none;
  }

  a:hover .help-name {
    text-decoration: underline;
  }

  .help-key {
    font-family: 'Courier New', monospace;
  }

  .help-name {
    font-size: 0.9rem;
  }
</style>

<main>
  <time id="js-clock"></time>
  <form id="js-search-form" autocomplete="off">
    <input id="js-search-input" type="text" autofocus>
  </form>
</main>

<aside id="js-sidebar">
  <h1>~</h1>
  <ul id="js-help"></ul>
</aside>

<script>
  'use strict';

  function $(s) {
    return document.querySelector(s);
  };

  var Clock = (function() {
    var clock = $('#js-clock');

    function pad(num) {
      return ('0' + num.toString()).slice(-2);
    }

    function setTime() {
      var date = new Date();
      var hours = date.getHours();
      if (hours > 12) {
          hours -= 12;
      } else if (hours === 0) {
         hours = 12;
      }
      clock.innerHTML = pad(hours) + ' ' + pad(date.getMinutes());
    }

    setTime();
    setInterval(setTime, 1000);
  })();

  var Help = (function(config) {
    var head = $('head');
    var sidebar = $('#js-sidebar');
    var searchHelp = $('#js-help');

    config.commands.forEach(function(command) {
      head.insertAdjacentHTML(
        'beforeend',
        '<link rel="prerender" href="' + command.url + '">'
      );

      searchHelp.insertAdjacentHTML(
        'beforeend',
        '<li><a href="' + command.url + '">' +
          '<span class="help-key">' +
            command.key + config.searchDelimiter + ' ' +
          '</span>' +
          '<span class="help-name">' +
            command.name +
          '</span>' +
        '</a></li>'
      );
    });

    document.addEventListener('keydown', function(event) {
      if (event.keyCode === 27) sidebar.removeAttribute('data-toggled');
    });

    return {
      toggle: function() {
        var toggle = sidebar.getAttribute('data-toggled') !== 'true';
        sidebar.setAttribute('data-toggled', toggle);
      }
    };
  })(config);

  var Form = (function(config) {
    var searchForm = $('#js-search-form');
    var searchInput = $('#js-search-input');
    var urlRegex = /(\b(https?|file):\/\/[-A-Z0-9+&@#\/%?=~_|!:,.;]*[-A-Z0-9+&@#\/%=~_|])/i;

    searchForm.addEventListener('submit', function(event) {
      event.preventDefault();

      var q = searchInput.value.trim();
      var qSplit = q.split(config.searchDelimiter);
      var validCommand = false;
      var redirect = '';

      if (q === '' || q === '?') {
        Help.toggle();
        searchInput.value = '';
        return false;
      }

      redirect = q.match(new RegExp(urlRegex)) ? q
        : config.defaultCommand + encodeURIComponent(q);

      config.commands.forEach(function(command) {
        if (qSplit[0] === command.key) {
          if (qSplit[1] && command.search) {
            qSplit.shift();
            var search = encodeURIComponent(qSplit.join(config.searchDelimiter).trim());
            redirect = command.url + command.search + search;
          } else {
            redirect = command.url;
          }
        }
      });

      window.location.href = redirect;
    }, false);

    return { searchInput: searchInput };
  })(config);
</script>
