# service-oracle

[![Build Status](https://secure.travis-ci.org/digitaledgeit/service-oracle.png?branch=master)](http://travis-ci.org/digitaledgeit/service-oracle)

A service locator for NodeJS and the browser.

The all-knowing Oracle knows how to construct your services!

## Installation

    npm install --save service-oracle
    
## Usage
    
    var locator = require('service-oracle');
    
    //setup your services
    var services = locator()
    	.instance('Config', require('./config.json'))
    	.factory('Database', function(locator) {
    		var config    = locator.get('Config');
    		var database  = new Database(config.db);
    		return database;
    	})
    	.factory('Users', function(locator) {
    		var database  = locator.get('Database');
    		var users     = new Users(database);
    		return users;
    	})
    	.factory('Posts', function(locator) {
    		var database  = locator.get('Database');
    		var posts     = new Posts(database);
    		return posts;
    	})
    	.factory('BlogService', function(locator) {
    		var users     = locator.get('Users');
    		var posts     = locator.get('Posts');
    		var service   = new BlogService(users, posts);
    		return service;
    	})
    ;
    
    //somewhere else e.g. in your express routes
    var blog = services.get('BlogService');

## API

### Methods

#### new ServiceOracle()

Create a new service locator.

#### .has(name : string) : boolean

Check whether a service can be located.

#### .new(name : string) : *

Create a new instance of a service.

**Note**: Cannot be a service registered using the `.instance()` method.

#### .get(name : string) : *

Get a shared instance of a service.

#### .instance(name : string, value : *) : ServiceOracle

Register a shared instance of a service.

#### .factory(name : string, value : string|function : ServiceOracle

Register a function or module path as a service factory.

#### .locator(locator : ServiceOracle) : ServiceOracle

Register another locator to to locate services in.

## Changelog

v1.0.0

- changed `.set(name, instance)` to `.instance(name, instance)`
- added missing tests
- updated documentation

## License

The MIT License (MIT)

Copyright (c) 2015 James Newell