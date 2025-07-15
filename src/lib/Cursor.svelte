<script>
        // @ts-ignore
    //@ts-nocheck
   
    
  import { onMount } from 'svelte';

  let mouseX = 0;
  let mouseY = 0;
  let circleX = 0;
  let circleY = 0;

  const handleMouseMove = (event) => {
    mouseX = event.clientX;
    mouseY = event.clientY;
  };

  onMount(() => {
    // Update circle position on mouse move
    const updateCirclePosition = () => {
      const dx = mouseX - circleX;
      const dy = mouseY - circleY;

      // Adjust the magnetic force based on the distance
      const magneticForce = 0.1;
      circleX += dx * magneticForce;
      circleY += dy * magneticForce;

      // Update circle position
      const circleElement = document.getElementById('magneticCircle');
      if (circleElement) {
        circleElement.style.top = `${circleY}px`;
        circleElement.style.left = `${circleX}px`;
      }

      requestAnimationFrame(updateCirclePosition);
    };

    updateCirclePosition();
  });
</script>

<style>
  body {
    margin: 0;
    overflow: hidden;
  }

  .magnetic-circle {
    position: absolute;
    width: 50px;
    height: 50px;
    background-color: #3498db;
    border-radius: 50%;
  }
</style>

<div id="magneticCircle" class="magnetic-circle" on:mousemove="{handleMouseMove}"></div>
