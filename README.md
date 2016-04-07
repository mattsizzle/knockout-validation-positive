# knockout-validation-positive
Extends Knockout-Validation functionality by adding additional bindings and observables that polyfil a default un-validated and a positive validation state. This is most commonly needed for positive validation (think 'success' class in bootstrap) representation in a UI and for complex validation routines where a field may not need to show validation when empty.

I always ended up re-writing or copy & pasta this functionaility into projects with Knockout Validation so I figured it was time to modularize it.

This process is driven by the `extendedValidation` extender that pulls option from it's instanciation and overloads a clone of the ko.validation options object to insure that minimal settings are required and that is always plays nice with ko-validation

    extendedValidation: {
      successClass: 'success',
      errorClass: 'danger'
    },
    
Options:
- successClass: The className to expose when an element has positive validation applied. Mimic the behavior of `errorClass`
- errorClass: The className to expose when an element has negative validation applied. Mimic the behavior of `errorClass` and will be derived from that property of the ko-validation options object if not provided.

Binding Handlers:

We do not want to overload any of the ko-validation bindings so we introduce some new ones to use in combination with the Knockout-Validation library.

    ko.bindingHandlers['validationClass'] = {
        update: function (element, valueAccessor) {
            var value = valueAccessor();
            ko.bindingHandlers.css.update(element, value.validationState);
        }
    };
    
As you can see this binding will apply the `successClass`, `errorClass`, or a `null` class to an object that has this handler bound. This class is updated when validation occurs against the observable passed based on it's isValid() return value.




