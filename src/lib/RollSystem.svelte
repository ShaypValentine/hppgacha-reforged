<script>
    // @ts-nocheck

    import { currentUser, pb } from "./pocketbase";
    import Card from "./Card.svelte";
    import { onMount } from "svelte";
    import debounce from "lodash/debounce";

    onMount(async () => {
        await pb.collection("users").authRefresh();
    });

    const handleRoll = debounce(rollForCard, 300, {
        leading: false,
        trailing: true,
        maxWait: 300,
    });

    async function rollForCard() {
        const rollBtn = document.getElementById("rollBtn");
        rollBtn.disabled = true;
        const user = await pb.collection("users").getOne($currentUser.id);
        if (user.roll_available > 0) {
            let randomCard = await pb
                .collection("common_cards")
                .getList(1, 1, { sort: "@random" });

            await pb.collection("users").update(user.id, {
                roll_available: user.roll_available - 1,
            });

            let rolledCard = randomCard.items[0];
            await updateStatsAndInventory(user, rolledCard);

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

            let childrens = rolledCards.children;
            if (childrens.length > 6) {
                rolledCards.removeChild(childrens[0]);
            }
        }
        rollBtn.disabled = false;
    }

    async function updateStatsAndInventory(user, card) {
        try {
            const inventory = await pb.collection("inventory").getList(1, 50, {
                filter: 'card = "' + card.id + '" && user = "' + user.id + '" ',
            });

            if (inventory.items.length > 0) {
                await pb.collection("inventory").update(inventory.items[0].id, {
                    quantity: inventory.items[0].quantity + 1,
                });
            } else {
                await pb.collection("inventory").create({
                    user: user.id,
                    card: card.id,
                    quantity: 1,
                });
            }

            await pb.collection("common_cards").update(card.id, {
                stats_rolled: card.stats_rolled + 1,
            });

            await pb.collection("users").update(user.id, {
                stats_total_roll: user.stats_total_roll + 1,
            });
            console.log("updated");
        } catch (err) {
            console.log(err);
        }
    }
</script>

<button id="rollBtn" on:click={handleRoll}>
    Roll : {$currentUser.roll_available}
</button>

<div id="rolledCards" />

<style>
    #rolledCards {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: center;
        min-height: 205px;
    }
    #rollBtn {
        margin: 20px;
    }
</style>
