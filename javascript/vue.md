# Vue Notes #


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




