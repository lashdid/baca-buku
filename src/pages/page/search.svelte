<script>
    const url = new URL(window.location.href)
    const params = new URLSearchParams(url.search)
    
    let page = Number(params.get("page"))
    let query = params.get("query")

    async function mangaList() {
		try {

            let startIndex = (page - 1) * 20

			const request = await fetch(
				`https://www.googleapis.com/books/v1/volumes?q=${query}&maxResults=20&startIndex=${startIndex}`
			);
			const response = await request.json();
			const list = response.items;

            console.log(list)
			return list;

		} catch (error) {
			console.error(error);
		}
	}

    function nameShortener(name){
		return name.length > 20 ? name.slice(0, 20) + "…" : name;
	}

    console.log(mangaList())
</script>

<div class="container-fluid">
    <div class="bok">
        {#await mangaList()}
            <h5 class="text-white text-3xl font-bold text-center">Mencari ...</h5>
        {:then list}
            {#if list == undefined}
                <div class="text-white text-3xl font-bold text-center">Tidak Ditemukan</div>
            {/if}
            {#if list != undefined}
            <ul class="px-2 grid gap-4 grid-cols-2 md:grid-cols-5 lg:grid-cols-7">
                {#each list as book}
                    <!-- {#await promise}
                        <li class="flex justify-center">
                            <div class="box p-2">
                                <div class="w-full h-3/4 bg-gray-300 rounded-md animate-pulse"/>
                                <div class="w-full bg-gray-300 p-2 mt-1 rounded-md animate-pulse"/>
                            </div>
                        </li>
                    {:then manga}  -->
                        <li class="flex justify-center">
                            <a href="/page/detail?id={book.id}" target="_self">
                                <div class="box relative">
                                    <img src="{book.volumeInfo.imageLinks ? book.volumeInfo.imageLinks.smallThumbnail : '../src/assets/noImage.png'}" alt="" class="w-full h-full rounded-md object-cover">                                        
                                    <span class="absolute bottom-0 w-full bg-gradient-to-t from-black to-transparent p-3 text-white font-semibold rounded-b-md">{nameShortener(book.volumeInfo.title)}</span>
                                </div>
                            </a>
                        </li>
                    <!-- {/await} -->
                {/each}
            </ul>
            {/if}
            <div class="flex flex-row my-5 px-5 justify-between items-center">
                <a target="_self" href="/page/search?query={query}&page={page - 1}" class="{page == 1 ? 'hidden': ''} px-3 py-2 bg-white hover:bg-gray-200 font-bold text-2xl rounded-md">Sebelumnya</a>
                <h5 class="text-white font-semibold text-3xl">Page {page}</h5>
                <a target="_self" href="/page/search?query={query}&page={page + 1}" class="{list == undefined ? 'hidden' : ''} px-3 py-2 bg-white hover:bg-gray-200 font-bold text-2xl rounded-md">Selanjutnya</a>
            </div>
        {/await}
    </div>
</div>