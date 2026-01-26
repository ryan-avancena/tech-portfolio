<div class="main-container">
    <div class="bottom-text">
        <h1>RYAN AVANCENA</h1>
    </div>

    <div class="side-text">
        <h1>curiosity → systems → feeling</h1>

        <p>
            aspiring creative technologist seeking opportunities in art, tech, and sound design. 
        </p>
    </div>

    <div class="side-text-2">
        <p>
            my goal as a developer is to create innovative projects that feel personable to the user and myself. 
            
            i've always been interested in music production and art, and how systems can express themselves (despite being machines).
        </p>

        <!-- <p class="human-note">
            i care about how things feel as much as how they function.
        </p> -->

        <br>

        <p class="explore">→ explore selected work</p>
    </div>
</div>


<style>
    * {
        font-family: monospace;
    }

    .main-container {
        position: relative;      /* needed for absolute positioning inside */
        height: 80vh;
        padding: 1rem;
    }

    /* bottom text stays at bottom center */
    .bottom-text {
        position: absolute;
        bottom: 2%;         
        /* left: 50%; */
        /* transform: translateX(-50%); */
        font-size: clamp(2rem, 5vw, 5rem);
        max-width: 800px;
        /* text-align: center; */
    }

    /* side text on the right, vertically adjustable */
    .side-text {
        position: absolute;
        top: 20%;               /* distance from top */
        right: 30%;              /* distance from right */
        width: 250px;           /* or use % for responsiveness */
    }

    .side-text-2 {
        position: absolute;
        bottom: 10%;               /* distance from top */
        right: 10%;              /* distance from right */
        width: 400px;           /* or use % for responsiveness */
    }


    p {
        /* border: 2px solid red; */
        font-family: monospace;
    }

    .explore {
        font-size: 2rem;
        background-color: #272525;
        color: white;
    }

</style>