<script lang="ts">
  import {Greet} from '../wailsjs/go/main/App.js'
  import Slider from '$lib/Slider.svelte' 

  let resultText: string = "Please enter your name below 👇"
  let name: string

  let sliderValue: number = 50

</script>

<main>
  <Slider/>
</main>

<style>
</style>
