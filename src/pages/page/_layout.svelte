<script>
	let sideNav = false;
	let search = false;
    let query;

	function handle(e){
        if(e.key === "Enter" && query){
            window.location = `/page/search?query=${query}&page=1`
        }

        return false;
    }
</script>

<header class="ats">
	<div class="ts">
		<!-- <div
			id="mySidenav"
			class="sidenav animate__animated animate__slideInRight"
			style="display: {sideNav ? 'block' : 'none'}"
		>
			<a
				style="text-decoration: none;"
				href="javascript:void(0)"
				class="closebtn"
				on:click={() => (sideNav = false)}>&times;</a
			>
			<div class="nmr mx-auto text-auto-center" align="center">
				<a href="/" target="_self">Beranda</a>
				<a href="/#">Login Anggota</a>
				<a href="/#">Login Pustakawan</a>
				<a href="/#">Info Perpustakaan</a>
				<a href="/#">Contact</a>
			</div>
			<div class="prf mx-auto text-auto-center" align="center">
				<div>
					<img src="src/assets/sqw.jpg" class="pp" alt="profile" />
				</div>
				<div class="ket">
					<p>Squidward</p>
					<p>Mahasiswa Teknik Lorem Ipsum</p>
				</div>
				<button class="lgo">Logout</button>
			</div>
		</div> -->
		<div class="row">
			<div class="col-5">
				<img src="../src/assets/stb.png" class="stb" alt="icon" />
				<div class="tlss">
					<h2 style="margin-bottom: 0!important;">STIBA</h2>
					<p>E-Perpus</p>
				</div>
			</div>
			<div class="col-5">
				<div
					class="search-box w-1/3 justify-between"
					style="text-decoration: none!important;"
				>
					<input
						class="search-input appearance-none outline-none w-5/6 pl-3"
						type="text"
						name=""
						placeholder="Cari Buku"
						on:keypress={handle}
						on:blur={() => search = false}
                        bind:value={query}
					/>
					<a href="{search ? 'javascript:void(0)' : query ? `/page/search?query=${query}` : 'javascript:void(0)'}" target="_self" class="search-btn rounded md:rounded-full {query ? 'bg-blue-500' : ''}">
						<i class="fa fa-search md:text-3xl {query ? 'text-white' : ''}" />
					</a>
				</div>
			</div>
			<div class="col-5">
				<a href="/" class="bttn">
					<i class="fa fa-home md:text-3xl" />
				</a>
			</div>
		</div>
	</div>
</header>
<slot/>
