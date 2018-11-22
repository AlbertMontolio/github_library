Hi!

DOM access is expensive (in terms of performance), thus it should be avoided. A very simple example (without Angular): With plain HTML and JS you should never insert a list into the DOM item by item (each time changing the DOM), but you would create a fragment first in JS, add all the items, and when the fragment is ready, you would insert it into the DOM as a whole (-> only one DOM access).

Since Angular highly optimizes the DOM access by similar, but much more elaborated methods, you should only access the DOM directly in your code if you absolutely can't avoid it.

Jost

Mark as helpful Mark as top answer
Jost — Teaching Assistant  · an hour ago 
Two additional reasons, quoted from two different pages of the official Angular docs:

https://angular.io/guide/security

"The built-in browser DOM APIs don't automatically protect you from security vulnerabilities. For example, document, the node available through ElementRef, and many third-party APIs contain unsafe methods. Avoid directly interacting with the DOM and instead use Angular templates where possible."

https://angular.io/api/core/ElementRef

"Relying on direct DOM access creates tight coupling between your application and rendering layers which will make it impossible to separate the two and deploy your application into a web worker."

You will see during this course that the DOM manipulation model implemented in Angular hardly ever requires such a lower level direct access."


-------------------------

ng-template is a tag of angular, not a tag of the dom, right?

and angular, says, that ng-template has this attribute / property ngIf. then, we bind a true or false to this property. since its a ng-template, the ocntent inside will just be rendered if the expresion is true.

could you please correct my understanding

Jost — Teaching Assistant  · 2 hours ago 
Hi!

Yes, <ng-template> in Angular is basically the same like the native <template>. Like in normal HTML you can place here some HTML which is not rendered in the beginning. 

With normal HTML you would have to write some JS to render the code as a DOM Element somewhere. In Angular you have the ngIf directive to do this conditionally (and even the handy shortcut with *, not explicitly writing the ng-template which is used under the hood).


The <template> tag holds its content hidden from the client.

-----------------

Hi!

<pedant mode on> 

Unfortunately this seems to be a little more complicated:

With this commit the two booleans were replaced by two flags (as one single parameter):
https://github.com/angular/angular/pull/15045/commits/fd026a5c77421ddec119dc21a2733cbf173fef53

But simply setting RendererStyleFlags2.Important  as a fourth parameter has no effect.

The reason:

In plain ol' JS there are two different approaches to set style properties:

1. myElement.style.backgroundColor = 'red'; 
2. myElement.style.setProperty('background-color', 'red', 'important'); 

This means: in order to set the priority (!important) you have to use the more verbose second one, which also allows dash-case .

And this behaviour is not abstracted away in Render2's setStyle() . Therefore you would have to write ...
RendererStyleFlags2.DashCase | RendererStyleFlags2.Important (or 11  - both flags set - or the decimal equivalent 3 )
... as a fourth parameter in order to (only) set the CSS priority.

There seems to be no documentation about this fact, but here is the relevant source code:
https://github.com/angular/angular/blob/master/packages/platform-browser/src/dom/dom_renderer.ts#L182-#L189

  setStyle(el: any, style: string, value: any, flags: RendererStyleFlags2): void {
    if (flags & RendererStyleFlags2.DashCase) {
      el.style.setProperty(
        style, value, !!(flags & RendererStyleFlags2.Important) ? 'important' : '');
    } else {
      el.style[style] = value;
    }
  }
<pedant mode off> 


-------------------------------

Hi!

In general Angular creates an instance of a component if it detects a selector of this component sitting in the parent component's template. In an *ngFor loop this selector is not explicitly written several times, but it works the same.

What you wrote about property binding is correct all in all, but this does not the initialization.

Jost

-----------------------------------

with two way binding, how can we output in the CONSOLE, what the user types?



Jost — Teaching Assistant  · 2 days ago 
Hi!

This shouldn't work, since binding works inside a component, and console is a property of the window object, not of the component.

Of course you can console.log the user input simultaneously, calling a related method oninput.

Jost

-------------------------------------------------

The Bootstrap JavaScript can cause problems and usually third-party packages should be used to prevent conflicts with Angular. Two such options are ngx-bootstrap for Bootstrap 3 and ng-bootstrap for Bootstrap 4.

There are a few other UI frameworks, such as Angular Material, primeNG and Kendo; each offering their own advantages and disadvantages. Kendo is particularly amazing in my opinion, but I haven't had time to learn it yet.

--------------------------------------------------------


# section 10

Hi!

ngOnChanges  is limited to a very special case: If you want to detect in a child component, when the value from its parent component (passed with @Input()) changes.

Please have a look at example 1.2 in this list:

https://www.udemy.com/the-complete-guide-to-angular-2/learn/v4/questions/1711500

-----------------------

Hey..

The lifecycle of a service class is maintained by Angular itself. The service class is instantiated right at the beginning as you would have provided it in the providers section of app.module.ts. So, you simply cannot use ngOnInit in the service class. The reason why you get this.ingredients undefined is because it is never called. Hence, you don't see the console.log too!

--SrihariGR

----------------------------

Related to the question itself I'd like to add to Vikhyath's answer: In a service you have to put initialization logic into its constructor (or better call an init method in the constructor). But related to this special case it's important to note that the initialization happens right at the beginning, as Vikhyath pointed out, and not after initialization of the component where this service is injected.

-------------------------------

As Max explained in detail in section 9, it depends on the place where a service is provided if you get one single instance of it app-wide, or multiple instances with limited scopes.

In the constructor you are passing the related instance to the component. You don't create a new one in the constructor. 



s 18

Hi!

Section 20 is about authentication. But since the course is focused on Angular, we use firebase's auth module there.

In Max' MEAN stack course on Udemy you can learn how to handle JSON Web Tokens directly. in that course they are sent in the Headers with "Authentication" : "Bearer ..." .

Jost


--------------------

a promise always returns sth

-------------------------------------------

Hi!

There are many options dependent on the needs of your app. I guess you mean something like this ...

https://stackoverflow.com/a/41536393

...?

One example for a different approach, related to your buttons-question: If your button styling is only used in two components, you don't need to put it into the global styles. In the components you have a styleUrls array, so you can not only point to one CSS file with the rules for this component but also to a second one which contains the rules for the buttons (i.e. both components share the same CSS).

In this way normally the styles.css should not become too big. But it should be possible to @import there from other files with global styles.

Jost
-------------------------------------------










