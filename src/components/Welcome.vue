<template>
  <div>
    <h3>Welcome</h3>
    <div class="card">
      <img src="https://i2.hdslb.com/bfs/face/f7c909737e967f8b9600a500d92cde7e4f9f325f.jpg@150w_150h.jpg" />
      <div class="info">
        <span class="name">刘德华</span>
        <span class="decs">1555511111</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {}
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
.card {
  display: flex;
  width: 220px;
  transition: display 0.2s;

  &:hover {
    background: linear-gradient(45deg, transparent 95%, #ff013c 95%);
    transition: display 0.2s;
  }

  img {
    width: 40px;
    height: 40px;
    border-radius: 50%;
  }

  .info {
    span {
      display: block;
    }
  }
}
</style>
