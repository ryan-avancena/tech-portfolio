<script lang="ts">
    import { onMount } from "svelte";

    let asciiEl: HTMLPreElement;
    let audioEl: HTMLAudioElement;

    const ASCII_RAMP = " .:-=+*#%@";
    const HEIGHT = 80;
    let WIDTH = 0;

    let audioCtx: AudioContext;
    let analyser: AnalyserNode;
    let dataArray: Uint8Array;
    let source: MediaElementAudioSourceNode;

    let gainNode: GainNode;
    let distortion: WaveShaperNode;
    let convolver: ConvolverNode;
    let wetGain: GainNode;
    let dryGain: GainNode;

    let smoothBars: number[] = [];
    let smoothY: number[] = [];
    let playing = false;

    // slider state
    let gain = 1;
    let reverb = 0;
    let distortionAmount = 0;

    const CHARS = "•";

    function rand(min = 0, max = 1) {
        return Math.random() * (max - min) + min;
    }


    function updateWidth() {
        const charWidth = 6;
        WIDTH = Math.floor(window.innerWidth / charWidth);
    }

    function createImpulse(ctx: AudioContext, seconds = 2) {
        const rate = ctx.sampleRate;
        const length = rate * seconds;
        const impulse = ctx.createBuffer(2, length, rate);

        for (let c = 0; c < 2; c++) {
            const channel = impulse.getChannelData(c);
            for (let i = 0; i < length; i++) {
            channel[i] =
                (Math.random() * 2 - 1) * Math.pow(1 - i / length, 2);
            }
        }

        return impulse;
    }

    function makeDistortionCurve(amount = 50) {
        const n = 44100;
        const curve = new Float32Array(n);
        for (let i = 0; i < n; i++) {
            const x = (i * 2) / n - 1;
            curve[i] = Math.tanh(amount * x);
        }
        return curve;
    }

    function setupAudio() {
        audioCtx = new AudioContext();

        analyser = audioCtx.createAnalyser();
        analyser.fftSize = 2048;
        analyser.smoothingTimeConstant = 0.85;

        dataArray = new Uint8Array(
            new ArrayBuffer(analyser.fftSize)
        );

        gainNode = audioCtx.createGain();
        distortion = audioCtx.createWaveShaper();
        convolver = audioCtx.createConvolver();
        wetGain = audioCtx.createGain();
        dryGain = audioCtx.createGain();

        gainNode.gain.value = gain;
        wetGain.gain.value = reverb;
        dryGain.gain.value = 1 - reverb;

        distortion.curve = null;
        distortion.oversample = "4x";

        convolver.buffer = createImpulse(audioCtx);

        source = audioCtx.createMediaElementSource(audioEl);

        // wiring
        source.connect(gainNode);
        gainNode.connect(distortion);

        distortion.connect(analyser);

        distortion.connect(dryGain);
        distortion.connect(convolver);

        convolver.connect(wetGain);

        dryGain.connect(audioCtx.destination);
        wetGain.connect(audioCtx.destination);
    }

    async function toggle() {
        if (!audioCtx) setupAudio();

        if (!playing) {
            await audioCtx.resume();
            await audioEl.play();
            playing = true;
            draw();
        } else {
            audioEl.pause();
            playing = false;
        }
    }

    function logIndex(x: number, width: number, fftBins: number) {
        const minLog = Math.log10(1);
        const maxLog = Math.log10(fftBins);

        const logX =
            minLog + (x / width) * (maxLog - minLog);

        return Math.floor(Math.pow(10, logX));
    }


    function draw() {
        if (!playing) return;

        analyser.getByteFrequencyData(
            dataArray as Uint8Array<ArrayBuffer>
        );

        const grid: string[][] = Array.from({ length: HEIGHT }, () =>
        Array(WIDTH).fill(" ")
        );

        const fftBins = dataArray.length;

        for (let x = 0; x < WIDTH; x++) {
            const i = logIndex(x, WIDTH, fftBins);
            const v = dataArray[i] ?? 0; // 0 → 255

            const target = (v / 255) * HEIGHT;

            if (smoothBars[x] === undefined) smoothBars[x] = target;
            smoothBars[x] += (target - smoothBars[x]) * 0.3;

            const barHeight = Math.floor(smoothBars[x]);
            const energy = v / 255;

            for (let y = 0; y < barHeight; y++) {
                const row = HEIGHT - 1 - y;

                // vertical energy falloff
                const falloff = 1 - y / HEIGHT;

                // flicker tied to signal energy
                const jitter = rand(-0.15, 0.15);

                let idx = Math.floor(
                    (energy * falloff + jitter) * (ASCII_RAMP.length - 1)
                );

                idx = Math.max(0, Math.min(ASCII_RAMP.length - 1, idx));
                
                const wobble = Math.floor(rand(-1, 1));
                const xx = x + wobble;

                if (xx >= 0 && xx < WIDTH) {
                    grid[row][xx] = ASCII_RAMP[idx];
                }
            }

            if (energy > 0.85 && Math.random() > 0.7) {
                const sparkRow = HEIGHT - barHeight - 1;
                if (sparkRow >= 0) {
                    const SPARKS = "@#%&*";
                    grid[sparkRow][x] =
                        SPARKS[Math.floor(Math.random() * SPARKS.length)];
                }
            }

        }

        asciiEl.textContent = grid.map(r => r.join("")).join("\n");
        requestAnimationFrame(draw);
    }



    // reactive amp controls
    $: if (gainNode) gainNode.gain.value = gain;

    $: if (wetGain && dryGain) {
    wetGain.gain.value = reverb;
    dryGain.gain.value = 1 - reverb;
    }

    $: if (distortion) {
    distortion.curve =
        distortionAmount === 0
        ? null
        : makeDistortionCurve(distortionAmount * 80);
    }

    onMount(() => {
        updateWidth();
        window.addEventListener("resize", updateWidth);

        return () => {
            window.removeEventListener("resize", updateWidth);
            audioEl?.pause();
            audioCtx?.close();
        };
    });
</script>

<h1>ASCII VISUALIZER</h1>

<hr>

<h3><i>sound in an ascii space</i></h3>


<button on:click={toggle}>
    {playing ? "❚❚" : "▶"}
</button>

<audio bind:this={audioEl} src="/YUKON.mp3" crossorigin="anonymous">
    YUKON.mp3
</audio>

<pre bind:this={asciiEl}></pre>

<div class="sliders">
    <label>
        gain
        <input type="range" min="0" max="2" step="0.01" bind:value={gain} />
    </label>

    <label>
        reverb
        <input type="range" min="0" max="1" step="0.01" bind:value={reverb} />
    </label>

    <label>
        distortion
        <input type="range" min="0" max="0.25" step="0.001" bind:value={distortionAmount} />
    </label>
</div>


<style>


pre {
    font-family: monospace;
    font-size: 7px;
    line-height: 1;
    white-space: pre;
    user-select: none;
    width: 90%;       /* fill width */
    margin: 0;
    padding: 0;
    overflow: hidden;  /* prevents scrolling inside <pre> */
    display: block;
    box-sizing: border-box;
}

.sliders {
    margin: 12px 0;
    display: flex;
    gap: 12px;
}

label {
    display: flex;
    flex-direction: column;
    font-size: 12px;
}

button {
    background-color: transparent;
    color: black;
    border: none;
    cursor: pointer;
    padding: 0;
    font: inherit;
}

</style>
