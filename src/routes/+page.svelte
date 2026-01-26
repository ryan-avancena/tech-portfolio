<script>
    import Carousel from "../components/Carousel.svelte";
    import Screen from "../components/Screen.svelte";
    import Experience from "../components/Experience.svelte";
    import Landing from "../components/Landing.svelte";
    import About from "../components/About.svelte";
  </script>
  
  <main>
    <Carousel>
      <Screen>
        <Landing/>
      </Screen>
  
      <Screen>
        <Experience/>
      </Screen>
  
      <Screen>
        <About/>
      </Screen>
    </Carousel>
  </main>
  
  <style>
    main {
      font-family: Arial, Helvetica, sans-serif;
      background-color: #FFFEF8;
    }

    
  </style>
  