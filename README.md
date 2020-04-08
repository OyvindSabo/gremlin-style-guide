# The Gremlin Style Guide

A style guide for the Gremlin query language

![Gremlint AMC Gremlin 1920x1080.png](https://cdn.steemitimages.com/DQmSYAxsBQJPxfxzA5HFXzsPSmTTQiNGoeWY143WXpjLQ4X/Gremlint%20AMC%20Gremlin%201920x1080.png)

### Use soft tabs (spaces) for indentation

This ensures that your code looks the same for anyone, regardless of their text editor settings.

```Java
// Bad - indented using hard tabs
g.V().
  hasLabel('person').as('person').
  properties('location').as('location').
  select('person','location').
    by('name').
    by(valueMap())

// Good - indented using spaces
g.V().
∙∙hasLabel('person').as('person').
∙∙properties('location').as('location').
∙∙select('person','location').
∙∙∙∙by('name').
∙∙∙∙by(valueMap())
```

### Use two spaces for indentation

Two spaces makes the purpose of the indent clear, but does not waste too much space. Of course, more spaces are allowed when indenting from an already indented block of code.

```Java
// Bad - Indented using four spaces
g.V().
    hasLabel('person').as('person').
    properties('location').as('location').
    select('person','location').
        by('name').
        by(valueMap())

// Good - Indented using two spaces
g.V().
  hasLabel('person').as('person').
  properties('location').as('location').
  select('person','location').
    by('name').
    by(valueMap())
```

### Use indents wisely

- No newline should ever have the same indent as the line starting with the traversal source g.
- Use indents when the step in the new line is a modulator of a previous line.
- Use indents when the content in the new line is an argument of a previous step.
- If multiple anonymous traversals are passed as arguments to a function, each newline which is not the first of line of the step should be indented to make it more clear where the distinction between each argument goes. If this is the case, but the newline would already be indented because the step in the content in the new line is the argument of a previous step, there is no need to double-indent.
- Don't be tempted to add extra indentation to vertically align a step with a step in a previous line.

```Java
// Bad - No newline should have the same indent as the line starting with the traversal source g
g.V().
group().
by().
by(bothE().count())

// Bad - Modulators of a step on a previous line should be indented
g.V().
  group().
  by().
  by(bothE().count())

// Good
g.V().
  group().
    by().
    by(bothE().count())

// Bad - You have ignored the indent rules to achieve the temporary satisfaction of vertical alignment
g.V().local(union(identity(),
                  bothE().count()).
            fold())

// Good
g.V().
  local(
    union(
      identity(),
      bothE().count()).
    fold())

// Bad - When multiple anonymous traversals are passed as arguments to a function, each newline which is not the first of line of the step should be indented to make it more clear where the distinction between each argument goes.
g.V().
  has('person','name','marko').
  fold().
  coalesce(
    unfold(),
    addV('person').
    property('name','marko').
    property('age',29))

// Good - We make it clear that the coalesce step takes two traversals as arguments
g.V().
  has('person','name','marko').
  fold().
  coalesce(
    unfold(),
    addV('person').
      property('name','marko').
      property('age',29))
```

### Add linebreak after punctuation if the query contains nested methods

While adding the linebreak before the punctuation looks good in most cases, it introduces alignment problems when not all lines start with a punctuation. You never know if the next line should be indented relative to the punctuation of the previous line or the method of the previous line. Adding the punctuation before the linebreak also means that you can know if you have reached the end of the query without reading the next line.

```Java
// Bad
g.V().by(
       inE('category')
       .count())

// Bad
g.V().by(
       inE('category')
      .count())

// Bad
g.V().by(
        inE('category')
        .count())

// Bad
g.V().by(
        inE('category')
       .count())

// Good
g.V().by(
        inE('category').
        count())
```

### Add linebreak and indentation of two spaces for nested methods

```Java
// Bad
g.V().hasLabel('movie').
      as('a','b').
      where(inE('rated').count().is(10))

// Bad
g.V().hasLabel('movie').
      as('a','b').
      where(
      inE('rated').
      count().
      is(10))

// Good
g.V().hasLabel('movie').
      as('a','b').
      where(
        inE('rated').
        count().
        is(10))
```

### Place all trailing parentheses on a single line instead of distinct lines

```Java
// Bad
g.V().hasLabel('movie').
      by(
        inE('rated').
        values('stars').
        mean()
      ).
      order().
      limit(10)

// Good
g.V().hasLabel('movie').
      by(
        inE('rated').
        values('stars').
        mean()).
      order().
      limit(10)
```

### Use `//` for single line comments. Place single line comments on a newline above the subject of the comment.

```Java
// Bad
g.V().hasLabel('movie')
     .values('year') // Find the year in which the oldest movie was produced
     .min()

// Good
g.V().hasLabel('movie')
     // Find the year in which the oldest movie was produced
     .values('year')
     .min()
```

### Use single quotes for strings

Because minimalism

```Java
// Bad
g.V().hasLabel("foo")

// Good
g.V().hasLabel('foo')
```

### Write idiomatic Gremlin code

If there is a simpler way, do it the simpler way. Use the Gremlin methods for what they're worth.

```Java
// Bad
g.V().outE()
     .inV()

// Good
g.V().out()


// Bad
g.V().has('name', 'alice')
     .outE()
     .hasLabel('bought')
     .inV()
     .values('name')

// Good
g.V().has('name','alice')
     .out('bought')
     .values('name')


// Bad
g.V().hasLabel('foo')
     .has('name', 'bar')

// Good
g.V().has('foo', 'name', 'bar')
```
