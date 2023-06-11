div, p, ul - Type selector. Selects all elements of specific type.
'#id id selector. Selects all elements with id: <ul id='long'>.
'#fancy span - Descendant selector. Selects any <span> elements that are child id="fancy".
.classname - select all elements with class='classname'.
* - universal selector

Combinations:
ul.important - selects all <ul> elements that have class='important'
p, .fun - selects all <p> elements as well as all elements with class='fun'
p * - selects all elements inside all <p> elements

Siblings:
p + .intro - Adjacent sibling selector, selects every element with class='intro' that directly follows <p>
A ~ B - General sibling selector, selects all B that follow an A
A > B - Child selector, selects all B that are direct children of A
p:first-child - selects <p> elements that are first children of some other element
span:only-child - selects the <span> elements that are the only child of some other element
span:last-child - selects all <span> elements that are the last child
:nth-child(8) - selects every element that is the 8th child of another element
:nth-last-child(2) - selects all second-to-last child elements

Combinations:
div p:first-child - selects all first child <p> elements that are in <div>
ul :only-child - selects all elements that are the only child inside <ul>
ul li:only-child - selects all <li> elements that are the only child inside another element <ul>
ul li:last-child - selects the last-child <li> elements inside another element <ul>
div p:nth-child(2) - selects every second <p> in every <div>

Types and attributes:
span:first-of-type - selects the first<span> in any element
div:nth-of-type(2) - selects the second instance of a div
span:nth-of-type(6n+2) - selects every 6th instance of <span>, starting from (and including) the second instance
p span:only-of-type - selects a <span> within any <p> if it is the only <span> in there
div:last-of-type - selects the last <div> in every element
div:empty - selects all empty <div> elements
:not(#fancy) - selects all elements that do not have id='fancy'
a[href] - selects all <a> elements that have a href='anything'
input[type='checkbox'] - selects all checkbox input elements
[attribute^='value'] - Attribute starts with selector
[attribute$='value'] - Attribute ends with selector
[attribute*='value'] - Attribute wildcard selector

Combinations:
.example:nth-of-type(odd) - selects all odd instances of the <class='example'>
p span:last-of-type - selects the last <span> in every <p>
div:not(:first-child) - selects every <div> that is not a first child
:not(.big, .medium) - selects all elements that do not have class='big' or class='medium'

#css