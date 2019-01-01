

# The Gremlin Style Guide
A style guide for the Gremlin query language

### Use soft tabs (spaces) for indentation
This ensures that your code looks the same for anyone, regardless of their text editor settings.
```Java
// Bad
g.V().label().
      groupCount() // Indented using hard tabs

// Good
g.V().label().
∙∙∙∙∙∙groupCount() // indented using spaces
```

### Vertically align unnested methods after initial node selection
```Java
// Bad
g.V().hasLabel('movie').values('year').min()

// Bad
g.V().
  hasLabel('movie').
  values('year').
  min()

// Good
g.V().hasLabel('movie').
      values('year').
      min()
```

### Add linebreak after punctuation
While adding the linebreak before the punctuation looks good in most cases, it introduces alignment problems when not all lines start with a punctuation. You never know if the next line should be indented relative to the punctuation of the previous line or the method of the previous line.
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