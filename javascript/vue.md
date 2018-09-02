# Vue Notes #




```
<button v-on:click="$emit('remove', index)">delete</button>

<div v-for="(row, index) in items"
  v-bind:key="index"
  v-bind:index="index"
  v-bind:item="row"
  v-on:remove="$store.dispatch('people/deleteByIndexAndLoad', index)"
  v-on:error="$store.dispatch('error/handleError', index)"
>
</div>
```

```
{
  namespaced: true,
  state: {
    items: []
  },
  actions: {
    action: function (ctx, data) {
      ctx.commit('MUTATION_NAME');
      ctx.dispatch('ACTION_NAME', data, { root: true });
    }
  },
  mutations: {
    MUTATION_NAME: function (state, payload) {
      state.items = payload;
    }
  }
}
```

```
//  https://stackoverflow.com/questions/36170425/detect-click-outside-element
Vue.directive('click-outside', require('../common/directives/click-outside'));
```

VueJS Productivity Tips:
https://www.youtube.com/watch?v=7lpemgMhi0k

```
watch: {
  searchText: {
    handler: 'fetchUserList',
    immediate: true
  }
}
```
same as:
```
{
  created: function () {
    this.fetchUserList();
  },
  watch: {
    searchText: function () {
      this.fetchUserList();
    }
  }
}
```


Resetting VueRouter Components
```
<router-view :key="$route.fullPath"></router-view>
```

```
<template>
  <input
    v-bind:value="value"
    v-on:input="$emit('input', $event.target.value)"
  />
</template>
```
