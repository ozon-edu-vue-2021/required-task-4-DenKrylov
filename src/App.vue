<template>
  <div id="app">
    <Form />
  </div>
</template>

<script>
import Form from "./components/Form.vue";

export default {
  name: "App",
  components: {
    Form,
  },
};
</script>

<style>
#app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: rgb(255, 255, 255);
  box-sizing: border-box;
}
* {
  box-sizing: border-box;
}
*::selection {
  background: rgb(111, 119, 135);
  color: rgb(28, 30, 33);
}
body {
  display: flex;
  justify-content: center;
  height: 100%;
  background: rgb(52, 56, 61);
}

</style>
