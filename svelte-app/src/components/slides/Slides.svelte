<script>
    import Slide from "./Slide.svelte";
    export let Data;
    export let Rotation;
    export let Auto;
    export let Interval;

    let current = 0;
    let slideInterval;

    function setSlideAutomation() {
        if (Auto) {
            clearInterval(slideInterval);
            slideInterval = setInterval(nextHandler, Interval);
        }
    }
    setSlideAutomation();

    let buffer = Data;
    buffer[current].selected = true;

    function next() {
        nextHandler();
        setSlideAutomation();
    }

    function nextHandler() {
        //console.log('next enter', rotation, current);
        buffer[current].selected = false;
        if (current < buffer.length - 1) {
            current++;
        } else if (rotation) {
            current = 0;
        }
        buffer[current].selected = true;
        console.log("next exit", rotation, current);
    }

    function prev() {
        prevHandler();
        setSlideAutomation();
    }

    function prevHandler() {
        //console.log('prev enter', rotation, current);
        buffer[current].selected = false;
        if (current > 0) {
            current--;
        } else if (rotation) {
            current = buffer.length - 1;
        }
        buffer[current].selected = true;
        console.log("prev exit", rotation, current);
    }
</script>

<div class="slider">
    {#each buffer as Slide}
        <Slide {Slide} />
    {/each}
</div>
<div class="buttons">
    <button id="prev" on:click={prev} />
    <button id="next" on:click={next} />
</div>

<style>
    .slider {
        position: relative;
        overflow: hidden;
        height: 100vh;
        width: 100vw;
        top: -7px;
        left: -7px;
    }

    .buttons button#next {
        position: absolute;
        top: 50%;
        right: 15px;
    }

    .buttons button#next:before {
        font-family: "FontAwesome";
        content: "\f054";
        padding: 0 4px 0 6px;
    }

    .buttons button#prev {
        position: absolute;
        top: 50%;
        left: 15px;
    }

    .buttons button#prev:before {
        font-family: "FontAwesome";
        content: "\f053";
        padding: 0 6px 0 4px;
    }

    .buttons button {
        border: 2px solid #ddd;
        background-color: transparent;
        color: #ddd;
        cursor: pointer;
        padding: 13px 15px;
        border-radius: 50%;
        outline: none;
    }

    .buttons button:hover {
        border: 2px solid #fff;
        background-color: #fff;
        color: #333;
    }
</style>
