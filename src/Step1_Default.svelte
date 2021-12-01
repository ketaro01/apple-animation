<script lang="ts">
  import { onDestroy, onMount } from 'svelte';

  let canvas: HTMLCanvasElement = null;
  let context: CanvasRenderingContext2D = null;
  let img: HTMLImageElement = null;
  let frame = 0;
  const frameCount = 147;
  const currentFrame = index => (
    `https://www.apple.com/105/media/us/airpods-pro/2019/1299e2f5_9206_4470_b28e_08307a42f19b/anim/sequence/large/01-hero-lightpass/${index.toString().padStart(4, '0')}.jpg`
  );

  const updateImage = index => {
    frame = index;
    img = new Image();
    img.src = currentFrame(index);
    context.drawImage(img, 0, 0);
  };

  const animationFrame = () => {
    const html = document.documentElement;
    const scrollTop = html.scrollTop;
    const maxScrollTop = html.scrollHeight - window.innerHeight;
    const scrollFraction = scrollTop / maxScrollTop;
    const frameIndex = Math.min(frameCount - 1, Math.ceil(scrollFraction * frameCount));
    requestAnimationFrame(() => {
      updateImage(frameIndex + 1);
    });
  };

  onMount(async () => {
    canvas = document.getElementById('hero-lightpass') as HTMLCanvasElement;
    context = canvas.getContext('2d');

    canvas.width = 1158;
    canvas.height = 770;


    animationFrame();

    window.addEventListener('scroll', animationFrame);
  });
  onDestroy(() => {
    window.removeEventListener('scroll', animationFrame);
  });
</script>

<div class="animation-area">
  <div class="sticky-element">
    <div class="sequence-element">
      <canvas id="hero-lightpass" />
    </div>
  </div>
</div>
<span class="frame-index">{frame}</span>

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
