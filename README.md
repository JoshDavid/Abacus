# HTML Generation for Dyalog APL

## Getting Started

The following code assumes the HTML API is instantiated as H, that is:

~~~
      H←#.HTML.Main 
~~~

The `New` function creates a new HTML element:

~~~
      d←H.New 'html'
~~~

An element and all of its children are composed into HTML
using the DOM2HTML function

~~~
      H.DOM2HTML d  
<!DOCTYPE html>              
<html></html>     
~~~

The 'html' element is special-cased to include the HTML! declaration.

The `New` function optionally takes a left argument to be 
the parent of the newly created element:

~~~
      hd←d H.New 'head'
      mt←hd H.New'meta'
      b←d H.New'body'
      DOM2HTML d
<!DOCTYPE html>                                                                    
<html>                                                                             
  <head>                                                                           
    <meta></meta>                                                                  
  </head>                                                                          
  <body></body>                                                                    
</html>
~~~

Attributes may be directly assigned into an element:

~~~
      mt.charset←'utf-8'
      H.DOM2HTML d
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"></meta>
  </head>
  <body></body>
</html>
~~~

The New function takes an optional second item in its right argument,
to add content to an element at creation time: 

~~~
      h1←b H.New 'h1' 'Introduction'
      H.DOM2HTML d
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"></meta>
  </head>
  <body>
    <h1>Introduction</h1>
  </body>
</html>
~~~

Additional content may be added to an existing element using 
the Add function:

~~~
      _←h1 H.Add ' to Writing HTML'
      H.DOM2HTML d                                                                 
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"></meta>
  </head>
  <body>
    <h1>Introduction to Writing HTML</h1>
  </body>
</html>
~~~ 

Paragraphs with bold and italics may be constructed:

~~~
      p←b H.New 'p' 'This is some text.'
      _←p H.New 'em' 'This is emphasized.'
      _←p H.Add 'This is not.'
      H.DOM2HTML d 
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"></meta>
  </head>
  <body>
    <h1>Introduction to Writing HTML</h1>
    <p>
      This is some text.
      <em>This is emphasized.</em>
      This is not.
    </p>
  </body>
</html>      
~~~

Content is simple text or array of elements. Elements may be constructed
independently of the document and then directly added
to an element:

~~~                      
      ps←H.New¨{'p' ⍵}¨'This is a paragraph.' 'This is another paragraph.'
      b.Content,←ps
      H.DOM2HTML d
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8"></meta>
  </head>
  <body>
    <h1>Introduction to Writing HTML</h1>
    <p>
      This is some text.
      <em>This is emphasized.</em>
      This is not.
    </p>
    <p>This is a paragraph.</p>
    <p>This is another paragraph.</p>
  </body>
</html>
~~~ 

## Working with the DOM

Once an HTML document (or any element, usually with children) is created, the 
elements can be extracted from the DOM tree into a simple vector:  

~~~
       e←Elements d
~~~

From this list it is simple to select and modify elements based on any of their
attributes or types, names, ids, or content. For example, to change all h2 and h3
elements to h4:

~~~
       e←Elements d 
       h←e/⍨e.Type∊'h2' 'h3'
       h.Type←'h4' 
~~~

However, a couple of additional functions are provided. The `ElementsWhere` function
selects elements where an attribute equals a particular value:  

~~~
       a←d ElementsWhere 'Type' 'p'
       c←d ElementsWhere 'class' 'myclass'
~~~

The `ElementById` function selects a single element by its id:   

~~~
       zip←d ElementById 'zipcode'
       Zip.Content←'33040'
~~
