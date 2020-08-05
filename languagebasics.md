
```pharo
object-receiver message
```

### primitive types

Chars: preceeded by $ e.g. `h` is `$h`
numbers: as usual
Strings: enclosed in single quotes `'` e.g. `'hello'`


### Arrays

Literal arrays: by enclosing in hash with parantheses as a space separated list of items
e.g.
`#(1 2 3)`

#### Methods on arrays

`arrayobj size`
`arrayobj isEmpty`
`arrayobj at: 1` - **note: array indexes start at 1, not 0**

### Messages

Messages are sent to objects, which we usually refer to as "methods" on a class/instance.

Three kinds 
1. Unary
2. Binary
3. Keyword

#### Unary message

They have the following form:
`object message`
e.g. `false not`

### Binary message (operators)

Form is following:
`anObject aMessage anotherOBject`

e.g. `21 * 2`
or
`41 <= 42`
`'hello' , ' world'` - here comma is binary op that concatenates two strings i.e. `hello world` is returned

### Keyword message
Messages often need to pass arguments to the receiver so that it can performits task; this is what keyword-based messages are for.

```pharo
'hello' at: 1
>>>$h
```
The selector `at:` consists of a single keyword that ends with a colon, sig-nifying that it should be followed by an argument;

```pharo
'hello' copyFrom: 1 to: 3
>>> 'hel'
```
This is one single message, whose selector is really `copyFrom:to:`
You can see in traditional function calls first one is function name, rest are argument names starting from second argument.
`obj c` has something similar e.g. `['hello' copyFrom: 1 to: 3]` in objC syntax

They have following general form:

`anObject aKey: anotherObject aKey2: anotherObject2`
You can think of this as sending message `aKey:aKey2:` message
to `anObject` with `anotherObject` and `anotherObject2` as arguments.

### Message execution order

unary messages are evaluatedfirst, then binary messages, and finally keyword-based messages.
e.g.
```pharo
string' asUppercase copyFrom: -1 + 2 to: 6 - 3
>>> STR
// first asUppercase executes
// then binary exprs + and -
// final copyFrom executes
```

When message precedence does not match what you mean, you can force theexecution order using parentheses.

```pharo
('string' asUppercase first: 9 / 3) reversed
>>> 'RTS
```

### Classes and instantiation

Where do new objects come from? Well, in Pharo, object creation is just an-other form of computation, so it happens by sending a message to the classitself.

Classes are really objects that are known by name, so they provide a usefulentry point to the system; creating new objects is just a particular use-case.Some classes understand messages that return specific instances, like theclassFloatthat understands the message `pi`


