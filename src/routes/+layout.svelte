<script>
	import '../app.css';
	import { Navbar } from 'flowbite-svelte';
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

<main class="bg-[url(background.jpg)] bg-cover bg-center bg-no-repeat px-20 py-14">
	<slot></slot>
</main>

<Footer class="border-2 p-4">
	<FooterCopyright by="created by Abdur Rehman Shaikh." />
</Footer>
