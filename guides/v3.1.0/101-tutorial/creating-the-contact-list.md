Now that we have our HTML and styles all figured out, we'll create our first
component - the ContactList. This list is on the left hand side of our
application, and should contain all of the contacts that are available to our
user.

## What is a Component?

Components are Ember's most fundamental construct. They are reusable,
self-contained elements that have a template and optionally a backing class
with any logic that is part of the component's functionality. You can think of
components as extensions to HTML - they allow us to create our own custom tags
like `div` or `input`, with custom layouts, styles, and behaviors.

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

```handlebars {data-filename="app/templates/application.hbs"}
{{yield}}



and copy the markup from the application
template to the new component
