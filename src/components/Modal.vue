<template>
  <transition name="modal">
    <div class="modal">
      <div
        ref="modal_wrapper"
        class="modal-wrapper"
        @mousedown="close"
        @keydown.esc="close"
        tabindex="0"
      >
        <div class="modal-container" :style="boxStyle" :class="custom_width" @mousedown.stop>
          <div class="modal-close">
            <slot name="close" :close="close">
              <div @click="close">&times;</div>
            </slot>
          </div>
          <div class="modal-body">
            <slot name="body"></slot>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
export default {
  name: 'Modal',
  props:{
    custom_width:{
      default:null
    },
    width:{
      type:String,
      default:'400'
    },
    maxWidth:{
      type:String,
      default:null
    }
  },
  data() {
    return {}
  },
  methods: {
    close() {
      this.$emit('close')
    }
  },
  computed:{
    boxStyle(){
      return {
        width:this.width,
        'max-width':this.maxWidth
      }
    }
  },
  mounted() {
    this.$refs.modal_wrapper.focus()
  }
}
</script>

<style lang="scss">
.modal {
  position: fixed;
  left: 0px;
  top: 0px;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.7);
  display: table;
  transition: opacity 0.3s ease;
  overflow-y: auto;
  &-wrapper {
    display: table-cell;
    vertical-align: middle;
  }
  &-container {
    width: 700px;
    max-height: calc(100vh - 5rem);
    overflow-y: auto;
    margin: 0 auto;
    padding: 15px;
    background-color: #fff;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
    transition: all 0.3s ease;
    position: relative;
    
  }
  &-close {
    z-index: 999999;
    position: sticky;
    top: -10px;
    float: right;
    cursor: pointer;
    right: -10px;
    width: 25px;
    height: 25px;
    line-height: 22px;
    border-radius: 50%;
    text-align: center;
    background: #fff;
    border: 1px solid lighten(#aaacac, 15%);
    color: #aaacac;
  }
  &-body {
    padding: 0;        
  }
}
/* auto-applied to elements with transition="modal" by Vue.js. */
.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>