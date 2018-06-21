Now that we have our HTML and styles all figured out, we'll create our first
component - the `ContactList`. This list is on the left hand side of our
application, and should contain all of the contacts that are available to our
user. But first, let's talk about what components are, and how we can use them
to build applications.

## What is a Component?

Components are Ember's most fundamental construct. They are reusable,
self-contained elements that have a template and optionally a backing class
with any logic that is part of the component's functionality. You can think of
components as extensions to HTML - they allow us to create our own custom tags
like `<div>`, `<input>`, or `<select>`, with custom layouts, styles, and
behaviors.

You can tell whether or not something is a component by looking at it in your
template. Components are invoked using capital-case notation in angle brackets:

```handlebars
<!-- standard HTML div -->
<div>

  <!-- standard HTML input -->
  <input type="checkbox">

  <!-- component -->
  <CustomInput/>

  <!-- block style component -->
  <CustomButton>
    Save
  </CustomButton>

</div>
```

Components allow us to break down our application into reusable and composeable
parts, so we're not repeating ourselves constantly. They give us a way to
encapsulate behavior and keep our code neat and organized.

## Generating a Component

We'll start by using Ember-CLI to generate a basic component for us. Run the
generate command with `component` to get started:

```sh
ember generate component contact-list
```

or, for short:

```sh
ember g component contact-list
```

Ember-CLI should create three files:

```sh
installing component
  create app/components/contact-list.js
  create app/templates/components/contact-list.hbs
installing component-test
  create tests/integration/components/contact-list-test.js
```

The first two files are the template and the backing class for our component,
and the last file is a basic test file. We'll return to testing later on, for
the moment lets focus on the component.

## Adding a Template

Open up the component's template file at `app/templates/components/contact-list`.
You should see one line only with `{{yield}}` in it. This is how components can
use block form, which is when you pass a template to a component upon invocation
(e.g. `<CustomButton>Foo</CustomButton>`). We'll return to this later, for now
remove the `{{yield}}` helper and copy the markup for the contact list from the
application template:

```handlebars {data-filename="app/templates/components/contact-list.hbs" data-diff="-1,+2,+3,+4,+5,+6,+7,+8,+9,+10,+11,+12,+13,+14,+15,+16,+17,+18"}
{{yield}}
<nav>
  <input placeholder="search" />
  <ul>
    <li>
      <a class="active">
        <h4>Zoey</h4>
        <p>zoey@emberjs.com</p>
      </a>
    </li>
    <li>
      <a>
        <h4>Tomster</h4>
        <p>tomster@emberjs.com</p>
      </a>
    </li>
  </ul>
</nav>
```

And then remove the markup from the application template and replace it with the
`<ContactList>` component:

```handlebars {data-filename="app/templates/application.hbs" data-diff="+2,-3,-4,-5,-6,-7,-8,-9,-10,-11,-12,-13,-14,-15,-16,-17,-18,-19"}
<div class="container">
  <ContactList/>
  <nav>
    <input placeholder="search" />
    <ul>
      <li>
        <a class="active">
          <h4>Zoey</h4>
          <p>zoey@emberjs.com</p>
        </a>
      </li>
      <li>
        <a>
          <h4>Tomster</h4>
          <p>tomster@emberjs.com</p>
        </a>
      </li>
    </ul>
  </nav>

  ...
</div>
```

Save your changes, and you should see the application rebuild and reload.
Everything should appear the exact same as before, and if you inspect the DOM
you'll find that our HTML has remained unchanged:

This is because Ember is rendering the `ContactManager` component in the
application template, which puts _its_ template wherever we invoked it. Because
the `ContactList` component only has a template right now, we don't see anything
dynamic. It's just the HTML being inserted directly.

Let's change that!

First, lets add some data to our component. We'll add a couple of javascript
objects that represent the contacts in the list, Zoey and Tomster. Open up the
component file at `app/components/contact-list.js` and add a contacts array
to the class:

```js {data-filename="app/components/contact-list.js" data-diff="+4,+5,+6,+7,+8,+9,+10,+11,+12,+13"}
import Component from '@ember/component';

export default class ContactList extends Component {
  contacts = [
    {
      name: 'Zoey',
      email: 'zoey@emberjs.com',
    },
    {
      name: 'Tomster',
      email: 'tomster@emberjs.com',
    },
  ];
}
```

Now, back in the template, we can loop over this array using the `{{each}}`
helper. This helper recieves an array and a template, and loops over the array,
repeating the template for each item in the array. It yields the item to the
template, so we can dynamically insert the name and email properties of each
item in the template, like so:

