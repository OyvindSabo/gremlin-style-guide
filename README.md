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