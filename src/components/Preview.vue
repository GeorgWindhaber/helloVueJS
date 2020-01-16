<template>
  <li class="box">
    <h4>
      {{name}}
      <span class="read" v-if="read">read</span>
    </h4>

    <p v-if="text">{{text.slice(0, 150)}}...</p>
    <p v-else>Nothing detail-worthy happenend on this launch 🚀</p>
  </li>
</template>

<script>
export default {
  name: "Preview",
  props: {
    name: String,
    text: String,
    read: Boolean
  }
};
</script>

<style>
.box {
  border: 1px solid #000;
  border-radius: 5px;
  padding: 0.5em;
  margin-bottom: 1em;
}

.read {
  color: #a8a8a8;
}
</style>