<template>
  <!-- <h1>滚轮下滑</h1> -->
  <div class="background">
      <span>HELLO</span>
  </div>
</template>

<script setups>  
export default {  
  mounted() {  
    const background = document.querySelector('.background');  
  
    document.addEventListener('scroll', () => {  
      const scrollY = window.scrollY;  
  
      if (scrollY !== 0) {  
        background.style.backgroundPosition = `calc(50% + ${scrollY}px) calc(50% + ${scrollY}px)`;  
      } else {  
        background.style.backgroundPosition = '50% 50%'; 
      }  
    });  
  },  
  beforeDestroy() {  
    // 清理事件监听器
    document.removeEventListener('scroll', this.scrollHandler);  
  }
};  
</script>

<style scoped>

body,html{
  margin: 0;
  padding: 0;
  height: 200vh;
  overflow-x: hidden;
}
.background{
  background-image: url('flower.jpg');
  background-size: cover;
  background-position: 50% 50%;
  height: 150vh;
  font: 900 25rem '';
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  position: relative;
  text-align: center;
  overflow: hidden;
}
.background::before{
  content: "";
  background-size: cover;
  background-image: inherit;
  background-position: 50% 50%;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  z-index: -99;
}
/* h1{
  position: absolute;
  text-align: center;
  width: 100%;
  letter-spacing: 10px;
  color: #ffffff;
} */
</style>
