<template>
  <div class="home">
    <header class="d-flex align-items-center">
        <a class="logo">
            <span class="icon-logo"></span>
        </a>
    </header>
    <dashboard msg="Welcome to Upload Files App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Dashboard from '@/components/Dashboard.vue'

export default {
  name: 'home',
  components: {
    Dashboard
  }
}
</script>
<style>
  .home {
    background-color: #f9fcff;
    height: 100vh;
  }
    header {
        height: 60px;
        background: #fff;
        box-shadow: 0 0 1px 0 #d2deed;
        padding: 0 1.2rem;
    }
    .icon-logo {
        display: inline-block;
        width: 36px;
        height: 26px;
        background-image: url('../assets/image/Logo.svg');
        background-size: contain;
        background-repeat: no-repeat;
        cursor: pointer;
    }
  .upload-start {
      width: 25%;
      color: #FFFFFF!important;
      padding: 12px 5px!important;
      font-size: 11px;
      font-weight: bold;
      line-height: 15px;
      border-radius: 2px;
      -webkit-border-radius: 2px;
      -moz-border-radius: 2px;
      background-color: #1A8CFF!important;
      text-transform: uppercase;
      outline: none;
      border-color: transparent!important;
  }
</style>
