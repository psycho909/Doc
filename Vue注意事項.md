# 不要在使用v-for的同一元素上使用v-if

```vue
<ul>
  <li
    v-for="game in games"
    v-if="game.isActive"
    :key="game.slug"
  >
    {{ game.title }}
  <li>
</ul>
```

