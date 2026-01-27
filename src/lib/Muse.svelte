<script lang="ts">
  import { onMount } from "svelte";
  import '$lib/info.css';

  let Plotly: any;

  onMount(async () => {
    const Plotly = (await import("plotly.js-dist-min")).default;

    const res = await fetch("/data/visualization_embeddings.json");
    const data = await res.json();

    const x = data.map((d: any) => d.x);
    const y = data.map((d: any) => d.y);
    const z = data.map((d: any) => d.z ?? 0);
    const text = data.map((d: any) => `${d.Title} by ${d.Artist}`);
    const ids = data.map((d: any) => d.ID);

    const trace = {
      x: Array(x.length).fill(0),
      y: Array(y.length).fill(0),
      z: Array(z.length).fill(0),
      text,
      customdata: ids,
      mode: "markers",
      type: "scatter3d",
      marker: {
        size: 5,
        color: "#272525", // main portfolio color for points
        opacity: 0.9
      },
      hovertemplate: "<span style='font-family:monospace;'><b>%{text}</b></span><extra></extra>"
    };

    const layout = {
      margin: { l: 0, r: 0, b: 0, t: 0 },
      hovermode: "closest",
      font: {
        family: "monospace",
        color: "#272525",
        size: 12
      },
      scene: {
        xaxis: {
          showgrid: false,
          showticklabels: false,
          ticks: "",
          zeroline: false,
          showbackground: true,
          backgroundcolor: "#D9D9D9"
        },
        yaxis: {
          showgrid: false,
          showticklabels: false,
          ticks: "",
          zeroline: false,
          showbackground: true,
          backgroundcolor: "#A3A3A3"
        },
        zaxis: {
          showgrid: false,
          showticklabels: false,
          ticks: "",
          zeroline: false,
          showbackground: true,
          backgroundcolor: "#727272"
        },
        aspectmode: 'manual', // manually control aspect
        aspectratio: { x: 1, y: 1, z: 0.6 }, // flatten z-axis to reduce empty space
        camera: { eye: { x: 1.2, y: 1.2, z: 0.6 } } // adjust initial rotation
      }
    };


    Plotly.newPlot("visualizer", [trace], layout).then(() => {
      // Animate points on load
      Plotly.animate(
        "visualizer",
        { data: [{ x, y, z }], traces: [0] },
        { transition: { duration: 1000, easing: "cubic-in-out" }, frame: { duration: 1000 } }
      );

      // Handle plot click → show annotation
      const plotElement = document.getElementById("visualizer");
      if (!plotElement) return;

      plotElement.addEventListener("plotly_click", (event: any) => {
        const point = event.points[0];
        const idx = point.pointNumber;

        const annotation = {
          x: x[idx],
          y: y[idx],
          z: z[idx],
          text: `<b>${text[idx]}</b>`,
          showarrow: true,
          arrowcolor: "#272525",
          font: { color: "#272525", size: 14, family: "monospace" },
          ax: 0,
          ay: -30
        };

        Plotly.relayout("visualizer", { "scene.annotations": [annotation] });
      });
    });
  });
</script>

<div class="heading">
  <h1>MUSE</h1><h3>dec 2024 - may 2025</h3>
</div>

<hr>

<h3><i>music exploration with natural language processing</i></h3>
<div id="visualizer" style="width: 100%; height: 500px;"></div>
<!-- <p style="text-align: center;"><i>example visualizer - hover over the points</i></p> -->

<p>
  MUSE is a web application designed for DJs who want to curate live sets faster and more intuitively.
  The goal of the project is to let SoundCloud-based DJs explore their tracks inside a dynamic 3D vector space —
  making it easier to discover transitions, identify energy shifts, and build cohesive playlists.
</p>



<div class="text-content">
  <h2>INSPIRATION</h2>
  <p>
    One of my favorite courses I took in school was Natural Language Processing — specifically how computers
    learn to understand and represent text. A core concept in NLP is embeddings — where words or sentences
    are mapped into a vector space that captures relationships and meaning. We can use this concept with lyrics,
    genres, or the key of songs!
  </p>

  <h2>HOW DOES IT WORK?</h2>
  <p>
    Each track is retrieved from a user's SoundCloud and converted into a numerical embedding.
    These embeddings are then visualized inside an interactive 3D environment. You can rotate the space, filter
    tracks, and click on points to preview songs.  
  </p>
  <p>
    The application uses Python (Flask) for machine learning, data processing, and front-end rendering,
    and Node.js for Tunebat API requests to retrieve track metadata, including tags, BPM, key, and more.
  </p>
</div>

<style>
  #visualizer {
    border-radius: 12px;
    box-shadow: #272525;
    margin: 2rem 0;
    background-color: #F5F5F5; /* matches paper_bgcolor */
  }
</style>