```handlebars {data-filename="app/templates/components/contact-list.hbs" data-diff="-4,-5,-6,-7,-8,-9,-10,-11,-12,-13,-14,-15,+16,+17,+18,+19,+20,+21,+22,+23"}
<nav>
  <input placeholder="search" />
  <ul>
    <li>
      <a class="active">
        <h4>Zoey</h4>
        <p>zoey@emberjs.com</p>
      </a>
    </li>
    <li>
      <a>
        <h4>Tomster</h4>
        <p>tomster@emberjs.com</p>
      </a>
    </li>
    {{#each this.contacts as |contact|}}
      <li>
        <a class="active">
          <h4>{{contact.name}}</h4>
          <p>{{contact.email}}</p>
        </a>
      </li>
    {{/each}}
  </ul>
</nav>
```

You should see the exact same output as before in your browser, only this time
it's dynamic and based on data in your component. Let's add another contact to
the list to see it update:

```js {data-filename="app/components/contact-list.js" data-diff="+13,+14,+15,+16"}
import Component from '@ember/component';

export default class ContactList extends Component {
  contacts = [
    {
      name: 'Zoey',
      email: 'zoey@emberjs.com',
    },
    {
      name: 'Tomster',
      email: 'tomster@emberjs.com',
    },
    {
      name: 'Bob',
      email: 'bob@emberjs.com',
    },
  ];
}
```

You should now see "Bob" in the list of contacts:

[pic]

### What are these curlies?

We already introduced components, which are one of the features of the
Handlebars templating language and are marked by capital-cased elements (like
`<ContactList>`), but in the template above we added three more concepts:

1. **Helpers** like `{{each}}` are like components, but much simpler. They are
are pure functions that take a number of arguments, and in some cases a
template, and return any kind of output. They can be used to format numbers
into currency, to perform boolean logic, to define objects or arrays, or to
fetch data asynchronously.

  Ember comes with a default set of helpers that include some basic operations,
  like `{{each}}`, or `{{if}}` which allows us to conditionally render or hide
  parts of a template. Helpers can receive primitive values (e.g.
  `{{format-number 1234}}`) or variables, which can come from local variables
  provided by yields (see below) or from the backing component class. In our
  example, we pass `this.contacts` to the `{{each}}`, which refers to the
  contacts field we declared in the component body.

  When used with a template (also known as "block form"), helpers must be
  invoked with a hashtag at the beginning (`{{#...}}`), and have a closing
  helper (`{{/...}}`) at the end of the template block, similar to HTML.

2. **Yielding**, which is the `as |contact|` part of the `{{#each}}`. Helpers
and components can both yield values to their template block, giving users
a public API that they can work with in their own usage of the component.

  You can think of components like functions in this way, and yielding is like a
  callback. It allows a higher level component to receive a child template and
  call it where it chooses to fill in details, making components more reusable
  and composeable:

  ```js
    fs.readFile('example.txt', {}, (err, data) => {
      console.log(data);
    });
  ```
  ```handlebars
    <Dropdown as |close|>
      <button onclick={{close}}>
        Close
      </button>
    </Dropdown>
  ```

3. **Bindings** like `{{contact.name}}`. These render the value that they
reference directly into the template, without any formatting, and update the
value in the template as it changes. You'll notice that both of our bindings in
this template refer to `contact`, which is a local variable yielded by
`{{each}}`. Bindings can also refer directly to the component using `{{this}}`
(e.g. `{{this.foo}}`)

Alright, now we're dynamically rendering some HTML based on values in our
Javascript! Next, let's add some functionality to that search bar so we can
filter through contacts.

## Adding Search Functionality

We can use native `onkeypress` event handler to add an action to the search input
that will send its value to the component class. We'll use another built in
helper here, `{{action}}`:

```handlebars {data-filename="app/templates/components/contact-list.hbs" data-diff="-2,+3"}
<nav>
  <input placeholder="search" />
  <input placeholder="search" onkeypress={{action this.updateSearchValue}} />
  <ul>
    {{#each this.contacts as |contact|}}
      <li>
        <a class="active">
          <h4>{{contact.name}}</h4>
          <p>{{contact.email}}</p>
        </a>
      </li>
    {{/each}}
  </ul>
</nav>
```

Next, in the component class we'll add the `updateSearchValue` method. The
method is called directly from the native element's `onkeypress` hook, so it
receives the native javascript event as its argument. We can get the original
target of the event (the input) and the get the value of the input, and set it
on the component:

