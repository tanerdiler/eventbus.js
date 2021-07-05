# JEventbus Javascript NPM Package - Event Driven Architecture

[![Build Status](https://travis-ci.org/tanerdiler/eventbus.js.svg?branch=master)](https://travis-ci.org/tanerdiler/eventbus.js)
[![Coverage Status](https://coveralls.io/repos/github/tanerdiler/eventbus.js/badge.svg?branch=master)](https://coveralls.io/github/tanerdiler/eventbus.js?branch=master)

eventbus is a sequential event/listener mechanism that based on Event Driven Architecture.

## What is the goal of this project?

Eventbus triggers listeners sequentially for an event. The most important benefit of this approach is that it allows you to split your business logic into small pieces. Each piece is a listener and each listener has own logic (single responsbility principle). I used this approach in many Javascript projects. It allows me to inject logics into any step and that made application more flexible. While triggering, if one listener returns false, it brakes execution of next listeners. So, this feature allows you to control logic by injecting stoppers.

## Installation

```bash
$ npm install --save eventbus
```

## Usage


```javascript
var eventbus = require('the.eventbus');

var MyListener = function()
{
   this.onClick = function(source){
   
	console.log(source['firstname']);
   }
}

eventbus.event('onClick');
eventbus.listener(new MyListener()).listen('onClick');
eventbus.event('onClick').fire({firstname:'taner'});


var mailSent = false;
var stock = 100;

var OrderMailSender = {
	sendAnEmail : function(source){
		mailSent = true;
        }
}

var StockManager = {
	decStock : function(source){
		stock = stock - source.amount;
	}
}

eventbus.event('OrderReady').setDefaultMethod('onOrderDone');

eventbus.listener(OrderMailSender).withMethod('sendAnEmail').listen('OrderReady');
eventbus.listener(StockManager).withMethod('decStock').listen('OrderReady');

eventbus.event('OrderReady').fire({food:'Pizza', amount: 5});


var listener = eventbus.listener(CustomListener).
            withMethod('doSomething').
            listen('SomethingHappened').
            desc('When something is happened, CustomListener writes some info to console.');

```

## Versions
### 1.0.4
#### Added
- Developer can write a short desc about event&listener conf.
### 1.0.3
#### Added
- Calling method named different than default method of event. With this feature, you can give meaningful name to method.
### 1.0.2
#### Added
- Specifiying default method that would be triggered when event fired
- Event name will be assigned as default method by putting on prefix if needed, if method name is not specified.
#### Removed
- Automatically putting 'on' prefix to event name  while creating an event

## Test

This module is well tested. You can run:

- `npm test` to run the tests under Node.js.

![Test results](https://github.com/tanerdiler/types.js/blob/master/test-results.png)

## License

[MIT](LICENSE)

