<template>
  <!-- 

  author: 靓仔锋

  Attributes: 
    - width : 宽度                        默认: 80
    - height: 高度                        默认: 80
    - border-radius: 圆角度数             默认: 6
    - right: 距离浏览器右边距离            默认: 40
    - bottom: 距离浏览器底部距离           默认: 50
    - background-color: 背景颜色          默认: #fff
    - border: 是否加边框                  默认: false
    - border-color: 边框颜色              默认: #DDD
    - icon: 字体图标                      默认: 无
    - icon-color: 字体图标颜色            默认: #ddd
    - text: 文本                         默认: Top
    - text-color: 文本颜色               默认: #A1A1A1

  Events: 
    - click   点击方法
  
  slot: 
    - icon  字体图标插槽          
    - text  文本插槽

  提示:
    如若要修改文字和字体图标样式以上属性没有可以使用插槽

  
  -->
  <div
    v-show="isshow"
    class="backtop"
    :style="{
    'width': ($attrs.width || 80) + 'px',
    'height':  ($attrs.height || 80) + 'px',
    'border-radius': ($attrs['border-radius'] || 6 )+'px' ,
    'right': ($attrs.right || 40 ) + 'px',
    'bottom': ($attrs.bottom || 50 ) + 'px',
    'background-color': $attrs['background-color'] || '#fff',
    'border-width': border ? '1px' : '0',
    'border-color': $attrs['border-color'] || '#DDDDDD'
     }"
    @click="goToTop"
  >
    <slot name="icon">
      <span
        class="arrows"
        :class="$attrs.icon"
        :style="{
          color: $attrs['icon-color'] || '#ddd'
        }"
      ></span>
    </slot>
    <slot class="flex-center-center" name="text">
      <p
        class="backtop__text"
        :style="{
        color: $attrs['text-color'] || '#A1A1A1'
      }"
      >{{text}}</p>
    </slot>
  </div>
</template>
<script>
export default {
  inheritAttrs: false,
  props: {
    border: {
      type: Boolean,
      default: false
    },
    text: {
      type: String,
      default: "Top"
    }
  },
  data() {
    return {
      isshow: false,
      clearTimeId: null
    };
  },
  mounted() {
    window.addEventListener("scroll", this.handleScroll, true);
  },
  methods: {
    goToTop() {
      this.$emit("click");
      clearInterval(this.clearTimeId);
      this.clearTimeId = setInterval(() => {
        let scrollTop =
          document.documentElement.scrollTop || document.body.scrollTop;
        let step = scrollTop - Math.ceil(scrollTop / 10);
        if (step <= 0) {
          clearInterval(this.clearTimeId);
          return
        }
        if (document.documentElement.scrollTop) {
          document.documentElement.scrollTop = step;
        } else {
          document.body.scrollTop = step;
        }
      }, 10);
    },

    handleScroll(e) {
      var scrollTop =
        document.documentElement.scrollTop || document.body.scrollTop;
      if (scrollTop > 200 && !this.isshow) {
        this.isshow = true;
      } else if (scrollTop <= 200 && this.isshow) {
        this.isshow = false;
      }
    }
  },
  watch: {
    "$route.path"(val) {
      if (!this.clearTimeId) {
        clearInterval(this.clearTimeId);
      }
    }
  }
};
</script>
<style lang="scss" scoped>
.backtop {
  cursor: pointer;
  position: fixed;
  flex-direction: column;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 200;
  border-style: solid;
  box-shadow: 0px 0px 10px rgba(0, 0, 0, 0.16);
  .arrows {
    margin-bottom: 10px;
  }
}
</style>