<template>
  <div class="card choose-test-card">
    <div class="row">
      <div class="col l3 m3 s6">
        <div class="card-item title">
          <slot name="predicate-mine" />{{ test.title }}
        </div>
      </div>
      <div class="col l3 m3 s6">
        <div class="card-item">{{ test.category }}</div>
      </div>
      <div class="col l3 m3 s6">
        <div class="card-item">{{ test.language }}</div>
      </div>
      <div class="col l3 m3 s6">
        <div class="card-item">
          <div
            class="lightner"
            :class="{ 'lightner-red': !isDone, 'lightner-green': isDone }"
          ></div>
        </div>
      </div>
      <div class="col s12">
        <div class="card-item">
          <!-- <app-preloader v-if="loading" /> -->
          <slot name="action-button"></slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Все что закомментировано планировалось для прелоадера на каждом компоненте
// Не вышло: Обновление книги сопоставить с прелоадером на компоненте, конкретно
// на том что вызвал обновление книги.

import { ref } from "@vue/reactivity";
import { isDoneFromLocalStorage } from "../assets/js/TestHandlers/testHandlers";
import { onMounted } from "@vue/runtime-core";
// import { useGeneralBook } from "../use/generalBook";
// import appPreloader from "../components/appPreloader.vue";
export default {
  // components: {
  //   appPreloader,
  // },
  props: {
    test: {
      type: Object,
      required: true,
    },
  },
  emits: ["bookChanged"],
  setup(props) {
    const isDone = ref(false);
    const loading = ref(false);
    // const { add: addToGeneralBook } = useGeneralBook();

    onMounted(() => {
      isDone.value = isDoneFromLocalStorage(props.test.title);
    });

    // const add_toGeneral = async (title) => {
    //   console.log(111111);
    //   loading.value = true;
    //   await addToGeneralBook(title);
    //   emit("bookChanged");
    //   loading.value = false;
    // };

    return { isDone, loading };
  },
};
</script>

<style lang="less" scoped>
@import url("../assets/css/mainStyles.less");
.choose-test-card {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 70px;
  margin: 10px 0;
}
.lightner {
  width: 20px;
  height: 20px;
  border-radius: 10px;
  &-red {
    background-color: red;
    box-shadow: 0 0 7px rgba(255, 0, 0, 80%);
  }
  &-green {
    background-color: #4aac4d;
    box-shadow: 0 0 7px #a5d6a7;
  }
}
.row {
  width: 100%;
  margin: 0;
}
.card-item {
  min-height: 40px;
}
.title {
  font-weight: 700;
}
@media only screen and (max-width: 600px) {
  .card-item {
    &:not(:last-child) {
      border-right: 1px solid @choosen-menu-options-color;
      padding-right: 7px;
    }
    &:not(:first-child) {
      padding-left: 10px;
    }
  }
}
</style>
