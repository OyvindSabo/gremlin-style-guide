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

### Vertically align unnested methods after initial node selection

```Java
// Bad
g.V().hasLabel('movie').values('year').min()

// Bad
g.V()
  .hasLabel('movie')
  .values('year')
  .min()

// Good
g.V().hasLabel('movie')
     .values('year')
     .min()
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
