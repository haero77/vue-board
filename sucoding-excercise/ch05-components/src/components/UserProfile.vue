<template>
  <div>
    <!-- 자식 컴포넌트에서 부모 컴포넌트로 이벤트 발신  -->
    <!-- 이벤트 발신: 인라인 핸들러 발신  -->
    <button @click="parentEventCall">Hello 클릭</button>
  </div>
  <div>
    <p>이름: {{ userName }}</p>
    <p>나이: {{ age }}</p>
  </div>
</template>


<script>
export default {
  // 발신 가능한 부모 컴포넌트의 이벤트 명시
  emits: ['print-hello'],

  // 속성값 전달 받을 시 props 속성 사용
  props: {
    userName: {
      type: String, // userName 속성값이 문자열인지 검사
      required: true, // userName 속성값이 필수인지 검사
      validator: function (value) {
        return value.length > 1;
      }
    },
    age: {
      type: Number,
      default: function () {
        return 10;
      },
    }
  },
  methods: {
    // 메서드 핸들러 방식의 이벤트 발신
    parentEventCall() {
      // $emit 내장 메서드 사용하여 부모 컴포넌트로 이벤트 발신할 때,
      // 첫 번째 인자로 부모 컴포넌트 이벤트 명시.
      // 두 번째 인자부터 부모 컴포넌트 이벤트의 '매개변수'로 값을 전달함.
      this.$emit('print-hello', '철수', 30);
    }
  },
  created() {
    // props의 기본 자료형은 string이다.
    console.log('typeof age:', typeof this.age);
  }
}
</script>


<style scoped>

</style>
