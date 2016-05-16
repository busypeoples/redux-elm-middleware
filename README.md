# redux-elm-middleware

> Elm middleware for redux :sparkles:

[![Build Status](https://travis-ci.org/stoeffel/redux-elm-middleware.svg?branch=master)](https://travis-ci.org/stoeffel/redux-elm-middleware)
[![codecov](https://codecov.io/gh/stoeffel/redux-elm-middleware/branch/master/graph/badge.svg)](https://codecov.io/gh/stoeffel/redux-elm-middleware)
[![Dependency Status](https://david-dm.org/stoeffel/redux-elm-middleware.svg)](https://david-dm.org/stoeffel/redux-elm-middleware)
[![npm version](https://badge.fury.io/js/redux-elm-middleware.svg)](https://badge.fury.io/js/redux-elm-middleware)

## Installation

You need to install redux-elm-middleware for js and elm.

```bash
$ npm i redux-elm-middleware -S
$ elm package install redux-elm-middleware
```

## Usage

### Setup Redux Middleware

```js
import createElmMiddleware from 'redux-elm-middleware'

// create a worker of your elm reducer
const elmStore = window.Elm.Reducer.worker();

// create the middleware
const { run, elmMiddleware } = createElmMiddleware(elmStore)

// create the redux store and pass the elmMiddleware
const store = createStore(reducer, {}, compose(
  applyMiddleware(elmMiddleware),
  window.devToolsExtension ? window.devToolsExtension() : f => f
));

// you need to run the elm middleware and pass the redux store
run(store)
```

### Createing a Reducer in Elm

A reducer in elm looks like a normal [TEA](https://github.com/evancz/elm-architecture-tutorial) module without the view.
```elm
port module Reducer exposing (Model, Msg, init, update, subscriptions) -- Name of the module must match the worker


import Redux

-- define ports for all actions which should be handled by the elm reducer
port increment : (Maybe Int -> msg) -> Sub msg

-- define all subscriptions of your reducer
subscriptions : Model -> Sub Msg
subscriptions _ =
    Sub.batch
        [ increment <| always Increment
        -- ...
        ]

-- MODEL
-- UPDATE

-- START THE REDUCER
main =
    Redux.program
        { init = init
        , update = update
        , subscriptions = subscriptions
        }
```


## Motivation

* write bulletproof businesslogic
* handle state and effects
  * pure
  * in one place
  * with a safetynet
* still have the rich react/redux ecosystem at your paws
  * components
  * middlewares
    * routing
    * persistent state (localstorage)
    * offline support
    * ui state ( redux-ui )
* sneak a nice functional language into your projects
* don't have to commit 100% to it
* slowly convert a redux/react app into elm

## Running the Example

* `npm install`
* `npm run example`
* open 127.0.0.1:8080


## Feedback and contributons welcome!