```js {data-filename="app/components/contact-list.js" data-diff="+4,+5,+6,+7"}
import Component from '@ember/component';

export default class ContactList extends Component {
  updateSearchValue(event) {
    this.set('searchValue', event.target.value);
  }

  contacts = [
    {
      name: 'Zoey',
      email: 'zoey@emberjs.com',
    },
    {
      name: 'Tomster',
      email: 'tomster@emberjs.com',
    },
    {
      name: 'Bob',
      email: 'bob@emberjs.com',
    },
  ];
}
```

Note that we have to use `this.set` to update the state of the component. Using
`this.set` tells Ember that something has been updated and that it should
invalidate and cached values that rely on the updated value. If the value is
bound in the template, directly or indirectly, this will cause Ember rerender.
For now, nothing depends on our `searchValue` property, so nothing will update.

This is a pretty common pattern, and it would add a lot of boilerplate if we
were to have to add a method every time we wanted to update a value from an
input! Luckily, we have a shorthand in the form of another helper - `mut`. `mut`
is a helper that returns a function that does exactly what we did in the method
above:

```handlebars {data-filename="app/templates/components/contact-list.hbs" data-diff="-2,+3"}
<nav>
  <input placeholder="search" onkeypress={{action this.updateSearchValue}} />
  <input placeholder="search" onkeypress={{action (mut this.searchValue) value="target.value"}} />
  <ul>
    {{#each this.contacts as |contact|}}
      <li>
        <a class="active">
          <h4>{{contact.name}}</h4>
          <p>{{contact.email}}</p>
        </a>
      </li>
    {{/each}}
  </ul>
</nav>
```

The first thing you'll notice that the `(mut)` helper up there is surrounded by
parentheses instead of curly brackets. This is another feature of Handlebars -
helpers can call other helpers, and when they invoke nested helpers they use
parentheses. Otherwise, the syntax is exactly the same.

The second thing you'll notice is that we pass `this.searchValue` directly into
mut. This tells `mut` to set the `this.searchValue` to whatever is passed to it
(_mut_ating the value).

For the last part, we know that the `onkeypress` handler is going to pass the
function that `mut` returns the `change` event, but that's not what we want!
However, `{{action}}` has an option that allows us to specify a lookup path to
pass instead, which is the `value="target.value"` portion we added.

Now we can remove the action method we added before:

```js {data-filename="app/components/contact-list.js" data-diff="-4,-5,-6,-7"}
import Component from '@ember/component';

export default class ContactList extends Component {
  updateSearchValue(event) {
    this.set('searchValue', event.target.value);
  }

  contacts = [
    {
      name: 'Zoey',
      email: 'zoey@emberjs.com',
    },
    {
      name: 'Tomster',
      email: 'tomster@emberjs.com',
    },
    {
      name: 'Bob',
      email: 'bob@emberjs.com',
    },
  ];
}
```

### The `filteredContacts` computed

Now that we have the search value in our component, we need to use it to
actually filter the list. To do that, we'll use a computed property:

```js {data-filename="app/components/contact-list.js" data-diff="+10,+11,+12"}
export default class ContactList extends Component {
  contacts = [
    {
      name: 'Zoey',
      email: 'zoey@emberjs.com',
    },
    {
      name: 'Tomster',
      email: 'tomster@emberjs.com',
    },
    {
      name: 'Bob',
      email: 'bob@emberjs.com',
    },
  ];

  @computed('contacts', 'searchValue')
  get filteredContacts() {
    return this.contacts.filter(contact => contact.name.match(this.searchValue));
  }
}
```

As you can see, `filteredContacts` is a standard class getter function, and it
returns the `contacts` array filtered by the `searchValue`. The only unusual
thing we've added here is the `@computed` decorator. This decorator memoizes the
getter - it caches its value until one of the dependent keys passed to it (in
this case, `'contacts'` and `'searchValue'`) changes. This way we don't do more
work than we need to. It also allows Ember to know when a value has updated and
when it needs to rerender, otherwise it wouldn't know to rerender the `each`
loop when `searchValue` changes.

Let's update the template to use our new `filteredContacts` property:

```handlebars {data-filename="app/templates/components/contact-list.hbs" data-diff="-4,+5"}
<nav>
  <input placeholder="search" onkeypress={{action (mut this.searchValue) value="target.value"}} />
  <ul>
    {{#each this.contacts as |contact|}}
    {{#each this.filteredContacts as |contact|}}
      <li>
        <a class="active">
          <h4>{{contact.name}}</h4>
          <p>{{contact.email}}</p>
        </a>
      </li>
    {{/each}}
  </ul>
</nav>
```
