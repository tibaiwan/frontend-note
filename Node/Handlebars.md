# Handlebars 常用语法

[handlebars 官网](https://handlebarsjs.com/)

### 简单示例
```
<div class="entry">
  <h1>{{title}}</h1>
  <h1>{{article.title}}</h1>
  <h1>{{article/title}}</h1>
  <div class="body">
    {{{body}}}
  </div>
</div>
```
Note: If you don't want Handlebars to escape a value, use the "triple-stash", {{{

### HTML 转义
template:
```
<div class="entry">
  <h1>{{title}}</h1>
  <div class="body">
    {{{body}}}
  </div>
</div>
```
context:
```
{
  title: "All about <p> Tags",
  body: "<p>This is a post about &lt;p&gt; tags</p>"
}
```
results:
```
<div class="entry">
  <h1>All About &lt;p&gt; Tags</h1>
  <div class="body">
    <p>This is a post about &lt;p&gt; tags</p>
  </div>
</div>
```
escape parameters:
```
Handlebars.registerHelper('link', function(text, url) {
  text = Handlebars.Utils.escapeExpression(text);
  url  = Handlebars.Utils.escapeExpression(url);
  var result = '<a href="' + url + '">' + text + '</a>';
  return new Handlebars.SafeString(result);
});
```

## 块级表达式
context:

```
{
  people: [
    {firstName: "Yehuda", lastName: "Katz"},
    {firstName: "Carl", lastName: "Lerche"},
    {firstName: "Alan", lastName: "Johnson"}
  ]
}
```
template:
```
{{#list people}}{{firstName}} {{lastName}}{{/list}}
```
Create a helper named list to generate our HTML list
```
Handlebars.registerHelper('list', function(items, options) {
  var out = "<ul>";

  for(var i=0, l=items.length; i<l; i++) {
    out = out + "<li>" + options.fn(items[i]) + "</li>";
  }

  return out + "</ul>";
});
```

### 模板内备注
template:

```
<div class="entry">
  {{!-- only output author name if an author exists --}}
  {{#if author}}
    <h1>{{author.firstName}} {{author.lastName}}</h1>
  {{/if}}
</div>
```
```
<div class="entry">
  {{! This comment will not be in the output }}
  <!-- This comment will be in the output -->
</div>
```
### Helpers
Handlebars helpers can be accessed from any context in a template. You can register a helper with the Handlebars.registerHelper method.

template:
```
<div class="post">
  <h1>By {{fullName author}}</h1>
  <div class="body">{{body}}</div>

  <h1>Comments</h1>

  {{#each comments}}
  <h2>By {{fullName author}}</h2>
  <div class="body">{{body}}</div>
  {{/each}}
</div>
```
context and helpers:
```
var context = {
  author: {firstName: "Alan", lastName: "Johnson"},
  body: "I Love Handlebars",
  comments: [{
    author: {firstName: "Yehuda", lastName: "Katz"},
    body: "Me too!"
  }]
};

Handlebars.registerHelper('fullName', function(person) {
  return person.firstName + " " + person.lastName;
});
```

### Partials
Handlebars partials allow for code reuse by creating shared templates.

template:
```
<div class="post">
  {{> userMessage tagName="h1" }}

  <h1>Comments</h1>

  {{#each comments}}
    {{> userMessage tagName="h2" }}
  {{/each}}
</div>
```
using this partial and context:
```
Handlebars.registerPartial('userMessage',
    '<{{tagName}}>By {{author.firstName}} {{author.lastName}}</{{tagName}}>'
    + '<div class="body">{{body}}</div>');
var context = {
  author: {firstName: "Alan", lastName: "Johnson"},
  body: "I Love Handlebars",
  comments: [{
    author: {firstName: "Yehuda", lastName: "Katz"},
    body: "Me too!"
  }]
};
```

### Built-In Helpers

> if: If its argument returns false, undefined, null, "", 0, or [], Handlebars will not render the block.
```
<div class="entry">
  {{#if author}}
    <h1>{{firstName}} {{lastName}}</h1>
  {{else}}
    <h1>Unknown Author</h1>
  {{/if}}
</div>
```
> Each: You can iterate over a list using the built-in each helper.
```
<ul class="people_list">
  {{#each people}}
    <li>{{this}}</li>
  {{/each}}
</ul>
```
You can optionally provide an {{else}} section which will display only when the list is empty.
```
{{#each paragraphs}}
  <p>{{this}}</p>
{{else}}
  <p class="empty">No content</p>
{{/each}}
```
When looping through items in each, you can optionally reference the current loop index via {{@index}}
```
{{#each array}}
  {{@index}}: {{this}}
{{/each}}
```
Additionally for object iteration, {{@key}} references the current key name
```
{{#each object}}
  {{@key}}: {{this}}
{{/each}}
````
> with

context:
```
{
  title: "My first post!",
  author: {
    firstName: "Charles",
    lastName: "Jolley"
  }
}
```
template:
```
<div class="entry">
  <h1>{{title}}</h1>

  {{#with author}}
  <h2>By {{firstName}} {{lastName}}</h2>
  {{/with}}
</div>
```
define known references:
```
<div class="entry">
  <h1>{{title}}</h1>

  {{#with author as |myAuthor|}}
  <h2>By {{myAuthor.firstName}} {{myAuthor.lastName}}</h2>
  {{/with}}
</div>
```
