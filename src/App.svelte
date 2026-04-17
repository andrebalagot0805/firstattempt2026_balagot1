<script>
  import LoginNew from './lib/LoginNew.svelte';
  import DashboardNew from './lib/DashboardNew.svelte';

  let isLoggedIn = false;
  let user = null;

  function handleLogin(event) {
    user = event.detail;
    isLoggedIn = true;
  }

  function handleLogout() {
    isLoggedIn = false;
    user = null;
  }
</script>

<main>
  {#if !isLoggedIn}
    <LoginNew on:login={handleLogin} />
  {:else}
    <DashboardNew {user} on:logout={handleLogout} />
  {/if}
</main>

<style>
  main {
    height: 100vh;
    display: flex;
    flex-direction: column;
  }
</style>