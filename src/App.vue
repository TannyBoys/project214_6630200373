<template>
  <div class="container">
    <header>
        <h1>ประวัติการศีกษาของผม</h1>
    </header>
    <Myinfo/>
    <Courseinfo/>
    <Addcourseinfo/>
  </div>
</template>

<script>
import {EventBus} from './event-bus';
import Myinfo from './components/myinfo.vue';
import Courseinfo from './components/courseinfo.vue';
import Editmyinfo from './components/editmyinfo.vue';
export default {
  name:"App",
  components: {Myinfo, Courseinfo,Editmyinfo},
}
</script>

<style>
body {
  font-family: "Itim", cursive;
  line-height: 1.5;
  background-color: rgb(104, 12, 12);
}

header {
  box-shadow: 0 2px 7px #242323;
  margin: 7px;
  margin-top: 1rem;
  padding: 10px;
  background: #333;
  color: white;
  text-align: center;
}
</style>