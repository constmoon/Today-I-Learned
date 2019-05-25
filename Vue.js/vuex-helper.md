# Vue helper
Vuex에 선언한 속성들을 뷰 컴포넌트에서 더 쉽게 불러올 수 있도록 연결해주는 역할을 한다.

* state → mapState
* getters → mapGetters
* mutations → mapMuations
* actions → mapActions

헬퍼를 사용하지 않는다면 `this.$store.state.uid`, `this.$store.dispatch("fetchPosts")`로 접근한다. 
헬퍼를 사용하면 하나의 컴포넌트에서 여러 개의 모듈을 바인딩하는 것이 쉬워지며, 코드의 가독성 또한 좋아지는 장점이 있다.

## helper의 사용법
- 헬퍼를 사용하고자하는 컴포넌트에서 해당 헬퍼를 import한다.
- 싱글파일 컴포넌트 자체에서 사용할 computed 속성과 함께 사용할 수 없으므로, `...`(Spread Operator)를 사용하여 추가해야한다.
```
// App.vue
import { mapActions } from "vuex"

export default {
  computed: {
  ...mapGetters(['uid'])
  },
  methods: {
  ...mapActions(['fetchPosts'])
  }
}
```
