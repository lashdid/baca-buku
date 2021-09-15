<script>
    const url = new URL(window.location.href)
    const params = new URLSearchParams(url.search)

    async function bookDetail() {
		try {
            let id = params.get("id")
			const request = await fetch(
				`https://www.googleapis.com/books/v1/volumes/${id}`
			);
			const result = await request.json();

			return result;

		} catch (error) {
			console.error(error);
		}
	}

    console.log(bookDetail())
</script>

{#await bookDetail()}
    <h5 class="text-white text-3xl font-bold text-center">Tunggu Sebentar ...</h5>
{:then book} 
    <div class="w-full md:w-2/3 flex flex-col mx-auto bg-white md:rounded-md p-5 space-y-5 my-3">
        <div class="flex flex-col space-y-5 mt-5 w-full">
            <div class="flex flex-col space-y-3 md:space-y-0 md:flex-row md:space-x-5">
                <img src={book.volumeInfo.imageLinks ? book.volumeInfo.imageLinks.thumbnail : '../src/assets/noImage.png'} alt="" class="rounded-md w-1/3 mx-auto md:mx-0">
                <div class="flex flex-col space-y-3">
                    <h3 class="text-5xl font-bold text-center">{book.volumeInfo.title}</h3>
                    <span class="text-2xl">Penerbit : {book.volumeInfo.publisher}</span>
                    <span class="text-2xl">
                        Penulis :
                        {#if book.volumeInfo.authors}
                            {#each book.volumeInfo.authors as author}
                                <span>{author} </span>
                            {/each}
                        {:else}
                            Tidak Ditemukan
                        {/if} 
                    </span>
                    <span class="text-2xl">Konsep Buku : {book.volumeInfo.categories ? book.volumeInfo.categories[0] : "Tidak Ditemukan"}</span>
                    <span class="text-2xl">Jumlah Halaman : {book.volumeInfo.pageCount}</span>
                    <span class="text-2xl">Tanggal Terbit : {book.volumeInfo.publishedDate ? book.volumeInfo.publishedDate : "Tidak Ditemukan"}</span>
                    <div class="flex flex-row space-x-3 items-center text-3xl mt-5">
                        <i class="fa fa-star text-yellow-500" />
                        <span class="font-bold">{book.volumeInfo.averageRating ? book.volumeInfo.averageRating : "0"}</span>
                    </div>
                </div>
            </div>
            <h6 class="text-2xl font-bold">Kategori :</h6>
            {#if book.volumeInfo.categories}
                <div class="flex flex-wrap">
                    {#each book.volumeInfo.categories as category}
                        <span class="mr-3 mb-2 px-3 py-2 bg-gray-300 font-bold text-2xl rounded-md">{category}</span>
                    {/each}
                </div>
            {:else}
                Tidak Ditemukan
            {/if}
        </div>
        <div class="flex flex-col">
            <h6 class="text-2xl font-bold">Deskripsi :</h6>
            <span>{@html book.volumeInfo.description ? book.volumeInfo.description : "Tidak Ditemukan"}</span>
        </div>
        <span class="flex flex-row">
            <a target="_blank" href={book.volumeInfo.previewLink} class="text-2xl text-semibold text-blue-600 hover:text-blue-500">Info >></a>
        </span>
        <div class="flex flex-row space-x-3 items-center">
            <a target="_blank" href={book.accessInfo.webReaderLink} class="px-3 py-2 bg-blue-500 hover:bg-blue-600 text-white hover:text-white font-bold text-2xl rounded-md">{book.saleInfo.saleability == "FREE" ? "Baca ( Gratis )" : "Baca Sampel"}</a>
            {#if book.saleInfo.saleability == "NOT_FOR_SALE"}
                <span class="text-gray-500">E-book tidak tersedia</span>
            {/if}
            {#if book.saleInfo.saleability == "FOR_SALE"}
                <a target="_blank" href={book.saleInfo.buyLink} class="px-3 py-2 bg-blue-500 hover:bg-blue-600 text-white hover:text-white font-bold text-2xl rounded-md {book.saleInfo.saleability != "FOR_SALE" ? "hidden" : ""}">Beli ( {book.saleInfo.retailPrice.currencyCode} {book.saleInfo.retailPrice.amount.toLocaleString()} )</a>
            {/if}
        </div>
        <b class="rounded-md p-2 bg-gray-200 text-bold {book.saleInfo.saleability == "FREE" ? "hidden" : ""}">Perhatian : Jika sampel tidak dapat dibaca, silahkan pergi ke laman "Info"</b>
    </div>
{/await}