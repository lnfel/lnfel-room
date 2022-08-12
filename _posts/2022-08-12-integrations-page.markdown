---
title: UI changes and integrations page
date: 2022-08-12 09:14:00 PM
categories: [Vue, Bootstrap, Element-UI, jQuery]
tags: [enhancement, ux, ui]
img_path: /assets/imgs/
image:
    path: integrations-page-mock-up.png
    width: 674
    height: 340
    alt: Integration Page Mock-up
---

# Prolouge: Navigation UI Enhancement

Creating a new page for the website is easy. What's difficult is where to insert the navigation element for the new section. I dove into the existing navigation component to see where to put the new link.

## How to override an override? The answer: you don't

The navigation component is a **customized** bootstrap navbar, it collapses on smaller screens and expands into a horizontal menu on large screens and up. What I mean by customized is that the existing css is littered with overrides to bootstrap default layout using the most horrendous css feature of all, the `!important` keyword specifier.

I removed some of `!important` declarations so I could specify new css properties without overriding the override. C in CSS meant **cascading** and it was named after it for a reason.

## The curse of legacy from the most successful JavaScript framework of all time

On the JavaScript side of things, I ought to face an inevitable storm. **Vue 2** is the main client for handling interactivity on the webpage alognside with familiar names I've known for the past 2 decade. 

The navigation menu is awesome, interaction is enhanced by smooth scrolling to page sections using **jQuery** along with Bootstrap **Scrollspy**.

Cool, now I wanted to try and add the new link using Vue Router's `router-link` component. So I did and for whatever reason in the whole milky way, **Evan You** decided to append leading slash to the link's generated href attribute. The hardcoded links uses the native hash selector (`#section`) to jump between sections on the page. What I have now is something like `/#section` :(. Sizzlejs is now yelling at me that it didn't know where to look for `/#section`, returning `unrecognized expression error`.

> I went and have gone for the hardcoded links route

I went and have gone for the hardcoded links route, only to realized after three days that it was jQuery giving the error. It uses Sizzle to validate selectors passed on the jQuery selector `$()`. Obviously if we have something like:

```js
$(".nav-item a").on("click", function (event) {
    let href = $(this).attr("href"); // <- "/#section"

    $("html, body").stop().animate({
        scrollTop: $(href).offset().top - 50
    }, 1500, "easeInOutExpo");

    event.preventDefault();
});
```
**Sizzle unrecognized expression** `/#section` happens when calling `$(href).offset().top`. Trimming the leading slash fixes the issue.

```js
$(".nav-item a").on("click", function (event) {
    let href = $(this).attr("href");

    // trim leading slash
    href = href.replace(/\//g, '');

    $("html, body").stop().animate({
        scrollTop: $(href).offset().top - 50
    }, 1500, "easeInOutExpo");

    event.preventDefault();
});
```
With this we can now safely use `router-links` together with jQuery selector. Wish I had realized this sooner.

## Footsteps leave no trace

The drawback with controlling navigation links with JavaScript is that we are forced to used `event.preventDefault()`. Literally throwing away all native functionalities provided by the browser. After clicking several anchor links and seeing the transition from one section to another, I was happy. But when I pressed the browser back button it threw me to the an empty tab where I started. I was expecting to scroll smoothly back to one of the previous sections on the page.

> History API to the rescue!

History API to the rescue! I just need to push the navigated links to the history stack after successfully scrolling to a section.

```js
$("html, body").stop().animate({
    scrollTop: $(href).offset().top - 50
}, 1500, "easeInOutExpo");

window.history.pushState({}, '', href);
```

Then to go back from previous history we can listen to windows `popstate` event.

```js
$(window).bind('popstate', function (event) {
    const state = event.originalEvent.state;
    const href = window.location.hash;

    if (state) {
        $("html, body").stop().animate({
            scrollTop: $(href).offset().top - 50
        }, 1500, "easeInOutExpo");
    }
});
```

Here is the image of the updated navigation bar complete with UX enhancements.

![Mobile Navigation](nav-mobile.png){: .shadow width="300" height="286" }
_Mobile Navigation_
![Desktop Navigation](nav-desktop.png){: .shadow width="500" height="76" }
_Desktop Navigation_


# Integrations Page

> Creating a new page for the website is easy

So here is the mock up for the intial layout of the integrations page.

![Integration Page Mock-up](integrations-page-mock-up.png){: .shadow width="500" height="252" }
_Mobile Navigation_