# Syed Abdul Aziz — Portfolio Site

**Live:** https://syedabdulaziz-creator.github.io

A single-page personal portfolio with a notebook-and-terminal theme, built with plain HTML, CSS, and JavaScript — no frameworks, no build tools. This README explains not just what the project is, but *why* it's built the way it is, so you can defend it in an interview or conversation without hesitation.

---

## 1. What it does

- Shows a bio, skills, projects, and contact info in a page styled to look like ruled notebook paper.
- The hero section is a **working terminal** — you type real commands (`about`, `skills`, `projects`, `contact`, `clear`) and it responds, and scrolls the page to the matching section.
- Fully responsive (works on mobile and desktop) and hosted for free on **GitHub Pages**.

## 2. Tech stack — and why

| Choice | Why |
|---|---|
| Plain HTML/CSS/JS, no framework | It's one page with no complex state. React/Vue would add build tooling for no real benefit. Knowing when *not* to reach for a framework is itself a skill worth showing. |
| GitHub Pages for hosting | Free, and directly serves static files from a repo — no server needed for a static site like this. |
| Google Fonts (Kalam + JetBrains Mono) | Kalam (handwritten style) for headings gives the "notebook" feel; JetBrains Mono (a monospace/code font) for body text reinforces the "terminal/developer" feel. Two fonts, two roles — one for personality, one for readability. |
| CSS custom properties (`:root { --paper: ... }`) | All colors are defined once at the top and reused everywhere via `var(--name)`. Change a color in one place, it updates the whole site. This is how real production CSS is organized. |

## 3. How the page is structured (HTML)

The page is one long `index.html` file, broken into `<section>` blocks: `about`, `skills`, `projects`, `learning`, `contact`. Each section has an `id` attribute (e.g. `id="about"`) — that ID is what lets the terminal jump to it.

## 4. How the terminal works (JavaScript) — the part most likely to get asked about

This is the one genuinely "engineered" part of the site, so understand it well:

```js
var responses = {
  about: "Syed Abdul Aziz — first-year CSE @ MJCET...",
  skills: "...",
  // etc.
};
```
This is a **JavaScript object** acting like a lookup table (a dictionary): the command word is the key, the response text is the value. When you type a command, the code does `responses[cmd]` to instantly find the matching reply — this is an O(1) lookup, not a search through a list.

```js
input.addEventListener('keydown', function (e) {
  if (e.key !== 'Enter') return;
  ...
});
```
This attaches an **event listener** to the text input box. Every time a key is pressed inside it, this function runs. It checks if the key was `Enter`; if not, it does nothing and exits early.

**Step by step for what happens when you type `about` and hit Enter:**
1. `input.value` reads whatever text is currently in the box.
2. The text is trimmed (removes extra spaces) and lowercased, so `About` and `about ` both work.
3. `printPrompt(raw)` echoes what you typed back onto the screen, styled like a terminal line.
4. The code checks `responses[cmd]` — if that key exists in the object, it prints the matching response.
5. `scrollToSection('about')` runs, which finds the HTML element with `id="about"` and calls `.scrollIntoView()` to smoothly scroll the page there.
6. The input box is cleared (`input.value = ''`) so it's ready for the next command.
7. If the command isn't in the `responses` object, it prints a "command not found" message instead — this is the fallback/else case.

The blinking cursor and typing animation on load are done with **CSS animations** (`@keyframes blink`) and `setTimeout()` calls that delay each line of the intro by a fixed number of milliseconds, so lines appear one after another instead of all at once.

## 5. How the "notebook" look is done (CSS)

- The faint horizontal lines are made with `repeating-linear-gradient` — a CSS function that repeats a pattern (transparent, then a thin colored line) down the page, instead of using dozens of actual `<hr>` elements.
- The red vertical margin line is a single `::before` pseudo-element positioned absolutely down the left side of the page.
- The "sticky note" effect on project cards uses another `::before` pseudo-element, rotated slightly (`transform: rotate(-2deg)`) and colored semi-transparent yellow, positioned to look like a piece of tape.

## 6. Design decisions worth explaining if asked

- **Why a terminal?** I'm a CS student — a terminal is the most literal way to say "I write code" without needing a paragraph of text to say it.
- **Why not a JS framework?** The site has no dynamic data fetching, no user accounts, no complex UI state — a static page with a bit of vanilla JS is the right-sized tool. Reaching for React here would be over-engineering.
- **Why is content separated into commented sections?** So the file can be edited by hand later (new projects, updated skills) without needing to touch the CSS or JS — separation of content from logic.

## 7. File structure

```
index.html   → everything: HTML structure, CSS in a <style> tag, JS in a <script> tag
README.md    → this file
```

## 8. How to run it locally

No build step needed — just double-click `index.html` and it opens in your browser. To edit and re-check, save the file and refresh the browser tab.
