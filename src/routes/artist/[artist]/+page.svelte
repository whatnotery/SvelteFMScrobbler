<script>
    import { page } from '$app/stores';

async function getAlbums() {
    const res = await fetch(`http://ws.audioscrobbler.com/2.0/?method=artist.gettopalbums&artist=${$page.params.artist}&api_key=a616b292b50e4f5ecb60671554c2965d&format=json`);
    const data = await res.json();
    if (res.ok) {
        return data.topalbums.album;
    } else {
        throw new Error(res);
    }
    }
    let promise = getAlbums();
</script>

<h1>{$page.params.artist}'s Discography</h1>

{#await promise}
	<p>...waiting</p>
{:then album}
    {#each Object.entries(album) as [key, album] }
    <img src='{album.image[3]['#text']}' alt='Album cover for {album.name}'>
        <h2> <a href="./{$page.params.artist}/album/{album.name}">{album.name} </a> </h2>
       
    {/each}
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}