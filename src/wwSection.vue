<template>
  <div 
    class="modal-container" 
    :style="backdropStyle"
  >
    <transition name="fade" mode="out-in">
      <wwLayout 
        v-if="show" 
        class="modal-dropzone" 
        direction="column" 
        path="modalContent" 
        :class="content.position" 
        :style="modalContentStyle"
      ></wwLayout>
    </transition>
  </div>
</template>

<script>
export default {
  props: {
    content: { type: Object, required: true },
    /* wwEditor:start */
    wwEditorState : { type: Object, required: true }
    /* wwEditor:end */
  },
  data() {
    return {
      wwDefaultContent: {
        modalContent: []
      }
    }
  },
  computed: {
    show () {
      /* wwEditor:start */
      return this.content.display || this.wwEditorState.sidepanelContent.displayInEditor;
      /* wwEditor:end */
      return this.content.display;
    },
    backdropStyle() {
      return {
        backgroundColor: (this.show && this.content.backdrop) ? this.content.backdropColor : 'transparent',
        pointerEvents: (this.show && this.content.backdrop) ? 'auto' : 'none'
      };
    },
    modalContentStyle() {
      if (this.content.position !== 'custom') {
        return {}
      }
      
      return {
          top: this.content.positionTop,
          left: this.content.positionLeft,
        }
    }
  }
};
</script>

<style lang="scss" scoped>
.modal-container {
  display: flex;
  isolation: isolate;
  position: fixed;
  top: 0;
  height: 100vh;
  z-index: 30;
  transition: background-color 0.5s;
}

.modal-dropzone {
  position: absolute;
  min-height: 100px;
  min-width: 100px;
  height: inset;
  width: inset;
}

// .my-section {
//   display: flex;
//   flex-direction: column;
//   align-items: center;
//   min-height: 200px;
//   h1 {
//     font-size: 32px;
//   }
//   p {
//     margin-top: 12px;
//   }
// }

.topLeft {
  top: 0;
  left: 0;
}
.topMiddle {
  top: 0;
  left: 50%;
  transform: translate(-50%);
}
.topRight {
  top: 0;
  right: 0;
}
.middleLeft {
  top: 50%;
  left: 0;
  transform: translateY(-50%);
}
.middle {
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
.middleRight {
  top: 50%;
  right: 0;
  transform: translateY(-50%);
}
.bottomLeft {
  bottom: 0;
  left: 0;
}
.bottomMiddle {
  bottom: 0;
  left: 50%;
  transform: translate(-50%);
}
.bottomRight {
  bottom: 0;
  right: 0;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}


</style>
