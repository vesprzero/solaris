<script>
	import { Warp, LiquidMetal } from '@devmischief/shaders-svelte';
	let { shaderType } = $props();
	let shaders = $derived(shaderType);
</script>

<div class="shaders">
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
			fit="contain"
		/>
	{/if}
	{#if shaders == 'box'}
		<LiquidMetal
			width={'100vw'}
			height={'100vh'}
			colorBack="#000000"
			colorTint="#6b759e"
			shape="none"
			repetition={1}
			softness={1}
			shiftRed={0}
			shiftBlue={0}
			distortion={0}
			contour={1}
			angle={162}
			scale={1.06}
			rotation={0}
			offsetX={0}
			offsetY={0}
			speed={0.3}
			fit="contain"
		/>
	{/if}
	{#if shaders == 'meatball'}
		<LiquidMetal
			width={'100vw'}
			height={'100vh'}
			colorBack="#000000"
			colorTint="#6b759e"
			shape="metaballs"
			repetition={3}
			softness={1}
			shiftRed={0.26}
			shiftBlue={0.28}
			distortion={0}
			contour={0.31}
			angle={0}
			scale={0.6}
			rotation={34}
			offsetX={0.08}
			offsetY={0}
			speed={1}
			fit="contain"
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
	}
</style>
