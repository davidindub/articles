# Dark Mode using CSS Custom Properties

![](media/dark-mode-image.jpg)

There was a time when dark mode was a niche feature preferred by developers who spent long days staring at code on a screen. Times have changed, and in 2023, 80% of people use their devices in dark mode. Landing on a web page that doesn't support it can be eye-searing!

In this article, we'll explore how to implement dark mode using CSS custom properties and JavaScript for users who want to toggle it on and off.

***

CSS custom properties (sometimes called variables) allow us to define values we want to reuse throughout a stylesheet.

We'll define a simple color palette on the `:root` pseudo-class. This gives the variables global scope, as we're setting them as high up the cascade as they can go.

Here we're setting a pure white background with dark grey text.

```css
:root {
    --color-background: #ffffff;
    --color-text-headings: #444444;
    }
```

It's best to give custom properties semantic names to make the style sheet easier to read.

When we want to reference these properties, we'll use the `var()` function like this:

```css
body {
    background-color: var(--color-background);
}

h1, label {
	color: var(--color-text-headings);
}
```

Now we just need to change the values of the custom properties to update our whole page.

## Option 1: CSS Only

This is the simplest way to add dark mode to your website, respecting the users system preferences but doesn't allow the user to flip between dark and light mode outside their system wide preference.

We'll use a media query to check `prefers-color-scheme`, and if it is set to dark, redefine the color palette on the root element:

```css
@media (prefers-color-scheme: dark) {
	:root {
	    --color-background: #121212;
	    --color-text-headings: #FFFFFF;
	    }
    }
```
Here we're using an almost-black background with white. It's best not to use white on pure black background for dark mode as it increases eye strain. `#121212` is the color suggested by the [Material Design guidelines for Dark Mode](https://m2.material.io/design/color/dark-theme.html#color-usage-and-palettes).

And that's it!

The page now matches the user's system preference, but if you want to add a dark mode toggle to your site, you'll need to use JavaScript.


## Option 2: Using JavaScript

Define your color palette on the `:root` as above, then instead of a media query, define your dark mode styles using a CSS ID selector of `#dark`:

```css
#dark {
    --color-background: #121212;
    --color-text-headings: #FFFFFF;
    }
```

Next you will need a checkbox to let the user toggle dark mode.

```html
<label for="darkModeCheckbox">Enable Dark Mode:</label>
<input type="checkbox" id="darkModeCheckbox">
```
You may want to use CSS to style this as a toggle switch.

[W3 Schools - How to: Toggle Switch](https://www.w3schools.com/howto/howto_css_switch.asp)

Now on to the JavaScript. First let's create some variables to reference the root element and checkbox, so our code is easy to understand.

```js
const root = document.querySelector(":root");
const checkbox = document.querySelector("#darkModeCheckbox");
```

Next we'll write a function that adds or removes the `#dark` class to the root. It takes one boolean argument, `enabled`

```js
function darkMode(enabled) {
  if (enabled) {
    root.id = "dark";
    checkbox.checked = "true";
  } else {
    root.removeAttribute("id");
  }
}
```

When the page loads, we need to check the user's system preferences.

If they have dark mode enabled on their system, we want to respect their choice and make sure the checkbox is ticked:

```js
document.addEventListener("DOMContentLoaded", () => {
  if (
    window.matchMedia &&
    window.matchMedia("(prefers-color-scheme: dark)").matches
  ) {
    darkMode(true);
  }
});
```

We also need an event listener on the checkbox that will add and remove the `#dark` class to the `:root`.

```js
checkbox.addEventListener("click", (e) => {
  if (e.target.checked) {
    darkMode(true);
  } else {
    darkMode(false);
  }
});
```

Now the page will always load using the users system preference, but the option is there for them to flip between light and dark modes.

You can see all of this in action [in a Codepen](https://codepen.io/davidindub/pen/bGmovOm).


### References

[Increditools - Dark Mode Usage Statistics 2023: How Popular Is It?](https://increditools.com/dark-mode-usage-statistics/)

[MDN Web Docs - Using CSS custom properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties)