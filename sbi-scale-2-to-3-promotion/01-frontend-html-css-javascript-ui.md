# 1. Front-end Technologies

## The one story

A customer opens the bank's mobile web page to check a balance.

- The page has words, a logo, a box to type the account number, and a button. That skeleton is **HTML**.
- The logo is blue, the button is round, and the page fits a phone screen. That look is **CSS**.
- When the customer taps the button, the page checks that the account number is not empty and then asks the bank's server for the balance. That behaviour is **JavaScript**.
- The bank does not build every screen from zero each time. It uses a ready-made **UI framework** such as React, Angular, or Vue, the way a branch uses standard forms instead of designing a new form for every customer.

**Front end** means everything that runs on the customer's device and that the customer can see or touch. It is also called the **client side**, because the customer's device is the client.

**Say it in one line:** The front end is the face of the application. HTML is the bones, CSS is the clothes, JavaScript is the movement, and a UI framework is the workshop that helps us build that face faster.

---

## HTML — HyperText Markup Language

### What the letters mean

- **HyperText** means text that can jump. A link is hypertext. You tap "View statement" and you go to another page.
- **Markup** means we mark the text to say what each piece is. This line is a heading. This line is a paragraph. This box is a form.
- **Language** here is a set of rules a browser understands. A browser is the program that opens web pages: Chrome, Edge, Firefox, Safari.

HTML is a markup language. It describes structure. It is not a programming language, because it does not calculate, loop, or make decisions by itself.

### A tiny page, read like English

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Balance enquiry</title>
  </head>
  <body>
    <h1>Check your balance</h1>
    <p>Enter the account number and tap the button.</p>
    <form>
      <label>Account number</label>
      <input type="text" name="account">
      <button type="submit">Show balance</button>
    </form>
  </body>
</html>
```

Read it slowly:

| Word | Simple meaning |
| --- | --- |
| `<!DOCTYPE html>` | Tells the browser: "This is a modern HTML5 page." |
| `<html>` | The whole page starts here and ends at `</html>`. |
| `<head>` | Information about the page. The customer usually does not see this part. The title and links to CSS live here. |
| `<title>` | The name shown on the browser tab. |
| `<body>` | Everything the customer sees. |
| `<h1>` | The main heading. `h1` is the biggest. `h2`, `h3` are smaller headings. |
| `<p>` | A paragraph, meaning a normal block of text. |
| `<form>` | A group of boxes the customer fills and sends. |
| `<label>` | The words next to a box, so the customer knows what to type. |
| `<input>` | The actual box. `type="text"` means letters and numbers. `name="account"` is the name the server will see. |
| `<button>` | A button. `type="submit"` means "send this form". |

### Tags, elements, and attributes

These three words appear again and again.

- A **tag** is the marker in angle brackets. `<p>` is an opening tag. `</p>` is a closing tag. The slash means "this part ends".
- An **element** is the whole piece: opening tag, content, and closing tag. Example: `<p>Hello</p>`.
- An **attribute** is extra information inside a tag. In `<input type="text" name="account">`, `type` and `name` are attributes. `text` and `account` are their values.

Some tags do not wrap content. `<input>` and `<img>` are empty elements. They carry their meaning in attributes.

### Important HTML pieces you should recognise

**Text**

- `<h1>` to `<h6>`: headings, from most important to least important.
- `<p>`: paragraph.
- `<br>`: a line break.
- `<strong>`: important text, usually shown bold.
- `<em>`: emphasised text, usually shown italic.

**Links and pictures**

- `<a href="https://bank.example/statement">View statement</a>`
  - `<a>` means anchor, which is the old name for a link.
  - `href` means hypertext reference, which is the address the link opens.
- `<img src="logo.png" alt="Bank logo">`
  - `src` is the source, the file address of the picture.
  - `alt` is alternative text. A screen reader reads it aloud for a customer who cannot see the picture. If the picture fails to load, the browser can show this text.

**Lists**

- `<ul>` is an unordered list. The items usually show as bullets.
- `<ol>` is an ordered list. The items are numbered.
- `<li>` is one list item.

**Tables**

A mini statement on a page is often a table.

- `<table>` is the whole table.
- `<tr>` is a table row.
- `<th>` is a header cell, such as "Date" or "Amount".
- `<td>` is a normal data cell.

**Forms — very important for banking screens**

A form collects input and sends it.

Common input types:

| `type` | What the customer gets |
| --- | --- |
| `text` | A normal typing box |
| `password` | A box that hides the characters |
| `number` | A box meant for numbers |
| `email` | A box meant for an email address |
| `date` | A date picker |
| `checkbox` | A small box you tick, such as "I agree" |
| `radio` | A round choice where only one option in a group can be selected |
| `submit` | A button that sends the form |
| `hidden` | A value sent to the server that the customer does not see |

Other form words:

- `placeholder` is grey hint text inside an empty box, such as "Enter 11 digit account number".
- `required` means the customer cannot submit the form while this box is empty.
- `maxlength` limits how many characters can be typed.
- `<select>` and `<option>` make a drop-down list.
- `<textarea>` is a big box for many lines, such as a complaint.

The browser can do light checking. The real checking must also happen on the server, because a customer can bypass the browser.

**Semantic HTML**

Semantic means "the tag name says what the content means".

| Semantic tag | Meaning |
| --- | --- |
| `<header>` | The top area of a page or section |
| `<nav>` | A set of navigation links |
| `<main>` | The main content of the page |
| `<section>` | A grouped part of the page |
| `<article>` | A self-contained piece, such as a news item |
| `<aside>` | Side content |
| `<footer>` | The bottom area, often with help links and copyright |
| `<button>` | Something you click to do an action |

Older pages used `<div>` for every box. A `div` is only a division, a plain box with no meaning. Semantic tags help accessibility tools, search, and other developers.

**Accessibility**

Accessibility means a person with low vision, no vision, or limited hand movement can still use the page. Labels on form fields, `alt` text on images, proper headings, and keyboard use are part of this. For a public bank, this is a service duty, not a decoration.

### HTML5

HTML5 is the modern version of HTML. It added semantic tags, better form types, audio, video, and local storage in the browser. When someone says "HTML" in a current exam, they almost always mean HTML5.

**Say it in one line:** HTML tells the browser what each part of the page is. It builds the skeleton. It does not style the page and it does not calculate.

---

## CSS — Cascading Style Sheets

### What the letters mean

- **Style** means look: colour, size, spacing, position.
- **Sheet** means a list of style rules, often kept in a separate file such as `styles.css`.
- **Cascading** means several rules can apply to the same item, and the browser follows an order to decide which rule wins.

CSS describes presentation. HTML describes structure. Keeping them separate means one team can change the bank's colours without rewriting the account form.

### A rule, read like English

```css
button {
  background-color: #1f4e79;
  color: white;
  padding: 12px 20px;
  border-radius: 8px;
}
```

- `button` is the **selector**. It chooses which HTML to style. Here, every button.
- `background-color`, `color`, `padding`, and `border-radius` are **properties**.
- `#1f4e79`, `white`, `12px 20px`, and `8px` are **values**.
- The whole block is one **rule**.

### How you point at the right piece

| Selector | Example | What it selects |
| --- | --- | --- |
| Element | `p` | Every paragraph |
| Class | `.warning` | Every element with `class="warning"` |
| ID | `#balance` | The one element with `id="balance"` |
| Descendant | `form input` | An input that sits inside a form |
| Group | `h1, h2` | Both h1 and h2 |

A **class** can be reused on many elements. An **id** is meant to be unique on one page. In an exam, if they ask "which one should be unique?", the answer is id.

### The cascade: which rule wins

When two rules fight, the browser looks at:

1. **Importance.** A normal rule loses to a more specific important rule. You do not need the rare keyword for the exam. Remember the idea: some rules are stronger.
2. **Specificity.** A rule aimed at one id is stronger than a rule aimed at a class. A class is stronger than a plain element name. An inline style written inside the HTML tag is stronger still.
3. **Source order.** If two rules are equally strong, the one written later wins.

That is why it is called cascading. Styles flow down, and the strongest applicable rule remains.

**Inheritance** is a cousin idea. Some properties, such as font and colour, pass from a parent element to its children. If the body text is black, paragraphs inside the body are black unless a rule says otherwise. Padding and margin do not inherit in the same way.

### The box model

Every element is a box. From the inside outward:

1. **Content** — the words or the picture.
2. **Padding** — empty space inside the border, around the content.
3. **Border** — the line around the padding.
4. **Margin** — empty space outside the border, between this box and the next box.

Memory picture: content is the gift, padding is the tissue paper, border is the box, margin is the air between this gift and the next gift.

If a button looks cramped, increase padding. If two buttons collide, increase margin.

### Layout words

Old pages used tables or floats to place things. Modern pages use these:

