<script lang="ts">
  import { onDestroy, onMount } from 'svelte';

  let canvas: HTMLCanvasElement = null;
  let context: CanvasRenderingContext2D = null;
  let frame = 0;
  const frameCount = 147;
  const currentFrame = index => (
    `https://www.apple.com/105/media/us/airpods-pro/2019/1299e2f5_9206_4470_b28e_08307a42f19b/anim/sequence/large/01-hero-lightpass/${index.toString().padStart(4, '0')}.jpg`
  );
  let images = [];

  const preloadImages = (frameCount) => {
    return Promise.all(Array.from(Array(frameCount)).map((_, index) => {
      return new Promise((resolve) => {
        const image = new Image();
        image.src = currentFrame(index + 1);
        images[index] = image;
        images[index].onload = () => resolve();
      });
    }))
  };

  const updateImage = index => {
    frame = index + 1;
    context.drawImage(images[index], 0, 0);
  };

  const animationFrame = () => {
    const html = document.documentElement;
    const scrollTop = html.scrollTop;
    const maxScrollTop = html.scrollHeight - window.innerHeight;
    const scrollFraction = scrollTop / maxScrollTop;
    const frameIndex = Math.min(frameCount - 1, Math.ceil(scrollFraction * frameCount));
    requestAnimationFrame(() => {
      updateImage(frameIndex);
    });
  };

  onMount(async () => {
    await preloadImages(frameCount);

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

<div class="top-area">
  <h1>TOP AREA 1</h1>
</div>
<div class="top-area2">
  <h1>TOP AREA 2</h1>
</div>
<div class="animation-area">
  <div class="sticky-element">
    <div class="sequence-element">
      <canvas id="hero-lightpass" />
    </div>
  </div>
</div>
<div class="bottom-area">
  <h1>BOTTOM AREA 1</h1>
</div>
<div class="bottom-area2">
  <h1>BOTTOM AREA 2</h1>
</div>
<span class="frame-index">{frame}</span>

<style lang="scss">
  .top-area, .bottom-area {
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .top-area {
    height: 200vh;
  }
  .top-area2 {
    height: 60vh;
  }
  .bottom-area {
    height: 120vh;
  }
  .bottom-area2 {
    height: 40vh;
  }
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
