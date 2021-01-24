<template>
  <div class="welcome">
    <h3>Welcome</h3>
    <div :class="['box', { loading }]">
      <div v-for="(e, i) in users" :key="i" :class="['card']">
        <img :src="src" />
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
      loading: true,
      users: [{}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}], // 默认必须有空元素
      USERS: [
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
      ],
      src: ' ', // 没空格会显示挂掉的图片
      SRC: 'https://i2.hdslb.com/bfs/face/f7c909737e967f8b9600a500d92cde7e4f9f325f.jpg@150w_150h.jpg'
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    init() {
      setTimeout(() => {
        this.loading = false
        this.users = this.USERS
        this.src = this.SRC
      }, 500)
    }
  }
}
</script>

<style lang="less">
:root {
  --loading-grey: #ededed;
}
.welcome {
  h3 {
    text-align: left;
    line-height: clamp(1rem, 2.5vw, 3rem);
    font-size: clamp(1rem, 2.5vw, 3rem);
  }
  .box {
    margin-top: 10px;
    display: grid;
    gap: 10px;
    grid-template-columns: repeat(auto-fill, 180px);
    grid-template-rows: repeat(auto-fill, 76px);
  }
  .card {
    display: flex;
    align-items: center;
    padding: 10px;
    box-sizing: border-box;
    background-color: #fff;

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
      .name {
        width: 50px;
        height: 20px;
        margin-bottom: 10px;
      }
      .decs {
        width: 110px;
        height: 22px;
      }
    }
  }
  // 核心代码
  // 1. 默认必须有空元素
  // 2. .loading加到每个对应的子元素理也可，但是会增加html中的工作量
  .loading img,
  .loading .info .name,
  .loading .info .decs {
    background-color: var(--loading-grey);
    background: linear-gradient(100deg, rgba(255, 255, 255, 0) 40%, rgba(255, 255, 255, 0.5) 50%, rgba(255, 255, 255, 0) 60%) var(--loading-grey);
    background-size: 200% 100%;
    background-position-x: 180%;
    animation: 1s loading ease-in-out infinite;
  }

  @keyframes loading {
    to {
      background-position-x: -20%;
    }
  }
}
</style>
