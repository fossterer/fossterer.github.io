# Writing meaningful unit tests in Angular applications (Part-2)

Expertise level: Novice

Continuing from our [previous example](writing-focussed-unit-tests-angular.md) where a button click calls out to a method, we can put in the following *spec* in the same *suite*

````javascript
  fit('should result in a notification when *Send Via Email* button is clicked', () => {
    const buttonOnClickhandler = spyOn(component, 'sendViaEmail');
    component.selectedItems.length = 1; // Mocking the length of items - To enable buttons for click events

    const buttons = fixture.debugElement.queryAll(By.css('button'));
    const sendViaEmailButton = buttons[1];
    fixture.detectChanges();

    sendViaEmailButton.nativeElement.click();

    fixture.whenStable().then( () => {
      expect(buttonOnClickhandler).toHaveBeenCalledWith('sendViaEmail');
    });
  });
````

Observe how we pick one particular button out of multiple shown on the page.

In case you are wondering, once you arrive at a *Native Element* i.e. by means of operation chain `fixture.debugElement.query(By.css('elementType').nativeElement`, you can use all [DOM operations](https://www.w3schools.com/jsref/prop_element_classlist.asp) over it such as appending to innerHTML, adding or removing classes etc.
