---
title: Payment and address form best practices
subhead: Maximize conversions by helping your users complete name, address and payment forms as quickly and easily as possible.
authors:
  - samdutton
scheduled: true
date: 2020-11-16
updated: 2020-11-16
description: Maximize conversions by helping your users complete name, address and payment forms as quickly and easily as possible.
hero: hero.jpg
alt: Businessman using a payment card to make a payment on a laptop computer.
tags:
  - blog
  - forms
  - identity
  - layout
  - mobile
  - payments
  - security
  - ux
---

<!-- codelabs:
  - codelab-payment-and-address-form-best-practices
 -->

<!--  {% YouTube 'alGcULGtiv8' %} -->

Well-designed forms help users and increase conversion rates. One small fix can make a big 
difference! 

{% Aside 'codelab' %}
  If you would prefer to learn these best practices with a hands-on tutorial,
  check out the [payment and address form best practices codelab](/codelab-payment-and-address-form-best-practices).
{% endAside %}

Here is an example of a simple payment form that demonstrates all of the best practices:

{% Glitch {
  id: 'payment-form',
  path: 'index.html',
  height: 480
} %}

Here is an example of a simple form for name and address that demonstrates all of the best practices:

{% Glitch {
  id: 'address-form',
  path: 'index.html',
  height: 480
} %}

## Checklist

