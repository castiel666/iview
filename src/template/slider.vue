<template>
  <section class="box">
    <div class="head">
      head
    </div>
    <div class="content">
      <div class="left" ref="left">
        <ul>
          <li v-for="(item, index) in left" :key="item" :class="{current: currentIndex == index}" @click="selectItem(index, $event)">
            <span class="left-item">{{item}}</span>
          </li>
        </ul>
      </div>
      <div class="right" ref="right">
        <ul>
          <li class="right-item right-item-hook" v-for="item in right" :key="item.name">
            <h2>{{item.name}}</h2>
            <ul>
              <li v-for="num in item.content" :key="num.name">
                <div>{{item.name+num}}</div>
              </li>
            </ul>
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>
<script>
import BScroll from 'better-scroll'
export default {
  data () {
    return {
      left: ['a', 'b', 'c', 'd', 'e', 'f'],
      right: [
        {
          name: 'a',
          content: ['1', '2', '3', '4', '5']
        },
        {
          name: 'b',
          content: ['1', '2', '3', '4', '5']
        },
        {
          name: 'c',
          content: ['1', '2', '3', '4', '5']
        },
        {
          name: 'd',
          content: ['1', '2', '3', '4', '5']
        },
        {
          name: 'e',
          content: ['1', '2', '3', '4', '5']
        },
        {
          name: 'f',
          content: ['1', '2', '3', '4', '5']
        },
      ],
      listHeight: [],
      scrollY: 0, //实时获取当前y轴的高度
      clickEvent: false
    }
  },
  methods: {
    _initScroll () {
      //better-scroll的实现原理是监听了touchStart,touchend事件，所以阻止了默认的事件（preventDefault）
      //所以在这里做点击的话，需要在初始化的时候传递属性click,派发一个点击事件
      //在pc网页浏览模式下，点击事件是不会阻止的，所以可能会出现2次事件，所以为了避免2次，可以在绑定事件的时候把$event传递过去
      this.lefts = new BScroll(this.$refs.left, {
        click: true
      })
      this.rights = new BScroll(this.$refs.right, {
        probeType: 3  //探针的效果，实时获取滚动高度
      })
      //rights这个对象监听事件，实时获取位置pos.y
      this.rights.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.round(pos.y))
      })
    },
    _getHeight () {
      let rightItems = this.$refs.right.getElementsByClassName('right-item-hook')
      let height = 0
      this.listHeight.push(height)
      for(let i = 0; i < rightItems.length; i++){
        let item = rightItems[i]
        height += item.clientHeight
        this.listHeight.push(height)
      }
    },
    selectItem(index,event){
      this.clickEvent = true
      //在better-scroll的派发事件的event和普通浏览器的点击事件event有个属性区别_constructed
      //浏览器原生点击事件没有_constructed所以当时浏览器监听到该属性的时候return掉
      if(!event._constructed){
        return
      }else{
        let rightItems = this.$refs.right.getElementsByClassName('right-item-hook')
        let el = rightItems[index]
        this.rights.scrollToElement(el, 300)
      }
    }
  },
  mounted () {
    this.$nextTick(() => {
      this._initScroll()
      this._getHeight()
    })
  },
  computed: {
    currentIndex () {
      for(let i = 0; i < this.listHeight.length; i ++){
        let height = this.listHeight[i]
        let height2 = this.listHeight[i + 1]
        //当height2不存在的时候，或者落在height和height2之间的时候，直接返回当前索引
        //>=height，是因为一开始this.scrollY=0,height=0
        if(!height2 || (this.scrollY >= height && this.scrollY < height2)){
          return i
        }
        if(this.listHeight[this.listHeight.length - 1] - this.$refs.right.clientHeight <= this.scrollY){
          if(this.clickTrue){
            return this.currentNum
          }else{
            return (this.listHeight.length - 1)
          }
        }
      }
      //如果this.listHeight没有的话，就返回0
      return 0
    }
  }
}
</script>
<style scoped>
.content{
  display: flex;
  position: absolute;
  top:100px;
  bottom:100px;
  width:100%;
  overflow: hidden;
  background: #eee;
}
.left{
  flex: 0 0 80px;
  width:80px;
  background-color: #f3f5f7;
}
  .left li{
    width: 100%;
    height: 100%;
  }
  .current{
    background-color: red;
  }
  .left-item{
    display: block;
    width:100%;
    height:100px;
    line-height: 50px;
    text-align: center;
    border-bottom:1px solid yellow;
  }
  .right{
    flex: 1;
  }
  .right-item li{
    width:100%;
    height:100px;
    line-height:100px;
    text-align: center;
    border-bottom: 1px solid yellow;
  }
  *{
    list-style: none;
    margin: 0;
    padding: 0;
  }
</style>
