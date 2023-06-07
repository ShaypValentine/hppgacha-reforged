<script>
  import Footer from "./lib/Footer.svelte";
  import Login from "./lib/Login.svelte";
  import RollSystem from "./lib/RollSystem.svelte";
  import { currentUser, pb } from "./lib/pocketbase";
</script>

<main>
  <h1>HppGacha:RE</h1>
  {#if $currentUser}
    <RollSystem />
  {/if}
</main>
<Footer />