- **Normal flow** means blocks stack downward, one after another.
- **Flexbox** lays items in a row or a column and can space them out. A header with a logo on the left and a "Logout" link on the right is a flex row.
- **Grid** lays items in rows and columns together. A dashboard with six equal cards is a grid.
- **Position** can pull an item out of the normal flow. `relative`, `absolute`, `fixed`, and `sticky` are the common values. A sticky header stays visible while you scroll.

You do not need to write flex code in the exam. You need to know that flexbox and grid are the modern layout tools.

### Responsive design

Customers use a phone, a tablet, and a desktop. **Responsive design** means one page changes its layout to fit the screen.

Tools:

- **Viewport** is the visible area of the screen. The HTML viewport setting tells the phone to use its real width.
- A **media query** is a CSS rule that applies only when a condition is true. Example: "when the screen is narrower than 600 pixels, stack the menu vertically."
- **Relative units** such as `%`, `em`, and `rem` scale. A fixed width of `1200px` can overflow a phone.
- **Mobile first** means you design the small screen first, then add rules for larger screens.

A bank app that looks correct on a branch desktop and broken on a customer's phone has failed responsive design.

### Other CSS words worth knowing

| Word | Meaning |
| --- | --- |
| `color` | Text colour |
| `background` | The colour or picture behind the content |
| `font-family` | The typeface, such as Arial |
| `font-size` | How large the text is |
| `font-weight` | How thick the text is. Bold is a heavy weight |
| `text-align` | Left, right, or centre |
| `display` | How the box participates in layout. `block`, `inline`, `flex`, `grid`, and `none` are common |
| `display: none` | The element is not shown |
| Class toggle | JavaScript adds or removes a class, and CSS changes the look. Example: an error class turns a box red |

Three ways to attach CSS:

1. **Inline** — the style sits inside the tag. Hard to reuse.
2. **Internal** — a `<style>` block in the page head. Useful for one page.
3. **External** — a separate `.css` file linked from many pages. This is the usual professional way. The browser can also cache that file.

**Say it in one line:** CSS decides how HTML looks and how it rearranges itself on different screens. The box model, the cascade, flexbox, grid, and media queries are the heart of it.

---

## JavaScript

### What it is

JavaScript is a programming language that runs in the browser. It makes the page alive.

HTML is the form. CSS is the colour of the form. JavaScript is the clerk who looks at what you typed and reacts.

A file is often named `app.js`. The page loads it with a `<script>` tag.

Java and JavaScript are different languages. The similar name is a historical accident. Java is a common back-end language in banks. JavaScript is the language of the browser.

### What a script can do on a banking page

- Check that the amount is greater than zero before sending.
- Show "Password and confirm password do not match" immediately.
- Open and close a menu.
- Show a spinner while the balance is being fetched.
- Update only the balance figure without reloading the whole page.
- Remember a harmless preference, such as language, in the browser.

### The few programming ideas you need

**Variable** — a named box that holds a value.

```javascript
let accountNumber = "12345678901";
const bankName = "Example Bank";
```

- `let` means the value may change later.
- `const` means the name should keep pointing at the same value. Use it when you do not plan to reassign it.

**Data types** — the kind of value.

| Type | Example | Meaning |
| --- | --- | --- |
| String | `"Hello"` | Text |
| Number | `2500.50` | A number |
| Boolean | `true` or `false` | Yes or no |
| Array | `["Savings", "Current"]` | A list |
| Object | `{ name: "Asha", balance: 2500 }` | A group of named values |
| Null | `null` | Intentionally empty |
| Undefined | `undefined` | Not given a value yet |

**Operator** — a symbol that does work. `+` adds or joins text. `===` checks that two values are equal in value and type. `>` checks greater than. `&&` means and. `||` means or.

**Condition** — a decision.

```javascript
if (amount <= 0) {
  showError("Enter an amount greater than zero");
} else {
  sendRequest();
}
```

**Loop** — repeat work. A loop can walk through every row of a mini statement and build the table.

**Function** — a named packet of steps you can run whenever you need it.

```javascript
function isValidAccount(accountNumber) {
  return accountNumber.length === 11;
}
```

**Array methods you may see**

- `push` adds an item.
- `length` tells how many items.
- `forEach` visits every item.
- `map` builds a new list by transforming each item.
- `filter` keeps only the items that pass a test.

You rarely need to write these in a promotion paper. You need to recognise that JavaScript can store lists and walk through them.

### The DOM

**DOM** means Document Object Model. After the browser reads HTML, it builds a tree of objects in memory. JavaScript can read and change that tree.

Example of the idea:

- Find the element whose id is `balance`.
- Change its text to `₹ 25,000`.

