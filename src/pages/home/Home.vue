<template>
  <div>
    <home-header></home-header>
    <home-swiper :list="swiperList"></home-swiper>
    <home-icons :list="iconList"></home-icons>
    <home-recommend :list="recommendList"></home-recommend>
    <home-weekend :list="weekendList"></home-weekend>
      
  </div>
</template>

<script>
//Composition API
import HomeHeader from './components/Header'
import HomeSwiper from './components/Swiper'
import HomeIcons from './components/Icons'
import HomeRecommend from './components/Recommend'
import HomeWeekend from './components/Weekend'
import axios from 'axios'
import { useStore } from 'vuex'
import { ref, onMounted} from 'vue'
// import func from 'vue-editor-bridge'
export default {
  name: 'Home',
  components: {
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  setup() {
    const swiperList = ref([])
    const iconList = ref([])
    const recommendList = ref([])
    const weekendList = ref([])
    const store = useStore()
    const city = store.state.city  
    async function getHomeInfo() {
       let res = await axios.get('/api/index.json?city=' + city)
        res = res.data
          if (res.ret && res.data) {
            const result = res.data
            swiperList.value = result.swiperList
            iconList.value = result.iconList
            recommendList.value = result.recommendList
            weekendList.value = result.weekendList
          }
     }
     onMounted(() => {
        getHomeInfo()
     })
     return { swiperList, iconList, recommendList, weekendList }
  }
//   data () {
//     return {
//       lastCity: '',
//       swiperList: [],
//       iconList: [],
//       recommendList: [],
//       weekendList: []
//     }
//   },
//   computed: {
//     ...mapState(['city'])
//   },
//   methods: {
//     getHomeInfo () {
//       axios.get('/api/index.json?city=' + this.city)
//         .then(this.getHomeInfoSucc)
//     },
//     getHomeInfoSucc (res) {
//       res = res.data
//       if (res.ret && res.data) {
//         const data = res.data
//         this.swiperList = data.swiperList
//         this.iconList = data.iconList
//         this.recommendList = data.recommendList
//         this.weekendList = data.weekendList
//       }
//     }
//   },
//   mounted () {
//     this.lastCity = this.city
//     this.getHomeInfo()
//   },
//   activated () {
//     if (this.lastCity !== this.city) {
//       this.lastCity = this.city
//       this.getHomeInfo()
//     }
//   }
}
</script>

<style>

</style>
