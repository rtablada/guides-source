Now that we've bootstrapped our Rolodex app, we can start to setup the
application shell. We'll start by setting up some basic HTML and CSS that will
provide the overall structure for the app, and then begin adding our first
components.

As a reminder, here is the mockup for our app, with the portions we'll be adding
highlighted

## Adding Markup

Let's start by adding some basic markup to mock out our application. We'll
create the entire app in static HTML first, and then we'll turn it into
components and hook it up to our data.

Add the following markup to `application.hbs`:

```handlebars {data-filename="app/templates/application.hbs"}
<div class="container">
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

  <section>
    <h1>Zoey</h1>

    <ul class="contact-items">
      <li>
        <div class="contact-item-type">
          phone
        </div>
        <div class="contact-item-value">
          555-1232
        </div>
      </li>
      <li>
        <div class="contact-item-type">
          email
        </div>
        <div class="contact-item-value">
          zoey@emberjs.com
        </div>
      </li>
      <li>
        <div class="contact-item-type">
          note
        </div>
        <div class="contact-item-value">
          Met at EmberConf!
        </div>
      </li>
    </ul>

    <button class="edit-button">
      Edit
    </button>
  </section>
</div>
```

And add the following styles to `app/styles/app.css`:

```css {data-filename="app/styles/app.css"}


```

Ember CLI should pick up the changes and rebuild the application, and you should
now be able to see the following in your browser:


Looks just like our mockup! However, this is only static content so far. We can
tweak the HTML and CSS at this point to make sure we know what we're shooting
for, and once we've got the base structure down we can move onto adding
components. But first, let's talk about what's going on with Ember here:

#### What is the Application Template?

The application template, located at `/app/templates/application.hbs`, is the
entry point for every Ember.js application. In a sense, it's like the
`index.html` or `main` function for our app - it's the first thing the app
renders, and everything in our app comes either from this template, or from a
component or subroute (via `{{outlet}}`s) that is included in this template.

If you're familiar with HTML, you'll notice that the template doesn't include
things like the `<html>` or `<body>` tags, script tags, or style tags. Ember
handles all of these details through its build system, providing a
zero-configuration baseline that allows you to focus on shipping app code.
If you do need to customize the outer HTML, its located at `app/index.html`.

#### What is an `hbs` File?

You also may have noticed that the extension of the application template is not
`html`. That's because Ember uses Handlebars, which is a templating language
that compiles down to HTML.

_All_ valid HTML _is_ valid Handlebars, it is a superset of the language. If you
know HTML, you're already pretty familiar with Handlebars! What Handlebars adds
is the ability to add dynamic content via bound properties, components, and
helpers.

Dynamic content in Handlebars templates is generally surrounded by either double
curly braces (e.g. `{{foo}}`), or in the case of components, by angle brackets
in capital case (e.g. `<ModalDialog></ModalDialog`). So far our template doesn't
have any dynamic content, it's plain HTML and CSS. In the next step, we'll
extract our first component, the `ContactList`.
