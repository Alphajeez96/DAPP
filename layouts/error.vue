<template>
  <div>
    <h1 v-if="error.statusCode === 404">Page not found</h1>
    <h1 v-else>An error occurred</h1>
    <!-- <button @click="$router.go(-1)">Home page</button> -->
  </div>
</template>

<script>
export default {
  props: ["error"],
  layout: "error",
};
</script>
