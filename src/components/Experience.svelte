<script lang="ts">
    const projects = [
      {
        title: "MUSE",
        description: "embeddings + music",
        content: "This is the detailed content for Project One."
      },
      {
        title: "LAHD",
        description: "application developer",
        content: "This is the detailed content for Project Two."
      },
      {
        title: "MORE PROJECTS",
        description: "school + out of school",
        content: "This is the detailed content for Project Three."
      },
      {
        title: "ACM AI LEAD",
        description: "my contribution to CSUF's largest tech club",
        content: "This is the detailed content for Project Four."
      },
      {
        title: "FULLYBEYOND",
        description: "CSUF's first designathon",
        content: "This is the detailed content for Project Five."
      },
      {
        title: "FULLYHACKS",
        description: "CSUF's annual 24-hour hackathon",
        content: "This is the detailed content for Project Six."
      }
    ];
  
    let activeIndex = 0;
</script>
  
  
<div class="projects-layout">
    <!-- LEFT: cards -->
    <div class="cards-grid">
      {#each projects as project, i}
        <button
            type="button"
            class="card {i === activeIndex ? 'active' : ''}"
            on:pointerdown|stopPropagation
            on:click={() => (activeIndex = i)}
            >
    
            <h3>{project.title}</h3>
            <p>{project.description}</p>
        </button>
    
      {/each}
    </div>
  
    <!-- RIGHT: content -->
    <div class="project-detail">
      <h2>{projects[activeIndex].title}</h2>
      <p>{projects[activeIndex].content}</p>
    </div>
  </div>
  
  
<style>
  
  
.projects-layout {
    font-family: monospace;
    display: grid;
    grid-template-columns: minmax(300px, 300px) 1fr; /* cards left, content right */
    gap: 2rem;
    height: 90%;
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
  padding: 2rem;
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
  