<template>
  <el-container>
    <el-header>
      <headerBar></headerBar>
    </el-header>
  <el-container>
    <el-aside width='auto'>
      <sideBar :contraction='arrow' @selecChange='menuChange'></sideBar>
      <el-popover
        placement="bottom-end"
        trigger="click"
        v-model="showUpList">
        <globalUploader @addFile='addNums' @pop='showPop'></globalUploader>
        <el-badge :value="fileNum" slot="reference" class="el-icon-sort-item" type='primary' :hidden="fileNum === 0">
          <i class='el-icon-upload2'></i>
        </el-badge>
      </el-popover>
    </el-aside>
    <el-main>
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item v-for='(item, index) in menu' :key='index'>{{item}}</el-breadcrumb-item>
      </el-breadcrumb>
      <router-view></router-view>
    </el-main>
  </el-container>
</el-container>
</template>
<script>
import headerBar from '../components/headerBar'
import sideBar from '../components/sideBar'
import globalUploader from '../components/tool/globalUploader.vue'
export default {
  components: {
    headerBar,
    sideBar,
    globalUploader
  },
  data () {
    return {
      arrow: true,
      input: '',
      fileNum: 0,
      showUpList: false
    }
  },
  methods: {
    menuChange (e) {
      switch (e) {
        case '1-1':
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.overview')])
          this.$router.push('./index')
          break
        case '1-2':
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.transaction')])
          this.$router.push('./blockInfor')
          break
        case '1-3':
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.nodeList')])
          this.$router.push('./group')
          break
        case '2-1':
          this.$store.commit('set_menu', [this.$t('sideBar.topic'), this.$t('sideBar.topicList')])
          this.$router.push('./topicList')
          break
        case '2-2':
          this.$store.commit('set_menu', [this.$t('sideBar.topic'), this.$t('sideBar.subcription')])
          this.$router.push('./subcription')
          break
        case '2-3':
          this.$store.commit('set_menu', [this.$t('sideBar.topic'), this.$t('sideBar.statistics')])
          this.$router.push('./statistics')
          break
        case '4-1':
          this.$store.commit('set_menu', [this.$t('sideBar.engine'), this.$t('sideBar.ruleMana')])
          this.$router.push('./rule')
          break
        case '4-2':
          this.$store.commit('set_menu', [this.$t('sideBar.engine'), this.$t('sideBar.sources')])
          this.$router.push('./dataBase')
          break
        case '4-3':
          this.$store.commit('set_menu', [this.$t('sideBar.engine'), this.$t('sideBar.timerSchedule')])
          this.$router.push('./timerSchedule')
          break
        case '5-1':
          this.$store.commit('set_menu', [this.$t('sideBar.fileTranspoart'), this.$t('file.transpoartList')])
          this.$router.push('./fileTranspoart')
          break
      }
    },
    getMenu () {
      let url = this.$route.path
      switch (url) {
        case '/':
          this.$store.commit('set_active', '1-1')
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.overview')])
          this.$router.push('./index')
          break
        case '/index':
          this.$store.commit('set_active', '1-1')
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.overview')])
          break
        case '/blockInfor':
          this.$store.commit('set_active', '1-2')
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.transaction')])
          break
        case '/transactionInfor':
          this.$store.commit('set_active', '1-2')
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.transaction'), this.$t('sideBar.transactionDetial')])
          break
        case '/group':
          this.$store.commit('set_active', '1-3')
          this.$store.commit('set_menu', [this.$t('sideBar.blockChainInfor'), this.$t('sideBar.nodeList')])
          break
        case '/topicList':
          this.$store.commit('set_active', '2-1')
          this.$store.commit('set_menu', [this.$t('sideBar.topic'), this.$t('sideBar.topicList')])
          break
        case '/subcription':
          this.$store.commit('set_active', '2-2')
          this.$store.commit('set_menu', [this.$t('sideBar.topic'), this.$t('sideBar.subcription')])
          break
        case '/statistics':
          this.$store.commit('set_active', '2-3')
          this.$store.commit('set_menu', [this.$t('sideBar.topic'), this.$t('sideBar.statistics')])
          break
        case '/rule':
          this.$store.commit('set_active', '4-1')
          this.$store.commit('set_menu', [this.$t('sideBar.engine'), this.$t('sideBar.ruleMana')])
          break
        case '/ruleDetail':
          this.$store.commit('set_active', '4-1')
          this.$store.commit('set_menu', [this.$t('sideBar.engine'), this.$t('sideBar.ruleMana'), this.$t('sideBar.ruleDetail')])
          break
        case '/dataBase':
          this.$store.commit('set_active', '4-2')
          this.$store.commit('set_menu', [this.$t('sideBar.engine'), this.$t('sideBar.sources')])
          break
        case '/timerSchedule':
          this.$store.commit('set_active', '4-3')
          this.$store.commit('set_menu', [this.$t('sideBar.engine'), this.$t('sideBar.timerSchedule')])
          break
        case '/fileTranspoart':
          this.$store.commit('set_active', '5-1')
          this.$store.commit('set_menu', [this.$t('sideBar.fileTranspoart'), this.$t('file.transpoartList')])
          break
      }
    },
    addNums (e) {
      this.fileNum = e
    },
    showPop (e) {
      this.showUpList = true
    }
  },
  mounted () {
    let vm = this
    let H = document.body.clientWidth
    if (H < 740) {
      vm.arrow = false
    } else {
      vm.arrow = true
    }
    window.onresize = function () {
      let w = document.body.clientWidth
      if (w < 740) {
        vm.arrow = false
      } else {
        vm.arrow = true
      }
    }
  },
  created () {
    this.getMenu()
  },
  computed: {
    menu () {
      return this.$store.state.menu
    },
    lang () {
      return this.$store.state.lang
    },
    msgList () {
      return this.$store.state.msg
    }
  },
  watch: {
    lang (nVal) {
      this.getMenu()
    },
    $route (to, from) {
      this.msgList.forEach(e => {
        e.close()
      })
    }
  }
}
</script>
