1. What is Vue?
Vue is a JavaScript framework for building user interfaces.
It builds on top of standard HTML, CSS and JavaScript, and provides a declarative and component-based programming model that helps us efficiently develop user interfaces, be it simple or complex.

2. Two core features of Vue?
Declarative Rendering: Vue extends standard HTML with a template syntax that allows us to declaratively describe HTML output based on JavaScript state.
Reactivity: Vue automatically tracks JavaScript state changes and efficiently updates the DOM when changes happen.

3. Single-File Components?
Vue components using an HTML-like file format called Single-File Component (also known as *.vue files, abbreviated as SFC). 
A Vue SFC, as the name suggests, encapsulates the component's logic (JavaScript), template (HTML), and styles (CSS) in a single file.

4. Options API?
With Options API, we define a component's logic using an object of options such as data, methods, and mounted. 
Properties defined by options are exposed on this inside functions, which points to the component instance.

5. Composition API?
With Composition API, we define a component's logic using imported API functions. 
In SFCs, Composition API is typically used with <script setup>. 
The setup attribute is a hint that makes Vue perform compile-time transforms that allow us to use Composition API with less boilerplate. 
For example, imports and top-level variables / functions declared in <script setup> are directly usable in the template.

	Installation:
		> npm init vue@latest

		✔ Project name: … <your-project-name>
		✔ Add TypeScript? … No / Yes
		✔ Add JSX Support? … No / Yes
		✔ Add Vue Router for Single Page Application development? … No / Yes
		✔ Add Pinia for state management? … No / Yes
		✔ Add Cypress for testing? … No / Yes

		> cd <your-project-name>
		> npm install
		> npm run dev

	Creating an application:
		Every Vue application starts by creating a new application instance with the createApp function:

		import { createApp } from 'vue'

		const app = createApp({
		  /* root component options */
		})

	The Root Component:
		The object we are passing into createApp is in fact a component. Every app requires a "root component" that can contain other components as its children.

		import { createApp } from 'vue'
		// import the root component App from a single-file component.
		import App from './App.vue'

		const app = createApp(App)

	Mounting the App:
		An application instance won't render anything until its .mount() method is called. 
		It expects a "container" argument, which can either be an actual DOM element or a selector string

		<div id="app"></div>

		app.mount('#app')

6. Template Syntax?
Vue uses an HTML-based template syntax that allows you to declaratively bind the rendered DOM to the underlying component instance's data. 
All Vue templates are syntactically valid HTML that can be parsed by spec-compliant browsers and HTML parsers.
Under the hood, Vue compiles the templates into highly-optimized JavaScript code. 
Combined with the reactivity system, Vue is able to intelligently figure out the minimal number of components to re-render and apply the minimal amount of DOM manipulations when the app state changes.

7. Text Interpolation?
The most basic form of data binding is text interpolation using the "Mustache" syntax (double curly braces):
	<span>Message: {{ msg }}</span>
The mustache tag will be replaced with the value of the msg property from the corresponding component instance. It will also be updated whenever the msg property changes.

8. Raw HTML?
The double mustaches interprets the data as plain text, not HTML. In order to output real HTML, you will need to use the v-html directive:

	<p>Using text interpolation: {{ rawHtml }}</p>
	<p>Using v-html directive: <span v-html="rawHtml"></span></p>

Using text interpolation: <span style="color: red">This should be red.</span>
Using v-html directive: This should be red.

9. Attribute Bindings?
Mustaches cannot be used inside HTML attributes. Instead, use a v-bind directive:

	<div v-bind:id="dynamicId"></div>

The v-bind directive instructs Vue to keep the element's id attribute in sync with the component's dynamicId property. 
If the bound value is null or undefined, then the attribute will be removed from the rendered element.

		Shorthand:	<div :id="dynamicId"></div>

10. Boolean Attributes?
Boolean attributes are attributes that can indicate true/false values by its presence on an element. 
For example, disabled is one of the most commonly used boolean attributes.

	v-bind works a bit differently in this case:	<button :disabled="isButtonDisabled">Button</button>

