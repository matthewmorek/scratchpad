# Scratchpad

> Simple note-taking app, build with VueJS, Webpack, and Tachyons.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

## Methodology
This section will try and articulate my inner thoughts on how the app came to be, including the decision-making process.

### 1. Picking a toolkit
Since I never worked with either VueJS or React, I decided to give the former a go and see how things work. Vue seem to have a nice automated project setup tool called `vue-cli`, which I used as a boilerplate to kick things off.

### 2. Configuring
The set of defaults for the project were mostly fine for me, although I have never worked with Webpack before (never needed a transpiler, and Grunt/Gulp were fine for running basic tasks), so I had to configure a few things, add `node-sass` support, and so on.

### 3. Laying the foundations
I dropped in my go-to `.sass-lint.yml` and replaced `AirBnB` JS linter config with `Semistandard` which I’m more comfortable with. Then I added static resources (fonts and `index.html`.

Next came the baseline functionality, which after some trial and error finally started working together. Leaning heavily on VueJS documentation and examples, I've been able to create a basic app flow, storing data in VM temporarily.

After the baseline was done, I decided to make the editor UI a bit nicer and added some baseline CSS scaffolding. To speed the development and styling I used Tachyons.

### 4. Extending the baseline
Once the visuals were done, I started adding more functionality: LocalStorage support, exporting, creating notes faster, etc.

Since I'm generally pretty conscious about the quality of my code, I didn't have too much to clean or refactor to make the app work reliably and perform decently in poor network conditions.

---

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
