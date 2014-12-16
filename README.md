# rs-container

Temporarily transform polymer components into basic HTMLElements for
easy stubbing.

# Usage

declarative:

```
<rs-container downgrade='x-api'>
  <!-- do stuff -->
</rs-container>
```

imperative:

```
window.RS.downgrade('x-api');
// do stuff
window.RS.upgrade('x-api');
```

# Case

When you drop a polymer component on your page it will go through its
entire lifecycle and load dependencies. This is problematic if you want
to inject custom behavior into a components dependencies for testing
purposes.

More concretely, suppose we've defined the following elements:

```

<polymer-element name="x-api"></polymer-element>
<polymer-element name="x-model">
  <template>
    <x-api></x-api>
  </template>
</polymer-element>

```

If we drop `x-model` on the page, `x-api` is fully loaded with lifecycle
methods already called before we get our first reference to it. This
makes stubbing for tests difficult. Even more complicating, the browser
only lets us define a custom element once, so it's not like we can
redefine what `x-api` means in each of our tests.

We need a mechanism to stub `x-api` BEFORE x-model is created.

Use `rs-container`!

### declarative

If you wrap your test case in a `rs-container` and add `x-api` to the
downgrade list, `x-api` will be transformed into a simple `HTMLElement`.
Other `x-api` on the page will continue to function as expected (in case
you still want original behavior for other tests.

```
<rs-container downgrade='x-api'>
  <x-model>
    <!-- the x-api child is a simple HTMLElement -->
  </x-model>
</rs-container>

<x-model>
  <!-- the x-api child is the actual Polymer Element -->
</x-model>
```

### imperative

Alternatively you could do this imperativley using the `RS.downgrade`
and `RS.upgrade` methods.

```
var RS = window.RS;

RS.downgrade('x-api');

// the child x-api element is just a div
document.createElement('x-model');

RS.upgrade('x-api');

// the child x-api element is the real deal
document.createElement('x-model');

```
