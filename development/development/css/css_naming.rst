Naming Conventions
------------------

Naming conventions in CSS are hugely useful in making your code more strict, more transparent, and more informative.

A good naming convention will tell you and your team

-  what type of thing a class does
-  where a class can be used
-  what (else) a class might be related to

The naming convention we follow are as follows

-  Hyphen (``-``) delimited strings
-  Layer namespacing
-  A variation on BEM-like naming for action modifiers

It’s worth noting that not all the naming conventions are normally useful in the CSS side of development; they really come into their own when viewed in HTML.

Hyphen Delimited
~~~~~~~~~~~~~~~~

All strings in classes are delimited with a hyphen (``-``), like so:

.. code:: css

    .page-head { }

    .sub-content { }

Camel case and underscores are not used for regular classes; the following are incorrect:

.. code:: css

    .pageHead { }

    .sub_content { }

Modified BEM-like Naming
~~~~~~~~~~~~~~~~~~~~~~~~

For larger, more interrelated pieces of UI that require a number of classes, we use a modified BEM-like naming convention.

BEM, meaning Block, Element, Modifier, is a front-end methodology coined by developers working at Yandex. Whilst BEM is a complete methodology, here we are only concerned with its naming convention. Further, the naming convention here only is BEM-like; the principles are exactly the same, but the actual syntax differs.

BEM splits components’ classes into three groups:

-  Block: The sole root of the component.
-  Element: A component part of the Block.
-  Modifier: A variant or extension of the Block.

To take an analogy (note, not an example):

.. code:: css

    .dropdown { }
    .dropdown-item { }
    .dropdown--active { }

Elements are delimited with one (1) hyphen (``-``), and Modifiers are delimited by two (2) hyphens (``--``).

Here we can see that ``.dropdown {}`` is the Block; it is the sole root of a discrete entity. ``.dropdown-item {}`` is an Element; it is a smaller part of the ``.dropdown {}`` Block. Finally, ``.dropdown--active {}`` is a Modifier; it is a specific variant of the ``.dropdown {}`` Block.

Starting Context
^^^^^^^^^^^^^^^^

Your Block context starts at the most logical, self-contained, discrete location. To continue with our dropdown-based analogy, we’d not have a class like ``.header-dropdown {}``, as the header is another, much higher context. We’d probably have separate Blocks, like so:

.. code:: css

    .header { }

    .header-nav { }


    .dropdown { }

    .dropdown-item { }

If we did want to denote a ``.dropdown {}`` inside a ``.header {}``, it is more correct to use a selector like ``.header .dropdown {}`` which bridges two Blocks than it is to increase the scope of existing Blocks and Elements.

A more realistic example of properly scoped blocks might look something like this, where each chunk of code represents its own Block:

.. code:: css

    .page { }


    .content { }


    .footer { }

    .footer-copyright { }

Incorrect notation for this would be:

.. code:: css

    .page { }

    .page-content { }

    .page-footer { }

    .page-copyright { }

It is important to know when BEM scope starts and stops. As a rule, BEM applies to self-contained, discrete parts of the UI.

More Layers
^^^^^^^^^^^

If we were to add another Element—called, let’s say, ``.dropdown-link {}``—to this ``.dropdown {}`` component, we would not need to step through every layer of the DOM. That is to say, the correct notation would be ``.dropdown-link {}``, and not ``.dropdown-item-link {}``. Your classes do not reflect the full paper-trail of the DOM.

Layer Namespacing
~~~~~~~~~~~~~~~~~

There are a number of common problems when working with CSS at scale, but the major two that namespacing aims to solve are clarity and confidence:

-  **Clarity:** How much information can we glean from the smallest possible source? Is our code self-documenting? Can we make safe assumptions from a single context? How much do we have to rely on external or supplementary information in order to learn about a system?
-  **Confidence:** Do we have enough knowledge about a system to be able to safely interface with it? Do we know enough about our code to be able to confidently make changes? Do we have a way of knowing the potential side effects of making a change? Do we have a way of knowing what we might be able to remove?

This gets further complicated when dealing with `ITCSS`_. Knowing what layer a class is coming from is not always apparent. To combat this and provide complete transparency we use layer based namespacing.

In no particular order, here are the individual namespaces and a brief description. We’ll look at each in more detail in a moment, but the following list should acquaint you.

