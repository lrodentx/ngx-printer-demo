# NgxPrinterDemo

A easy to use service to print the window, parts of a window (div) or an image.
Printing of Angular Templates or Components is possible.

You can print opening a new window / tab or using the current window.
A directive can also be used to mark and store an HTML-element.

Several usefull directives can further assist you.

Start the demo app with *ng serve*.

# Usage
1. Add to main *app.module* imports
2. Use the *NgxPrinterService* where you like via DI (private printerService: NgxPrinterService)
3. Use the the functions provided by the service

**Easy start**
The easiest way to print an HTML Element is 
HTML:
```html
<div ngxPrintItemMarker directPrint="true"></div>
```

## Options
### Open new window
The service prints by opening a new window.
There is an option *printWindowOpen* to change this behavoir.
Set printWindowOpen=false;

You can also set this option in .forRoot while importing the
module to the *app.module*

```javascript
    imports: [
    BrowserModule,
    NgxPrinterModule.forRoot({printOpenWindow: true})
  ],
```
### Rendering time
Before openening the print window the service to some time to render the print content.
Default time is 200ms.
You can adjust the time using the property *timeToWaitRender* (also in forRoot).

### Default CSS-Class
When printing to the current window the service creates an component with a
css-class called 'default'.
You can override this class name using the property *renderClass* (also in forRoot).
Make sure that the class is placed in your global styles (styles.css).

## Functions - How to print 
### Print current window
`this.printerService.printCurrentWindow();`

### Print div by id
`this.printerService.printDiv('idOfDivToPrint');`

### Print image src/url directly
`this.printerService.printImg('assets/bratwurst.jpg');`

### Print Angular TemplateRef or Component
```javascript
 @ViewChild('PrintTemplate')
  private PrintTemplateTpl: TemplateRef<any>;

printTemplate() {
    this.printerService.printAngular(this.PrintTemplateTpl);
  }
```
Beware: To print a component the component needs to be known by service (copy source and add it to entry
component of app.module).
Otherwise use printHTMLElement instead.

### Print HTML Element
```javascript
 @ViewChild(LittleDummyComponent, {read: ElementRef}) PrintComponent: ElementRef;

 printHTMLElementToCurrent() {
    this.printerService.printHTMLElement(this.PrintComponent.nativeElement);
  }
```

## Event of print window
If you want to check whether the print window is open or not subscribe to observable
*$printWindowOpen*

```javascript
this.printWindowSubscription = this.printerService.$printWindowOpen.subscribe(val => {
      console.log('Print window is open:', val);
});
```

## Directive ngxPrintItem
 There is an directive ngxPrintItem to mark and store an HTML-Element as an item which can be printed 
 later and anyhwere on the page.
 An id has to be set.
 These items are stored in the services observable *printerService.$printItems*.
 Use the async pipe to subscribe and the function `printPrintItem`  to print the item.

HTML:
```html
<span id="firstPrintItem" ngxPrintItem printName="First one" >A <b>first</b> span with an ngxPrintItem directive</span>

<div *ngFor="let prinItem of $printItems | async">
  <span>{{prinItem.id}} - {{prinItem.printName}}</span>
  <button (click)="printItem(prinItem)">Print me!</button>
</div>
```
TS:
```javascript
  printItem(itemToPrint: PrintItem) {
    this.printerService.printPrintItem(itemToPrint);
  }
});
```

## Directive ngxPrintItemButton
If you want to print a single item from the list of ngxPrintItems (see above) without ts you can use the diretive ngxPrintItemButton. You have to know the id of the printItem.
The Click-Event to print the item will be automatically set.

```html
<button ngxPrintItemButton printItemId="firstPrintItem">Print first item directly</button>
```

## Directive ngxPrintItemMarker
If you want to indicate an item as printable you can use this directive.
The default class adds a little printer symbol to the top left of the html element.

### Show marker/icon
Just display marker.

HTML:
```html
<div ngxPrintItemMarker>
```

### Print after marker click
If you click the little printer print starts immediately.

HTML:
```html
<div ngxPrintItemMarker directPrint="true"></div>
```

### Customization
You can override the default class with the `customClass` property.
Make sure that the css class is globally accessable e.g. put it into "styles.css".
You can also only override the image using the property `backgroundImage`. You can
speficy the data for the *background-url*.

### Event 
You can listen to the event 'printClicked'.

HTML:
```html
<div id="printDivMarker" ngxPrintItemMarker (printClicked)="printerMarkerClicked()">

<div id="printDivMarker" ngxPrintItemMarker customClass="mymarker" >
```

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory. Use the `--prod` flag for a production build.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via [Protractor](http://www.protractortest.org/).

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI README](https://github.com/angular/angular-cli/blob/master/README.md).
