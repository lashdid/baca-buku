<script>
	let search = false;
    let query;

	let popularBooks = [
		"Da Vinci Code",
		"Harry Potter and the Deathly Hallows",
		"Harry Potter and the Philosopher's Stone",
		"Harry Potter and the Order of the Phoenix",
		"Fifty Shades of Grey",
		"Harry Potter and the Goblet of Fire",
		"Harry Potter and the Chamber of Secrets",
		"Harry Potter and the Prisoner of Azkaban",
		"Angels and Demons",
		"Harry Potter and the Half-blood Prince:Children's Edition",
		"Fifty Shades Darker",
		"Twilight",
		"Girl with the Dragon Tattoo,The:Millennium Trilogy",
		"Fifty Shades Freed",
		"Lost Symbol",
		"New Moon",
		"Deception Point",
		"Eclipse",
		"Lovely Bones",
		"Curious Incident of the Dog in the Night-time"
	]

	window.$(document).ready(function() {
		window.$('#loadingSlider').lightSlider({
			autoWidth:true,
			loop:true,
			pager:false,
			onSliderLoad: function() {
				window.$('#loadingSlider').removeClass('cS-hidden');
			} 
		});  
	});

	function handle(e){
        if(e.key === "Enter" && query){
            window.location = `/page/search?query=${query}&page=1`
        }

        return false;
    }

	async function bookList() {
		try {

			let keyword = popularBooks[Math.floor(Math.random() * 21)]
			const request = await fetch(
				`https://www.googleapis.com/books/v1/volumes?q=${keyword}&maxResults=20`
			);
			const response = await request.json();
			const list = response.items;

			window.$(document).ready(function() {
				window.$('#slider').lightSlider({
					autoWidth:true,
					loop:true,
					pager:false,
					onSliderLoad: function() {
						window.$('slider').removeClass('cS-hidden');
					} 
				});  
			});
			
			console.log(list)
			return list;

		} catch (error) {
			console.error(error);
		}
	}

	function nameShortener(name){
		return name.length > 20 ? name.slice(0, 20) + "…" : name;
	}
</script>

<main class="flex flex-col w-full h-full my-20 px-3 space-y-8 items-center">
	<div class="flex flex-row items-center justify-center space-x-3">
		<img src="/img/stb.png" class="w-32" alt="icon" />
		<div class="flex flex-col text-4xl font-bold">
			<h2>STIBA</h2>
			<p>E-Perpus</p>
		</div>
	</div>
	<div
		class="flex flex-row bg-white w-full md:w-2/3 justify-between mx-auto rounded-full md:p-2 px-2 items-center"
		style="text-decoration: none!important;"
	>
		<input
			class="search-input appearance-none outline-none w-full p-5"
			type="text"
			name=""
			placeholder="Cari Buku"
			on:keypress={handle}
			on:blur={() => search = false}
			bind:value={query}
		/>
		<a href="{search ? 'javascript:void(0)' : query ? `/page/search?query=${query}&page=1` : 'javascript:void(0)'}" target="_self" class="search-btn rounded-full p-5 {query ? 'bg-blue-500' : ''}">
			<i class="fa fa-search md:text-3xl {query ? 'text-white' : ''}" />
		</a>
	</div>
	<div class="container-fluid" >
		<div class="bk">
			{#await bookList()}
				<ul id="loadingSlider" class="flex flex-row">
					{#each new Array(9) as _}
						<li class="flex justify-center">
							<!-- svelte-ignore a11y-invalid-attribute -->
							<a href="javascript:void(0)">
								<div class="box relative">
									Loading
								</div>
							</a>
						</li>
					{/each}
				</ul>
				{:then list}
				<ul id="slider" class="flex flex-row">
					{#each list as book}
						<li class="flex justify-center">
							<a href="/page/detail?id={book.id}" target="_self">
								<div class="box relative">
									<img src="{book.volumeInfo.imageLinks ? book.volumeInfo.imageLinks.smallThumbnail : '/img/noImage.png'}" alt="" class="w-full h-full rounded-md object-cover">                                        
									<span class="absolute bottom-0 w-full bg-gradient-to-t from-black to-transparent p-3 text-white font-semibold rounded-b-md">{nameShortener(book.volumeInfo.title)}</span>
								</div>
							</a>
						</li>
					{/each}
				</ul>
			{/await}
		</div>
	</div>
</main>
