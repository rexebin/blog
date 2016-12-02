I tried lazy loading and named router-outlet and found following characteristics:

1. auxiliary routes must be children to a parent route with path not being ''. They won't work as children of empty path route.
1. auxiliary routes are isolated within the parent route. Their matching rule is completely different from standard Angular routing rule.
1. auxiliary routes' path must be single level name or parameter, like list or :id etc, without / in it. Path like detail/:id won't work.
1. if we lazy load the module with auxiliary routes, we need to redirect the empty path route to a route with non-empty path, which has component that holds the named <router-outlet> and its children would be auxiliary routes and other child routes.
1. As far as I know, there is no way to load a default named outlet. The only way to load named outlets are to specify outlets in url (parentRoute/(outletName:path)), or using Router Link directive([routerLink]=['parentRoutes',{outlets: {outletName:'outletPath'}}], or programatically (this.router.navigate(['parentRoutes',{outlets: {outletName:'outletPath'}}])
1. To hide a named outlet, specify outlets: {outletName: null} in url, routerLink or code.