-  ``o-``: Signify that something is an Object, and that it may be used in any number of unrelated contexts to the one you can currently see it in. Making modifications to these types of class could potentially have knock-on effects in a lot of other unrelated places. Tread carefully.
-  ``c-``: Signify that something is a Component. This is a concrete, implementation-specific piece of UI. All of the changes you make to its styles should be detectable in the context you’re currently looking at. Modifying these styles should be safe and have no side effects.
-  ``u-``: Signify that this class is a Utility class. It has a very specific role (often providing only one declaration) and should not be bound onto or changed. It can be reused and is not tied to any specific piece of UI. You will probably recognize this namespace from libraries and methodologies like `SUITcss`_.
-  ``t-``: Signify that a class is responsible for adding a Theme to a view. It lets us know that UI Components’ current cosmetic appearance may be due to the presence of a theme. Vastly improves templating for large projects
-  ``s-``: Signify that a class creates a new styling context or Scope. Similar to a Theme, but not necessarily cosmetic, these should be used sparingly—they can be open to abuse and lead to poor CSS if not used wisely.
-  ``is-``, ``has-``: Signify that the piece of UI in question is currently styled a certain way because of a state or condition. This stateful namespace is gorgeous, and comes from `SMACSS`_. It tells us that the DOM currently has a temporary, optional, or short-lived style applied to it due to a certain state being invoked.
-  ``_``: Signify that this class is the worst of the worst—a hack! Sometimes, although incredibly rarely, we need to add a class in our markup in order to force something to work. If we do this, we need to let others know that this class is less than ideal, and hopefully temporary (i.e. do not bind onto this).

Even from this short list alone, we can see just how much more information we can communicate to developers simply by placing a character or two at the front of our existing classes.

CSS Variables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Kebap-case
^^^^^^^^^^

We specifically use kebab-case (lowercase words separated by hyphens) for CSS variable names. This aligns with the convention used by Bootstrap itself and other CSS frameworks.
Kebab-case improves readability and avoids naming conflicts, especially when working with custom properties.

Here's an example:

.. code:: css

    :root {
        --primary-color: #1abc9c;
        --secondary-color: #34495e;
        --text-color: #ecf0f1;
    }

    .btn {
        background-color: var(--primary-color);
        color: var(--text-color);
    }

Variable prefixed
^^^^^^^^^^^^^^^^^

To organize your CSS variables effectively, a prefix system shall be used:

- Project-wide variables:
    Use the --phpbb- prefix for variables that apply to all styles. These variables will likely define core aspects of the UI, like colors, spacing, and fonts.

- Theme-specific variables:
    For variables that are specific to a particular style (e.g. prosilver), use the style's name as a prefix followed by a hyphen (-). This helps distinguish theme-specific customizations from project-wide styles.

**Example:**

.. code:: css

    :root {
        /* Project-wide variables */
        --phpbb-primary-color: #1abc9c;
        --phpbb-secondary-color: #34495e;
        --phpbb-text-color: #ecf0f1;

        /* Prosilver-specific variables */
        --prosilver-background-color: #f5f5f5;
        --prosilver-border-color: #ddd;
    }

    .button {
          background-color: var(--phpbb-primary-color);
          color: var(--phpbb-text-color);
    }

    /* Prosilver-specific style */
    .content {
        background-color: var(--prosilver-background-color);
        border: 1px solid var(--prosilver-border-color);
    }

In this example:

- `--phpbb-primary-color`, `--phpbb-secondary-color` and `--phpbb-text-color` are project-wide variables that shall be accessed throughout the codebase.
- `--prosilver-background-color` and `--prosilver-border-color` are specific to the prosilver style, allowing for easy customization without affecting other styles.

**Benefits of Prefixed Variables:**

- Improved Organization: Clearly separates project-wide styles from style-specific customizations.
- Reduced Conflicts: Prevents naming clashes between variables of different scopes.
- Maintainability: Makes it easier to find and manage variables across your CSS code.

**Additional Tips:**

- Keep variable names concise while maintaining clarity.
- Consider using a linter (like `stylelint`) or code formatter to enforce consistent naming conventions.
- Document your variables in a central location for easy reference.

By following these guidelines, you'll create a well-structured and maintainable system for managing CSS variables in your phpBB Extension or Style.

Further Reading
^^^^^^^^^^^^^^^

   -  `UI Selector Namspacing`_

JavaScript Hooks
~~~~~~~~~~~~~~~~

As a rule, it is unwise to bind your CSS and your JS onto the same class in your HTML. This is because doing so means you can’t have (or remove) one without (removing) the other. It is much cleaner, much more transparent, and much more maintainable to bind your JS onto data attributes.

example:

.. code:: html

    <input type="submit" class="btn" data-execute="addSomething" value="Follow" />

This means that we can have an element elsewhere which can carry the style of ``.btn {}``, but without the behavior of ``data-execute="addSomething"``.

``data-*`` Attributes
^^^^^^^^^^^^^^^^^^^^^

It is our preferred practice to use ``data-*`` attributes as JS hooks. ``data-*`` attributes, as per the spec, are typically used to store custom data private to the page or application’. however since you are already binding this attribute to your js, it makes sense to use the same attribute as the js hook.

.. _ITCSS: https://www.youtube.com/watch?v=1OKZOV-iLj4
.. _SUITcss: https://suitcss.github.io/
.. _SMACSS: https://smacss.com/
.. _UI Selector Namspacing: https://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/
