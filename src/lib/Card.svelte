<script>
    export let name;
    export let image;
</script>

<div class="card">
    <p>{name}</p>
    <img width="100" height="100" src={image} alt={name} />
</div>

<style>
    .card {
        display: inline-block;
        margin: 10px;
        padding: 10px;
        border: 1px solid #eee;
        border-radius: 5px;
        text-align: center;
        background-color: #212529;
    }
</style>
