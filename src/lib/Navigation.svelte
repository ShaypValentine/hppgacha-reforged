<script>
    import { Router, Route, Link } from "svelte-routing";
</script>

<div class="navigation">
    <Link to="/"><button>Home</button></Link>
    <Link to="/inventory"><button>Inventory</button></Link>
</div>

<style>
    .navigation {
        display: flex;
        justify-content: center;
        padding: 20px;
    }
    button {
        margin: 0 5px;
    }
</style>
