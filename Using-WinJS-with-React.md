
[Source: [issue #717](https://github.com/winjs/winjs/issues/717#issuecomment-63427539) by [rigdern](https://github.com/rigdern)]

You can try using https://github.com/rigdern/react-winjs, an unofficial React wrapper around WinJS's controls. It applies the technique described below.

You can wrap the WinJS ListView in a React component like this:

```js
  var ListView = React.createClass({
    displayName: 'ListView',
    shouldComponentUpdate: function () {
      // React will call the render function 1 time. Returning false here prevents
      // React from calling the render function additional times which allows the
      // ListView to remain in control of its DOM rather React.
      return false;
    },
    componentDidMount: function () {
      // Instantiate a ListView on this React component's DOM element.
      this.listView = new WinJS.UI.ListView(this.getDOMNode(), {
        itemTemplate: this.props.itemTemplate,
        itemDataSource: this.props.itemDataSource
      });
    },
    componentWillUnmount: function () {
      // Dispose the ListView when the React component is removed from the DOM.
      this.listView.dispose();
    },
    render: function () {
      // We will instantiate a ListView on this div in componentDidMount.
      return <div/>;
    }
  });
```

And then you can make use of the ListView component like this:
```js
  // WinJS code for creating an item renderer and a data source

  var renderer = WinJS.Utilities.markSupportedForProcessing(function (itemPromise) {
    return itemPromise.then(function (item) {
      var d = document.createElement("div");
      d.textContent = item.data.title;
      return d;
    });
  });

  var data = [];
  for (var i = 0; i < 100; i++) {
    data.push({ title: "Item " + i});
  }
  var dataSource = new WinJS.Binding.List(data).dataSource;

  // React code for defining the app's markup

  var App = React.createClass({
    displayName: 'App',
    render: function () {
      return (
        <div>
          <h1>App Title</h1>
          <ListView itemTemplate={renderer} itemDataSource={dataSource} />
          // Additional markup for other components of the app
          // ...
        </div>
      );
    }
  });

  React.render(<App/>, document.getElementById('app'));
```

If you need to update some property on the ListView React component (e.g. indexOfFirstVisible, scrollPosition), you should look into implementing the [componentWillReceiveProps](http://facebook.github.io/react/docs/component-specs.html#updating-componentwillreceiveprops) function.

Additional resources which may be useful in learning how to wrap third party libraries as React components:
- [Using ReactJS and KendoUI Together](http://ifandelse.com/using-reactjs-and-kendoui-together/)
- [React Book: Integrating third-party libraries](http://www.reactbook.org/book/chapter5)

