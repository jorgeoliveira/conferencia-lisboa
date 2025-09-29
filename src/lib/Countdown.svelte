<script>
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';

    export let targetDate;
    export let containerClass = '';
    export let blockClass = '';
    export let numberClass = '';
    export let labelClass = '';

    let days = 0,
      hours = 0,
      minutes = 0,
      seconds = 0;

    const calculateTimeRemaining = () => {
      const now = new Date().getTime();
      const difference = targetDate - now;

      days = Math.max(0, Math.floor(difference / (1000 * 60 * 60 * 24)));
      hours = Math.max(0, Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)));
      minutes = Math.max(0, Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60)));
      seconds = Math.max(0, Math.floor((difference % (1000 * 60)) / 1000));
    };

    const addLeadingZero = (value) => {
      return value < 10 ? `0${value}` : value;
    };

    onMount(() => {
      const interval = setInterval(calculateTimeRemaining, 1000);
      calculateTimeRemaining(); // Initial call
      return () => clearInterval(interval);
    });
</script>

<div class="countdown-container {containerClass}">
    <div class="time-block {blockClass}">
        {#key days}
            <span class="number {numberClass}" in:fade={{ duration: 300 }}>{addLeadingZero(days)}</span>
        {/key}
        <span class="label {labelClass}">Dias</span>
    </div>
    <div class="time-block {blockClass}">
        {#key hours}
            <span class="number {numberClass}" in:fade={{ duration: 300 }}>{addLeadingZero(hours)}</span>
        {/key}
        <span class="label {labelClass}">Horas</span>
    </div>
    <div class="time-block {blockClass}">
        {#key minutes}
            <span class="number {numberClass}" in:fade={{ duration: 300 }}>{addLeadingZero(minutes)}</span>
        {/key}
        <span class="label {labelClass}">Minutos</span>
    </div>
    <div class="time-block {blockClass}">
        {#key seconds}
            <span class="number {numberClass}" in:fade={{ duration: 300 }}>{addLeadingZero(seconds)}</span>
        {/key}
        <span class="label {labelClass}">Segundos</span>
    </div>
</div>

<style>
    /* All styles have been removed to make the component reusable. */
    /* Styling will be handled by the parent component (+page.svelte). */
</style>
