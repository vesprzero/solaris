<script>
	import gsap from 'gsap';
	import Bg from '$lib/utils/bg.svelte';
	import logo from '$lib/assets/logo.png';
	import pencil from '$lib/assets/icons/pencil.png';
	import close from '$lib/assets/icons/close.png';
	import boxPrev from '$lib/assets/previews/boxPreview.png';
	import meatPrev from '$lib/assets/previews/meatballPreview.png';
	import smokePrev from '$lib/assets/previews/smokePreview.png';
	let editLauncher = $state(pencil);
	let bgSelect = $state(false);
	let bg = $state('waves');
	let fullscreen = $state(false);
	let now = $state(new Date());
	const time = $derived(
		now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' })
	);

	$effect(() => {
		const tick = setInterval(() => (now = new Date()), 1000);
		return () => clearInterval(tick);
	});
	const bgOptions = [
		{ key: 'waves', preview: smokePrev },
		{ key: 'box', preview: boxPrev },
		{ key: 'meatball', preview: meatPrev }
	];
	function launchEditor() {
		bgSelect = !bgSelect;
		if (editLauncher == pencil) {
			editLauncher = close;
			gsap.to('.edit', {
				background: 'rgba(0, 0, 0, 0.4)',
				backdropFilter: 'blur(12px)',
				width: 'calc(100% - 20px)',
				ease: 'expo.out',
				duration: 0.3,
				onComplete: () => {
					gsap.to('.edit', {
						height: 200,
						ease: 'expo.out',
						duration: 0.3
					});
				}
			});
		} else {
			editLauncher = pencil;
			gsap.to('.edit', {
				height: 50,
				ease: 'expo.out',
				duration: 0.3,
				onComplete: () => {
					gsap.to('.edit', {
						background: 'rgba(255, 255, 255, 0)',
						backdropFilter: 'blur(0px)',
						width: 50,
						ease: 'expo.out',
						duration: 0.5
					});
				}
			});
		}
	}

	function toggleFullscreen() {
		if (document.fullscreenElement) {
			document.exitFullscreen();
		} else {
			document.documentElement.requestFullscreen();
		}
	}
</script>

<svelte:document onfullscreenchange={() => (fullscreen = !!document.fullscreenElement)} />

<Bg shaderType={bg} />
<div class="logo" onclick={toggleFullscreen} class:hidden={fullscreen}>
	<img src={logo} alt="Logo" />
</div>
<div class="edit" class:hidden={fullscreen}>
	<div class="editButtonDiv" onclick={launchEditor}>
		<img class="editButton" src={editLauncher} alt="Edit" />
	</div>
	{#if bgSelect}
		<div class="bgSelect">
			{#each bgOptions as option}
				<button class="bgOption" class:active={bg == option.key} onclick={() => (bg = option.key)}>
					<img src={option.preview} alt={option.key} />
				</button>
			{/each}
		</div>
	{/if}
</div>
<div class="displayContainer">
	<div class="clockDisplay">
		<p class="clock">{time}</p>
	</div>
</div>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		-webkit-user-select: none; /* Safari */
		-moz-user-select: none; /* Firefox */
		-ms-user-select: none; /* Internet Explorer/Edge */
		user-select: none; /* Standard syntax */
		padding-left: 10px;
	}
	.logo {
		height: 60px;
		width: 60px;
		display: flex;
		justify-content: center;
		align-items: center;
		transition-duration: 0.2s;
		margin: 10px;
	}
	.logo img {
		height: 60px;
		width: auto;
		margin: 0px;
		padding: 0px;
	}
	.logo:hover {
		transform: rotate(120deg);
	}
	.edit {
		height: 50px;
		width: 50px;
		position: fixed;
		bottom: 10px;
		right: 10px;
		border-radius: 8px;
		backdrop-filter: blur(0px);
		z-index: 10;
	}
	.editButtonDiv {
		height: 40px;
		width: 40px;
		position: fixed;
		bottom: 10px;
		right: 10px;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.editButtonDiv .editButton {
		height: 30px;
		width: 30px;
		transition-duration: 0.3s;
	}
	.editButtonDiv:hover .editButton {
		filter: invert(100%);
	}
	.editButtonDiv:active .editButton {
		transform: scale(0.5);
	}
	.hidden {
		display: none;
	}
	.bgSelect {
		height: 100%;
		width: 100%;
		display: flex;
		align-items: center;
		gap: 12px;
		padding: 0 20px;
	}
	.bgOption {
		width: 200px;
		height: 120px;
		padding: 0;
		border: 2px solid rgba(255, 255, 255, 0.3);
		border-radius: 8px;
		background: rgba(255, 255, 255, 0.1);
		overflow: hidden;
		cursor: pointer;
		transition:
			border-color 0.2s,
			transform 0.2s;
		animation: fadeIn 0.3s ease 0.35s both;
	}
	.bgOption img {
		width: 100%;
		height: 100%;
		object-fit: cover;
		display: block;
	}
	.bgOption:hover {
		border-color: rgba(255, 255, 255, 0.6);
		transform: scale(1.03);
	}
	.bgOption:active {
		transform: scale(0.97);
	}
	.bgOption.active {
		border-color: #ffffff;
	}
	.displayContainer {
		position: absolute;
		height: 100vh;
		width: 100vw;
		margin: 0;
		top: 0;
		left: 0;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.clockDisplay {
		height: 100px;
		width: 250px;
		background-color: rgba(255, 255, 255, 0.192);
		border-radius: 8px;
		backdrop-filter: blur(20px);
		display: flex;
		align-items: center;
		justify-content: center;
	}
	.clock {
		margin: 0;
		color: #ffffff;
		font-size: 40px;
		font-variant-numeric: tabular-nums;
	}
	@keyframes fadeIn {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
</style>
