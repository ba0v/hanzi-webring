# hanzi webring

a webring exclusive to sites with a chinese-character (hanzi) domain name. any TLD, as long as the domain itself uses hanzi.

## requirements

- your site's domain name must contain Chinese characters (hanzi). any TLD is fine.
- your site should be live and reasonably stable (no permanent "under construction" pages).

## how to join

1. fork this repo
2. add your entry to `members.json`:
   ```json
   { "name": "your-site-name", "url": "https://your-domain" }
   ```
3. open a pull request

once merged, your site is part of the ring. the prev / next links on every member's webring widget will include you.

## adding the widget to your site

drop this into your page. it's self-contained — style the `#webring` selectors however fits your site, or leave the defaults.

```html
<nav id="webring" aria-label="webring navigation">
  <span>hanzi webring</span>
  <a id="webring-prev" href="#">&larr;</a>
  <a id="webring-next" href="#">&rarr;</a>
</nav>

<style>
  #webring { display: flex; align-items: center; gap: 0.6rem; font-family: sans-serif; font-size: 0.95rem; }
  #webring a { text-decoration: none; color: inherit; opacity: 0.6; }
  #webring a:hover { opacity: 1; }
</style>

<script>
  fetch("https://raw.githubusercontent.com/ba0v/hanzi-webring/main/members.json")
    .then((res) => res.json())
    .then((members) => {
      const self = "your-site-name"; // must match your "name" in members.json
      const i = members.findIndex((m) => m.name === self);
      document.getElementById("webring-prev").href = members[(i - 1 + members.length) % members.length].url;
      document.getElementById("webring-next").href = members[(i + 1) % members.length].url;
    });
</script>
```

replace `your-site-name` with the exact `"name"` you used in your `members.json` entry.

## members

see [`members.json`](./members.json) for the current list.
