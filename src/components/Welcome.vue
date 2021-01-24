<template>
  <div class="welcome">
    <h3>Welcome</h3>
    <div class="box">
      <div v-for="(e, i) in users" :key="i + e.info" class="card">
        <img src="https://i2.hdslb.com/bfs/face/f7c909737e967f8b9600a500d92cde7e4f9f325f.jpg@150w_150h.jpg" />
        <div class="info">
          <span class="name">{{ e.name }}</span>
          <span class="decs">{{ e.info }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' },
        { name: '刘德华', info: '15151515151' }
      ]
    }
  },

  methods: {
    // console.log(this);
  }
}
</script>

<style lang="less" scoped>
h3 {
  text-align: left;
}
h3 {
  line-height: clamp(1rem, 2.5vw, 3rem);
  font-size: clamp(1rem, 2.5vw, 3rem);
}
.welcome {
  .box {
    display: grid;
    gap: 10px;
    grid-template-columns: repeat(auto-fill, 170px);
    grid-template-rows: repeat(auto-fill, 60px);
  }
}
.card {
  display: flex;
  padding: 10px;
  box-sizing: border-box;

  &:hover {
    background: linear-gradient(45deg, transparent 85%, #ff013c 85%);
  }

  img {
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }

  .info {
    margin-left: 10px;
    text-align: left;
    span {
      display: block;
    }
  }
}
</style>
