<div class="main-container">
    <div class="bottom-text">
        <h1>RYAN AVANCENA</h1>
    </div>

    <div class="side-text">
        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris a elit tincidunt, rutrum velit et, eleifend augue. 
            Nam pulvinar a felis vel vestibulum. Sed aliquet magna a nisi lobortis tincidunt. Mauris in purus urna. 
            Sed erat ante, ornare in mauris ut, feugiat finibus nulla. Aliquam non lobortis mauris, ac porta enim.
        </p>
    </div>

    <div class="side-text-2">
        <p>
            Proin ut erat ultrices, consequat massa id, luctus libero. Pellentesque faucibus orci in consectetur vehicula. 
            Aliquam cursus vel ligula vel mattis. Nulla commodo turpis porta, mollis turpis quis, maximus tellus. 
            Suspendisse nunc ante, gravida ut odio quis, tincidunt suscipit ligula. 
            Suspendisse ut facilisis est, id faucibus lectus.
        </p>
    </div>
</div>


<style>
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
        bottom: 25%;               /* distance from top */
        right: 10%;              /* distance from right */
        width: 400px;           /* or use % for responsiveness */
    }


    p {
        /* border: 2px solid red; */
        font-family: monospace;
    }

</style>