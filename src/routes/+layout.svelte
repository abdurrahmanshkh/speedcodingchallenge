<script>
	import '../app.css';
	import { Navbar, NavBrand } from 'flowbite-svelte';
	import { Footer, FooterCopyright } from 'flowbite-svelte';
</script>

<Navbar class="border-2 p-4">
	<div>
		<span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">
			IET-KJSIT
		</span>
	</div>
	<div>
		<span class="self-center whitespace-nowrap text-xl font-semibold dark:text-white">
			Speed Coding Challenge
		</span>
	</div>
</Navbar>

<main class="p-10">
	<slot></slot>
</main>

<Footer class="border-2 p-4">
	<FooterCopyright href="https://abdurrahmanshkh.pages.dev/" by="created by Abdur Rehman Shaikh." />
</Footer>
