<template>
  <div id="quote">
    <h2><i>"{{quote}}"</i> K.W.</h2>
  </div>
</template>

<script>
export default {
  name: "Quote",
  props: ["quote"]
};
</script>

<style lang="scss">
#quote{
  background-color: bisque;
  padding: 1%;
  margin-top: 3%;
}
</style>
