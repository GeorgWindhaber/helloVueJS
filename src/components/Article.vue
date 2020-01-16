<template>
  <div id="fullArticle">
    <h2>{{name}}</h2>
    <h4>{{excerpt}}</h4>
    <p>{{text}}</p>
  </div>
</template>

<script>
export default {
  name: "Article",
  props: {
    name: String,
    excerpt: String,
    text: String
  }
};
</script>

<style>
#fullArticle {
  margin: 0.5em;
}
</style>