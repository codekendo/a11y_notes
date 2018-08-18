# a11y_notes



## Info

These are accessbility notes.  



### For and ID
```html
<input id="roo" type="checkbox">
<label for="roo">
```



### 5 Aria


#### Creating a custom checkbox

```html
<div role="checkbox"
     aria-checked="true">
    Recieve promotional offers
</div>
<!-- Role='checkbox' name/label='Recieve promotional offers' state='checked'
 -->
```


Aria only modifys accessibility tree. So you will have to add in keyboard event
handling, add focusability, modify element behavior, modify element appearance.


#### Common implementation

Role, Name/Label, state, value

#### List of Roles

![list of roles](./images/roles.png)


#### Role Notes 

Make sure that tabindex and role are used in the same place. So when the
keyboard lands on the element the role is conveyed

Abstract Roles cannot be used on its own. 

#### Links


[Aria Spec 1.1](https://www.w3.org/TR/wai-aria-1.1/)

[Aria 1.0 Roles](https://www.w3.org/TR/wai-aria-1.0/#roles)

[Aria 1.1 Roles](https://www.w3.org/TR/wai-aria-1.1/#roles)

[Aria 1.1 practice guide(draft)](https://www.w3.org/TR/wai-aria-practices-1.1/)

[Definition of Roles](https://www.w3.org/WAI/PF/aria/roles#role_definitions)

[Aria Taxonomy](https://www.w3.org/TR/wai-aria-1.1/img/rdf_model.svg)

[HTML Aria(list of default aria roles built into HTML elements)(No Roles means you )](https://www.w3.org/TR/html-aria/)

[Aria ComboBox reference](https://www.w3.org/TR/wai-aria-practices-1.1/#combobox)

[Hidden Text ](http://webaim.org/techniques/css/invisiblecontent/)
#### aria labels

##### notes aira 

aria labels overides text content on screen readers 
```html
<button aria-label="Close">
    x
</button>
<!-- Close will be read not x -->
```

aria-labelledby allows another element to be used as its label for text
content. it can take a list of ids and the text content gets concat into one
labelledby takes precedence over aria-label and text context.

```html
<div role="radio" aria-labelledby="lb01"></div>
<span id="lb01">Coffee</span>
```

#### aria Relationship attributes

`aria-owns` reshapes the accessibility tree. (e.g. )

```html
<div role="menu">
    <div role="menuitem" aria-haspopup="true">
        New
    </div>
    <!-- more items -->
    <div aria-owns="submenu"></div>
</div><!-- menu -->
<div role="menu" id="submenu">
    <div role="menuitem">
        Document
    </div>
</div>
```

`aria-activedescendant`

```html

<div role="listbox" tabindex="0" aria-activedescendant="i7">
    <!--  -->
    <div role="option" id="i5">Item 5</div>
    <div role="option" id="i6">Item 6</div>
    <div role="option" id="i7">Item 7</div>
    <div role="option" id="i8">Item 8</div>
    <!-- -->
</div>

<!-- When the user focus on the box the user is focused on the item
this can be used in conjunction with combobox so when the user sets the 
-->

```


`aria-describedby`

```html
<label for="pw">Password:</label>
<input type="password" id="pw" aria-describedby="pw-help">
<div id="pw-help" hidden>
    Password must be at least 12 characters
</div>

<!-- like aria-labeledby helps describe the element that may be hidden from the dom -->

```


`aria-posinset` and `aria-setsize`


```html

<div role="listbox">
	<div role="option" aria-posinset="857" aria-setsize="1000">Item 857 </div>
	<div role="option" aria-posinset="858" aria-setsize="1000">Item 858 </div>
</div>


<!-- Useful when you lazy load a really long list and want the assistive tech
to show the certain list that is available to the user -->

```




How to hide a element from the accesbility tree



```html

<button style="visibility: hidden;"></button>

<div style="display: none"></div>

<span hidden></span>
```

position off screen 

```css
.screenreader{
    position:absolute;
    left:-10000px;
    width:1px;
    height:1px;
    overflow:hidden;
}
```

you can use aria-describeby or aria-labeledby to reference hidden elements


A way to exclude elements from the aria tree such as aria hidden true
This removes all decendents from the accesbility tree.
With the exception of aria labeledby or describedby 

This is useful when you have a slider



<div class="deck">
	<div class="slide" aria-hidden="true"> Sales Targets </div>
	<div class="slide"> Quarterly Sales </div>
	<div class="slide" aria-hidden="true"> Action Items </div>
</div>

aria-hidden default state is false.



Aria Live

```html
<div class="alertbar" aria-live="assertive">
    Could not connect!
</div>
```

aria-live values are `off` `polite` `assertive`
off means nothing happens
polite will alert the user to the change after the user finishes what he or she is doing.
for something important but not urgent


assertive will interupt the user to whatever he or she is doing


aria-atomic

Aria atomic considers the who region as one to be updated.

```html
<span aria-labelledby="birthdayLb1" aria-live="polite" aria-atomic="true">
<input type="number" id="day" value="18">
<input type="number" id="month" value="10">
<input type="number" id="year" value=1983>


<!-- This will announce the fill content of the date widget the user changes the date -->
<!-- aria atomic the default value is false -->

</span>


```


aria-relevant


indicates what type of changes need to present to the user






<table>
	<thead>
		<tr>
			<th>Value</th>
			<th>Description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td> additions: </td>
			<td> Element nodes are added to the DOM within the live region </td>
		</tr>
		<tr>
			<td>removals</td>
			<td>Text or element nodes within the live region are removed from the DOm</td>
		</tr>
		<tr>
			<td> text: </td>
			<td>Text is added to any DOM descendant nodes of the live region.</td>
		</tr>
		<tr>
			<td> all: </td>
			<td> Equivalent to the combination of all values, "additions removals text." </td>
		</tr>
		<tr>
			<td>additions text (default)</td>
			<td>Equivalent to the combination of values, "additions text".</td>
		</tr>
	</tbody>
</table>



aria-relevant="additions"
```html
<!-- Chat application that will announce when any change made to the chat history box -->
<div aria-live="polite" aria-relevant="additions" class="chat-history">
	<div class="chat-bubble left">
		<div class="offscreen">Cathy says</div> Sure! </div>
</div>
```


aria-relevant="text"

```html
<div class="alertbar" 
     aria-live="assertive" 
     aria-atomic="true"
     aria-relevant="text"> 
        Could not connect! 
</div>

<!--  Changing text will trigger this asstive tech to announce the change -->


```


aria-busy="true"

lets the asstive tech know to ignore changes to this element,
meant for when things are loading or connectivity is off
Once it is loaded it should be set to false.

```html
<div aria-live="polite" aria-busy="true" class="chat-history">
	<div class="chat-bubble left">
        <div class="offscreen"> Cathy says </div> 
    Sure! </div>
</div>
```




---

Focus styles

```css
/*remove styles anti-pattern DO NOT DO THIS*/
:focus{
    outline:0;
}


:focs{
    outline:1px dotted #FFF;
}

/* hover states */
button:hover{
    background:#ddd;
    color:#fff;
    text-decoration:underline;
}


/* changing the box shadow has more support across browser rather than just changing the color   */
button:focus{
outline:0;
box-shadow:0,0 8px 3px rgba(255, 255, 255, 0.8);
}


/* targeting radio circles */

.radio:focus{
    outline:0;
}
.radio:focus::before{
    box-shadow: 0 0 1px 2px #5b9dd9;
}
```


Focus


Ring A focus ring is something the denotes focus mostly seen in keyboards but
not mouse events. found in button elements To shim it check out

links for focusring
[focusring wicg](https://github.com/WICG/focus-visible)
[Proposing CSS input modality](https://www.oreilly.com/ideas/proposing-css-input-modality)
[moz-focusring](https://developer.mozilla.org/en-US/docs/Web/CSS/:-moz-focusring)






```css

.toggle[aria-pressed="true"] {

}


```
[MDN css attribute selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors)



---
responsive design
```html

<meta name="viewport" content="width=device-width, initial-scale=1">

<!-- user-scalable=no is an anti-pattern -->

```

48dp minimum touch target size both top and bottom

32dp margin touch target so that user do not inadverterly touch another button.


dp= device independent pixel


links

[Webaim checklist](http://webaim.org/standards/wcag/checklist#sc1.4.4)
[google dev responsive design](https://developers.google.com/web/fundamentals/design-and-ux/ux-basics/)
[material design for accessibility recommendation for touch target ](https://material.google.com/usability/accessibility.html#accessibility-layout)



links for contrast
[https://webdev.ink/2018/04/24/Text-Contrast-for-Web-Pages/#wdi-contrast-chart](https://webdev.ink/2018/04/24/Text-Contrast-for-Web-Pages/#wdi-contrast-chart)

These are miniumns:
text and images of text have a contrast ratio of at least 4.5:1
large text (over 18 points or 14 point bold has a contrast ratio nof at least 3:1)

Magic number is 7:1 having higher contrast is better (higher left number)


check site performance open up audits pane in chrome dev tools
then use accessbility audits to see failing elements


[
](https://dequeuniversity.com/rules/axe/2.2/color-contrast?application=lighthouse
)


do not just use color as the sole method for convey content or distinguishing visual elements.

do not just use color to indicate links use visual 

http://webaim.org/standards/wcag/checklist#sc1.4.1

use nocoffee extension to check for high contrast.