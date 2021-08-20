<script>
  export let scale = 0.8;
  export let posX = 100;
  export let posY = 100;

  const minScale = 0.1;
  const scaleStep = 0.001;

  let willChange = false;
  let timer;

  const wheel = (e) => {
    willChange = true;
    clearTimeout(timer);

    timer = setTimeout(() => {
      willChange = false;
    }, 250);

    if (e.ctrlKey) {
      let rect = e.currentTarget.getBoundingClientRect();

      let mouseX = e.clientX - rect.left - posX;
      let mouseY = e.clientY - rect.top - posY;

      let delta = e.deltaY * scaleStep;
      let newscale = scale - delta;
      if (newscale <= minScale) {
        newscale = minScale;
      } else {
        posX += (mouseX / scale) * delta;
        posY += (mouseY / scale) * delta;
        scale = newscale;
      }
    } else {
      posX -= e.deltaX * 2;
      posY -= e.deltaY * 2;
    }
  };

  let startX = 0;
  let startY = 0;
  let wrapper;

  const pointerDown = (e) => {
    if (e.buttons == 4) {
      let rect = e.currentTarget.getBoundingClientRect();
      startX = e.clientX - rect.left - posX;
      startY = e.clientY - rect.top - posY;
    }
    wrapper.setPointerCapture(e.pointerId);
  };

  const pointerMove = (e) => {
    if (!wrapper.hasPointerCapture(e.pointerId)) return;
    willChange = true;
    clearTimeout(timer);

    timer = setTimeout(() => {
      willChange = false;
    }, 250);

    let rect = e.currentTarget.getBoundingClientRect();

    let mouseX = e.clientX - rect.left - posX;
    let mouseY = e.clientY - rect.top - posY;

    if (e.buttons == 4) {
      posX += mouseX - startX;
      posY += mouseY - startY;
    }
  };

  const pointerUp = (e) => {
    wrapper.releasePointerCapture(e.pointerId);
  };

  // [ ] middle button
  // [ ] broken then panded with anchor link
</script>

<div
  bind:this={wrapper}
  class="wrapper"
  on:wheel|preventDefault={wheel}
  on:pointermove|preventDefault={pointerMove}
  on:pointerdown={pointerDown}
  on:pointerup={pointerUp}
>
  <div
    class="client"
    style="{willChange
      ? 'will-change: transform;'
      : ''} transform: translate({posX}px,
    {posY}px) scale({scale});"
  >
    <slot />
  </div>
</div>

<style>
  .wrapper {
    background: #f9f9f9;
    overflow: hidden;
  }
  .client {
    transform-origin: 0 0;
  }
</style>
