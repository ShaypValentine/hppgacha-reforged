<script>
  // @ts-nocheck

  import Footer from "./lib/Footer.svelte";
    import Inventory from "./lib/Inventory.svelte";
  import Navigation from "./lib/Navigation.svelte";
  import RollSystem from "./lib/RollSystem.svelte";
  import { currentUser, pb } from "./lib/pocketbase";
  import { Router, Route, Link } from "svelte-routing";
</script>

<main>
  <h1>HppGacha:RE</h1>
  <Router>
    <nav>
      {#if $currentUser}
        <Navigation />
      {/if}
    </nav>
    <div>
      <Route path="/">
        {#if $currentUser}
          <RollSystem />
        {/if}
      </Route>
      <Route path="/inventory">
        <Inventory />
      </Route>
    </div>
  </Router>

</main>
<Footer />
