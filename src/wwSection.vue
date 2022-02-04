<template>
  <div 
    class="modal-container" 
    :style="backdropStyle"
  >
    <transition :name="content.animation" mode="out-in">
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
import { presets } from './settings.js'

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
        return presets[this.content.position]
      }

      return {
          '--top': this.content.positionTop,
          '--left': this.content.positionLeft,
        }
    },
  }
};
</script>

<style lang="scss" scoped>
:root {
    --top: auto;
    --right: auto;
    --bottom: auto;
    --left: auto;
    --translateY: 0%;
    --translateX: 0%;
}

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
  top: var(--top);
  right: var(--right);
  bottom: var(--bottom);
  left: var(--left);
  transform: translate(var(--translateX), var(--translateY));
}

.fade-enter-active,
.fade-leave-active,
.fromTop-enter-active,
.fromTop-leave-active,
.fromRight-enter-active,
.fromRight-leave-active,
.fromBottom-enter-active,
.fromBottom-leave-active,
.fromLeft-enter-active,
.fromLeft-leave-active,
.zoomIn-enter-active,
.zoomIn-leave-active,
.zoomOut-enter-active,
.zoomOut-leave-active {
  transition: all 300ms ease;
}

.fade-enter-from, .fade-leave-to {
  opacity: 0;
}

.fromTop-enter-from {
    opacity: 0;
    transform: translate(var(--translateX), -100%);
}
.fromTop-leave-to {
    opacity: 0;
    transform: translate(var(--translateX), -100%);
}

.fromRight-enter-from {
    opacity: 0;
    transform: translate(100%, var(--translateY));
}
.fromRight-leave-to {
    opacity: 0;
    transform: translate(100%, var(--translateY));
}

.fromBottom-enter-from {
    opacity: 0;
    transform: translate(var(--translateX), 100%);
}
.fromBottom-leave-to {
    opacity: 0;
    transform: translate(var(--translateX), 100%);
}

.fromLeft-enter-from {
    opacity: 0;
    transform: translate(-100%, var(--translateY));
}
.fromLeft-leave-to {
    opacity: 0;
    transform: translate(-100%, var(--translateY));
}

.zoomIn-enter-from {
    opacity: 0;
    transform: translate(var(--translateX), var(--translateY)) scale(1.5);
}
.zoomIn-leave-to {
    opacity: 0;
    transform: translate(var(--translateX), var(--translateY)) scale(1.5);
}

.zoomOut-enter-from {
    opacity: 0;
    transform: translate(var(--translateX), var(--translateY)) scale(0.5);
}
.zoomOut-leave-to {
    opacity: 0;
    transform: translate(var(--translateX), var(--translateY)) scale(0.5);
}



</style>
