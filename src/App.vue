<template>
  <div id="app">
    <MultiplePhones :phonesInputString="sampleString[1]" />
  </div>
</template>

<script>
import MultiplePhones from "./components/MultiplePhones.vue";

export default {
  name: "App",
  components: {
    MultiplePhones,
  },
  data() {
    return {
      sampleString: [
        "+123 456 7889;WA: abc123;hello",
        "+123 9851 1111 ; WA: +44 741 579 1111; old: +33 7 53 41 11 11 ; mobile: sdfgsdfg",
        "Home: 234-8643; mobile: 7513-98 (don’t call after 20); +28-7546-38436",
      ],
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
