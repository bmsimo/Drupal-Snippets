# Javascript

## Drupal JavaScript API

Drupal provides a JavaScript API that allows you to interact with Drupal from JavaScript. The API is available in the global Drupal object. The Drupal object is defined in core/misc/drupal.js. The Drupal object contains the following properties:

- Drupal.behaviors: An object that contains all behaviors that are attached to the page. Behaviors are attached to the page using the Drupal.attachBehaviors() function.
- Drupal.t: A function that translates strings. See the section on Drupal.t() for more information.
- Drupal.formatPlural: A function that translates strings with pluralization. See the section on Drupal.formatPlural() for more information.
- Drupal.checkPlain: A function that sanitizes strings. See the section on Drupal.checkPlain() for more information.
- Drupal.url: A function that generates URLs. See the section on Drupal.url() for more information.
- Drupal.settings: An object that contains settings defined in the Drupal.settings JavaScript object. See the section on Drupal.settings for more information.
- Drupal.ajax: An object that contains all Ajax requests that are attached to the page. Ajax requests are attached to the page using the Drupal.ajax() function.
- Drupal.displace: An object that contains all displace requests that are attached to the page. Displace requests are attached to the page using the Drupal.displace() function.

## How to use Drupal JavaScript API

Your Javascript should always be wrapped in a closure to avoid polluting the global namespace. The Drupal JavaScript API is available in the global Drupal object. The Drupal object is defined in core/misc/drupal.js.

```javascript
// Example of a javascript file.
(function($, Drupal) {
  "use strict";

  Drupal.behaviors.babel_terra = {
    attach: function(context, settings) {
      // Your code here.
  };
})(jQuery, Drupal);
```


Drupal provides JavaScript functions  for manipulating and sanitizing strings. The two most useful are Drupal.checkPlain and Drupal.formatPlural. Drupal.checkPlain lets you ensure a string is safe for output into the DOM; it is useful when working with user-provided input. Drupal.formatPlural ensures that a string containing a count of items is pluralized correctly.

## Drupal.checkPlain()

- checkPlain handles replacing any instance of &amp;, &quot;, &lt;, or &gt; with their HTML-encoded counterparts. Using Drupal.checkPlain in your JavaScript will help ensure that any HTML entities in user provided text are properly encoded before being displayed.

```javascript
  Drupal.checkPlain = function (str) {
    str = str.toString()
      .replace(/&/g, '&amp;')
      .replace(/"/g, '&quot;')
      .replace(/</g, '&lt;')
      .replace(/>/g, '&gt;');
    return str;
  };
```



## Drupal.formatPlural()

formatPlural is passed the total new comment count (an integer) as well as the string to use when the count is singular and another to use for the plural case. The optional additional args and options arguments are not used in this case. Looking back at the source of Drupal.formatPlural we can see that it first loads drupalTranslations. It then compares the number passed in via the first count parameter to see if its value is 1. If it is, the singular case (properly translated) is used. If the count parameter is more than 1, the plural translation is used instead.

```javascript
  Drupal.formatPlural = function (count, singular, plural, args, options) {
    args = args || {};
    args['@count'] = count;

    var pluralDelimiter = drupalSettings.pluralDelimiter;
    var translations = Drupal.t(singular + pluralDelimiter + plural, args, options).split(pluralDelimiter);
    var index = 0;

    // Determine the index of the plural form.
    if (typeof drupalTranslations !== 'undefined' && drupalTranslations.pluralFormula) {
      index = count in drupalTranslations.pluralFormula ? drupalTranslations.pluralFormula[count] : drupalTranslations.pluralFormula['default'];
    }
    else if (args['@count'] !== 1) {
      index = 1;
    }

    return translations[index];
  };

// Example 
Drupal.formatPlural(results[nodeID].new_comment_count, '1 new comment', '@count new comments')

```
