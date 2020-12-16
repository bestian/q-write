<template>
  <q-page>
    <div class="block" v-for="(item, idx) in items" :key = "idx" :style = "{top: item.y + 'px'}">
        <span>{{ item.w }}</span>
    </div>
    <q-input filled bottom-slots :autofocus="true" name="" v-model="hit" label="打字射擊" @keydown.enter = "fire(hit)"/>
    <q-btn @click = "fire(hit)">射擊！</q-btn>
    <div class="score">目前 {{ score }} 分</div>
    <div class="score">目前 {{ hearts }} 心</div>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      items: [],
      words: ['大', '中', '小', '天', '早', '心', '太'],
      t: 0,
      score: 0,
      hearts: 5
    }
  },
  methods: {
    die () {
      console.log('die')
      this.score = 0
      this.hearts = 5
      this.items = []
    },
    fire (hit) {
      this.score += this.items.filter((o) => { return o.w === hit }).length
      this.items = this.items.filter((o) => { return o.w !== hit })
      this.hit = ''
      this.$forceUpdate()
    },
    addItem () {
      const w = this.words[Math.floor(Math.random() * this.words.length)]
      console.log(w)
      this.items.push({ w: w, y: 0, hide: false })
    },
    go () {
      this.t++
      if (this.t % 100 === 0) {
        this.addItem()
      }
      this.items = this.items.map((o) => {
        o.y++
        if (o.y === 400) {
          this.hearts--
        }
        return o
      }).filter((o) => { return o.y < 400 })
      if (this.hearts === 0) {
        this.die()
      }
      this.$forceUpdate()
    }
  },
  mounted () {
    this.addItem()
    setInterval(this.go, 50)
  }
}
</script>

<style type="text/css" scoped="">
  .block {
    position: absolute;
    left: 40vw;
    font-size: 36px;
    border: 3px ridge black;
    width: 54px;
    height: 54px;
    text-align: center;
  }
  .score {
    background-color: #9f9;
    padding: 1em 1em;
    width: 200px;
  }
</style>
