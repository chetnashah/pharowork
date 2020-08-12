
```pharo
object-receiver message
```

message/methodname/selector are interachangable terms.

### primitive types

Chars: preceeded by $ e.g. `h` is `$h`
numbers: as usual
Strings: enclosed in single quotes `'` e.g. `'hello'`

### Strings

String concatenation happens by comma operator e.g.
`'hello' , ' world'` returns `'hello world'`.

### symbols

A Symbol is a String which is guaranteed to be globally unique. 
Symbols are prefix by hash sign i.e. `#`.
use `asSymbol` message/selector on string object to get symbol
use `asString` message/selector on symbol to get string

e.g.
`'Hi' asSymbol`
gives us
`#Hi` symbol

and
`#ProfStef asString.` will give us `'ProfStef'`.

### Equality

Message `==` returns true if the two objects are the SAME


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
`recvrObject selector`
e.g. `false not`

### Binary message (operators)

They require exactly one selector/messageName and one argument.
The selector should be from `+`, `-`, `*`, `/`, `&`, `=`, `>`, `|`, `<`, `~` and `@`.
Form is following:
`recvrObject selector argumentObject`

e.g. `21 * 2`
or
`41 <= 42`
`'hello' , ' world'` - here comma is binary op that concatenates two strings i.e. `hello world` is returned

### Keyword message
Messages often need to pass arguments to the receiver so that it can performits task; this is what keyword-based messages are for.
Keyworded messages require one or more argument.

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

`anObject selectorWord1: argumentObject1 word2: argumentObject2`
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

**"Between messages of similar precedence, expressions are executed from left to right"**
No exceptions, even `2 + 2 * 10.` returns `40` instead of `22`.
so use parentheses for arithmetic expressions to mean what you mean

### Classes and instantiation

Where do new objects come from? Well, in Pharo, object creation is just an-other form of computation, so it happens by sending a message to the classitself.

Classes are really objects that are known by name, so they provide a usefulentry point to the system; creating new objects is just a particular use-case.Some classes understand messages that return specific instances, like theclassFloatthat understands the message `pi`

### BlockClosure

Represented by `[ code ]`. these are objects that are only evaluated when some variant
of `value` message is sent to them.

```
[:x | x+2] value: 10.   //returns 12

[:x :y| x + y] value:3 value:5.    // returns 8
```
### variable definition

Done by two vertical pipes and var name in between e.g.
```pharo
| box |
box := 20@30 corner: 60@90.
box containsPoint: 40@50
```

"Blocks can be assigned to a variable then executed later.

Note that `|b|` is the declaration of a variable named 'b' and that ':=' assigns a value to a variable.

Select the three lines then Print it:"
```
|b|
b := [:x | x+2].
b value: 12.
```

### Expression sequences

Expressions separated by periods `.` are evaluated in sequence.
```pharo
| box |
box := 20@30 corner: 60@90.
box containsPoint: 40@50
```

### Cascaded messages

PHaro offers a way to send multiple messages to the same receiverobj using a semicolon `;`
e.g. `expression msg1; msg2`
is equivalent to 
```
expression msg1.
expression msg2.
```

### Conditionals

"Conditionals are just messages sent to Boolean objects"
```
1 < 2
  ifTrue: [100]
  ifFalse: [42].
```

### Looping

looping can be done by using `to:do:` or `to:do:by:` messages/selectors on integers.
Otherwise done using iteration methods/selectors on iterable objects

#### Iterators

The message `do:` is sent to a collection of objects (`Array`, `Set`, `OrderedCollection`), evaluating the block for each element.
```pharo
//forEach
#(11 38 3 -2 10) do: [:each |
     Transcript show: each printString; cr].

// map over collection/iterable
#(11 38 3 -2 10) collect: [:each | each abs]. // #(11 38 3 2 10)
#(11 38 3 -2 10) collect: [:each | each odd]. // #(true false true false false)

// filter a collection/iterable
#(11 38 3 -2 10) select: [:each | each odd]. // #(11 3)
#(11 38 3 -2 10) select: [:each | each > 10]. // #(11 38)
```

### Object model

Objects are instances of their class. Usually, we send the message #new to a class for creating an instance of this class.

The message #allInstances sent to a class answers an Array with all instances of this class.

#### Reflection methods

`someOBjName definition` to get the definition of something

`someObj selectors` to get all the selectors of the class
`someOBj browse` to browse code for obj.
`someObj inspect` to inspect the obj

`(Classname>>selectorname) definition.` gives you definition of the selector.
```
"Take a look at method #ifFalse:ifTrue: source code of class True:"

(True>>#ifFalse:ifTrue:) definition.

"Or just its comment:"

(True>>#ifFalse:ifTrue:) comment.

"Here's all the methods I implement:"

ProfStef selectors.
```

Checking if a selector exists on a class/obj:
`ProfStef respondsTo: #goToNextLesson.` returns `true`/`false` depending on selector existing or not.

Removing a selector on an obj:
`ProfStef class removeSelector: #goToNextLesson.` removes the selector named `goToNextLesson` in Profstef class.

Looking up selectors:
`(ProfStef lookupSelector:#next)` when the `next` selector exists returns us `ProfStef>>#next`

execute method:
`ProfStef default executeMethod: (ProfStef lookupSelector:#next).`


### Transcript

 The Transcript is an object that is often used for logging system messages. It is a kind of system console. 

### System browser

The System Browser, also known as "Class Browser", is one of the key tools used for programming.

// insert system browser image here

### Using spotter

The fastest (and probably the coolest) way to find a class is to use Spotter. Pressing `Shift+Enter` opens Spotter, a very powerful tool for finding classes, methods, and many other related actions.

`#implementors methodname` to see all implementors of a method treated as a protocol
`#messages methodname` in spotter to see all definitions

### Categories vs packages

Pharo packages were implemented as "categories" (a group of classes). With the newer versions of Pharo, the term category is being deprecated, and replaced exclusively by package.



