# Writing meaningful unit tests in Angular applications (Part-1)

Expertise level: Novice

Let's say we are building an Angular application. A given page has a *button* which when clicked calls out to a method defined in a corresponding *.ts* file *i.e*

````html
                    <button pButton type="button"
                            class="ui-button action-button action-button-dark-blue hover"
                            [disabled]="selectedItems.length===0 ? true : false " 
                            (click)="saveItems('sendViaEmail')">
                            save, Export & Email
                    </button>
````

Corresponding definition of `saveItems(string)` is in a .ts file

````javascript
 /**
   * Hook method to handle initializations of operations - _save_, and/or _email_ over an array of selected items.
   *
   * Sends the final result to a notification service. (Not shown here)
   *
   * Uses item-selection.http.service.ts (a shared service implementation not shown here)
   *
   * @param
   *  {string} operation The operation type should be one of *save*, *saveExport* and *sendViaEmail*
   * @returns _None_
   * @memberof ItemExportComponent
   */
  saveItems(operation: string) {
    this.loading = true;

    return this.itemSelectionHttpService.saveData(this.currentProject.id, this.templateId, this.selectedItems,
      operation, this.templateTypeId)
      .subscribe(res => {
        this.loading = false;
        if (res['isSuccessful'] === true) {
          switch (operation) {
            case 'save':
              this.notificationService.showSuccess('Saved successfully');
              break;
            case 'sendViaEmail':
              this.notificationService.showSuccess('Saved successfully. Please check your inbox for the export');
              break;
          }
        } else {
          this.notificationService.showError('Operation failed.');
        }
      });
  }
````

In this article, we are going to see how we test the service. Tests for actual UI component are discussed in [another post](writing-unit-tests-for-ui-events-angular.md).

We see that this method calls out to two other services. Let's use a stubbed service for one while still calling out the real implementation in the other. Below is the snippet of test from corresponding *.spec.ts* file

````javascript
fdescribe('ItemExportComponent', () => {
  let component: ItemExportComponent;
  let fixture: ComponentFixture<ItemExportComponent>;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [ItemExportComponent],
      providers: [
        { provide: NotificationService, useClass: NotificationsServiceStub },
        { provide: UninterestingService1, useClass: UninterestingService1Stub },
        { provide: itemSelectionHttpService, useClass: itemSelectionHttpServiceStub },
      ],
      schemas: [NO_ERRORS_SCHEMA]
    })
      .compileComponents();
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(ItemExportComponent);
    component = fixture.componentInstance;
    fixture.detectChanges();
  });
  
  fit('should accept operation type save and email', () => {
    const originalNotificationservice = fixture.debugElement.injector.get(NotificationService);

    const showSuccessInstance = spyOn(originalNotificationservice, 'showSuccess');
    const itemSelectionHttpServiceInstance = spyOn(itemSelectionHttpServiceStub.prototype, 'saveData')
      .and.returnValue(of(new Object({ 'isSuccessful': true })));

    component.saveItems('sendViaEmail');
    expect(itemSelectionHttpServiceInstance).toHaveBeenCalled();
    expect(showSuccessInstance).toHaveBeenCalledWith('Saved successfully. Please check your inbox for the export');
  });

});
````

Hey, what's that `fit(...)` usage you ask? That's Jasmine's it() method (a.k.a *spec*) with *focus* so `ng test` doesn't run 1000s of otherwise existing tests in the project. So is the case with the `fdescribe(...)` (a.k.a *focussed suite*). You can also use prefix *x* to exclude (i.e. *ignore*). There, you learned something !

This is how our *stub* definition looks:

````javascript
@Injectable()
export class itemSelectionHttpServiceStub {

  saveData(ProjectId: number, templateId: number, exportData: export[],
    operation: string, exportTempId: number): Observable<any> {
    return of(new Object());
  }
}
````

Continue to [part-2](writing-unit-tests-for-ui-events-angular.md) to see how we can test action over the actual *UI element*

**Take home exercise:** Define your own stub for *NotificationService* so the test shown above runs without errors
