# React Router

React it's self is just a view library that does not come with any build in routing mechanisms. However there are many
routing libraries build to work with React.

The library that has picked up the most adoption is [React Router](https://github.com/ReactTraining/react-router).

Currently React Router is in it's 4th version which was a fairly large rewrite and took on a fairly
 different paradigm than it's predecessors.
 
React Router 4 is purely **Component** based and offers a collection of Navigation Component that are composed 
declaratively just like any other Component in your App.

### Why do you need a router? 

The main job of a routing library is to handle the navigation within the application, this includes things like:

1. Mappings urls to specific view

2. Handling request and path parameters within urls

3. Respond to browser navigation events

4. Smoothing inconsistencies between browsers

### Do you always need a router?

Not every application will need a full blown routing solution and there are many articles on the topic of if your app
needs one:

[James K Nelson -- Do I Even Need A Routing Library](http://jamesknelson.com/even-need-routing-library/)


[Free Code Camp - You might not need React Router](http://jamesknelson.com/even-need-routing-library/)

You can even write your own solution leveraging browsers native navigation events or use a smaller library like
[history](https://github.com/ReactTraining/history) (the history library used behind the scenes in React Router).

But if you do decide you want to use a pre-build route solution React Router is a very mature library with a strong
community behind it.

**TODO** List modules here