The disabled attribute will be included if isButtonDisabled has a truthy value. 
It will also be included if the value is an empty string, maintaining consistency with <button disabled="">. For other falsy values the attribute will be omitted.

11. Dynamically Binding Multiple Attributes?
If you have a JavaScript object representing multiple attributes that looks like this:

		data() {
		  return {
		    objectOfAttrs: {
		      id: 'container',
		      class: 'wrapper'
		    }
		  }
		}

You can bind them to a single element by using v-bind without an argument:	<div v-bind="objectOfAttrs"></div>

12. Using JavaScript Expressions?
	{{ number + 1 }}

	{{ ok ? 'YES' : 'NO' }}

	{{ message.split('').reverse().join('') }}

	<div :id="`list-${id}`"></div>

	Expressions Only:	Each binding can only contain one single expression, so the following will NOT work:

		<!-- this is a statement, not an expression: -->
		{{ var a = 1 }}

		<!-- flow control won't work either, use ternary expressions -->
		{{ if (ok) { return message } }}

	Calling Functions:	It is possible to call a component-exposed method inside a binding expression:

		<span :title="toTitleDate(date)">
		  {{ formatDate(date) }}
		</span>

13. Directives?
Directives are special attributes with the v- prefix. Vue provides a number of built-in directives, including v-html and v-bind which we have introduced above
		
		<p v-if="seen">Now you see me</p>

14. Arguments?
Some directives can take an "argument", denoted by a colon after the directive name. 
For example, the v-bind directive is used to reactively update an HTML attribute:
	
	<a v-bind:href="url"> ... </a>
	<!-- shorthand -->	<a :href="url"> ... </a>

Here href is the argument, which tells the v-bind directive to bind the element's href attribute to the value of the expression url. 
In the shorthand, everything before the argument (i.e. v-bind:) is condensed into a single character, :.

Another example is the v-on directive, which listens to DOM events:

	<a v-on:click="doSomething"> ... </a>
	<!-- shorthand -->	<a @click="doSomething"> ... </a>

Here the argument is the event name to listen to: click. 
v-on is one of the few directives that also have a corresponding shorthand, with its shorthand character being @. 

15. Dynamic Arguments?
It is also possible to use a JavaScript expression in a directive argument by wrapping it with square brackets:

	<a v-bind:[attributeName]="url"> ... </a>
	<!-- shorthand -->	<a :[attributeName]="url"> ... </a>

Here attributeName will be dynamically evaluated as a JavaScript expression, and its evaluated value will be used as the final value for the argument. 
For example, if your component instance has a data property, attributeName, whose value is "href", then this binding will be equivalent to v-bind:href.

Similarly, you can use dynamic arguments to bind a handler to a dynamic event name:

	<a v-on:[eventName]="doSomething"> ... </a>
	<!-- shorthand -->	<a @[eventName]="doSomething">

In this example, when eventName's value is "focus", v-on:[eventName] will be equivalent to v-on:focus.

	Dynamic Argument Value Constraints:
	Dynamic arguments are expected to evaluate to a string, with the exception of null. The special value null can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

	Dynamic Argument Syntax Constraints:
	Dynamic argument expressions have some syntax constraints because certain characters, such as spaces and quotes, are invalid inside HTML attribute names. 
	For example, the following is invalid:

		<!-- This will trigger a compiler warning. -->
		<a :['foo' + bar]="value"> ... </a>

	If you need to pass a complex dynamic argument, it's probably better to use a computed property.

	When using in-DOM templates (templates directly written in an HTML file), you should also avoid naming keys with uppercase characters, 
	as browsers will coerce attribute names into lowercase:

		<a :[someAttr]="value"> ... </a>

	The above will be converted to :[someattr] in in-DOM templates. If your component has a someAttr property instead of someattr, your code won't work.

16. Modifiers?
Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. 
For example, the .prevent modifier tells the v-on directive to call event.preventDefault() on the triggered event:

	<form @submit.prevent="onSubmit">...</form>
