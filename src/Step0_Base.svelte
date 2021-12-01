<script lang="ts">
</script>

<div class="animation-area">
  <div class="sticky-element">
    <div class="sequence-element">
      <canvas id="hero-lightpass" />
    </div>
  </div>
</div>

<style lang="scss">
  .animation-area {
    height: 9000px;
    position: relative;
    .sticky-element {
      height: 900px;
      position: sticky;
      top: 0;
      .sequence-element {
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        canvas {
          background-color: blue;
        }
      }
    }
  }
  .frame-index {
    position: fixed;
    top: 0;
    right: 0;
    padding: 10px;
    color: white;
  }
</style>
