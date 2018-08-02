## The following is the default vue-router configurations.
* As we can see the bundled file `app.a92b8aac.js` after we built the application, both of the home page and about page
need to load the bundled js file that has `5.11 kb`.
```
import Vue from "vue";
import Router from "vue-router";
import Home from "./views/Home.vue";
import About from "./views/About.vue";

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: "/",
      name: "home",
      component: Home
    },
    {
      path: "/about",
      name: "about",
      component: About
    }
  ]
});
```

```
$ npm run build

File                                 Size               Gzipped
dist\js\chunk-vendors.64ea2280.js    102.32 kb          36.16 kb
dist\js\app.a92b8aac.js              5.11 kb            1.81 kb
dist\css\app.06a63df8.css            0.42 kb            0.26 kb
```

## The following is an example of lazy loading routes.
* Removed the static imports for the home and about components.
* Instead, we will dynamically load the component on demands.
* Now, the home page loads total `2.76 kb` file sizes and the about page loads total `0.44 kb` file sizes.
Without lazy loading routes, each page needs to load `5.11 kb`. 
```
import Vue from "vue";
import Router from "vue-router";

Vue.use(Router);

export default new Router({
  routes: [
    {
      path: "/",
      name: "home",
      component: () => import(/* webpackChunkName: "home" */ "./views/Home.vue")
    },
    {
      path: "/about",
      name: "about",
      component: () => import(/* webpackChunkName: "about" */ "./views/About.vue")
    }
  ]
});
```

```
$ npm run build

File                                 Size               Gzipped
dist\js\chunk-vendors.64ea2280.js    102.32 kb          36.16 kb
dist\js\app.634b028c.js              4.01 kb            1.80 kb
dist\js\home.c780fe69.js             2.76 kb            0.92 kb
dist\js\about.472b240a.js            0.44 kb            0.31 kb
dist\css\app.8e6cbdf0.css            0.25 kb            0.19 kb
dist\css\home.26b950f4.css           0.17 kb            0.13 kb
```