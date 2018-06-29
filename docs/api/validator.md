# Validator API

The validator offers an API to add new fields and trigger validations.

## API

### Properties

|Name  | Type  | Description  |
|---------|---------|---------|
| errors | [`ErrorBag`](/api/errorbag.md)| Instance of the ErrorBag class to manage errors. |
| fields     | [`FieldBag`](https://github.com/baianat/vee-validate/blob/master/src/core/fieldBag.js)| Instance of the FieldBag class to manage fields. |
| locale | `string` | The Currently activated locale. |

### Methods

|Name  | Return Type  |Description  |
|---------|---------|---------|
|attach(field: Field | FieldOptions) | `Field` | attaches a new field to the validator. |
| validate(descriptor?: String, value?: any, options?: Object) | `Promise<boolean>` | Validates the matching fields of the provided [descriptor](#field-descriptor). |
| pause() | `void` | Disables validation. |
| resume() | `void` | Enables validation. |
| detach(name: string, scope?: string) | `void` | Detaches the field that matches the name and the scope of the provided values. |
| extend(name: string, rule: Rule, options?: ExtendOptions) | `void` | Adds a new validation rule. The provided rule param must be a [valid Rule function or object](/guide/custom-rules.md). |
| reset(matcher?: Object) | `void` | Resets field flags for all scoped fields. Resets all fields if no scope is provided. |

#### Scoped Reset() Usage
```js
let matcher = {
    scope: 'form-1',
    vmId: this.$validator.id
}

this.$validator.reset(matcher);
```

### Validate API

The validate method is the primary way to trigger validation, all arguments are optional but that will produce different results depending on which arguments you did provide.

#### Field Descriptor

The field descriptor is a string that can have the following forms:

```js
// validate all fields.
validator.validate();

// validate a field that has a matching name with the provided selector.
validator.validate('field');

// validate a field within a scope.
validator.validate('scope.field');

// validate all fields within this scope.
validator.validate('scope.*');

// validate all fields without a scope.
validator.validate('*');
```

#### Value

The value argument is optional, if the value is not passed to the `validate()` method, it will try to resolve it using the internal value resolution algorithm. When the value is passed, the algorithm will be skipped and that value will be used instead.

#### Validation Options

You can pass the options to modify the behavior of the validation, the options is an object that can contain the following:

|Property |Type       |Default    |Description  |
|---------|:---------:|:---------:|-------------|
|silent   | Boolean   | `false`   | If true the validate method will return the validation result without modifying the errors or the flags. |
|initial  | Boolean   | `false`   | If true the rules marked as [non-immediate](/guide/custom-rules.md#non-immediate-rules) will be skipped during this call, used to prevent initial validation from triggering backend calls. |
