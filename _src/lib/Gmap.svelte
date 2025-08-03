<!-- Slider.svelte -->
<script>
    import { openSlider } from '$lib/stores.js';
  </script>
  
  <style>
    .slider {
      position: fixed;
      top: 0;
      right: 0;
      width: 100vw;
      max-height: 100vh; /* Set your preferred max height */
      height: 100vh;
      overflow: hidden;
      transition: height 0.3s ease-out;
      background-color: #fff;
      border-bottom: 1px solid #ccc;
      z-index: 1000;
    }
  
    .content {
      padding: 20px;
    }
  
    .close-button {
      cursor: pointer;
      position: absolute;
      top: 10px;
      right: 10px;
    }
  </style>
  
  <div class="slider" style="{`height: ${$openSlider ? '100vh' : '0'}`}">
    

      <iframe width="100%" height="100vh" frameborder="0" scrolling="no" marginheight="0" marginwidth="0" src="https://maps.google.com/maps?width=100%25&amp;height=100vh&amp;hl=en&amp;q=1%20Grafton%20Street,%20Dublin,%20Ireland+(My%20Business%20Name)&amp;t=&amp;z=14&amp;ie=UTF8&amp;iwloc=B&amphttps://www.google.com/maps/dir//Estrada+do+Pa%C3%A7o+do+Lumiar,Campus+do+Lumiar,Edificio+L,1649-038,+Lisboa/@38.77113,-9.2606634,12z/data=!4m8!4m7!1m0!1m5!1m1!1s0xd1933cd6d1688c3:0x82aef634c506a946!2m2!1d-9.1782624!2d38.7711589?entry=ttu;output=embed"></iframe>
    <div class="close-button" on:click={() => openSlider.set(false)}>Close</div>
  </div>
  