# vueexam

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## Project remarks

### We used HTML5 drag & drop API directly instead of a vue-draggable API which incorporates these drag & drop events. The former allows for more controlability while the latter would make datatransfer easier (no need to use emits etc.) and has other builtin features (such as drag & drop visual effects).
