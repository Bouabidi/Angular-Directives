# Angular-Directives
Angular Directives Explained: what are they, how to use them, and to build our own.

What is an Angular Directive?
Directives are the functions which will execute whenever Angular compiler finds it. Angular Directives enhance the capability of HTML elements by attaching custom behaviors to the DOM.
Angular directives are categorized into three categories.
1. Attribute Directives
2. Structural Directives
3. Components

## Attribute Directives
Attribute Directives are responsible for manipulating the appearance and behavior of DOM elements. We can use attribute directives to change the style of DOM elements. These directives are also used to hide or show particular DOM elements conditionally. Angular provides many built-in Attribute Directives like NgStyle, NgClass, etc. We can also create our own custom Attribute Directives for our desired functionality.

## Structural Directives
Structural Directives are responsible for changing the structure of the DOM. They work by adding or removing the elements from the DOM, unlike Attribute Directives which just change the element’s appearance and behavior.

The three most popular built-in Structural Directives Angular provides are NgIf, NgFor, and NgSwitch.

## Using Angular Built-In Directives

### NgStyle 
Is an Attribute directive used to change the styling of any DOM element on the basis of some condition.
 ```javascript
<p [ngStyle]="{'background': isBlue ? 'blue' : 'red'}"> I am an Attribute Directive</p>
```
In the above code snippet, we are adding a blue background if the value of isBlue variable is true. If the value of isBlue variable is false, then the background of the above element will be red.

### NgIf 
Is a structural directive used to add elements into the DOM according to some condition.
<p *ngIf="show">I am a Structural Directive</p>
In the above code snippet, the whole paragraph will remain in the DOM if the value of show variable is true, else it will kick off from the DOM.

### NgFor
The *ngFor directive is used to repeat a portion of HTML template once per each item from an iterable list (Collection).
We have an array of objects here that contains people's names and their ages.

Now, we will use *ngFor to display these names in the interface.

 ```javascript
<ul>
  <li *ngFor="let item of friendslist">
    {{ item.name }} {{ item.age }}
  </li>
</ul>
```
## Custom Attribute Directive
Creating a custom directive is just like creating an Angular component. To create a custom directive we have to replace @Component decorator with @Directive decorator.

The steps to create custom attribute directive.
1. Create a class decorated with @Directive().
2. Assign the attribute directive name using selector metadata of @Directive() decorator enclosed with bracket [] .
3. Use ElementRef class to access DOM to change host element appearance.
4. Use @Input() decorator to accept user input in our custom directive.
5. Use @HostListener() decorator to listen events in custom attribute directive.
6. Configure custom attribute directive class in application module in the declarations metadata of @NgModule decorator.

 ```javascript
import { Directive, ElementRef, AfterViewInit } from '@angular/core';

@Directive({ 
     selector: '[cpDefaultTheme]' 
})
export class CPDefaultThemeDirective implements AfterViewInit {
    constructor(private elRef: ElementRef) {
    }
    ngAfterViewInit(): void {
       this.elRef.nativeElement.style.color = 'blue';
       this.elRef.nativeElement.style.fontSize = '20px';
    }		
} 
```
AfterViewInit is the lifecycle hook that is called after a component view has been fully initialized. To use AfterViewInit, our class will implement it and override its method ngAfterViewInit().

### Directive using @Input()
To accept input within directive we need to declare a property decorated with @Input(). We must use property name same as selector name. If we want to use different property name from selector name then use alias with @Input().

 ```javascript
import { Directive, ElementRef, Input, AfterViewInit } from '@angular/core';

@Directive({ 
     selector: '[cpColor]' 
})
export class CPColorDirective implements AfterViewInit {
    @Input() cpColor: string;
    constructor(private elRef: ElementRef) { 
    }
    ngAfterViewInit(): void {
	this.elRef.nativeElement.style.color = this.cpColor;
    }
} 
```
Look into the above code, selector name and property name are the same. If we want to use alias we can use as follows.

 ```javascript
@Input('cpColor') myColor: string; 

to use our custom directive:
<h4 [cpColor]="titleColor"> cpColor Directive Demo using Bracket []</h4>
<h4 bind-cpColor="titleColor"> cpColor Directive Demo using bind- prefix  </h4>
<h4 cpColor="{{titleColor}}"> cpColor Directive Demo using Interpolation</h4> 
```
If we want to accept more than one input then create more than one properties decorated with @Input().

 ```javascript
import { Directive, ElementRef, Input, AfterViewInit } from '@angular/core';

@Directive({ 
     selector: '[cpCustomTheme]' 
})
export class CPCustomThemeDirective implements AfterViewInit {
    @Input() tcolor: string;
    @Input() tsize: string;	
    constructor(private elRef: ElementRef) {
    }
    ngAfterViewInit(): void {
	this.tcolor = this.tcolor || 'green';
	this.tsize = this.tsize || '20px';
        this.elRef.nativeElement.style.color = this.tcolor;
	this.elRef.nativeElement.style.fontSize = this.tsize;
   }	
}
```
to use our custom directive:
<div cpCustomTheme tcolor="blue" tsize="30px"> cpCustomTheme Directive Demo with Custom Settings</div> 

### Directive using @HostListener()

If we want to change element appearance in DOM on any event then we need to listen event in our custom directive. To listen event we will use Angular @HostListener() decorator in our custom directive. The event name will be assigned to @HostListener() decorator.

 ```javascript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({ 
     selector: '[defColOnEvent]' 
})
export class DefaultColorOnEventDirective {
   constructor(private elRef: ElementRef) { 
   }
   @HostListener('mouseover') onMouseOver() {
     this.changeColor('red');
   }
   @HostListener('mouseleave') onMouseLeave() {
     this.changeColor('black');
   }
   private changeColor(color: string) {
     this.elRef.nativeElement.style.color = color;
   }  
} 
```
to use our custom directive:
<p defColOnEvent> defColOnEvent Directive Demo</p>

We can also use @HostListener() with @Input() to get user input.

 ```javascript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({ 
     selector: '[dynamicColOnEvent]' 
})
export class DynamicColorOnEventDirective {
   @Input('dynamicColOnEvent') dynamicColor: string;
   @Input() defaultValue: string;
   constructor(private elRef: ElementRef) {
   }
   @HostListener('mouseover') onMouseOver() {
     this.changeBackgroundColor(this.dynamicColor || this.defaultValue);
   }
   @HostListener('mouseleave') onMouseLeave() {
     this.changeBackgroundColor('white');
   }
   private changeBackgroundColor(color: string) {
     this.elRef.nativeElement.style.backgroundColor = color;
   }  
}
```

to use our custom directive:
<p [dynamicColOnEvent]="myColor" defaultValue="blue"> dynamicColOnEvent Directive Demo</p> 


## Structural Directive for IF Condition
We will create a structural directive that will add a layout in DOM for a true condition otherwise it will delete it from DOM.

 ```javascript
import { Directive, TemplateRef, ViewContainerRef, Input } from '@angular/core';

@Directive({ 
     selector: '[cpIf]' 
})
export class CpIfDirective {
	constructor( private templateRef: TemplateRef<any>,
	             private viewContainer: ViewContainerRef) { }
	@Input() set cpIf(condition: boolean) {
	   if (condition) {
		  this.viewContainer.createEmbeddedView(this.templateRef);
	   } else {
		  this.viewContainer.clear();
	   } 
	}
} 
```
to use our custom directive:

   ```javascript
<div *cpIf="showCpIf">
  <b>Hello World!</b>
</div>
<ng-template [cpIf]="!showCpIf">
      <div>
	  <b>Not Available</b>
      </div>
</ng-template> 
  ```
