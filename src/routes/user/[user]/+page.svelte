<script>
    import { page } from '$app/stores';

    async function getLibrary() {
    const res = await fetch(`https://ws.audioscrobbler.com/2.0/?method=library.getartists&api_key=a616b292b50e4f5ecb60671554c2965d&user=${$page.params.user}&format=json`);
    const data = await res.json();
    if (res.ok) {
        return data.artists.artist;
    } else {
        throw new Error(res);
    }
    }
    let promise = getLibrary();
</script>

<h1>{$page.params.user}'s Library</h1>

{#await promise}
	<p>...waiting</p>
{:then artists}
    {#each Object.entries(artists) as [key, artist] }
        <h2> <a href="../artist/{artist.name}">{artist.name}</a> </h2>
        <p>Plays: {artist.playcount}</p>
    {/each}
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}