The customer sees the new balance. The original HTML file on the server did not change. Only the live page in this browser changed.

Important DOM words:

| Word | Meaning |
| --- | --- |
| `document` | The whole page, as an object JavaScript can talk to |
| `getElementById` | Find one element by its id |
| `querySelector` | Find the first element matching a CSS selector |
| `textContent` | The text inside an element |
| `innerHTML` | HTML inside an element. Treat untrusted text carefully, because it can become a security hole |
| `createElement` | Make a new element, such as a new table row |
| `classList.add` | Add a CSS class |
| `setAttribute` | Set an attribute such as `disabled` |

### Events

An **event** is something that happens: a click, a key press, a form submit, a page load.

An **event listener** is a function that waits for that event.

Story: the button sits quietly. The listener is a peon watching the button. When the customer clicks, the peon runs the function "validate and send".

Common events: `click`, `submit`, `change`, `input`, `load`, `keydown`.

### Talking to the server without reloading the page

Old websites sent a form and the browser loaded a brand new page. Modern pages often send a quiet request in the background and update one part of the screen.

Words for that idea:

- **AJAX** means Asynchronous JavaScript and XML. The historical name remains, even though modern replies are usually JSON, not XML.
- **Asynchronous** means "start this work, and do not freeze the page while waiting". The customer can still see the screen. When the answer arrives, a function handles it.
- **`fetch`** is the modern browser function that sends such a request.
- **JSON** means JavaScript Object Notation. It is a text format for data. Example: `{ "account": "123", "balance": 25000 }`. It is easy for both JavaScript and bank servers to read.
- **XML** is an older, more verbose text format still used by some bank interfaces. JSON is lighter and more common in new web screens.

A promise, in plain words, is JavaScript's way of saying "I will give you the result later, whether it succeeds or fails". `fetch` returns a promise.

### Client-side validation is not enough

JavaScript can say "this amount looks wrong" quickly, which is kind to the customer. A dishonest person can turn JavaScript off or send a crafted request directly to the server. So the server must repeat every important check: amount, account ownership, limits, and entitlements.

### Where JavaScript should not keep secrets

The customer can view the page source and the script. A password, an encryption key, or a hidden business rule inside JavaScript is not secret. Secrets belong on the server.

Browser storage words:

| Place | What it is | Caution |
| --- | --- | --- |
| Cookie | A small piece of data the browser stores and often sends back to the server | Session cookies must be protected. See the security chapter |
| `localStorage` | Data that stays in the browser until deleted | Do not store passwords or full card numbers |
| `sessionStorage` | Data that lasts until the tab is closed | Still visible to scripts on that page |

### A few modern JavaScript words

- **ES6** and later means modern JavaScript. `let`, `const`, arrow functions, classes, and modules arrived in this era. ES means ECMAScript, the official standard name.
- A **module** is a JavaScript file that exports some functions so another file can import them.
- **Node.js** is JavaScript running outside the browser, often on a server. The language is the same family. The place it runs is different. Node belongs to the back-end discussion when it is used as a server.

**Say it in one line:** JavaScript runs in the browser, changes the page, reacts to clicks, checks input for the customer's convenience, and requests data from the server. The server still makes the final decision.

---

## UI frameworks

### What "UI" means

**UI** means User Interface. It is the set of screens, buttons, messages, and layouts a person uses.

**UX**, User Experience, is the wider feeling: was the journey to check a balance short, clear, and calm? The syllabus names UI frameworks. Mention UX only if you want to show you know the neighbouring word.

A **framework** is a prepared structure. You fill in your bank's screens by following its rules. A **library** is a toolbox you call when you want. In daily speech people call React, Angular, and Vue frameworks. Angular is a full framework. React is often described as a library for building user interfaces, with a surrounding ecosystem that behaves like a framework. For the exam, grouping them as UI frameworks is acceptable and expected.

### Why a bank uses one

Without a framework, every screen reinvents how to update the page, how to share data, and how to route from "Accounts" to "Transfer". A framework gives:

- **Components** — reusable pieces. A "money input" component can appear on transfer, bill pay, and deposit screens.
- **Routing** — moving between screens inside one application, such as `/accounts` and `/transfer`.
- **State** — the current data of the screen, such as the typed amount and the selected account.
- **A standard way to talk to APIs.**

### Single Page Application

A **Single Page Application (SPA)** loads one HTML shell. After that, JavaScript swaps the content when the customer moves from "Home" to "Statement". The browser does not do a full white-flash reload every time.

The good side: it feels quick and app-like.

