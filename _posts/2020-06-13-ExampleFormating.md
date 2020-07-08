---
title: "Example Text Formatting Post"
layout: post
author: ""
publish_date: "6/13/2020"
last_modified: "6/27/2020"
---

This is a series of examples of how to do markup. For reference Keep it Simple uses [kramdown](https://kramdown.gettalong.org) which is a superset of Markdown. Think of it as version of Markdown with a lot of extra features.


# Header Level 1

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Velit sed ullamcorper morbi tincidunt ornare massa.

## Header Level 2

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Velit euismod in pellentesque massa placerat duis ultricies lacus sed.

### Header Level 3

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Amet risus nullam eget felis eget nunc lobortis mattis aliquam.

~~~markdown
# Header Level 1
## Header Level 2
### Header Level 3
~~~

{% include heading.html heading="Example XX-Large Heading" size="xx-large" %}

{% include heading.html heading="Example X-Large Heading" size="x-large" %}

{% include heading.html heading="Example Large Heading" size="large" %}

{% raw %}
~~~liquid
{% include heading.html heading="Example XX-Large Heading" size="xx-large" %}
{% include heading.html heading="Example X-Large Heading" size="x-large" %}
{% include heading.html heading="Example Large Heading" size="large" %}
~~~
{% endraw %}

{% include heading.html heading="Example Blockquote" size="large" %}

> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec in porttitor mauris. Pellentesque non feugiat mi, a facilisis felis. Curabitur ultricies tortor et urna malesuada aliquet. Pellentesque sagittis viverra est, id ornare libero vehicula elementum. Etiam vel tortor eu erat euismod accumsan. Phasellus facilisis lectus ut purus eleifend placerat. Sed non bibendum metus, vel tempus sapien. Aenean et massa ultrices nulla hendrerit fringilla. Aenean ante ex, sollicitudin pretium velit non, varius feugiat nibh. Maecenas id libero dolor. Duis finibus vitae lacus sed venenatis. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Quisque blandit nisi eget neque congue placerat. 
>
> <p style="text-align: right;">-Lorem Ipsum</p>

~~~markdown
> Quisque blandit nisi eget neque congue placerat.
>
> <p style="text-align: right;">-Lorem Ipsum</p>
~~~

{% include heading.html heading="Unordered List" size="large" %}

* Apples
* Oranges
* Grapes
* Pears

~~~markdown
* Apples
* Oranges
* Grapes
* Pears
~~~

{% include heading.html heading="Ordered List" size="large" %}

Ordered lists are interesting in that the actual number in front does not matter as long as it is a number followed by a period and a space.

1. First
2. Second
3. Third

---

1. First
3. Second
2. Third

~~~markdown
1. First
2. Second
3. Third

---

1. First
3. Second
2. Third
~~~

{% include heading.html heading="Linking" size="large" %}

Adding links is easy. Surround the word(s) with square brackets and follow with parenthese.

[Example](https://en.wikipedia.org/wiki/Main_Page)

```markdown
[Example](https://en.wikipedia.org/wiki/Main_Page)
```

If you wish to link to a page on your website you can [do it like so.](/about.html)

```markdown
If you wish to link to a page on your website you can [do it like so.](/about.html)
```

{% include heading.html heading="Indented Lists" size="large" %}

Indenting lists is a bit odd with kramdown. Pay special attention to the example markdown code. The symbol has to align with the first letter of the previous level. Keep in mind the number of spaces your tab is equivalent to in your editor!

This means for an unordered list:
* level 1 has 0 spaces in front
* level 2 has 2 space in front
* level 3 has 3 spaces in front

This means for an ordered list:
* level 1 has 0 spaces in front
* level 2 has 3 space in front
* level 3 has 6 spaces in front

---

* Fruit
  * Apples
    * Green Apples
  * Pears
* Vegetables
  * Green Beans
  * Peas

1. Favorite Drinks
   1. Coffee
   2. Tea
      1. Green Tea
      2. Earl Grey
   3. Water
2. Favorite Food
   1. Pizza
   2. Hot Dogs
   3. Hamburgers

~~~markdown
* Fruit
  * Apples
    * Green Apples
  * Pears
* Vegetables
  * Green Beans
  * Peas

1. Favorite Drinks
   1. Coffee
   2. Tea
      1. Green Tea
      2. Earl Grey
   3. Water
2. Favorite Food
   1. Pizza
   2. Hot Dogs
   3. Hamburgers
~~~

{% include heading.html heading="Definition" size="large" %}

{:#loremipsum}Lorem Ipsum Definition
: Integer luctus sodales augue vel suscipit. Integer non egestas erat, eu gravida lacus. Donec porttitor dignissim magna, tristique lacinia lorem. Aliquam tristique gravida accumsan. Vestibulum ultricies fringilla tincidunt. Aenean sed dui magna. Nam aliquet mollis arcu vitae pretium. Maecenas et felis eget est consequat elementum. Quisque ut velit sit amet sapien feugiat suscipit in semper turpis. Phasellus iaculis imperdiet diam.

~~~markdown
{:#loremipsum}Lorem Ipsum Definition
: Integer luctus sodales augue vel suscipit. Integer non egestas erat, eu gravida lacus. Donec porttitor dignissim magna, tristique lacinia lorem. Aliquam tristique gravida accumsan. Vestibulum ultricies fringilla tincidunt. Aenean sed dui magna. Nam aliquet mollis arcu vitae pretium. Maecenas et felis eget est consequat elementum. Quisque ut velit sit amet sapien feugiat suscipit in semper turpis. Phasellus iaculis imperdiet diam.
~~~

The {%raw%}{:#loremipsum}{%endraw%} defines a tag you can link to in order to jump around the page

[Click to jump to the Lorem Ipsum Definition]({{page.url}}#loremipsum)

{% include heading.html heading="Reference Linking" size="large" %}

[Refrence style links](https://kramdown.gettalong.org/syntax.html#reference-links) are useful when you are using the same link over and over again or if you want to just be able to update all your links in one spot.

This is a [reference style link][linkid] to a page. And [this][linkid] is also a link. As is [this][] and [THIS].

[linkid]: http://www.example.com/ "Optional Title"
[this]: https://www.google.com "Google"

~~~markdown
This is a [reference style link][linkid] to a page. And [this][linkid] is also a link. As is [this][] and [THIS].

[linkid]: http://www.example.com/ "Optional Title"
[this]: https://www.google.com "Google"
~~~

{% include heading.html heading="Footnotes" size="large" %}

Footnotes are an interesting kramdown feature that lets you define footnotes anywhere you like and when the page is generated all footnote defenitions are moved down to the bottom of the page and refrence links are made so your reader can jump to the footnote.

This is some text.[^1] Other text.[^footnote] Even more text.[^1]

[^1]: Some *crazy* footnote definition.

[^footnote]:
    > Blockquotes can be in a footnote.

        as well as code blocks

    or, naturally, simple paragraphs.

~~~markdown
This is some text.[^1] Other text.[^footnote] Even more text.[^1]

[^1]: Some *crazy* footnote definition.

[^footnote]:
    > Blockquotes can be in a footnote.

        as well as code blocks

    or, naturally, simple paragraphs.
~~~

{% include heading.html heading="Abbreviations" size="large" %}

Hover over the text that is underlined to see the full name of the [abbreviations.](https://kramdown.gettalong.org/syntax.html#abbreviations)

This is some text not written in HTML but in another language!

*[another language]: It's called Markdown

*[HTML]: HyperTextMarkupLanguage
{:.mega-big}

~~~markdown
This is some text not written in HTML but in another language!

*[another language]: It's called Markdown

*[HTML]: HyperTextMarkupLanguage
{:.mega-big}
~~~

{% include heading.html heading="Twitter" size="large" %}

Embedding a tweet is easy! Just go to [https://publish.twitter.com/](https://publish.twitter.com/) and paste in the link to the tweet you want to embedd.

<center>
<blockquote class="twitter-tweet" data-dnt="true" data-theme="light"><p lang="en" dir="ltr">I was considering the question &quot;Is a modern CPU fast enough for essentially all 2D gfx without acceleration&quot; and I realized that I don&#39;t actually know what hardware interface is used nowadays on desktops to render GUIs. Android draws everything with OpenGL ES, building vertex \</p>&mdash; John Carmack (@ID_AA_Carmack) <a href="https://twitter.com/ID_AA_Carmack/status/1272550808488419332?ref_src=twsrc%5Etfw">June 15, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 
</center>

{% include heading.html heading="Raw HTML" size="large" %}

You can even use raw HTML in your posts if you wish.

<p style="color:red">Integer vitae felis quis turpis varius volutpat.</p>

<audio controls>
  <source src="/assets/sound/BWV_543-prelude.ogg" type="audio/ogg">
  <source src="/assets/sound/BWV_543-prelude.mp3" type="audio/mp3">
  Your browser does not support the audio tag.
</audio>
Prelude from Prelude and Fugue in a minor, BWV 543, from Johann Sebastian Bach. Performed by Noah Horn

Hi my name is <ruby>Jesse<rt>Jes·​se</rt></ruby>. I think <ruby>onomamtopeia<rt>on·​o·​mato·​poe·​ia</rt></ruby>'s are fun.

<button type="button" onclick="alert('Hello world!')">Click Me!</button>

{%raw%}
```html
<p style="color:red">Integer vitae felis quis turpis varius volutpat.</p>

<audio controls>
  <source src="/assets/sound/BWV_543-prelude.ogg" type="audio/ogg">
  <source src="/assets/sound/BWV_543-prelude.mp3" type="audio/mp3">
  Your browser does not support the audio tag.
</audio>
Prelude from Prelude and Fugue in a minor, BWV 543, from Johann Sebastian Bach. Performed by Noah Horn

Hi my name is <ruby>Jesse<rt>Jes·​se</rt></ruby>. I think <ruby>onomamtopeia<rt>on·​o·​mato·​poe·​ia</rt></ruby>'s are fun.

<button type="button" onclick="alert('Hello world!')">Click Me!</button>
```
{%endraw%}
Donec tempor justo quis est varius, id faucibus ex viverra. Fusce elementum tempus dolor nec interdum. Aliquam erat volutpat. Ut faucibus justo vitae convallis luctus. Praesent ut quam vitae nisi mattis fermentum. In vitae suscipit arcu. Nulla aliquet velit libero, eget iaculis nisl sollicitudin sit amet.

Quisque sit amet enim iaculis, mattis ex et, mollis dui. Aenean pretium nec dolor id vehicula. Curabitur eu nunc tortor. In hac habitasse platea dictumst. Nam massa purus, ornare non varius ac, iaculis quis felis. Ut rutrum diam dui, rhoncus pretium eros viverra vitae. Pellentesque eu tortor augue. Mauris eu nunc ut risus tincidunt facilisis. Vivamus nec tempor turpis. Sed a nulla non risus mattis dictum non sit amet nisi. Maecenas non arcu magna. Nulla non odio gravida, iaculis urna in, euismod neque. Maecenas facilisis risus ligula, venenatis consequat nulla euismod quis. Ut vel nisi ut ligula dictum porta quis sed lacus. 

{% include heading.html heading="Footnote Refrences" size="large" %}