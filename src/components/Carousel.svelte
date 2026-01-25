<script lang="ts">
    let container: HTMLDivElement;
    let index = 0;
    const total = 3;
    
    let startX = 0;
    let currentX = 0;
    let isDragging = false;
    
    function goTo(i: number) {
      if (i < 0) index = total - 1;
      else if (i >= total) index = 0;
      else index = i;
    }
    
    // ----- Unified for pointer + touch -----
    function handleDown(clientX: number) {
      isDragging = true;
      startX = clientX;
      container.style.transition = "none";
    }
    
    function handleMove(clientX: number) {
      if (!isDragging) return;
      currentX = clientX - startX;
      container.style.transform = `translateX(calc(${-index * 100}vw + ${currentX}px))`;
    }
    
    function handleUp() {
      if (!isDragging) return;
      isDragging = false;
      container.style.transition = "transform 0.5s ease";
    
      if (Math.abs(currentX) > 80) {
        goTo(currentX < 0 ? index + 1 : index - 1);
      }
      currentX = 0;
    }
    
    // ----- Pointer events -----
    function onPointerDown(e: PointerEvent) {
        if ((e.target as HTMLElement).closest('button')) return;

        isDragging = true;
        startX = e.clientX;
    }

    function onPointerMove(e: PointerEvent) { handleMove(e.clientX); }
    function onPointerUp(e: PointerEvent) { handleUp(); container.releasePointerCapture(e.pointerId); }
    
    // ----- Touch events for mobile -----
    function onTouchStart(e: TouchEvent) { handleDown(e.touches[0].clientX); }
    function onTouchMove(e: TouchEvent) { handleMove(e.touches[0].clientX); }
    function onTouchEnd() { handleUp(); }
    </script>
    
    <div class="carousel-wrapper">
      <div
        class="carousel"
        bind:this={container}
        style="transform: translateX(-{index * 100}vw)"
        on:pointerdown={onPointerDown}
        on:pointermove={onPointerMove}
        on:pointerup={onPointerUp}
        on:pointerleave={onPointerUp}
        on:touchstart={onTouchStart}
        on:touchmove={onTouchMove}
        on:touchend={onTouchEnd}
      >
        <slot />
      </div>
    
      <!-- arrows -->
      <button class="arrow left" on:click={() => goTo(index - 1)}>‹</button>
      <button class="arrow right" on:click={() => goTo(index + 1)}>›</button>
    
      <!-- indicators -->
      <div class="indicators">
        {#each Array(total) as _, i}
          <button
            type="button"
            class="indicator {i === index ? 'active' : ''}"
            on:click={() => goTo(i)}
            aria-label={`Go to slide ${i + 1}`}
          ></button>   
        {/each}
      </div>
    </div>
    
  
  <style>
    .carousel-wrapper {
      position: relative;
      overflow: hidden;
      width: 100vw;
      height: 100vh;
    }
    
  
    .carousel {
        display: flex;
        height: 100%;
        transition: transform 0.5s ease;
        touch-action: pan-y; /* allows vertical scrolling but lets horizontal swipes pass */
        -webkit-user-drag: none; /* prevent image drag if you have images */
        user-select: none; /* prevent text selection on swipe */
    }

    .arrow {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: none;
      border: none;
      font-size: 2rem;
      cursor: pointer;
      opacity: 0.4;
      z-index: 10;
    }
  
    .arrow:hover {
      opacity: 1;
    }
  
    .left { left: 1rem; }
    .right { right: 1rem; }

    .indicators {
        position: absolute;
        bottom: 1.75rem;
        left: 50%;
        transform: translateX(-50%);
        display: flex;
        gap: 0.15rem;
        }

        .indicator {
            all: unset;
            width: 40px;
            height: 6px;
            /* border-radius: 4px; */
            background: #dadada;
            cursor: pointer;
            transition: background 0.3s ease, transform 0.3s ease;
        }


        .indicator.active {
        background: #272525;
        }

        .indicator:hover {
        transform: scaleX(1.1);
        }

  </style>
  