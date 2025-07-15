<script>
  // @ts-ignore
  //@ts-nocheck
  import { onMount } from "svelte";
  import Lenis from "@studio-freight/lenis";
  import "../app.scss";
  import "../style.css";

  onMount(async () => {
    const lenis = new Lenis({
      duration: 2.0,
      easing: (t) => (t === 1 ? 1 : 1 - Math.pow(2, -10 * t)),
      direction: "vertical",
      gestureDirection: "vertical",
      smooth: true,
      smoothTouch: false,
      touchMultiplier: 2,
    });

    function raf(time) {
      lenis.raf(time);
      requestAnimationFrame(raf);
    }

    requestAnimationFrame(raf);
  });
</script>

<head>
  <!-- ... other head elements ... -->
  <!-- ... other head elements ... -->
  <!-- ... other head elements ... -->
  <!-- ... other head elements ... -->
  <!-- ... other head elements ... -->
  <script
    src="https://www.google.com/recaptcha/api.js?render=6LfwFSMpAAAAABX_XBgRUsB26EBYotsSZASIQYSF"
  ></script>

</head>
<slot />
