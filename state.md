In the front-end state, or context, is the data reflecting the busines objects on the server.

## Authority

### Centralized
State can be stored monolithically, centralized on a singular object, as in a Redux store. 
It then has one interface that all components connect to. Components save, load, and get updates through the singular interface.

A singular interface means we can better control the input and output, and we know we have a single source of truth. 
For this reason, It is often easier to reason about. However, it fundementally breaks the encapsulation of components. 
They are now coupled to whatever application state mechanism you have. 

### Decentralized
State can also be stored in a distributed manner, where components own various pieces of state. State is then handed down 
the component chain as one would would pass normal properties. This allows proper encapsulation and allows components to own
whatever state is necessary to just them. But now we have state spread throughout the application, and it's harder to control.

Still, decentralized is preferred because encapsulation allows us to create components that are completely agnostic to the 
state system. This reinforces modularity and re-usability.

## Mutation
As state objects change, either by updates from the server, or user changes, components must be notified of the change. 
Components need to subscribe to changes. Changes can also be pushed onto them (for instance, by changing its property),
but then whatever thing owns that process must subscribe to changes.

There is then a pair of information to pass on to components that own the update process. One is the state itself, and
the other is a method of subscription. Here are some ways to broker this:

### Shared Resource Object

The state is an object, call it a resource, that has some method that allows the component to subscribe to it. The resource
is shared between the state provider and the components. When it updates, it notifies the component.

This is a good system, and is naturally OOP. However, now the state cannot pass by value, it must always reference the
resource object itself. It also means we must have a custom class for our state object, though this is often done anyway.
Also the component must now be aware of this mechanism, which recouples it to the state system.

### State Connector

Centralized systems like Redux connect the component to the state provider, in such a way that when the state updates, 
all connected components are updated. This is a subsciption, but doesn't use the state as a broker, and so when any state 
is updated, all components update. This introduces a large amount of process overhead, but is simple.

### Request for Context

Components can subscribe to state changes by requesting up the DOM tree to a provider. This provider could be at the window level,
making it essentially monolithic, or it could be anywhere in the DOM tree. But importantly it is above whatever component is requesting
the subscription, which matches how DOM trees most often mirror the state tree.

In edge cases, however, the DOM tree doesn't mirror the state tree, and so there must be some brokering to reoslve this. Also, this
request again couples component to the state system, although there's a recent [initiative to standardize this in webcomponents](https://github.com/webcomponents/community-protocols/issues/2),
which uses an event system to trigger events that request state (context) and subscription.

### Owner Owns Changes

This system uses the decentralized model an reinforces encapsulation by saying the components don't worry about subscribing to changes,
owners of those components must worry about updating them, usually by changing a property. 

The only gotcha is: changing the property is often changing an object back to itself, and some systems like LitElement won't be able to 
detect a change in the property and will therefore not do anything. There are two ways to solve this: One, if the owning component knows 
that the child component is is a LitElement, they can call the triggerUpdate() function to force the update. The other is to set the 
property to `null` and then back to the component, this will ensure the change propagates. 
