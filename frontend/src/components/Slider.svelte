<script lang="ts">
	import type { FormEventHandler } from "svelte/elements";

    let {
        value = $bindable(50),
        min = 0,
        max = 100,
        step = 1,
        label = "gain",
        height = "200px",
        showValue = true
    } = $props();

    let isDragging = $state(false);
    let sliderRef = $state();

    function handleInput(event: InputEvent) {
        value = parseFloat(event.target.value);
    }

    function handleMouseDown() {
        isDragging = true;
    }

    function handleMouseUp() {
        isDragging = false;
    }

    function handleMouseMove(event: MouseEvent) {
        if (!isDragging || !sliderRef) return;
       
        const rect = sliderRef.getBoundingClientRect();
        const y = event.clientY - rect.top;
        const percentage = 1 - (y / rect.height);
        const newValue = Math.max(min, Math.min(max, percentage * (max - min) + min));
        value = Math.round(newValue / step) * step;
    }

    // Calculate percentage for styling
    const percentage = $derived(((value - min) / (max - min)) * 100);
</script>

<style>
    .slider-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 12px;
        user-select: none;
    }

    .slider-label {
        font-size: 14px;
        font-weight: 500;
        color: #333;
    }

    .slider-wrapper {
        position: relative;
        width: 40px;
        cursor: pointer;
    }

    .slider-track {
        position: absolute;
        left: 50%;
        transform: translateX(-50%);
        width: 6px;
        height: 100%;
        background-color: #e0e0e0;
        border-radius: 3px;
        overflow: hidden;
    }

    .slider-input {
        position: absolute;
        width: 100%;
        height: 100%;
        opacity: 0;
        cursor: pointer;
        writing-mode: bt-lr;
        -webkit-appearance: slider-vertical;
    }

    .slider-thumb {
        position: absolute;
        left: 50%;
        transform: translate(-50%, 50%);
        width: 18px;
        height: 18px;
        background-color: white;
        border: 2px solid #0d6efd;
        border-radius: 50%;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
    }

    .slider-thumb.dragging,
    .slider-thumb:hover {
        transform: translate(-50%, 50%) scale(1.2);
        box-shadow: 0 3px 6px rgba(0, 0, 0, 0.3);
    }

    .slider-value {
        font-size: 16px;
        font-weight: 600;
        color: #0d6efd;
        min-width: 40px;
        text-align: center;
    }
</style>

<div class="slider-container">
    {#if label}
        <label class="slider-label">{label}</label>
    {/if}
   
    <div
        class="slider-wrapper"
        style="height: {height}"
        bind:this={sliderRef}
        onmousedown={handleMouseDown}
        onmouseup={handleMouseUp}
        onmousemove={handleMouseMove}
        role="slider"
        tabindex="0"
        aria-valuemin={min}
        aria-valuemax={max}
        aria-valuenow={value}
        aria-label={label}
    >
        <div class="slider-track"></div>
       
        <input
            type="range"
            {min}
            {max}
            {step}
            bind:value
            oninput={handleInput}
            orient="vertical"
            class="slider-input"
        />
       
        <div
            class="slider-thumb"
            style="bottom: {percentage}%"
            class:dragging={isDragging}
        ></div>
    </div>
   
    {#if showValue}
        <div class="slider-value">{value}</div>
    {/if}
</div>


