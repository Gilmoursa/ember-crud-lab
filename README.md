# Resource Library

## Objectives

* Build a route and route handler to create a new object. 
* build a template that contains a form for a new object.
* Build a route handler action for deleting an object. Use Handlebars `{{action="delete"}}` to make delete button. 
* Build a component to toggle editing state and handle edits.  


## Overview

In this app, you'll be building a library of educational resources. We'll be dealing with one model, the Resources model. Each resource represents an educational resource (a book, blog post, website, you name it). We'll be using the Active Model Adapter to connect to to an external API of educational resources. For the purposes of development and testing, however, we have configured a Javascript library called Mirage to stub external web requests to the API and provide you with some dummy seed data to work with. 

## Instructions

**Note:** In this lab, most of the templates have been provided for you. They contain some starter code that our test suite relies on. 

### Part I: Setting up the Adapter and the Model

* Install Active Model Adapter addon: `ember install active-model-adapter` in your terminal. 
* Generate an adapter with `ember generate adapter application`. Then, code your Active Model Adapter to connect to the following host: `https://dry-shore-2260.herokuapp.com`, with a namespace of `v1`.
* Now that your Ember app knows where to send requests for data (i.e. to the API above), let's set up our Resources model. 
* In `app/models/resource.js`, define your model to have a title, topic, URL and description. All of these will be of data type `'string'`.

### Part II: Building Routes and Route Handlers

* Define your router in `app/router.js` such that there is a route for `/resources`. This route should contain a nested route for `/new` and for an individual resource show page. 
* Define a route handler in `app/routes/resources.js` that will find and return all of the resource records. 
* Define a route hanlder in `app/routes/resources/resource.js` that finds and returns the record for a given resource, use the `resource_id` from params. 

Let's more on to some templates for now. We'll come back to the route handlers for creating a new resource and deleting a resource in a bit. 

### Part III: Templates

* On the `application.hbs` page, add a link to the resources page. 
{{#link-to 'resources' class="resources"}}Resources{{/link-to}} || {{#link-to 'index'}}Home{{/link-to}}.
* Let's create the page that the above link will bring the user to. In `templates/resources/index.hbs` we'll iterate over all of the resources. Iterate over the list of resources using the `#each` Handlebars helper. The title, topic and URL of each resource should be list items (`<li>`s), with the title also wrapped in `<h5>` tags. 

We'll come back to the show page for a given resource. We'll be using a component for that. More on that in a bit. 

### Part IV: Creating a New Resource

We already have a route defined for new resource page. Go ahead and open up `app/templates/resources/new.hbs`. This is where we will put our form for a new resource. 

* Build a form for a new resource using Handlebars `{{input}}` helpers. There should be an input field for title, topic, URL and description. Each of the input helpers just have an `id` of `title`, `topic`, `url` and `description`, respecitively. 
* The save button should contain an action of `'save'`. Use the `{{action}}` input helper. 
* Now let's build the route handler for creating a new resource. In `app/routes/resources/new.js`, build the route handler such that it sets `model` equal to a function that creates and returns a new resource. 
* Now, let's build the `'save'` action into this route handler. Define an action, `save`, that saves the resource and transitions to the resources index page. 

### Part V: Deleting a Resource

Now let's create the ability for a user to delete a resource. We'll put a delete button on the show page of an individual resources. 

* In `app/templates/resources/resource.hbs`, create a button with an `id` of `"delete"`. Give that button an `{{action}}` of `'delete'`. Now let's build the delete action. 
* In `app/routes/resources/resource.js` define an action, `delete`, that deletes the given record, saves the resource and transitions back to the resources index page. 

### Part VI: Building our Resource Component

Now, we'll build a component which will be responsible for rendering an individual resource on its show page. Then, we'll talk about using components, controller actions and something special called ember closure actions, to toggle between and editing and not editing state and to give our user the ability to edit a given resource.

* Generate our `show-resource` component with the following command in your terminal: `ember generate component show-resource`. 
* Open up `app/templates/components/show-component.hbs`. This is where we will display the given resource. Let's display the title of a resource in an `<h4>` and the url, topic and description each in their very on `<p>` tags. 
* In `app/templates/resources/resource.hbs` we'll call on our component and pass in the title, url, topic and description. 
* Now let's build out the ability of our component to toggle between editing states. In other words, let's get our component to be able to show a completed resource *or* a form to edit the given resource, based on the user's double clicking anywhere on our component. 
* In `app/components/show-resource.js`, define a property, `isEditing` and set it to `false`. 
* Now we'll use this property to add some logic to our component templae. In `app/templates/components/show-resource.js`: use a Handlerbars `{{if}}` statement to say that: if the `{{isEditing}}` property is `true`, show a form for editing the resource. Use `{{}}` input helpers to build your form. `{{else}}`, show the completed resource. 
* How will a user toggle between seeing the editing form and seeing the completed resource? We want them to be able to double click on the component to switch to viewing the edit form. In `app/components/show-resource.js`, define a property, `doubleClick`, and set it to a function that toggles the `isEditing` property. Great, now we can toggle back and forth between seeing completed resource and seeing our form. But how will we actually make and persist edits to the resource? We'll use Ember closure actions!


## Part VII: Building Our Closure Action

* Create a resources controller. In `app/controllers/resources/resource.js` we will define an action called `update`, that will be responsible for persisting changes to the resource.
* Once you define the `update` action in controller, we can pass it down into our component. 
* On the resource show page, `app/templates/resources/resource.hbs`, pass the `update` action into the component using the following synatx:

```
{{show-resource update=(action "update") title=model.title url=model.url topic=model.topic description=model.description}}
```

* Lastly, we need to tell our component how to call that `update` action and how to handle that action. 
* In the `show-resource` component, add a button to the bottom of your editing form that contains the `{{action update}}` helper. 
* In `app/components/show-resource.js`, define an `update` action that triggers the closure action, using `this.attrs.update()` *and* toggles the `isEditing` state (once the user hits the save button, they should no longer see the editing form).

That's it!
























