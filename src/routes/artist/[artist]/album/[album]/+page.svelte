<script>
    import { page } from '$app/stores';
    async function getLibrary() {
    const res = await fetch(`https://ws.audioscrobbler.com/2.0/?method=album.getinfo&artist=${$page.params.artist}&album=${$page.params.album}&api_key=a616b292b50e4f5ecb60671554c2965d&format=json`);
    const data = await res.json();
    if (res.ok) {
        return data.album;
    } else {
        throw new Error(res);
    }
    }
    let promise = getLibrary();
</script>

<h1>{$page.params.album} by <a href="/artist/{$page.params.artist}">{$page.params.artist}</h1>

{#await promise}
	<p>...waiting</p>
{:then albumData}
    <img src="{albumData["image"][3]["#text"]}" alt="album cover for {$page.params.album} by {$page.params.artist}">
    {#each Object.entries(albumData['tracks']) as [key, data] }
    {#each Object.entries(data) as [key, track] }
        <h3>{key}. <a href="https://www.youtube.com/results?search_query={track.name}+{$page.params.album}+{$page.params.artist}">{track.name}</a></h3>
    {/each}
    {/each}
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

