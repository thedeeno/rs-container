# <rs-container>

This element makes testing your Polymer components easier!

Use this element to temporarily transform polymer elements into simple 
div's for easier stubing and testing.

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

Suppose we have the following elements:

```

<polymer-element name="x-api"></polymer-element>
<polymer-element name="x-model">
  <template>
    <x-api></x-api>
  </template>
</polymer-element>

```

When you drop `x-model` on the page for a test it will create `x-api`
which will go through its entire lifecycle and load its dependencies. If
this is a heavy operation, or an operation that has side effects, this
makes testing difficult. We want `x-api` to be dump for our test.

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
