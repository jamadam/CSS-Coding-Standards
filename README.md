CSS-Coding-Standards
====================

Personal coding standards for crafting CSS

This is a living document. My goal is to update this as my knowledge evolves and promote more consistency in my own code.


## Guidelines
* Prefer one global stylesheet. ex. `style.css`
* Avoid using IDs for styling. IDs are great for anchor links and JS hooks, but avoid using them as styling hooks. [Source](http://oli.jp/2011/ids/)
* Include a link at the top of the stylesheet back to these coding standards.
* If you are minifing your CSS, include a link at the top to view the unminified CSS. `style.max.css` [Source](http://daneden.me/2012/07/max-css-in-depth/)
* Prefix all javascript-based selectors, (IDs, Classes) with `js-`. [Source](https://github.com/styleguide/javascript)
* Never reference `js-` prefixed class names from CSS files. `js-` base classes are to be used exclusively in JS files. [Source](https://github.com/styleguide/css)
* Use the `is-` prefix for state rules that are shared between CSS and JS.
* Avoid qualifying class names with type selectors. `Prefer .nav {...} instead of ul.nav {...}`


## Formatting CSS

* Prefer soft-tabs with 2 space indent.
* Add a single space before the opening bracket `{` in rule sets.
* Separate selectors and declarations by new lines. (multi-lined)
* Order your CSS rules alphabetically.
* Put prefixed rules before the unprefixed rules.
* Line up prefixed declarations so values are in line vertically.
* Add a space between properties and values after the colon. `display: block`
* Include a semi-colon after every declaration, including the last declaration.
* Use quotes when needed in selctors or values, prefer double quotes. `input[type="checkbox"]`
* Prefer shorthand hex values for colors. `background-color: #fff;`
* Use lowercase format for property and value names.
* When using zero based values, leave the value unitless. `margin: 0;`
* Omit leading “0”s in values. `font-size: .8em;`
* Place closing bracket of declaration block on its own line.
* Add one blank line bewteen rule sets.


**Formatting Example:**
```css
.success {
  background-color: #dff0d8;
  border-color: #d6e9c6;
  color: #468847;
  -webkit-border-radius: 15px;
     -moz-border-radius: 15px;
          border-radius: 15px;
  font-weight: 700;
  margin: .5em 0;
  padding: .3em;
}

.error {
  background-color: #f2dede;
  border-color: #eed3d7;
  color: #b94a48;
  -webkit-border-radius: 15px;
     -moz-border-radius: 15px;
          border-radius: 15px;
  font-weight: 700;
  margin: .5em 0;
  padding: .3em;
}
```
## File Stucture

Currently, the [most efficient way to serve CSS for multiple devices is via 1 stylesheet](http://andydavies.me/blog/2012/12/29/think-twice-before-using-matchmedia-to-conditionally-load-stylesheets/). Try to keep all styles in one stylesheet to avoid additional HTTP requests. 

### Ordering your styles

Order styles based on SMACSS structure with my own additions.

* **Custom Fonts (@font-face rules)**
* **Reset or Normalize**
* **Base**
* **Layout**
* **Modules**
* **States**

### Base Styles

These consist of all HTML elements. Create styles just for the HTML elements you have on your site. If your building a CMS theme, where content will be generated by users, create styles for all HTML elements in case users add them through the CMS.

If you find that you have to overwrite your base styles often, refactor your base styles so the are more generic and make better defaults.

### Layout Styles

In SMACSS, Johnathan talks about using the prefix `l-` to designate layout styles. I have not found a need for this in my stylesheets, though it is important to be aware of this practice. I know other developers have adopted this practice.

Layout styles should be wrappers for modules, these are things like your website's header, footer, body, sidebar. These will typically match up with ARIA Landmark Roles. Use classes for your layout styles. I've seen some people use attribute selectors like [role=header]. I'm on the fence about this, in one case it ties styles to just one instance and makes them less reusable, at the same time, you will probly only have one sidebar, one site footer, one site header. I prefer to add classes instead of attribute selectors for these instances.

### Modules

Modules make up a majority of a websites styles. There are three parts to modules.

* **Modules**
* **Modifiers**
* **Subcomponents**


Move common stlyes into a module and create module modifiers for unique instances.

Below as an example of an alert module with 2 modifiers.

**Module Example 1**
```css
.alert {
  -webkit-border-radius: 15px;
     -moz-border-radius: 15px;
          border-radius: 15px;
  font-weight: 700;
  margin: .5em 0;
  padding: .3em;
}

.alert--success {
  background-color: #dff0d8;
  border-color: #d6e9c6;
  color: #468847;
}

.alert--error {
  background-color: #f2dede;
  border-color: #eed3d7;
  color: #b94a48;
}
    
<div class="alert alert--success">
``` 

**Module Naming Conventions**

There are [various naming conventions](http://www.brettjankord.com/2013/03/06/more-thoughts-on-html-class-naming-conventions/). Below is my preferred naming convention for modules, modifiers, and subcomponents.

```css
.module {...}
.module--modifier {...}  /* Use double dash for modifiers */
.module-subcomponent {...}  /* Use single dash for subcomponents */
.module-subcomponent--modifier {...} /* A modifier on a subcomponent */
```

If your module name is two or more words, use camel case. This allow dashes to represent distinctions between modules, modifiers, and subcomponents. 

```css
.moduleName {...} /* Correct module naming */
.productReviews {...}  /* Correct module  naming */
.product-reviews {...} /* Incorrect module naming */
```

**Subcomponent Naming Conventions**

A header, body, and footer are common subcomponents of modules. In OOCSS, the these subcomponents have these class names: `.hd, .bd, .ft`

If I am abbreviating class names, I use 3 letters at minimum. `.hdr, .bdy, .ftr`
    
**Full example of the three parts of modules.**

```css
.entry {...} /* Module */
.entry-hdr {...}   /* Module Subcomponent */
.entry-title {...} /* Module Subcomponent */
.entry-meta {...}  /* Module Subcomponent */
.entry-bdy {...}  /* Module Subcomponent */
.entry-meta--sub {...} /* Module Subcomponent Modifier */
.entry--featured {...} /* Module Modifier */
```

### States

States styles are things like media queries, :hover, :focus, etc., and JavaScript states.

* Include state styles that after the styles they are affecting.
* Prefix JavaScript states with `js-`
* Prefer classes for JavaScript hooks compared to IDs or data-attributes. This allows them to be reusable, though still performant in querying the DOM.

As for generic states like, `js-is-hidden`, include these in a **State section** following your **Modules section** in your stylesheet.

## Comments & Documentation

I've adopted Nicholas Gallagehr's commenting guidelines from [Idiomatic CSS](https://github.com/necolas/idiomatic-css) with my own minor adjustments.

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Create a comment for each main section, base, layout, modules, and state.
* Create comments for sub-sections, typically I add these above modules.
* Add 3 lines before sub-section comments when they follow styles. See below example 2.
* There should be no lines sections and sub-section comments
* There should be no lines between sections/sub-sections and multiline comments.


**Comment types example inspired by Idiomatic CSS comments**
```
/*----------------------------------------------------------------------------
 Section comment block
----------------------------------------------------------------------------*/

/* Sub-section comment block
--------------------------------------*/

/**
 * The first sentence of the long description starts here and continues on this
 * line for a while finally concluding here at the end of this paragraph.
 *
 * The long description is ideal for more detailed explanations and
 * documentation. It can include example HTML, URLs, or any other information
 * that is deemed necessary or useful.
 */

/* Basic comment */
```

**Comment spacing example**
```css
/*----------------------------------------------------------------------------
 Modules
----------------------------------------------------------------------------*/
/* Post Entries
--------------------------------------*/
/**
 * Styles post entries and featured post entries. Included here could be more
 * details about the styles for other developers.
 */

.entry {...} 
.entry--featured {
  *display: inline; /* IE7 and lower fix */
  *zoom: 1; /* IE7 and lower fix */
}


/* Buttons
--------------------------------------*/
.btn {...} 
.btn--primary {...}
```

SCSS Coding Standards
==========================

These standards have been adopted from Chris Coyier's Sass Style Guide article. [Source](http://css-tricks.com/sass-style-guide/)

* List extends first.
* List includes next, unless they are media queries.
* List regular styles next.
* List include media queries next.
* List nested selectors last.
* Use same order inside nested selectors.
* Add a line before nested selectors.

Example

    .weather {
  	  @extends %module;
		  @include transition(all 0.3s ease); 
		  background: LightCyan;

		  > h3 {
		    @include transform(rotate(90deg));
		    border-bottom: 1px solid white;
		  }
		}

* Use mixins for vendor prefixes.
* Limit nesting to 3 levels deep.
* Locally compile in expanded format.
* Deploy in compiled format.
* Variablize all colors with two levels.

Example

	// first we set descriptive variables:
	$darkgrey: #333;
	$blue: #f00;
	 
	// then we set functional variables:
	$text_color: $darkgrey;
	$link_color: $blue;
	$border_color: $blue;
	 
	.myClass{
	  color: $text_color;
	  border-color: $border_color;
	}
	a{
	  color: $link_color;
	}
	
* Variablize all common numbers, and numbers with meaning.

Example

		$zHeader: 2000;
		$zOverlay: 5000;
		$zMessage: 5050;

		.header {
		  z-index: $zHeader;
		}
		.overlay {
		  z-index: $zOverlay;
		}
		.message {
		  z-index: $zMessage;
		}


* Nest and name your media queries. [Source](http://css-tricks.com/naming-media-queries/)

Example

	.sidebar {
	  float: right;
	  width: 33.33%;
	  @include breakpoint(small) {
	  	width: 25%;
	  }
	}

* Add shame file at end of style.css for hot fixes [Source] (http://csswizardry.com/2013/04/shame-css/)

**Comments**

Be generous with comments.

**//** This type of comment gets stripped in **compiled code**

**/*  comment */** This type of comment gets stripped  **only in compressed compiled code**

**/!** This type of comment doest not get stripped in **compressed compiled code**

### Scss File Structure

This file structure is based on ideas from: [http://blog.55minutes.com/2013/01/smacss-and-rails/](http://blog.55minutes.com/2013/01/smacss-and-rails/)
```    
    root/css/
    └── style.scss
        │ 
        ├── compass
        │  
        ├── globals.scss
        │   ├── _variables.scss
        │   ├── _functions.scss
        │   └── _mixins.scss
        │    
        ├── custom fonts
        ├── normalize or reset
        ├── base
        ├── layout
        └── modules
        │   ├── alerts
        │   └── …
        │ 
        └── states (mainly JS states, other states should follow the styles they are affecting)
```
