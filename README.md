# The Gremlin Style Guide

An unofficial style guide for the Gremlin query language

![Gremlint AMC Gremlin 1920x1080.png](https://cdn.steemitimages.com/DQmSYAxsBQJPxfxzA5HFXzsPSmTTQiNGoeWY143WXpjLQ4X/Gremlint%20AMC%20Gremlin%201920x1080.png)

## Introduction

Having written a lot of Gremlin over the past years, I've built up some preferences about how queries should be formatted to be readable and beautiful. In this guide I have tried documenting some of these preferences.

One of the purposes of this style guide is to serve as guidance when working on [Gremlint](https://github.com/OyvindSabo/gremlint), a code formatter for Gremlin.

I would also be happy to see it used as an arena for discussions around best practices for Gremlin query formatting. If you see a rule you don't like or would like to have added, create an issue so we can get the discussion going.:smiley:

The examples used in this style guide are largely based on the [Apache TinkerPop Traversal Recipes](https://tinkerpop.apache.org/docs/current/recipes/) and the examples found in the [Apache TinkerPop Documentation](http://tinkerpop.apache.org/docs/3.4.6/reference/), but may differ in formatting.

The style guide draws inspiration from [The Clojure Style Guide](https://github.com/bbatsov/clojure-style-guide/blob/master/README.adoc), [The Cypher Style Guide](https://neo4j.com/developer/cypher-style-guide/) and of course [TinkerPop's own Gremlin style guide](https://tinkerpop.apache.org/docs/current/recipes/#style-guide).

## Rules

### Break long queries into multiple lines

What is considered too long depends on your application.

When breaking the query, not all parts of the traversal have to be broken up. First, divide the query into logical groups, based on which steps belong naturally together. For instance, every set of steps which end with an as()-step often belong together, as they together form a new essential step in the query.

If anoymous traversals are passed as arguments to another step, like a filter()-step, and it's causing the line to be too long, first split the line at the commas. Only if the traversal arguments are still too long, consider splitting them further.

```java
// Good (80 characters max width)
g.V().hasLabel('person').where(outE("created").count().is(P.gte(2))).count()

// Good (50 characters max width)
g.V().
  hasLabel('person').
  where(outE("created").count().is(P.gte(2))).
  count()

// Good (30 characters max width)
g.V().
  hasLabel('person').
  where(
    outE("created").
    count().
    is(P.gte(2))).
  count()
```

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

Two spaces makes the intent of the indent clear, but does not waste too much space. Of course, more spaces are allowed when indenting from an already indented block of code.

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
- If multiple anonymous traversals are passed as arguments to a function, each newline which is not the first step of the traversal should be indented to make it more clear where the distinction between each argument goes. If this is the case, but the newline would already be indented because the step in the content in the new line is the argument of a previous step, there is no need to double-indent.
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

### Keep as()-steps at the end of each line

The end of the line is a natural place to assign a label to a step. It's okay if the as()-step is in the middle of the line if there are multiple consecutive label assignments, or if the line is so short that a newline doesn't make sense. Maybe a better way to put it is to not start a line with an as()-step, unless you're using it inside a match()-step of course.

```java
// Bad
g.V().
  as('a').
  out('created').
  as('b').
  select('a','b')

// Good
g.V().as('a').
  out('created').as('b').
  select('a','b')

// Good
g.V().as('a').out('created').as('b').select('a','b')
```

### Add linebreak after punctuation, not before

While adding the linebreak before the punctuation looks good in most cases, it introduces alignment problems when not all lines start with a punctuation. You never know if the next line should be indented relative to the punctuation of the previous line or the method of the previous line. Switching between having the punctuation at the start or the end of the line depending on whether it works in a particular case requires much brainpower (which we don't have), so it's better to be consistent. Adding the punctuation before the linebreak also means that you can know if you have reached the end of the query without reading the next line.

```Java
// Bad - Looks okay, though
g.V().has('name','marko')
     .out('knows')
     .has('age', gt(29))
     .values('name')

// Good
g.V().
  has('name','marko').
  out('knows').
  has('age', gt(29)).
  values('name')

// Bad - Punctuation at the start of the line makes the transition from filter to select to count too smooth
g.V()
  .hasLabel("person")
  .group()
    .by(values("name", "age").fold())
  .unfold()
  .filter(
    select(values)
    .count(local)
    .is(gt(1)))

// Good - Keeping punctuation at the end of each line, more clearly shows the query structure
g.V().
  hasLabel("person").
  group().
    by(values("name", "age").fold()).
  unfold().
  filter(
    select(values).
    count(local).
    is(gt(1)))
```

### Add linebreak and indentation for nested traversals which are long enough to span multiple lines

```Java
// Bad - Not newlining the first argument of a function whose arguments span over multipe lines causes the arguments to not align.
g.V().
  hasLabel("person").
  groupCount().
    by(values("age").
      choose(is(lt(28)),
        constant("young"),
        choose(is(lt(30)),
          constant("old"),
          constant("very old"))))

// Bad - We talked about this in the indentation section, didn't we?
g.V().
  hasLabel("person").
  groupCount().
    by(values("age").
       choose(is(lt(28)),
              constant("young"),
              choose(is(lt(30)),
                     constant("old"),
                     constant("very old"))))

// Good
g.V().
  hasLabel("person").
  groupCount().
    by(
      values("age").
      choose(
        is(lt(28)),
        constant("young"),
        choose(
          is(lt(30)),
          constant("old"),
          constant("very old"))))
```

### Place all trailing parentheses on a single line instead of distinct lines

Aligning the end parenthesis with the step to which the start parenthesis belongs might make it easier to check that the number of parentheses is correct, but looks ugly and wastes a lot of space.

```Java
// Bad
g.V().
  hasLabel("person").
  groupCount().
    by(
      values("age").
      choose(
        is(lt(28)),
        constant("young"),
        choose(
          is(lt(30)),
          constant("old"),
          constant("very old")
        )
      )
    )

// Good
g.V().
  hasLabel("person").
  groupCount().
    by(
      values("age").
      choose(
        is(lt(28)),
        constant("young"),
        choose(
          is(lt(30)),
          constant("old"),
          constant("very old"))))
```

### Use `//` for single line comments. Place single line comments on a newline above the subject of the comment.

```Java
// Bad
g.V().
  has('name','alice').out('bought'). // Find everything that Alice has bought
  in('bought').dedup().values('name') // Find everyone who have bought some of the same things as Alice

// Good
g.V().
  // Find everything that Alice has bought
  has('name','alice').out('bought').
  // Find everyone who have bought some of the same things as Alice
  in('bought').dedup().values('name')
```

### Use single quotes for strings

Use single quotes for literal string values. If the string contains double quotes or single quotes, surround the string with the type of quote which creates the fewest escaped characters.

```Java
// Bad - Use single quotes where possible
g.V().has("Movie", "name", "It's a wonderful life")

// Bad - Escaped single quotes are even worse than double quotes
g.V().has('Movie', 'name', 'It\'s a wonderful life')

// Good
g.V().has('Movie', 'name', "It's a wonderful life")
```

### Write idiomatic Gremlin code

If there is a simpler way, do it the simpler way. Use the Gremlin methods for what they're worth.

```Java
// Bad
g.V().outE().inV()

// Good
g.V().out()


// Bad
g.V().
  has('name', 'alice').
  outE().hasLabel('bought').inV().
  values('name')

// Good
g.V().
  has('name','alice').
  out('bought').
  values('name')


// Bad
g.V().hasLabel('person').has('name', 'alice')

// Good
g.V().has('person', 'name', 'alice')
```
