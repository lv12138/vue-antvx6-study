<template>
  <div class="home">
    <el-container>
      <el-header>
<!--        <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect" router:true>-->
<!--          <el-menu-item index="/tab1" route="tab1">处理中心</el-menu-item>-->
<!--        </el-menu>-->
        <router-link to="/tab1">tab1</router-link>
      </el-header>
      <el-main>
        <router-view/>
      </el-main>
    </el-container>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  components: {
  },
  data () {
    return {
      activeIndex: '1'
    }
  },
  methods: {
    handleSelect (key, keyPath) {
      console.log(key, keyPath)
    }
  }
}
</script>
