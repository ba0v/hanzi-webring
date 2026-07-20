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

once merged, your site is part of the ring — the prev / next / random links on every member's webring widget will include you.

## adding the widget to your site

drop this into your page. it's self-contained — style the `#webring` selectors however fits your site, or leave the defaults.

```html
<nav id="webring" aria-label="webring navigation">
  <span id="webring-label">hanzi webring</span>
  <div id="webring-links">
    <a id="webring-prev" href="#">&larr;</a>
    <a id="webring-random" href="#">random</a>
    <a id="webring-next" href="#">&rarr;</a>
  </div>
</nav>

<style>
  #webring {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.3rem;
    font-family: sans-serif;
    font-size: 0.95rem;
  }
  #webring-links {
    display: flex;
    align-items: center;
    gap: 0.6rem;
  }
  #webring a {
    text-decoration: none;
    color: inherit;
    opacity: 0.6;
  }
  #webring a:hover {
    opacity: 1;
  }
</style>

<script>
  (function () {
    const MEMBERS_URL = "https://raw.githubusercontent.com/ba0v/hanzi-webring/main/members.json";
    const SELF = "your-site-name"; // must match your "name" in members.json

    fetch(MEMBERS_URL)
      .then((res) => res.json())
      .then((members) => {
        const prevLink = document.getElementById("webring-prev");
        const nextLink = document.getElementById("webring-next");
        const randomLink = document.getElementById("webring-random");
        if (!prevLink || !nextLink || !randomLink) return;

        const currentIndex = members.findIndex((m) => m.name === SELF);
        const prevIndex = (currentIndex - 1 + members.length) % members.length;
        const nextIndex = (currentIndex + 1) % members.length;

        prevLink.href = members[prevIndex].url;
        nextLink.href = members[nextIndex].url;

        randomLink.addEventListener("click", (e) => {
          e.preventDefault();
          const others = members.filter((_, i) => i !== currentIndex);
          const pick = others.length
            ? others[Math.floor(Math.random() * others.length)]
            : members[currentIndex];
          window.location.href = pick.url;
        });
      });
  })();
</script>
```

replace `your-site-name` with the exact `"name"` you used in your `members.json` entry.

## members

see [`members.json`](./members.json) for the current list.
