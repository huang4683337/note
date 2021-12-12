## 方法1 - xxx.vue

*html 中无代码提示*

```vue
<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class HelloWorld extends Vue {
  @Prop() private msg!: string;
}
</script>
```



## 方法2 - xxx.vue

```js
// Vue VSCode Snippets 插件下使用 vbase
// 选择 vue-ts
<script lang="ts">
import Vue from "vue";

export default Vue.extend({});
</script>
```



## 方法3 - xxx.tsx

```tsx
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class HelloWorld extends Vue {
    @Prop() private msg!: string;

    render() {
        return <div>{this.msg}</div>
    }
}
```