The care side: the first load can be heavy, search engines need extra care, and security still depends on the server. The back button and deep links must be handled by the router.

A **Multi Page Application** loads a new HTML page from the server for each screen. Many older bank sites work this way. Both styles are valid.

### The three names examiners expect

**React**

- Made by Meta and maintained as open source.
- The screen is a tree of components.
- A component is often a function that returns what should appear.
- When state changes, React updates the parts of the page that changed.
- **JSX** looks like HTML written inside JavaScript. A build step turns it into real JavaScript.
- **Virtual DOM** is a lightweight copy of the page structure that React compares with the previous copy, so it can touch the real DOM only where needed. You can explain it as "React calculates the smallest change, then applies it".
- React Native, a related tool, targets mobile apps. The syllabus focus is the web UI idea.

**Angular**

- Maintained by Google.
- A full framework: components, routing, forms, and HTTP calls are part of the official design.
- Written in **TypeScript**, which is JavaScript plus declared types. Types catch many mistakes before the app runs.
- Uses templates and a strong structure. Large teams like the fixed way of doing things.
- The word "AngularJS" means the old 1.x product. Modern Angular is the later rewrite. If an option says AngularJS and another says Angular, they are not the same generation.

**Vue**

- An approachable framework that can grow from a small page to a large SPA.
- Also uses components and a virtual DOM idea.
- Often praised for a gentle learning path.
- Common in many products, including some public-sector and enterprise screens.

### Words that sit around UI frameworks

| Word | Simple meaning |
| --- | --- |
| Component | A reusable piece of UI with its own look and data, such as `AccountCard` |
| Props | Inputs a parent passes to a child component, such as the account name |
| State | Data that can change and should cause the screen to update |
| Rendering | Turning the component description into what the customer sees |
| Binding | Connecting data to the screen so a change in one updates the other |
| SPA | One page shell, content swapped by JavaScript |
| Build tool | A tool such as Vite or Webpack that packs many source files into files the browser can load |
| Package manager | npm or yarn. They download the framework and other packages |
| Design system | The bank's own kit of colours, type, and components so every journey looks like the same bank |
| Bootstrap | A popular CSS framework of ready-made grid, buttons, and form styles. It is a UI kit, mainly CSS and a little JavaScript, older and simpler than React |
| Tailwind | A CSS approach where small utility classes style elements. It is styling, not a full application framework |

**Material Design** and similar systems are visual guidelines. A framework can follow them. The guideline is the look. The framework is the engineering structure.

### How a screen is born, end to end

1. A designer and a product owner agree the "Fund transfer" journey.
2. HTML-like templates describe the heading, the from-account list, the amount box, and the button.
3. CSS, often through the design system, applies the bank's colours and spacing.
4. JavaScript in the component watches the button.
5. On submit, the component calls an API.
6. While waiting, state becomes "loading" and a spinner shows.
7. On success, state becomes "done" and a receipt number shows.
8. On failure, state becomes "error" and a clear message shows.

The framework's job is to make steps 2 to 8 consistent on every screen.

**Say it in one line:** A UI framework helps teams build large front ends from reusable components. React, Angular, and Vue are the three names to remember. An SPA changes the screen without reloading the whole page.

---

## Front-end mistakes that become exam traps

| Trap | The clear idea |
| --- | --- |
| "HTML styles the page" | HTML structures the page. CSS styles it |
| "JavaScript is Java" | They are different languages |
| "Client-side validation is security" | It is a convenience. The server must validate again |
| "The customer cannot see JavaScript" | The customer can. Do not hide secrets in it |
| "React, Angular, and Vue run the core banking database" | They build the user interface. The database sits behind the server |
| "id and class are the same" | A class is reusable. An id should identify one element |
| "Semantic tags are only for beauty" | They describe meaning and help accessibility |

## A branch story you can retell

The passbook printing screen in a branch:

- HTML creates the title "Print passbook", the account box, and the print button.
- CSS makes it match the bank's blue and keeps the button large enough for a touch screen.
- JavaScript checks the account number length and asks the server for the last transactions.
- The UI framework supplies the same account box already used on the cash deposit screen, so staff learn it once.

## Remember these lines

- Front end = client side = what the user sees and operates.
- HTML = structure. CSS = presentation. JavaScript = behaviour.
- DOM = the live tree of the page. Events = clicks and other happenings. `fetch` = ask the server without a full reload.
- JSON = the usual text format for that conversation.
- React, Angular, and Vue = component-based UI frameworks. SPA = one shell, many views.
- Anything secret, and any decision that moves money, belongs on the server.
