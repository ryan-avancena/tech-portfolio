<script lang="ts">
  import { Muse } from '$lib';
  import { LAHD } from '$lib';
  import { FullyBeyond } from '$lib';
  import { AsciiVisualizer } from '$lib';
  import { AcmAI } from '$lib';

  let activeIndex = 0;

  const projects = [
    {
      title: 'MUSE',
      description: 'AI-powered music exploration',
      component: Muse
    },
    {
      title: 'ASCII VISUALIZER',
      description: "sounds in an ascii space",
      component: AsciiVisualizer
    },
    {
      title: 'LAHD',
      description: 'application developer',
      component: LAHD
    },
    {
      title: 'FULLYBEYOND 2025',
      description: "CSUF's first ever designathon",
      component: FullyBeyond
    },
    {
      title: 'ACM AI LEAD',
      description: "ai for csuf",
      component: AcmAI
    }

  ];
</script>

<div class="center-wrapper">
  <div class="projects-layout">
    <div class="cards-grid">
      {#each projects as project, i}
        <button
          type="button"
          class="card {i === activeIndex ? 'active' : ''}"
          on:pointerdown|stopPropagation
          on:click={() => (activeIndex = i)}
        >
          <h3>{project.title}</h3>
          <!-- <p>{project.description}</p> -->
        </button>
      {/each}
    </div>

    <div class="project-detail">
      <svelte:component this={projects[activeIndex].component} />
    </div>
  </div>
</div>
  
<style>
.center-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh; /* full viewport height */
  width: 100vw;  /* full viewport width */
}

.projects-layout {
  font-family: monospace;
  display: grid;
  grid-template-columns: minmax(120px, 120px) 1fr; /* cards left, content right */
  gap: 2rem;
  height: 90%;
  /* border: 2px solid red; */
  width: 65vw;
}

button {
    font-family: monospace;
}

/* LEFT SIDE GRID */
.cards-grid {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;

  position: relative;
  z-index: 2;
}



.project-detail {
  position: relative;
  z-index: 1;
}

/* CARD */
.card {
  appearance: none;
  border: none;
  background: #f5f5f5;
  cursor: pointer;
  color: #9e9e9e;
  display: block;
  /* width: 30%; */
  /* padding: 1.25rem; */

  transition: transform 0.25s ease, background 0.25s ease;
}


.card:hover {
  transform: translateY(-4px);
}

.card.active {
  background: #272525;
  color: white;
}

/* RIGHT SIDE CONTENT */
.project-detail {
  padding-left: 2rem;
  /* background: #fafafa; */
}

/* MOBILE */
@media (max-width: 900px) {
  .projects-layout {
    grid-template-columns: 1fr;
  }

  .cards-grid {
    grid-template-columns: 1fr;
  }
}


</style>
  