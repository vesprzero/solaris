<script>
	import { Warp, LiquidMetal, GemSmoke } from '@devmischief/shaders-svelte';
	let { shaderType } = $props();
	let shaders = $derived(shaderType);
	const presets = ['waves', 'box', 'meatball', 'coffee'];
	let customBG = $derived(!presets.includes(shaderType));
</script>

<div class="shaders" style:background-image={customBG ? `url("${shaders}")` : 'none'}>
	{#if shaders == 'waves'}
		<Warp
			width={'100vw'}
			height={'100vh'}
			colors={['#0a0a0f', '#151b2b', '#252b3d']}
			shape="checks"
			shapeScale={0.3}
			proportion={0.54}
			softness={1}
			distortion={0}
			swirl={0.84}
			swirlIterations={5}
			scale={1.75}
			rotation={0}
			offsetX={0}
			offsetY={0}
			speed={3}
			fit="cover"
		/>
	{:else if shaders == 'box'}
		<LiquidMetal
			width={'100vw'}
			height={'100vh'}
			colorBack="#9ea1a6"
			colorTint="#fafafa"
			shape="metaballs"
			repetition={1}
			softness={0.6}
			shiftRed={0}
			shiftBlue={0}
			distortion={0.2}
			contour={0.6}
			angle={0}
			scale={0.5}
			rotation={0}
			offsetX={0}
			offsetY={0}
			speed={0.5}
			fit="cover"
		/>
	{:else if shaders == 'meatball'}
		<GemSmoke
			width={'100vw'}
			height={'100vh'}
			colors={['#000000', '#1a1a1a', '#444444', '#888888', '#cccccc']}
			colorBack="#050505"
			colorInner="#1a1a1a"
			shape="metaballs"
			innerDistortion={0.6}
			outerDistortion={1}
			outerGlow={1}
			innerGlow={0.9}
			offset={-0.2}
			angle={90}
			size={0.3}
			scale={0.5}
			rotation={112}
			offsetX={0}
			offsetY={0}
			speed={0.2}
			fit="cover"
		/>
	{:else if shaders == 'coffee'}
		<Warp
			width={'100vw'}
			height={'100vh'}
			colors={['#1A120D', '#C66B2D', '#F5C77A']}
			shape="stripes"
			shapeScale={0.1}
			proportion={0.49}
			softness={1}
			distortion={0.22}
			swirl={0.82}
			swirlIterations={15}
			scale={1.3}
			rotation={0}
			offsetX={0.08}
			offsetY={-0.06}
			speed={1}
			fit="cover"
		/>
	{/if}
</div>

<style>
	.shaders {
		position: fixed;
		top: 0;
		left: 0;
		width: 100vw;
		height: 100vh;
		z-index: -1;
		background-size: cover;
		background-repeat: no-repeat;
		background-position: center center;
	}
</style>
