Reading notes for [ANGULAR ROUTER: EMPTY PATHS, COMPONENTLESS ROUTES, AND REDIRECTS](http://vsavkin.tumblr.com/post/146722301646/angular-router-empty-paths-componentless-routes)

# Empty-path route: 

Used to instantiate components without consuming url.

With Empty-path route, we can load data, guard child routes without consuming url. Combine empty path and componentless, we can do the same without consuming url and instanciate components.

# Mathcing strategy: 

1. default strategy is: "prefix", can configured explicitly: `pathMatch: 'prefix'`

1. another strateg is "full", can be configured: `pathMatch: 'full'`
 * mainly used for redirection
 * with the default "prefix", empty path would match all child routes.
 
# Absolute vs relative redirects

1. absolute redirect start with '/'
1. relative redirect start with name, relative to current path.


# Componentless route:

Componentless routes “consume” URL segments without instantiating components.

```
[
  {
    path: 'team/:id',
    children: [
      {
        path: '',
        component: TeamListComponent
      },
      {
        path: '',
        component: TeamDetailsComponent,
        outlet: 'aux'
      }
    ]
  }
]
```
The above configure loads both TeamListComponent and TeamDetailsComponent with url `team/11`. Both components can access the route parameters directly, so that they are independent and re-useable.

# Named outlet
Named outlet allow route to be rendered in specified router-outlet.
The following routes implement master/detail screen, the navigation to `team/11` will result in `team/11/(list/default//aux:details)`
```
[
  {
    path: 'team/:id',
    children: [
      { path: '', pathMatch: 'full', redirectTo: 'list' },
      { path: 'list', component: TeamListComponent,
        children: [
       { path: '', pathMatch: 'full', redirectTo: 'default' },
       { path: 'default', component: DefaultComponent }
        ]
      },
      { path: '', pathMatch: 'full', redirectTo: 'details' outlet: 'aux' }
    ]
  }
]
```


