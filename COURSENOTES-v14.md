# Angular Component Communication Course Notes
The following are course notes when watching this course (done with **Angular v5**)
but coding along with **Angular v14**


## product-shell.component.ts
- Properties require a default value.
  - CODE:
  ```
  pageTitle = 'Products';
  monthCount = 0;
  sub?: Subscription;
  ```
  - REASON: As of Angular v12, Angular is strongly typed by default and requires all variables to be initialized.

- `ngOnDestroy` requires checking for null or undefined subscription
  - CODE:
  ```
    if (this.sub) {
      this.sub.unsubscribe();
    }
  ```
  - REASON: With strict type checking, a property that could be null or undefined must be checked before using its properties or methods.

## product-shell-detail.component.ts
- Properties require a default value.
  - CODE:
  ```
  pageTitle = 'Product Detail';
  // Need to handle null to allow for no selected product.
  product: IProduct | null = null;  
  errorMessage = '';

  sub?: Subscription;
  ```
  - REASON: As of Angular v12, Angular is strongly typed by default and requires all variables to be initialized.

- `ngOnDestroy` requires checking for null or undefined subscription
  - CODE:
  ```
    if (this.sub) {
      this.sub.unsubscribe();
    }
  ```
  - REASON: With strict type checking, a property that could be null or undefined must be checked before using its properties or methods.

## product-shell-list.component.ts
- Properties require a default value.
  - CODE:
  ```
  pageTitle = 'Products';
  products: IProduct[] = [];
  errorMessage = '';

  // Need to handle null to allow for no selected product.
  selectedProduct: IProduct | null = null;
  sub?: Subscription;
  ```
  - REASON: As of Angular v12, Angular is strongly typed by default and requires all variables to be initialized.

- `ngOnDestroy` requires checking for null or undefined subscription
  - CODE:
  ```
    if (this.sub) {
      this.sub.unsubscribe();
    }
  ```
  - REASON: With strict type checking, a property that could be null or undefined must be checked before using its properties or methods.

