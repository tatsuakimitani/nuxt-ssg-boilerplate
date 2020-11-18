
  
<p align="center">
  <img width="400" src="https://raw.githubusercontent.com/dennisfrijlink/development-utilities/959061a8b1370b62a6b9e48549dc1d272564485e/images/Nuxt-SSG.svg" alt="logo of Nuxt Single Page Application Repository">
</p>
<h1 align="center">
  Static Page Generation Boilerplate - Nuxt.js
</h1>
<p align="center">
  A boilerplate for static site generation based on the Vue.js Framework, Nuxt.js
  </a>
</p>

## 🧐 What's inside
This Repository is an expansion of the [Nuxt SPA Boilerplate](https://github.com/dennisfrijlink/nuxt-spa-boilerplate) repository.

- [Quick start](#user-content--quick-start)
- [What is SSG?](#user-content-️-what-is-static-site-generation)
- [Generating Routes](#user-content-️-generating-routes)

## ✨ Quick start

1.  **Clone this repository.**

    ```sh
    git clone https://github.com/dennisfrijlink/nuxt-ssg-boilerplate.git
    ```

2.  **Start developing.**

    Navigate into your new site’s directory and start it up.

    ```sh
    cd nuxt-ssg-boilerplate/
    npm install
    npm run dev
    ```

3.  **Running!**

    Your site is now running at `http://localhost:3000`!

4.  **Generate for deploy**
    
    Generate a static project that will be located in the ``dist`` folder:
    ```bash
    $ npm run generate
    ```
## ⚙️ What is Static Site Generation
Static Site Generators are a  hybrid approach to web development that allow you to build a powerful, server-based website locally on your computer but pre-builds the site into static files for deployment.

### Advantages of Static Sites

#### 1) Fast

Static sites are fast! Since there are no database queries, no templates to render, and no client-server requests to process, a static site will always load faster.

#### 2) Secure

Dynamic websites are constantly at risk of attack. Drupal, which powers 5% of all websites (12+ million users) was  recently hacked  and Wordpress, which powers 26% of all websites, is notorious for frequent security vulnerabilities.

#### 3) Cheap

Static sites costs pennies per month to run on services like Amazon S3. Dynamic sites require the setup, hosting, and maintenance of a server which is much, much more expensive.

#### 4) Scalable

Unexpected traffic surges can crash a dynamic site. A static site is much better prepared as delivering static pages consumes very little server resources.

<p align="center">
  <img width="100%" src="https://raw.githubusercontent.com/dennisfrijlink/development-utilities/main/images/SSG%20lifecycle.png" alt="Lifecycle of Single Page Application">
</p>

## 🗺️ Generating routes
`nuxt generate` with `target: 'static'` will pre-render all your pages to HTML and save a payload file in order to mock `asyncData` and `fetch` on client-side navigation, this means **no**  **more HTTP calls to your API on client-side navigation.** By extracting the page payload to a js file, **it also reduces the HTML size** served as well as preloading it (from the in the header) for optimal performance.

````js
// nuxt.config.js 
export default {
  target: 'static'
}
````

Full static **doesn't** work with `ssr: 'false'` (which is the same as the deprecated `mode: 'spa'`) as this is used for client-side rendering only (Single Page Applications).

When using the `nuxt/http` library you can define the baseURL in the `nuxt.config.js`:

```js
// nuxt.config.js

export default {
  modules: [
    ['@nuxt/http', {
      baseURL: 'https://api.nuxtjs.dev/mountains'
    }]
  ]
}
```

Now we can fetch de data
````js
// pages/index.vue

<template>
  <div>
    <h1>Nuxt.js SSG Boilerplate</h1>
    <ul>
      <li v-for="mountain in mountains" :key="mountain.slug">
        <nuxt-link :to="mountain.slug">{{ mountain.title }}</nuxt-link>
      </li>
    </ul>
  </div>	
</template>

<script>
  export  default {
    name: 'index',
    async asyncData({ $http }) {
      const mountains = await $http.$get('/mountains') // https://api.nuxtjs.dev/mountains
      return { mountains }
    }
  }
</script>

````

Now we can make the dynamic page component based on the `slug` of every `mountain`. Here we fetch the data based on the `slug` we pass in the `index.vue` component

````js
// _slug.vue
<template>
  <div>
    <h1>{{ mountain.slug }}</h1>
    <img :src="mountain.image" :alt="mountain.slug">
  </div>
</template>

<script>
  export default {
    async asyncData({ $http, params }) {
    const mountain = await $http.$get(`/mountains/${params.slug}`) // https://api.nuxtjs.dev/mountains/aconcagua
    return { mountain }
  }
}
</script>
````
Now if we run the command
```bash
$ npm run generate
```
Nuxt.js will generate a route for every mountain fetched from the API url:
````bash
√ Generated route "/"                                                                                         
√ Generated route "/mount-kosciuszko"                                                                         
√ Generated route "/mount-blanc"                                                                              
√ Generated route "/aconcagua"                                                                                
√ Generated route "/vinson-massif"                                                                            
√ Generated route "/mount-everest"                                                                            
√ Generated route "/denali"                                                                                   
√ Generated route "/mount-kilimanjaro"
````
