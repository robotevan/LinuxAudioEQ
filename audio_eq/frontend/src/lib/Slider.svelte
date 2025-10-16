
<script lang="ts">
    import { createEventDispatcher } from "svelte";

    // Props (Svelte 5 style)
    const { min = -12,
	    max = 12,
	    step = 0.1,
	    ticks = 5 } = $props();

    const dispatch = createEventDispatcher();

    let value = $state(0);
    let displayValue = $derived(value.toFixed(2));

    // Handlers
    function onInput(event: Event) {
	value = parseFloat((event.target as HTMLInputElement).value);
	dispatch("change", { value });
    }

    function onBoxChange(event: Event) {
	const v = parseFloat((event.target as HTMLInputElement).value);
	if (!isNaN(v)) {
	    value = Math.min(max, Math.max(min, v));
	    dispatch("change", { value });
	}
    }
</script>

<style>
    .container {
	display: flex;
	flex-direction: column;
	align-items: center;
	width: 60px;
	margin: 0 8px;
    }

    .slider-wrap {
	position: relative;
	height: 150px;
	display: flex;
	align-items: center;
    }

    input[type="range"] {
	-webkit-appearance: none;
	width: 150px;
	height: 6px;
	transform: rotate(-90deg);
    }

    input[type="range"]::-webkit-slider-thumb {
	-webkit-appearance: none;
	appearance: none;
	width: 14px;
	height: 14px;
	border-radius: 50%;
	background: #2196f3;
	cursor: pointer;
	border: 1px solid #ccc;
    }

    input[type="range"]::-webkit-slider-runnable-track {
	background: linear-gradient(to right, #666, #aaa);
	height: 6px;
	border-radius: 3px;
    }

    .ticks {
	position: absolute;
	left: 50%;
	transform: translateX(16px);
	display: flex;
	flex-direction: column;
	justify-content: space-between;
	height: 100%;
	font-size: 0.7rem;
	color: #666;
    }

    .value-box {
	width: 50px;
	text-align: center;
	margin-top: 4px;
	border: 1px solid #ccc;
	border-radius: 4px;
	padding: 2px;
    }

    label {
	margin-top: 6px;
	font-size: 0.8rem;
	color: #333;
    }
</style>

<div class="container">
    <div class="slider-wrap">
	<input
	    type="range"
	    min={min}
	    max={max}
	    step={step}
	    value={value}
	    on:input={onInput}
	/>

	<!-- Tick labels -->
	<div class="ticks">
	    {#each Array(ticks) as _, i}
		{@const tickValue = max - ((max - min) / (ticks - 1)) * i}
		<span>{tickValue.toFixed(0)}</span>
	    {/each}
	</div>
    </div>

    <!-- Editable numeric box -->
    <input
	class="value-box"
	type="number"
	step={step}
	value={displayValue}
	on:change={onBoxChange}
    />

</div>
