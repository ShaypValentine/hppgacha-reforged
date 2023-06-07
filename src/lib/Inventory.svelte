<script>
    import { currentUser, pb } from "./pocketbase";
    import Card from "./Card.svelte";
    import { onMount } from "svelte";
    let inventory = [];
    onMount(async () => {
        await pb.collection("users").authRefresh();

        const user = await pb.collection("users").getOne($currentUser.id);
        const inventoryFetch = await pb
            .collection("inventory")
            .getList(1, 200, {
                filter: 'user = "' + user.id + '" ',
                expand: "card",
            });

        inventory = inventoryFetch.items;
        console.table(inventory);
    });
</script>
<h1>Inventory</h1>

<div id="inventoryCard">
    {#each inventory as item}
        <div>
            <Card
                name={item.expand.card.name}
                image={pb.files.getUrl(item.expand.card, item.expand.card.image)}
            />
            <p>{item.quantity}</p>
        </div>
    {/each}
</div>

<style>
    #inventoryCard {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        min-height: 205px;
    }
</style>