## All Modules
- As of Angular 12, strict typing is on by default. This requires that every variable have an initial value (unless the variable is optional) and that types are specified if they can't be inferred. (Some code on the slides shows variables without an initial default value.)
- TypeScript will infer the type when a specific default value is provided. No need to provide both the type and default. (Some code on the slides show both the type and a default value. The inferred types were removed from the provided code.)
- The folder structure of the starter files have changed slightly due to changes in the files generated by the Angular CLI. For example, there is no longer an e2e folder and no tslint.json. See the [`Angular: Getting Started`](https://app.pluralsight.com/library/courses/angular-2-getting-started-update) course for details on the files and folders generated by the Angular CLI.
- Updated from Bootstrap 3 to Bootstrap 5, which changed:
  - The `panel` to `card` layout.
  - Many of the form style classes.
  - Many of the validation style classes.
- Updated from RxJS v5 to v7 which changed:
  - How RxJS creation functions and operators are imported. Everything is now imported from `rxjs`.
  - Syntax of the `subscribe` method. It now only takes one parameter: either the next function or an object with `next` and `complete` properties.

  > See the [`Angular: Getting Started`](https://app.pluralsight.com/library/courses/angular-2-getting-started-update) or the [`RxJS in Angular: Reactive Development`](https://app.pluralsight.com/library/courses/rxjs-angular-reactive-development) course on Pluralsight for more information about RxJS.

## Module 2
- The blog is no longer maintained. Please use [the GitHub repository](https://github.com/DeborahK/Angular-Communication) for the most up-to-date information about this course.
- If you are new to Angular, consider watching the [`Angular: Getting Started`](https://app.pluralsight.com/library/courses/angular-2-getting-started-update) course first. Note that the `Angular First Look` course has not been updated for recent versions of Angular.
- Consider using the `APM-Start v14` files to code along with this course. The original `APM-Start` files are for Angular v5 and don't install easily with current versions of node. Use these course notes while coding along for any changes in the entered code, primarily for strict typing (added in v12).

## Module 3
See All Modules above.

## Module 4
See All Modules above.

When using @ViewChild and @ViewChildren, define the property as optional using a `?` and specify the appropriate type. The type is then the type OR undefined and the default value is undefined. The property can then be undefined, so must be checked for null or undefined before it is used.

**Changes when coding along:**

### <span style="color:blue">product-list.component.ts</span>
  ```
  @ViewChild('filterElement') filterElementRef?: ElementRef;
  ```
  - CHANGE: Added `?` to mark the property as optional.
  - REASON: As of Angular v12, Angular is strongly typed by default. Strong typing requires all variables to have a valid type and be initialized or marked as optional (unless the type can be inferred from the initial value). 

  ```
  ngAfterViewInit(): void {
    if (this.filterElementRef?.nativeElement) {
      this.filterElementRef.nativeElement.focus();
    }
  }
  ```
  - CHANGE: Added check including the safe navigation operator (`?`).
  - REASON: Now that the filterElementRef can be undefined (because it was defined as optional), the object must be checked using the safe navigation operator (`?`) before accessing its properties or methods. 

  ```
  @ViewChildren('filterElement, nameElement') inputElementRefs?: QueryList<ElementRef>;
  ```
  - CHANGE: Added `?` to mark the property as optional.
  - REASON: As of Angular v12, Angular is strongly typed by default. 

  ```
  @ViewChild(NgModel) filterInput?: NgModel;
  ```
  - CHANGE: Added `?` to mark the property as optional.
  - REASON: As of Angular v12, Angular is strongly typed by default. 

  ```
  ngAfterViewInit(): void {
    this.filterInput?.valueChanges?.subscribe(
      () => this.performFilter(this.listFilter)
    }
  }
  ```
  - CHANGE: Added check for null or undefined.
  - REASON: Now that the filterInput can be undefined (because it was defined as optional), the object must be checked using the safe navigation operator (`?`) before accessing its properties or methods. 

### <span style="color:blue">product-edit.component.ts</span>
This is code is part of the starter files and doesn't need to be entered. It is provided here for reference.
  ```
  saveProduct(): void {
    if (this.editForm?.valid && this.product) {
      this.productService.saveProduct(this.product)
        .subscribe({
          next: () => {
            if (this.product && this.originalProduct) {
              // Assign the changes from the copy
              this.originalProduct = { ...this.originalProduct, ...this.product };
              this.onSaveComplete();
            }
          },
          error: err => this.errorMessage = err
        });
    } else {
      this.errorMessage = 'Please correct the validation errors.';
    }
  }
  ```
  - CHANGES: 
    - Added the safe navigation operator (`?`) as needed for properties that could be null or undefined.
    - Added additional checking for null or undefined in the `if` statements.
    - Changed to new `subscribe` syntax.
    - Replaced Object.keys with the spread operator.
  - REASON: As of Angular v12, Angular is strongly typed by default. The RxJS `subscribe` syntax has changed. And the spread operator is easier to work with (and strongly type) than `Object.keys`. 

## Module 5
See All Modules above.

**Changes when coding along:**

### <span style="color:blue">criteria.component.ts

  ```
  listFilter = '';
  @Input() displayDetail = false;
  @Input() hitCount = 0;
  hitMessage = '';

  @ViewChild('filterElement') filterElementRef?: ElementRef;
  ```
  - CHANGE: Properties require a default value or must be declared as optional. Also, properties with a default value don't need a type if that type can be inferred.
  - REASON: As of Angular v12, Angular is strongly typed by default.

  ```
  ngAfterViewInit(): void {
    if (this.filterElementRef?.nativeElement) {
      this.filterElementRef.nativeElement.focus();
    }
  }
  ```
  - CHANGE: Added a safe navigation operator (`?`) because `filterElementRef` can be null or undefined.
  - REASON: With strict type checking, an object property that could be null or undefined must be checked before using its properties or methods.

### <span style="color:blue">product-list.component.ts

  ```
  @ViewChild(CriteriaComponent) filterComponent?: CriteriaComponent;
  parentListFilter = '';
  ```
  - CHANGE: Mark the `@ViewChild` property as optional so no default value is needed.
  - REASON: As of Angular v12, Angular is strongly typed by default. 

  ```
  ngAfterViewInit(): void {
    if (this.filterComponent) {
      this.parentListFilter = this.filterComponent.listFilter;
    }
  }
  ```
  - CHANGE: `filterComponent` property must be checked for null or undefined before using.
  - REASON: The `filterComponent` property has a default value of undefined, so that property must be checked for null or undefined before assigning to one of its properties or methods.

## Module 6
See All Modules above.

**Changes when coding along:**

### <span style="color:blue">criteria.component.ts

  ```
  private _listFilter = '';
  ```
  - CHANGE: Properties require a default value or must be declared as optional. Also, properties with a default value don't need a type if that type can be inferred.
  - REASON: As of Angular v12, Angular is strongly typed by default.

## Module 7
See All Modules above.

As of Angular v6, service registration is now defined in the `@Injectable` decorator of the service itself instead of in a module.

See [`Angular: Getting Started`](https://app.pluralsight.com/library/courses/angular-2-getting-started-update) for more information on registering services using the `@Injectable` decorator.

For more information on NgRx, check out [`Angular NgRx: Getting Started`](https://app.pluralsight.com/library/courses/angular-ngrx-getting-started)

**Changes when coding along:**

### <span style="color:blue">Angular CLI command
  ```
    ng g s products/product-parameter
  ```
  - CHANGE: No need for the -m property.
  - REASON: The service does *not* require registration in the module. The CLI instead generates the registration as part of the `@Injectable` decorator.

  > NOTE: The product.module.ts file no longer has nor needs a `providers` array.

### <span style="color:blue">product-parameter.service.ts

  ```
  import { Injectable } from '@angular/core';

  @Injectable({
    providedIn: 'root'
  })
  export class ProductParameterService {
    showImage = false;
    filterBy = '';

    constructor() { }

  }
  ```
  - CHANGES:
    - Register the service using the `@Injectable` decorator.
    - Properties require a default value or must be declared as optional. 
    - Properties with a default value don't need a type if that type can be inferred.
  - REASON
    - As of Angular v6, using the `@Injectable` decorator provides better "tree shaking" and is now best practice for registering services.
    - As of Angular v12, Angular is strongly typed by default.

### <span style="color:blue">product-list.component.ts

  ```
  @ViewChild(CriteriaComponent) filterComponent?: CriteriaComponent;
  ```
  - CHANGE: Mark the `@ViewChild` property as optional so no default value is needed.
  - REASON: As of Angular v12, Angular is strongly typed by default. 

  ```
  ngOnInit(): void {
    this.productService.getProducts().subscribe({
      next: products => {
        this.products = products;
        setTimeout(() => {
          if (this.filterComponent) {
            this.filterComponent.listFilter =
              this.productParameterService.filterBy;
          }
        })
      },
      error: err => this.errorMessage = err
    });
  }
  ```
  - CHANGE: The code to set the listFilter in the child component must be within a `setTimeout()` function to defer execution until the next evaluation cycle. The `subscribe` was also changed in the above code for RxJS v7.
  - REASON: To allow referencing the `ViewChild` property within the ngOnInit (instead of AfterViewInit) and to prevent an `Expression has changed after it was checked` error.
  - REFERENCE: [Angular Debugging "Expression has changed after is was checked": Simple Explanation (and Fix)](https://blog.angular-university.io/angular-debugging/)

## Module 8
See All Modules above.

As of Angular v6, service registration is now defined in the `@Injectable` decorator of the service itself instead of in a module.

See [`Angular: Getting Started`](https://app.pluralsight.com/library/courses/angular-2-getting-started-update) for more information on registering services using the `@Injectable` decorator.

**Changes when coding along:**

### <span style="color:blue">product.service.ts
> This is code is part of the starter files and doesn't need to be entered. It is provided here for reference.
  ```
  import { catchError, Observable, of, tap, throwError } from 'rxjs';
  ```
  - CHANGE: Import location is changed.
  - REASON: For RxJS 7, all Observable creation functions, operators, and features are imported from `rxjs`.

  ```
  @Injectable({
    providedIn: 'root'
  })
  export class ProductService {
  ```
  - CHANGE: Register the service using the `@Injectable` decorator.

  - REASON: As of Angular v6, using the `@Injectable` decorator provides better "tree shaking" and is now best practice for registering services.

> This code is part of the coding along.

  ```
  private products?: IProduct[];
  ```
  - CHANGE: Mark the `products` property as optional so it doesn't require a default value.
  - REASON: As of Angular v12, Angular is strongly typed by default.













## Module 9
See All Modules above.

### <span style="color:blue">product.service.ts

- `BehaviorSubject` is imported from `rxjs`.
  - CODE:
  ```
  import { BehaviorSubject, catchError, Observable, of, tap, throwError } from 'rxjs';
  ```
  - REASON: For RxJS 7, all Observable creation functions, operators, and features are imported from `rxjs`.
- `deleteProduct` requires checking the `products` property before finding the item to delete it.
  - CODE:
  ```
    tap(data => {
      if (this.products) {
        const foundIndex = this.products.findIndex(item => item.id === id);
        if (foundIndex > -1) {
          this.products.splice(foundIndex, 1);
          this.changeSelectedProduct(null);
        }
      }
    }),
  ```
  - REASON: The `products` property is initialized to undefined, so it requires checking before accessing one of its properties or methods.

- Change the `data` variable used for the emitted item in the RxJS pipelines to a more descriptive name or `()` if the emitted item is not used.
  - REASON: Easier to understand what the emitted item is if the variable name is more specific. And no variable needed if the emitted item isn't used.
