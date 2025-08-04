<script>
    // @ts-ignore
    // @ts-nocheck
    import { onMount } from 'svelte';
  
    export let targetDate;
  
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
  
    const updateCountdown = () => {
      calculateTimeRemaining();
    };
  
    onMount(() => {
      // Update the countdown immediately and then every second
      updateCountdown();
      setInterval(updateCountdown, 1000);
    });
  </script>
  
  
  
  
  

<div class=" row col-12 h-100">
    <div class="countdown text-white text-align-right">
        <div class="timer">
            <div class="days">{addLeadingZero(days)}</div>
            <div class="textDays">DIAS</div>
        </div>
        <div class="timer">
            <div class="hours">{addLeadingZero(hours)}</div>
            <div class="textHours">HORAS</div>
        </div>
        <div class="timer">
            <div class="minutes">{addLeadingZero(minutes)}</div>
            <div class="textMinutes">MIN.</div>
        </div>
        <div class="timer">
            <div class="seconds">{addLeadingZero(seconds)}</div>
            <div class="textSeconds">SEG.</div>
        </div>
    </div>
</div>

<style>
    .h--100 {
        min-height: 100vh;
        background-image: url("./event1.jpg");
    }
    .textSeconds,
    .textMinutes,
    .textHours,
    .textDays {
        font-size: 10px !important;
        text-decoration: none;
        display: block;
        font-weight: 500;
    }

    .text-align-right {
        text-align: right;
    }
    .p-y-md {
        padding-top: 60px !important;
        padding-bottom: 30px !important;
    }
    .countdown .timer > div {
        font-size: 24px;
        line-height: 24px;
        font-weight: 500;
        font-variant-numeric: slashed-zero;
    }
    .countdown .timer {
        display: inline-block;
        width: 75px;
        font-size: 14px;
        line-height: 18px;
        font-weight: 700;
        color: black;
        background: linear-gradient(rgb(170 170 170), rgb(97 97 97 / 0%));
        padding: 25px;
    }
</style>
