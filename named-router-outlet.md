# Named `<router-outlet>`

I tried work with lazy loading and named router-outlets and found the following facts: 

1. auxiliary routes must be children to a parent route with path **not** being `''`. They won't work as children of empty path route. It may be fixed soon.
1. auxiliary routes are isolated within the parent route. Their matching rule is completely different from standard Angular routing rule. 
1. if we lazy load the module with auxiliary routes, we need to redirect the default empty path route to a route with non-empty path, which has component that holds the named `<router-outlet>` and its children would be auxiliary routes and other child routes. (It may be fixed soon, same as point 1.)
1. you can load the named outlet by: url (`parentRoute/outletName:['outletPath', param]`), or using Router Link directive(`[routerLink]=['parentRoutes',{outlets: {outletName:['outletPath', param]}}]`, or programatically (`this.router.navigate(['parentRoutes',{outlets: {outletName:['outletPath', param]}}]`)

1. To remove a named outlet, specify `outlets: {outletName: null}` in url, routerLink or code.

1. Due to a bug(redirect empty path child to named route does not work), there is currently no elegant way to load a named route by default and be able to later remove it with the above method. It may be fixed soon. However, you could have an empty path with a dummy component and in its constructor, you navigate to a URL that loads the default named route. Or you could do it in the parent component.
