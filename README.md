# XML Query

Super small (~60 LOC) library for retrieving values and attributes from the XML AST generated by [xml-reader](http://github.com/pladaria/xml-reader).

Provides an easy-to-use jQuery-like interface.

## Install

```bash
npm install --save xml-query
```

## Usage

### Reading xml streams/strings

The following XML will be used to illustrate all examples

```xml
<message id="1001" date="2016-06-19">
    <from>Bob</from>
    <to>Alice</to>
    <subject>Hello</subject>
    <body>Bla bla bla</body>
</message>
```

Let's read the XML using [xml-reader](http://github.com/pladaria/xml-reader).

```javascript
const XmlReader = require('xml-reader');

const xml =
`<message id="1001" date="2016-06-19">
    <from>Bob</from>
    <to>Alice</to>
    <subject>Hello</subject>
    <body>Bla bla bla</body>
</message>`;

const ast = XmlReader.parseSync(xml);
```

### xmlQuery()

```javascript
const xmlQuery = require('xml-query');

// creating from single ast
const xq = xmlQuery(ast);

// creating from an array of asts
const xq = xmlQuery([ast, ...more]);
```

### .get()

Retrieve one of the elements.

```javascript
xmlQuery(ast).find('body').get(2); // returns the `subject` node
```

### .find()

Find by name. Including top level nodes and all its children.

```javascript
xmlQuery(ast).find('body'); // xmlQuery containing the body element
```

### .attr()

Get all attributes. If a name is provided, it returns the value for that key.

```javascript
xmlQuery(ast).attr(); // {id: '1001', date: '2016-06-19'}
xmlQuery(ast).attr('id'); // '1001'
```

### .children()

Returns a new xmlQuery object containing the children of the top level elements.

```javascript
xmlQuery(ast).children();
```

### .each()

Iterate over a xmlQuery object, executing a function for each element.

```javascript
xmlQuery(ast).each(node => console.log(node.name));
// from
// to
// subject
// body
```

### .map()

Iterate over a xmlQuery object, executing a function for each element. Returns the results in an array.

```javascript
xmlQuery(ast).map(node => node.name); // ['from', 'to', 'subject', 'body']
```

### .prop()

Get the value of a property for the first element in the set.

```javascript
xmlQuery(ast).prop('name'); // 'message'
```

### .text()

Get the combined text contents of each element, including their descendants

```javascript
xmlQuery(ast).find('subject').text(); // 'hello'
```

### .eq()

Returns a new XmlQuery object for the selected element by index

```javascript
xmlQuery(ast).children().eq(2); // xmlQuery containing the 'subject' node
```

### .first()

Returns a new XmlQuery object for the first element. Same as `.eq(0)`

```javascript
xmlQuery(ast).children().first(); // xmlQuery containing the 'from' node
```

### .last()

Returns a new XmlQuery object for the last element. Same as `.eq(length - 1)`

```javascript
xmlQuery(ast).children().last(); // xmlQuery containing the 'body' node
```

### .length

Number of elements. Returns the same result as `.size()`

```javascript
xmlQuery(ast).children().length; // 4
```

### .size()

Number of elements. Returns the same result as `.length`

```javascript
xmlQuery(ast).children().size(); // 4
```

## License

MIT
