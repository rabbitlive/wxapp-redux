* wxapp-redux


** Installation

wxapp-redux require **React-Redux** 5.x or later.

#+BEGIN_SRC shell
yarn add wxapp-redux
#+END

Then, you can use `webpack` or `Browserify` build source.


** Usage

make redux store with wxapp you can use `Provider(store, options)`, e.g

#+BEGIN_SRC javascript
Provider(store)({
  foo: 42		
})
#+END


Provider set `options.store` with redux store, then pass the options to global `App`.

Connect a `wxapp Page` with store, should use `connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])`

#+BEGIN_SRC javascript
connect(mapStateToProps, mapDispatchToProps)({
  foo: 42
})
#+END

TODO

