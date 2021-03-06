See [React Router Material](../../material/4_routing/readme.md) and [React Router Official Documentation](https://reacttraining.com/react-router/)

## Installation

Install **React Router 4** with `yarn`:

`yarn add react-router-dom`


## Step 1: Add a Router to your application

(Click [here](../../material/4_routing/readme.md) to learn more about `<Router>`)

The first step to integrating **React Router 4** with your application is to add in the root `<BrowserRouter>`.
This `Router` will need to be the parent of all your routing components so it should go fairly high up in your Component tree.

Start by importing `<BrowserRouter>` from the `react-router-dom` package you installed within `App.js`

```javascript 1.8
import {
    BrowserRouter as Router
} from 'react-router-dom'
```

Within `App.js` wrap all the existing content within the newly imported `Router`:



```javascript 1.8
            <Router>
                <div className="App">
                    <header className="App-header">
                        <Header/>
                    </header>
                    <Landing />
                </div>
            </Router>
```

## Step 2: Add Routes

(Click [here](../../material/4_routing/1_react_router_basics/readme.md) to learn more about `<Routes>` and `<Switches>`)

The next thing we need to do it start adding the different routes in.

Add `Route` and `Switch` to your existing import statement:

```javascript 1.8
import {
    BrowserRouter as Router,
    Route,
    Switch
} from 'react-router-dom'
```

Also import the component views you want to render on each `Route`

```javascript 1.8
import About from './components/About/About'
import SignIn from './components/SignIn/SignIn'
import Register from './components/Register/Register'
import Profile from './components/Profile/Profile'
import PageNotFound from './components/PageNotFound/PageNotFound'
```


Finally, within your `Router` define a `Switch` and the children `Routes`

```javascript 1.8
        <Router>
            <div className="App">
                <header className="App-header">
                    <Header/>
                </header>
                <Switch>
                    <Route path="/about" component={About} />
                    <Route path="/signin" component={SignIn} />
                    <Route path="/register" component={Register} />
                    <Route path="/profile" component={Profile} />
                    <Route path="/" exact component={Landing} />
                    <Route component={PageNotFound} />
                </Switch>
            </div>
        </Router>
```

As explained in the [React Router](../../material/4_routing/1_react_router_basics/readme.md) material, when the url matches one of
the `paths` specified on a `Route` the specified `component` will be rendered.

For the Routes wrapped in a `Switch`, only the first matching `Route` will be rendered.

The above will render the `<Landing>` component on the root path `/` or the `<About>` component on `/about` url.

You can try this now by hitting: `http://localhost:3000/` or `http://localhost:3000/about`.

Any unknown url should hit the 404 `<PageNotFound>` component.


As you build out your app register any new pages you create in this `<Router>`.


## Step 2: Adding Links

The next step is to add some real link functional to `Header.js`.

Start by importing `Link` from `react-router-dom`:

```javascript 1.8
import { Link } from 'react-router-dom'
```

Next replace all the `span` navigation with `Link` 

```javascript 1.8
        <div className="header-container">
            <Link to="/" className="home">Home</Link>
            <Link to="/about" className="nav-items">About</Link>
            <Link to="/signin" className="nav-items">Sign In</Link>
            <Link to="/register" className="nav-items">Create Account</Link>
        </div>
```

Now click on **Home** or **About** should cause the `<Landing>` or `<About>` component to render when clicked


## Step 3: Profile Parameters

(see [Parameters](../../material/4_routing/2_parameters/readme.md) for more information)

Next we will work on passing an `id` to the `<Profile>` component.

In `App.js` update the `<Profile>` Route to take a id parameter in the path `/profile/:id`:

`<Route path="/profile/:id" component={Profile} />`

Next, modify `Profile.js`, for now just add a `componentDidMount` lifecycle hook and print the id to the console:

```javascript 1.8
    componentDidMount() {
        const id = this.props.match.params.id
        console.log(`The profile id is ${id}`)
    }
```

Now you can hit `http://localhost:3000/profile/123` and you should see the profile id print the in debug console.

## Step 4: Authorized Route

(see [Authenicated Routes](../../material/4_routing/3_authenticated_routes/readme.md))

We may have certain routes we only want logged in users to be able to view so we need to add an Authenticated Route to
check if the user is logged in, if not direct them to the login page.

### Part 1

Create a new component called `AccountInformation`, which should only be accessed by logged in user.


```javascript 1.8
import React from 'react'

const AccountInformation = () => {
    return (
        <div>Account Information -- should only be viewable to logged in users</div>
    )
}

export default AccountInformation
```

### Part 2

Create a second new component called `AuthenticatedRoute`:

```javascript 1.8
import React from 'react'
import { Route, Redirect } from 'react-router-dom'

// Temporary fake authentication
const fakeAuth = {
    isAuthenticated: false
}

const AuthenticatedRoute = ({ component: Component, ...rest }) => (
    <Route {...rest} render={props => (
        fakeAuth.isAuthenticated ? (
            <Component {...props}/>
        ) : (
            <Redirect to={{
                pathname: '/signin',
                state: { from: props.location }
            }}/>
        )
    )}/>
)

export default AuthenticatedRoute
```

For now this component will check `fakeAuth` and if the user is authenticated render the passed in `<Component>` otherwise the `<Redirect>`
will send the user the signin in url.

### Part 3

Add the new `<AuthenticatedRoute>` into `App.js`.

Import your `<AuthenticatedRoute>` and `<AccountInformation>` components and add the new route within your existing `<Switch`>

```javascript 1.8
import AccountInformation from './components/AccountInformation/AccountInformation'
import AuthenticatedRoute from './components/AuthenticatedRoute/AuthenticatedRoute'
```

```javascript 1.8
...
<AuthenticatedRoute path="/account-information" component={AccountInformation} />
<Route component={PageNotFound} />
```

Now if you hit `http://localhost:3000/account-information` you should be redirected
to the sign in page.

 If `isAuthenticated` to `true` in the fake authentication in `AuthenticatedRoute` you should
 be able to access this url.


 Next up [Redux Lab](../2_redux/readme.md)
