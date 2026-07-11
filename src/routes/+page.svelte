<script>
	import '$lib/assets/style/main.css';
	import gsap from 'gsap';
	import Bg from '$lib/utils/bg.svelte';
	import Clock from '$lib/utils/clock.svelte';
	import logo from '$lib/assets/logoWhite.png';
	import pencil from '$lib/assets/icons/pencil.png';
	import close from '$lib/assets/icons/close.png';
	import boxPrev from '$lib/assets/previews/boxPreview.png';
	import meatPrev from '$lib/assets/previews/meatballPreview.png';
	import smokePrev from '$lib/assets/previews/smokePreview.png';
	import uploadImg from '$lib/assets/icons/upload.png';
	import { saveSetting, loadSetting } from '$lib/utils/localspace.js';
	import { onMount } from 'svelte';
	let editLauncher = $state(pencil);
	let bgSelect = $state(false);
	let bg = $state('waves');
	let fullscreen = $state(false);
	let loaded = $state(false);
	const maxSize = 15 * 1024 * 1024;

	const bgOptions = [
		{ key: 'waves', preview: smokePrev },
		{ key: 'box', preview: boxPrev },
		{ key: 'meatball', preview: meatPrev }
	];

	onMount(async () => {
		bg = await loadSetting('background', 'waves');
		loaded = true;
	});
	$effect(() => {
		if (loaded) saveSetting('background', bg);
	});
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
	/**
	 * @param {File} file
	 * @returns {Promise<string>}
	 */
	function readFileAsDataURL(file) {
		return new Promise((resolve, reject) => {
			const reader = new FileReader();
			reader.onload = () => resolve(/** @type {string} */ (reader.result));
			reader.onerror = () => reject(reader.error);
			reader.readAsDataURL(file);
		});
	}

	/** @param {Event} event */
	async function handleUpload(event) {
		const input = /** @type {HTMLInputElement} */ (event.target);
		const file = input.files?.[0];
		input.value = '';
		if (!file) return;
		if (file.size > maxSize) {
			alert('That image is too large (max 15MB)');
			return;
		}
		bg = await readFileAsDataURL(file);
	}
</script>

<svelte:document onfullscreenchange={() => (fullscreen = !!document.fullscreenElement)} />

<Bg shaderType={bg} />
<Clock />
<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
<div class="logo" onclick={toggleFullscreen} class:hidden={fullscreen}>
	<img src={logo} alt="Logo" />
</div>
<!-- svelte-ignore a11y_click_events_have_key_events -->
<!-- svelte-ignore a11y_no_static_element_interactions -->
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
			<label class="bgOption">
				<img class="uploadImg" src={uploadImg} alt="upload" />
				<input type="file" accept="image/*" onchange={handleUpload} hidden />
			</label>
		</div>
	{/if}
</div>

<style>
</style>
