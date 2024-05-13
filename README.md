# Vanilla SPA router

A client-side router for vanilla JavaScript projects. It allows navigating between pages without reloading.

## Usage

### Define a router

```javascript
const router = new Router({
  updateTitle: (title) => `${title} | My Website`
});
```

### Update an outlet element upon navigation

```javascript
const routerOutlet = document.getElementById("router-outlet");
router.onComponentUpdate((component) => {
  routerOutlet.replaceChildren(component);
});
```

### Handle anchor links

```javascript
document.addEventListener("click", (e) => {
  const { target } = e;

  if (!(target instanceof HTMLAnchorElement) || !("internal" in target.dataset))
    return;

  e.preventDefault();
  router.navigate(target.getAttribute("href"));
});
```

### Define routes

```javascript
router
  .setRoute("/", (_, response) => {
    const anchor = document.createElement("a");
    anchor.innerText = "Go to profile";
    anchor.href = "/profile/1";
    anchor.dataset.internal = "";
    response
      .setTitle("Home Page")
      .setComponent(anchor);
  })
  .setRoute("/profile/:id", (request, response) => {
    const anchor = document.createElement("a");
    anchor.innerText = "Go home";
    anchor.href = "/";
    anchor.dataset.internal = "";
    response
      .setTitle(`Profile ${request.params.id}`)
      .setComponent(anchor);
  })
  .setRoute("*", (request, response) => {
    const heading = document.createElement("h1");
    heading.innerText = `Cannot get ${request.pathname}`;
    response
      .setTitle("Page not found")
      .setComponent(heading);
  });
```

### Initialize routing

```javascript
router.start();
```
