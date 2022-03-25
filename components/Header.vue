<template>
  <header>
    <nav>
      <!-- Logo Here -->

      <n-link to="/">
        <p>dec<span class="logo">FundMe</span></p>
      </n-link>

      <!--  Button -->
      <div>
        <button @click="$emit('connect')" :disabled="isSupported">
          {{ isSupported ? "Connected" : "Connect" }}
        </button>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  props: {
    isSupported: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
<style scoped>
nav {
  display: flex;
  justify-content: space-between;
  padding: 1rem 2rem;
}

p {
  color: #94a5c5;
  font-size: 1.5rem;
  line-height: 32px;
  font-weight: 700;
  text-align: left;
  font-family: "Libre Baskerville", serif;
  margin-top: 0px;
  margin-bottom: 0px;
  margin-left: 0px;
  margin-right: 0px;
  position: relative;
  z-index: 4;
  display: inline-block;
}

p::before {
  content: "";
  position: absolute;
  height: 10px;
  width: 80%;
  bottom: 5px;
  background-color: #50d8af;
  display: block;
  z-index: -1;
}
span.logo {
  color: #1c4298;
  line-height: 20px;
  font-weight: 600;
  text-align: left;
  font-family: "Nunito", sans-serif;
  margin-top: 0px;
  margin-bottom: 0px;
  margin-left: 0px;
  margin-right: 0px;
}
</style>
