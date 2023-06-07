<script>
  import Footer from "./lib/Footer.svelte";
import Login from "./lib/Login.svelte";
  import RollButton from "./lib/RollButton.svelte";
  import { currentUser, pb } from "./lib/pocketbase";
</script>

<main>
  <h1>HppGacha:RE</h1>
  {#if $currentUser}
    <RollButton />
  {/if}
</main>
<Footer />
<style>
  .read-the-docs {
    color: #888;
  }
</style>
