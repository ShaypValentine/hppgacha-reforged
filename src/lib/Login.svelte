<script lang="js">
    import { currentUser, pb } from "./pocketbase";

    let username = "";
    let password = "";

    async function login() {
        const user = await pb
            .collection("users")
            .authWithPassword(username, password);
    }

    async function signUp() {
        try {
            const data = {
                username,
                password,
                passwordConfirm: password,
                roll_available: 4,
            };
            const createdUser = await pb.collection("users").create(data);
            await login();
        } catch (err) {
            console.error(err);
            console.log(err.message);
        }
    }

    function signOut() {
        pb.authStore.clear();
        location.reload();
    }
</script>

{#if $currentUser}
    <p>
        Signed in as {$currentUser.username}
        <button on:click={signOut}>Sign Out</button>
    </p>
{:else}
    <form on:submit|preventDefault>
        <input placeholder="Username" type="text" bind:value={username} />

        <input placeholder="Password" minlength="8" type="password" bind:value={password} />
        <button on:click={signUp}>Sign Up</button>
        <button on:click={login}>Login</button>
    </form>
{/if}
