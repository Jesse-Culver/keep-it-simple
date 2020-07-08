---
title: "Page Headers Explained"
layout: post
author: "Someone Different"
publish_date: "6/27/2020"
last_modified: ""
---

Each blog post has a header that has values that can be populated. Filling in or leaving these values blank has different effects on the blog post.

If a value you can be left blank it still needs to be listed and it needs to be kept "". The logic that helps generate the pages is looking for "" and if it doesn't see that it thinks you want to render.

```
title: "Example Post With a Different Author"
layout: post
author: "Someone Different"
publish_date: "6/27/2020"
last_modified: ""
```

{% include heading.html heading="Title" size="large" %}

The title value sets the web pages name in the vistor's web browser tab and it also sets the name that shows up in the post listing on the home page. In addition it of course sets the name at the very top of the post.

{% include heading.html heading="Layout" size="large" %}

The layout value sets the layout you wish to use for the page. Normally for a blog post you will want to use "post". Other pages like about and contact will use the "page" layout. Check out the layouts folder for some that are available and feel free to create your own. Keep in mind that layouts can be nested in other layouts. As an example if you open up the post.html or page.html you will see at the top that they are using the layout default.

If we look at default.html we will see

{%raw%}
```html
---
---

<!doctype html>

<html lang="en">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <head>
    <title>{{page.title}}</title> <!--This is the part that determines the page title on the tab in a web browser-->
    {% include head.html %}
  </head>
  <style>
    body {background-color: {{site.background-color}};}
  </style>
  <body>
    {{ content }}
    {% include footer.html %}
  </body>
</html>
```
{%endraw%}

The way this works is jekyll will first create the webpage using this layout then any other layout that uses this is getting it's content inserted where {%raw%}{{ content }}{%endraw%} is.

In general unless you are looking to customize this layout more you will not need to worry about this.

{% include heading.html heading="Author" size="large" %}

The author value is there if you have a site with multiple writers. If you leave this value blank then the author line will not be added.

{% include heading.html heading="Publish and Last Modified Dates" size="large" %}

The published date not only displays at the top of the page but it is also the value used to sort the posts on your homepage. Last modified date is an optional value you can have and when filled in will display the last modified date at the top so your reader knows when it was updated last. See the [Example Code Blocks](/2020/06/12/ExampleCodeBlocks.html) page for an example.