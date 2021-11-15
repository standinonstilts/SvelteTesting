<script>
    export let Slide;
</script>

<div
    class="slide"
    class:current={Slide.selected}
    style="background: url('{Slide.url}') no-repeat center top/cover;"
>
    <div class="content">
        <h1>{Slide.title}</h1>
        <p>{Slide.description}</p>
    </div>
</div>

<style>
    .slide {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        opacity: 0;
        transition: opacity 0.4s ease-in-out;
    }

    .slide.current {
        opacity: 1;
    }

    .slide .content {
        position: absolute;
        bottom: 70px;
        left: -600px;
        opacity: 0;
        width: 600px;
        background-color: rgba(255, 255, 255, 0.8);
        color: #333;
        padding: 35px;
    }

    .slide.current .content {
        opacity: 1;
        transform: translateX(600px);
        transition: all 0.7s ease-in-out 0.3s;
    }

    .slide .content h1 {
        margin-bottom: 10px;
    }
</style>
