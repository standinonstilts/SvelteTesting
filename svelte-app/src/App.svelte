<script>
	import Data from "../public/resources/data/slides.js";
	import MySlides from "./components/myslides/MySlides.svelte";
</script>

<!--HTML-->

<svelte:head>
	<title>{Data[0].title}</title>
</svelte:head>
<MySlides/>

<style>
</style>
