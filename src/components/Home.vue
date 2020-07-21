<template>
  <div>
    <img @click="showDrawer = !showDrawer" alt="Vue logo" src="@/assets/logo.png" class="mx-auto"/>
    <h1 class="text-xl">Click logo</h1>


    <!-- <transition name="fadeFast"> -->
      <Drawer v-if="showDrawer" 
        @close="showDrawer = false">
        <div>
          Native iOS Drawer
          <img alt="Vue logo" src="@/assets/logo.png" class="mx-auto"/>
          <img alt="Vue logo" src="@/assets/logo.png" class="mx-auto"/>
          <img alt="Vue logo" src="@/assets/logo.png" class="mx-auto"/>
          <img alt="Vue logo" src="@/assets/logo.png" class="mx-auto"/>
          <img alt="Vue logo" src="@/assets/logo.png" class="mx-auto"/>
          <img alt="Vue logo" src="@/assets/logo.png" class="mx-auto"/>
        </div>

      </Drawer>
    <!-- </transition> -->

  </div>
</template>

<script>
import Drawer from '@/components/Drawer.vue'

export default {
  components: {
    Drawer,
  },
  props: {
    msg: String,
  },
  data: function () {
    return {
      showDrawer: false,
    }
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* Fade fast transition */
.fadeFast-enter-active,
.fadeFast-leave-active {
  transition: opacity 0.1s ease;
}

.fadeFast-enter,
.fadeFast-leave-to {
  opacity: 0;
}

h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
