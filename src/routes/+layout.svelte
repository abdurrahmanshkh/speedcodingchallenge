<script>
import '../app.css';
import { Navbar } from 'flowbite-svelte';
import { Footer, FooterCopyright } from 'flowbite-svelte';
</script>

<svelte:head>
	<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
</svelte:head>

<div class="min-h-screen flex flex-col bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900">
	<!-- Enhanced Navbar -->
	<Navbar class="bg-white/10 backdrop-blur-md border-b border-white/20 shadow-lg">
		<div class="flex items-center justify-between w-full py-2">
			<div class="flex items-center space-x-4">
				<div class="flex items-center space-x-2">
					<div class="w-8 h-8 bg-gradient-to-r from-blue-500 to-purple-600 rounded-lg flex items-center justify-center">
						<span class="text-white font-bold text-sm">IET</span>
					</div>
					<span class="text-white font-semibold text-lg tracking-tight">IET-KJSIT</span>
				</div>
			</div>
			
			<div class="flex items-center space-x-2">
				<div class="hidden sm:block w-2 h-2 bg-green-400 rounded-full animate-pulse"></div>
				<span class="text-white font-medium text-lg bg-gradient-to-r from-blue-400 to-purple-400 bg-clip-text text-transparent">
					Speed Coding Challenge
				</span>
			</div>
		</div>
	</Navbar>

	<!-- Enhanced Main Content -->
	<main class="flex-1 relative overflow-hidden">
		<!-- Animated Background Elements -->
		<div class="absolute inset-0 overflow-hidden">
			<div class="absolute top-1/4 left-1/4 w-64 h-64 bg-blue-500/20 rounded-full blur-3xl animate-pulse"></div>
			<div class="absolute bottom-1/4 right-1/4 w-80 h-80 bg-purple-500/20 rounded-full blur-3xl animate-pulse delay-1000"></div>
			<div class="absolute top-3/4 left-1/3 w-48 h-48 bg-pink-500/20 rounded-full blur-3xl animate-pulse delay-2000"></div>
		</div>
		
		<!-- Content Container -->
		<div class="relative z-10 px-20 py-12">
			<slot></slot>
		</div>
	</main>

	<!-- Enhanced Footer -->
	<Footer class="bg-white/5 backdrop-blur-md border-t border-white/20 mt-auto">
		<div class="w-full flex items-center justify-center py-6">
			<FooterCopyright 
				by="Created by Abdur Rehman Shaikh" 
				class="text-white/70 font-medium"
				href="https://abdurrahmanshkh.vercel.app/"
			/>
		</div>
	</Footer>
</div>

<style>
	:global(body) {
		font-family: 'Inter', sans-serif;
	}
	
	@keyframes float {
		0%, 100% { transform: translateY(0px); }
		50% { transform: translateY(-10px); }
	}
	
	:global(.animate-float) {
		animation: float 3s ease-in-out infinite;
	}
</style>