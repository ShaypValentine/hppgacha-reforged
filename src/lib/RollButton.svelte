<script>
    import { currentUser, pb } from "./pocketbase";
    import Card from "./Card.svelte";
    async function rollForCard() {
        const user = await pb.collection("users").getOne($currentUser.id);
        if (user.roll_available > 0) {
            
            let randomCard = await pb
                .collection("common_cards")
                .getList(1, 1, { sort: "@random" });

            await pb.collection("users").update(user.id, {
                roll_available: user.roll_available - 1,
            });

            let rolledCard = randomCard.items[0];
            const url = pb.files.getUrl(rolledCard, rolledCard.image);
            const rolledCards = document.getElementById("rolledCards");

            new Card({
                target: rolledCards,
                props: {
                    // @ts-ignore
                    name: rolledCard.name,
                    // @ts-ignore
                    image: url,
                },
            });

            let childrends = rolledCards.children;
            if (childrends.length > 6) {
                rolledCards.removeChild(childrends[0]);
            }
        }
    }
</script>

<button on:click={rollForCard}> Roll : {$currentUser.roll_available} </button>

<div id="rolledCards" />

<style>
    #rolledCards {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
    }
</style>
