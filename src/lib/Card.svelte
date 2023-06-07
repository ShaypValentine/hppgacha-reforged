<script>
   export let name;
   export let image;
</script>

<div class="card">
    <p>{name}</p>
    <img width="100" height="100" src={image} alt="{name}" />
</div>
