<script>
        // @ts-ignore
    //@ts-nocheck

  import { readable } from 'svelte/store';
  import { onMount, onDestroy, setContext } from 'svelte';

  let showAlert = readable(true);

  // Export the showAlert function to be used in regular JavaScript
  setContext('showAlert', function showSvelteAlert(type, message, onClose) {
    showAlert.set(true);

    const timer = setTimeout(() => {
      showAlert.set(false);
      onClose();
    }, 5000);

    return () => {
      clearTimeout(timer);
      onClose();
    };
  });

  onMount(() => {
    return () => showAlert.set(false);
  });

  onDestroy(() => {
    onClose();
  });

  const handleClose = () => {
    showAlert.set(false);
    onClose();
  };
</script>

{#if $showAlert}
  <div class="alert" class:type>
    <button type="button" class="btn-close" aria-label="Close" on:click={handleClose}></button>
    {message}
  </div>
{/if}

<style>
  .alert {
    position: fixed;
    top: 1rem;
    right: 1rem;
    z-index: 1000;
    padding: 1rem;
    border-radius: 0.25rem;
    max-width: 400px;
  }

  .success {
    background-color: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
  }

  .error {
    background-color: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
  }

  .btn-close {
    float: right;
    font-size: 1.25rem;
    font-weight: bold;
    line-height: 1;
    color: #000;
    text-shadow: 0 1px 0 #fff;
    opacity: 0.5;
    cursor: pointer;
    background: none;
    border: none;
  }
</style>