* [Use meaningful HTML elements](#meaningful-html): `<form>`, `<input>`, `<label>`, and `<button>`.
* [Label each input with a `<label>`](#html-label).
* Use element attributes to [access built-in browser features](#element-attributes), in particular 
[`type`](#type-attribute) and [`autocomplete`](#autocomplete-attribute) with appropriate values.
* Avoid using `type="number"` for numbers such as payment card numbers that aren't meant to be 
incremented. Use `type="text"` and [`inputmode="numeric"`](#inputmode-attribute) instead.
* If an [appropriate autocomplete value](#autocomplete-attribute) is available for an `input`, 
`select` or `textarea`, you should use it.
* To help browsers autofill forms, give input `name` and `id` attributes [stable values](#stable-name-id) 
that don't change between page loads or website deployments.
* [Disable submit buttons](#submit-buttons) once they've been tapped or clicked.
* [Validate](#validate) during data entry and on form submission.
* Make [guest checkout](#guest-checkout) the default and make account creation simple once checkout 
is complete.
* Show [progress through the checkout process](#checkout-progress) in clear steps with clear calls 
to action.
* [Limit potential checkout exit points](#reduce-checkout-exits) by removing clutter and distractions.
* At checkout [show full order details](#order-control) and make it easy for orders to be adjusted.
* [Don't ask for data you don't need](#unneeded-data).
* [Ask for names with a single input](#name-inputs) unless you have a good reason not to.
* [Don't enforce Latin-only characters](#unicode-matching) for names and other data.
* [Allow for a variety address formats](#address-variety).
* Consider using a [single textarea for address](#address-textarea).
* Use [autocomplete for billing address](#billing-address).
* [Internationalize and localize](#internationalization-localization) where necessary.
* Consider avoiding [postal code address lookup](#postal-code-address-lookup).
* Use [appropriate payment card autocomplete values](#payment-form-autocomplete).
* Use a [single input for payment card numbers](#single-credit-card-input).
* [Avoid using custom elements](#avoid-custom-elements) that break the autofill experience.
* [Test in the field as well as the lab](#analytics-rum): page analytics, interaction analytics, and 
real-user performance measurement.
* [Test across browsers, devices and platforms](#test-platforms).

{% Aside %}
This article is about frontend best practices for address and payment forms. It does not explain 
how to implement transactions on your site. To find out more adding payment functionality to your 
website on the web, see [Web Payments](/payments).
{% endAside %}

## Use meaningful HTML {: #meaningful-html }

Use the elements and attributes built for the job: 
* `<form>`, `<input>`, `<label>`, and `<button>`
* `type`, `autocomplete` and `inputmode`

These enable built-in browser functionality, improve accessibility, and add meaning to your markup.

### Use HTML elements as intended {: #html-elements }

#### Put your form in a &lt;form&gt; {: #html-form }

You might be tempted not to bother wrapping your `<input>`s in a `<form>`, and to handle data 
  submission purely with JavaScript. 

Don't do it!

An HTML `<form>` gives you access to a powerful set of built-in features across all modern browsers, 
  and can help make your site accessible to screen readers and other assistive devices. A `<form>` 
  also makes it simpler to build basic functionality for older browsers with limited JavaScript 
  support, and to enable form submission even if there's a glitch with your code—and for the 
  small number of users who actually disable JavaScript.

If you have more than one page component for user input, make sure to put each in its own `<form>` 
  element. For example, if you have search and sign-up on the same page, put each in its own `<form>`.

#### Use `<label>` to label elements {: #html-label }

To label an `<input>`, `<select>` or `<textarea>`, use a [`<label>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/label).

You associate a label with an input by giving the label's *for* attribute the same value as 
the input's id.

```html
<label for="address-line1">Address line 1</label>
<input id="address-line1" …>
```
Use a single label for a single input: don't try to label multiple inputs with only one label. This 
works best for browsers, and best for screenreaders. A tap or click on a label moves focus to the input 
it's associated with, and screenreaders announce label text when the *label* or the label's *input* 
gets focus.

{% Aside 'caution' %}
[Placeholders](https://www.smashingmagazine.com/2018/06/placeholder-attribute/) can be handy, but 
don't use them on their own instead of labels. Once you start entering text in an input, the 
placeholder is hidden, so and it can be easy to forget what the input is for. The same is true if 
you use the placeholder to show the correct format for values such as dates. This can be especially 
problematic for users on phones, particularly if they're distracted or feeling stressed! 
{% endAside %}

#### Make buttons helpful {: #html-button }

Use [`<button>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)
for buttons! You can also use `<input type="submit">`, but there's no point in using a div or some 
other random element acting as a button. Button elements provide accessible behaviour and built-in 
form submission functionality, and can easily be styled. 

Give each form submit button a value that says what it does. For each step towards checkout, use 
a descriptive call-to-action that shows progress and makes the next step obvious. For example, label 
the submit button on your delivery address form **Proceed to Payment** rather than **Continue** 
or **Save**.

Consider disabling a submit button once the user has tapped or clicked it—especially when they're 
making payment or placing an order. Many users click buttons repeatedly, even if they're working fine. 
That can mess up checkout and add to server load. 

On the other hand, don't disable a submit button waiting on complete and valid user input. For 
example, don't just leave a **Save Address** button disabled because something is missing or invalid. 
That doesn't help the user—they may continue to tap or click the button and assume that it's broken. 
Instead, explain to the user what's gone wrong and what they need to do if they attempt to submit 
the form with invalid data. This is particularly important on mobile, where data entry is more 
difficult and missing or invalid form data may not be visible on the user's screen by the time they 
attempt to submit a form.

### Make the most of HTML attributes {: #html-attributes }

#### Make it easy for users to enter data

Use the appropriate input [`type` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/email) 
to provide the right keyboard on mobile and enable basic built-in validation by the browser. 

For example, use `type="email"` for email addresses and `type="tel"` for phone numbers.

{% Aside 'warning' %}
Using type="number" adds an up/down arrow to increment numbers, which makes no sense for data such as 
telephone, payment card or account numbers.

Instead, you should set `type="text"` (or leave off the attribute, since `text` is the default) and 
specify `inputmode="numeric"`, to get a numeric keyboard on mobile. 

To ensure that users get the right keyboard, some sites still use `type="tel"` for payment card 
numbers. However, `inputmode` is [very widely supported now](https://caniuse.com/input-inputmode), 
so you shouldn't have to do that, but check your users' browsers.
{% endAside %}

{: #avoid-custom-elements }

For dates, try to avoid using custom select elements, since they break the autofill experience if not 
properly implemented, and don't work on older browsers. For numbers such as birth year, consider 
using an input element rather than a select, since entering digits manually can be easier and less 
error prone than selecting from a long drop-down list—especially on mobile. Use `inputmode="numeric"` 
to ensure the right keyboard on mobile and add validation and format hints with text or a 
placeholder to make sure the user enters data in the appropriate format.

{% Aside %}
The [datalist](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/datalist) element enables 
a user to select from a list of available options and provides matching suggestions as the user enters text. 
Try out datalist for `text`, `range` and `color` inputs at [simpl.info/datalist](https://simpl.info/datalist).
For birth year input, you can compare a select with an input and datalist at [datalist-select.glitch.me](datalist-select.glitch.me).
{% endAside %}

For payment card and phone numbers use a single input: don't split the number into parts. That makes 
it easier for users to enter data, makes validation simpler, and enables browsers to autofill. 
Consider doing the same for other numeric data such as PIN and bank codes. 

<figure class="w-figure">
  <img src="images/credit-card-number-multiple-inputs.jpg" alt="Screenshot of payment form showing a 
  credit card field split into four input elements.">
  <figcaption class="w-figcaption">Don't use multiple inputs for a credit card number.</figcaption>
</figure>

#### Use autocomplete to improve accessibility and help users avoid re-entering data {: #autocomplete-attribute }

Using appropriate `autocomplete` values enables browsers to help users by securely storing data and 
autofilling `input`, `select` and `textarea` values. This is particularly important on mobile, and 
crucial for avoiding [high form abandonment rates](https://www.zuko.io/blog/8-surprising-insights-from-zukos-benchmarking-data). Autocomplete also provides [multiple accessibility benefits](https://www.w3.org/WAI/WCAG21/Understanding/identify-input-purpose.html).

If an appropriate autocomplete value is available for a form field, you should use it. [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete) has a full list of values and explanations of how to use 
them correctly.

{% Aside %}
As well as using appropriate autocomplete values, help browsers autofill forms by giving form fields 
`name` and `id` attributes [stable values](#stable-name-id) that don't change between page loads or 
website deployments.
{% endAside %}

By default, set the billing address to be the same as the delivery address. Reduce visual clutter by 
providing a link to edit the billing address (or use [`<summary>` and `<details>`](https://simpl.info/details/)) 
rather than displaying the billing address in a form.

![Example checkout page showing link to change billing address](images/review-order.png)

Use appropriate autocomplete values for billing name and address, just as you do for shipping address, 
so the user doesn't have to enter the same data twice. Add a prefix word to autocomplete attributes 
if you have different values for inputs with the same name in different sections.

```html
<input autocomplete="shipping address-line-1" ...>

...

<input autocomplete="billing address-line-1" ...>
```

#### Help users enter the right data {: #validation }

Try to avoid 'telling off' customers because they 'did something wrong'. Instead, help users complete 
forms more quickly and easily by helping them fix problems as they happen. Through the checkout process, 
customers are trying to give your company money for a product or service—your job is to assist them, 
not to punish them!

You can add [constraint attributes](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation#Intrinsic_and_basic_constraints) to specify acceptable values, including 
`min`, `max` and `pattern`.

For example, the following HTML specifies input for a birth year between 1900 and 2020. Using 
`type="number"` constrains input values to numbers only, within the range specified by `min` 
and `max`.

<!-- code example with link to Glitch -->

The following example uses `pattern="[\d ]{10,30}"` to ensure a valid payment card number, while 
allowing spaces:

<!-- code example with link to Glitch-->

The `:valid` and `:invalid` CSS pseudo-classes are set automatically, depending on whether a form 
field's value is valid.

<!-- Add demo with link to Glitch -->

Modern browsers also do basic validation for inputs with type `email` or `url`. 

<!-- Add demo with link to Glitch -->

On form submission, browsers automatically set focus on fields with problematic or missing required 
values. No JavaScript required! 

<figure class="w-figure">
  <img class="w-screenshot" src="./images/invalid-email.png" alt="Screenshot of a sign-in form in Chrome on desktop showing browser prompt and focus for an invalid email value.">
  <figcaption class="w-figcaption">Basic built-in validation by the browser.</figcaption>
</figure>

Alternatively, you may prefer to validate data on form submission and list all fields with invalid or 
missing values and then highlight them accordingly.

You should also use JavaScript to do more robust validation while users are entering data and when 
on form submission. Use the [Constraint Validation API](https://html.spec.whatwg.org/multipage/forms.html#constraints) 
(which is [widely supported](https://caniuse.com/#feat=constraint-validation)) to add custom 
validation using built-in browser UI to set focus and display prompts.

Find out more: [Use JavaScript for more complex real-time validation](https://developers.google.com/web/fundamentals/design-and-ux/input/forms#use_javascript_for_more_complex_real-time_validation).

{% Aside 'warning' %}
Even with client-side validation and data input constraints, you must still ensure that your 
back-end securely handles input and output of data from users.
{% endAside %}

#### Help users avoid missing required data {: #autocomplete-attribute }

Use the [`required` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/required) 
on inputs for mandatory values.

When a form is submitted [modern browsers](https://caniuse.com/mdn-api_htmlinputelement_required) 
automatically prompt and set focus on `required` fields with missing data, and you can use the 
[`:required` pseudo-class](https://caniuse.com/?search=required) to highlight required fields. No 
JavaScript required!

Add an asterisk to the label for every required field, and add a note at the start of the form to 
explain what the asterisk means.

## Simplify checkout {: #checkout-forms }

### Mind the mobile commerce gap! {: #m-commerce-gap}

Imagine that your users have a **fatigue budget**. Use it up, and your users will leave. 

You need to reduce friction and maintain focus, especially on mobile. Many sites get more **traffic** 
on mobile but more **conversions** on desktop—a phenomenon known as the [mobile commerce gap](https://www.comscore.com/Insights/Presentations-and-Whitepapers/2017/Mobiles-Hierarchy-of-Needs). Customers may simply prefer to 
complete a purchase on desktop, but lower mobile conversion rates are also a result of poor user 
experience. Your job is to **minimize** lost conversions on mobile and **maximize** conversions 
on desktop. [Research has shown](https://www.comscore.com/Insights/Presentations-and-Whitepapers/2017/Mobiles-Hierarchy-of-Needs) that there's a huge opportunity to provide a better mobile form experience. 

Most of all, users are more likely to abandon forms that look long, complex and without a sense of 
direction—especially on smaller screens and when users are distracted or in a rush. Ask for as 
little data as possible.

### Make guest checkout the default {: #guest-checkout }

For an online store, the simplest way to reduce form friction is to make guest checkout the default. 
Don't force users to create an account before making a purchase. Not allowing guest checkout is cited 
as a major reason for shopping cart abandonment.

<figure class="w-figure">
  <img class="w-screenshot" src="./images/shopping-cart-abandonment.png" alt="Reasons for shopping 
  cart abandonment during checkout.">
  <figcaption class="w-figcaption">From <a href="https://baymard.com/checkout-usability">baymard.com/checkout-usability</a></figcaption>
</figure>

You can offer account sign-up after checkout.  At that point, you already have most of the data you 
need to set up an account, so account creation should be quick and easy for the user. 

{% Aside 'gotchas' %}
If you do offer sign-up after checkout, make sure that the purchase the user just made is linked to 
their newly created account!
{% endAside %}

### Show checkout progress  {: #checkout-progress }

You can make your checkout process feel less complex by showing progress and making it clear what 
needs to be done next. 

<!--  image like this slide: https://docs.google.com/presentation/d/17nvDI-KX3k3-FyrX7qsAK1lrSN--sFV_E08fAg9DkK4/edit#slide=id.ga666574fc2_1_4  -->

You need to maintain momentum! For each step towards checkout, use descriptive buttons that make 
the next step obvious. For example, label the submit button on your shipping address form 
**Proceed to Payment** rather than **Continue** or **Save**.

<!-- image: like address-form-save.mp4 screencast -->

Use the `enterkeyhint` attribute on form inputs to set the mobile keyboard enter key label. For 
example, use `enterkeyhint="previous"` and `enterkeyhint="next"` within a multi-page form, 
`enterkeyhint="done"` for the final input in the form, and `enterkeyhint="search"` for a search 
input. 

<figure class="w-figure">
  <img class="w-screenshot" src="./images/enterkeyhint.png" alt="Two screenshots of an address form 
  on Android showing how the enterkeyhint input attribute changes the enter key button icon.">
  <figcaption class="w-figcaption">Enter key buttons on Android: 'next' and 'done'.</figcaption>
</figure>

The `enterkeyhint` attribute is [supported on Android and iOS](https://caniuse.com/mdn-html_global_attributes_enterkeyhint). 
You can find out more from the [enterkeyhint explainer](https://github.com/dtapuska/enterkeyhint).

Make it easy for users to go back and forth within the checkout process, to easily adjust 
their order, even when they're at the final payment step. Show full details of the order, not just 
a limited summary.

<!-- image: full summary compared with limited summary -->

Enable users to easily adjust item quantities from the payment page. Your priority at checkout is to 
avoid interrupting progress towards conversion.

### Remove distractions  {: #reduce-checkout-exits}

Limit potential exit points by removing visual clutter and distractions such as product promotions. 

<!-- image as per script -->

Many successful retailers even remove navigation and search from checkout. 

<!-- image as per script -->

Keep the journey focused. This is not the time to tempt users to do something else! 

<!-- image as per script -->

For returning users you can simplify the checkout flow even more by hiding data they don't need to 
see. For example: display the shipping address in plain text (not in a form) and allow users to 
change it via a link. 

<!-- image as per script -->

## Make it easy to enter name and address {: #address-forms}

### Only ask for the data you need {: #unneeded-data}

Before you start coding your name and address forms, make sure to understand what data is required. 
Don't ask for data you don't need! The simplest way to reduce form complexity is to remove unnecessary 
fields. That's also good for customer privacy and can reduce back-end data cost and liability.

### Use a single name input

Allow your users to enter their name using a single input, unless you have a business reason for 
separately storing given names, family names, honorifics or other name parts.

Using a single name input makes forms less complex, enables cut-and-paste, and makes autofill simpler.

In particular, unless you have good reason not to, don't bother adding a separate input for a 
prefix/title (like Mrs, Dr or Lord). Users can type that in with their name if they want to.

### Enable name autofill

Use `name` for a full name:

```html
<input autocomplete="name" ...>
```

If you really do have a good reason to split out name parts, make sure to to use appropriate 
autocomplete values:

* `honorific-prefix`
* `given-name`
* `nickname`
* `additional-name-initial`
* `additional-name`
* `family-name`
* `honorific-suffix`

### Allow international names {: #unicode-matching}

You might want to validate your name inputs, or restrict the characters allowed for name data. However, 
you need to be as unrestrictive as possible with alphabets. It's rude to be told your name is 'invalid'! 

For validation, avoid using regular expressions that only match Latin characters. Latin-only excludes
users with names or addresses that include characters that aren't in the Latin alphabet. Allow Unicode 
letter matching instead—and ensure your back-end supports Unicode securely as both input and output.

<!-- Glitch example as per unicode-letter-matching.mp4: -->

### Allow for a variety address formats {: #address-variety}

When you're designing an address form, bear in mind the bewildering variety of address formats, 
even within a single country. Be careful not to make assumptions about 'normal' addresses. (Take a 
look at [UK Address Oddities!](http://www.paulplowman.com/stuff/uk-address-oddities/) if you're not 
convinced!)

#### Make address forms flexible

Don't force users to [shoe-horn](https://en.wikipedia.org/wiki/Shoehorn#Turn_of_phrase) their address 
into form fields that don't fit.

For example, don't insist on a house number and street name in separate inputs, since many addresses 
don't use that format, and incomplete data can break browser autofill.

Be especially careful with `required` address fields. For example, addresses in large cities in the 
UK do not have a county, but many sites still force users to enter one.

Using two flexible address lines can work well enough for a variety of address formats.  

```html
<input autocomplete="address-line-1" id="address-line1" ...>
<input autocomplete="address-line-2" id="address-line2" ...>
```

Add labels to match:

```html/0-2,5-7
<label for="address-line-1">
Address line 1 (or company name)
</label>
<input autocomplete="address-line-1" id="address-line1" ...>

<label for="address-line-2">
Address line 2 (optional)
</label>
<input autocomplete="address-line-2" id="address-line2" ...>
```

<!-- Add Glitch -->

#### Consider using a single textarea for address {: #address-textarea}

The most flexible option for addresses is to provide a single `textarea`.

The textarea approach fits any address format, and it's great for cutting and pasting—but bear in 
mind that it may not fit your data requirements, and users may miss out on autofill if they previously 
only used forms with address-line1 and address-line2.

For a textarea, use `street-address` as the autocomplete value.

<!-- Glitch address form with textarea -->

#### Internationalize and localize your address forms {: #internationalization-localization} 

It's especially important for address forms to consider [internationalization and localization](https://www.smashingmagazine.com/2020/11/internationalization-localization-static-sites/), depending 
on where your users are located. 

Be aware that the naming of address parts varies as well as address formats, even within the same 
language.

```text
    ZIP code: US
 Postal code: Canada
    Postcode: UK
     Eircode: Ireland
         PIN: India
```

It can be irritating or puzzling to be presented with a form that doesn't fit your address or that 
doesn't the words you expect. 

Customizing address forms for multiple locales may be necessary for your site, but using techniques 
to maximize form flexibility (as described above) may be adequate. If you don't localize your address 
forms, make sure to understand the key priorities to cope with a range of address formats:
* Avoid being over-specific about address parts, such as insisting on a street name or house number.
* Where possible avoid making fields `required`. For example, addresses in many countries don't 
have a postal code, and rural addresses may not have a street or road name.
* Use inclusive naming: 'Country/region' not 'Country'; 'ZIP/postal code' not 'ZIP'.

Keep it flexible!

Here is an example of an address form that can work well in many locations.

<!-- Glitch example -->


#### Consider avoiding postal code address lookup {: #postal-code-address-lookup}

Some websites use a service to look up addresses based on postal code or ZIP. This may be sensible 
for some use cases, but you should be aware of the potential downsides.

Postal code address suggestion doesn't work for all countries—and in some regions, post codes can 
include a large number of potential addresses. 

<!-- image as per long-list-of-addresses.mp4 -->

It's difficult for users to select from a long list of addresses—especially on mobile if they're 
rushed or stressed. It can be easier and less error prone to let users take advantage of autofill, 
and enter their complete address filled with a single tap or click.

<!-- video of image as per full-name-autofill.mp4:  -->

* Use [appropriate payment card autocomplete values](#payment-form-autocomplete).
* Use a [single input for payment card numbers](#single-credit-card-input).
* [Avoid using custom elements](#avoid-custom-elements) that break the autofill experience.
* [Test in the field as well as the lab](#analytics-rum): page analytics, interaction analytics, and 
real-user performance measurement.
* [Test across browsers, devices and platforms](#test-platforms). 

## Simplify payment forms {: #general-guidelines }

Payment forms are the single most critical part of the checkout process. Poor payment form design is 
a [common cause of shopping cart abandonment](https://www.comscore.com/Insights/Presentations-and-Whitepapers/2017/Mobiles-Hierarchy-of-Needs). The [devil's in the details](https://en.wikipedia.org/wiki/The_devil_is_in_the_detail#cite_note-Titelman-1): small 
glitches can tip users towards abandoning a purchase, particularly on mobile. Your job is to design 
forms to make it as easy as possible for users to enter data.

### Help users avoid re-entering payment data {: #payment-form-autocomplete}

Make sure to add appropriate `autocomplete` values in payment card forms, including the payment card 
number, name on the card, and the expiry month and year:
* cc-number
* cc-name
* cc-exp-month
* cc-exp-year

This enables browsers help users by securely storing payment card details and correctly entering form 
data. Without autocomplete, users may be more likely to keep a physical record of payment card details, 
or store payment card data insecurely on their device.

{% Aside 'caution' %}
Don't add a selector for payment card type, since this can always be inferred from the payment card 
number.
{% endAside %}

### Avoid using custom elements for payment card dates

Custom elements can interrupt payment flow by breaking autofill, and don't work on older browsers. 
If all other payment card details are available from autocomplete but a user is forced to find their 
physical payment card to look up an expiry date, you're likely to lose a sale. Use standard HTML 
elements instead, and style them accordingly.

<!-- image -->

### Use a single input for payment card numbers

Some sites add multiple inputs for payment card and phone numbers. Don't do that! Use a single input 
instead. That makes it easier for users to type in data, and it enables browsers to autofill.

<!-- image -->

### Validate carefully

You should validate data entry both in realtime and before form submission. One way to do this is by 
adding a `pattern` attribute to a payment card input. If the user attempts to submit the payment form 
with an invalid value, the browser displays a warning message and sets focus on the input. No 
JavaScript required! 

<!-- Show Glitch -->

However, your `pattern` regular expression must be flexible enough to handle [the range of payment card 
number lengths](https://github.com/jaemok/credit-card-input/blob/master/creditcard.js#L35): from 14 
digits (or possibly less) to 20 (or more). You can find out more about payment card number structuring 
from [LDAPwiki](https://ldapwiki.com/wiki/Bank%20Card%20Number) 

Allow users to include spaces when they're entering a new payment card number, since this is how 
numbers are displayed on physical cards. That's friendlier to the user (you won't have to tell them 
'they did something wrong'), less likely to interrupt conversion flow, and it's straightforward to 
remove spaces in numbers before processing.

{% Aside %}
You may want to use a one-time passcode for identity or payment verification. However, asking users 
to manually enter a code or copy it from an email or an SMS text is error-prone and a source of friction. 
You can find out better ways to enable one-time passcodes from our article [SMS OTP form best practices](/sms-otp-form).
{% endAside %}

## Keep learning {: #resources }

* [Create Amazing Forms](https://developers.google.com/web/fundamentals/design-and-ux/input/forms)
* [Best Practices For Mobile Form Design](https://www.smashingmagazine.com/2018/08/best-practices-for-mobile-form-design/)
* [More capable form controls](/more-capable-form-controls)
* [Creating Accessible Forms](https://webaim.org/techniques/forms/)
* [Streamlining the Sign-up Flow Using Credential Management API](https://developers.google.com/web/updates/2016/04/credential-management-api)
* [Verify phone numbers on the web with the Web OTP API](https://web.dev/web-otp/)

Photo by [@rupixen](https://unsplash.com/@rupixen) on [Unsplash](https://unsplash.com/photos/Q59HmzK38eQ).