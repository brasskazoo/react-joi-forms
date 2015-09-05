# JoiForms

[![Build Status](https://travis-ci.org/appersonlabs/react-joi-forms.svg)](https://travis-ci.org/appersonlabs/react-joi-forms)

Dont fight with forms in React based apps again! En-Joi it! :)

```
npm install react-joi-forms
```

### What is JoiForms?
A library that lets you create forms easily and painlessly from [Joi](http://github.com/hapijs/joi) Schemas. From simple CRUD forms to complex ones with custom input components... JoiForms has you covered.

### Features
 - "It just works" out of the box!
 - Supports complex custom form layouts and field-sets
 - Works with any/all React UI libraries (material-ui, react-bootstrap...)
 - Works with ReactNative (not tested *yet* but it should...)
 - Flexible enough to build your own amazing UX


### So how easy is it to use?
This code will render a login field and use Joi to validate the users entry,using  the stock browser fields to render the UI.
```js
var JoiForm = require('react-joi-forms').JoiForm;

var ContactForm = React.createClass({
    schema: [
        Joi.string().email().label('Email Address').required(),
        Joi.string().label('Password').required()
    ],
    onSubmit(validationErrors, formValues) {
        // formValues = {emailAddress: 'test@test.com', password: 'password'}
        // Send formValues to an action to send via your API
    },
    render() {
        return (
            <JoiForm schema={this.schema} onSubmit={this.onSubmit} />
        );
    }
});
```

### Thats great for CRUD, but I need something more advanced!

So did we! You can also create a custom layout just as you normaly would, and just include our handy `FormSection` component wherever you want fields to be. Then just tag your Joi schema and sections.
```js
var reactJoiForm = require('react-joi-forms');
var JoiForm = reactJoiForm.JoiForm;
var FormSection = reactJoiForm.FormSection;

var ContactForm = React.createClass({
    schema: [
        Joi.string().label('Name').required().tags('personal-info'),
        Joi.string().label('Phone Number').required().tags('personal-info'),

        Joi.string().email().label('Street Address').required().tags('address'),
        Joi.string().label('City').required().tags('address'),
    ],
    onSubmit(validationErrors, formValues) {
        // formValues = {
        //  name: 'test@test.com',
        //  phoneNumber: '18005551212',
        //  streetAddress: 'where I live',
        //  city: 'city where I live',
        // }
        // Send formValues to an action to send via your API
    },
    render() {
        return (
            <JoiForm schema={this.schema} onSubmit={this.onSubmit}>
                <div className="some-complex-form-layout">
                    <FormSection tag="personal-info" />
                </div>
                <div className="some-other-form-layout">
                    <FormSection tag="address" />
                </div>
            </JoiForm>    
        );
    }
});
```

### OK, but I am using UI lib X, you promised I could!
```js
var JoiForm = require('react-joi-forms').JoiForm;

// We recommend putting this in its own file and just require it in so it can be shared
// between all your forms.
var customInputs = {
    textComponent: (value, options, events) => {
        var mask = options.masks[0] || 'text'
        delete options.masks;

        // your custom element here... for the sake of me being lazy in writing these docs
        // the "custom" UI is just re-implamenting the default. But you could put anything here
        return (
            <input {...options}
                   type={mask}
                   value={value}
                   onChange={events.onChange}
                   onFocus={events.onFocus}
                   onBlur={events.onBlur} />
        )
    },
}

var ContactForm = React.createClass({
    schema: [
        Joi.string().email().label('Email Address').required(),
        Joi.string().label('Password').required()
    ],
    onSubmit(validationErrors, formValues) {
        // formValues = {emailAddress: 'test@test.com', password: 'password'}
        // Send formValues to an action to send via your API
    },
    render() {
        return (
            <JoiForm schema={this.schema} onSubmit={this.onSubmit} {...customInputs} />
        );
    }
});
```

## Installation
```
npm install react-joi-forms
```

Note: In order to get joi working with webpack, we recommend adding this to your webpack webpack.config.js file:
```js
plugins: [
        new webpack.NormalModuleReplacementPlugin(/^(net|dns)$/, path.resolve(__dirname, 'server/lib/shim.js'))
    ]
```
where server/lib/shim.js is an empty file... This just removes the dependencies for some advanced email validation. Email validation still works, just it sticks to the text validation, not testing to see if the domain is real.


### Full API docs coming next week... it's time for me to start my weekend :)
