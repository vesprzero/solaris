<script>
	import gsap from 'gsap';
	let now = $state(new Date());

	const time = $derived(
		now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit', second: '2-digit' })
	);
	const date = $derived(
		now.toLocaleDateString([], { weekday: 'long', month: 'long', day: 'numeric' })
	);
	$effect(() => {
		const tick = setInterval(() => (now = new Date()), 1000);
		return () => clearInterval(tick);
	});
	$effect(() => {
		gsap.from('.clockDisplay > *', {
			y: 24,
			opacity: 0,
			duration: 0.9,
			stagger: 0.12,
			ease: 'expo.out'
		});
	});
</script>

<div class="displayContainer">
	<div class="clockDisplay">
		<p class="clock">{time}</p>
		<p class="date">{date}</p>
	</div>
</div>

<style>
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
		pointer-events: none;
	}
	.clockDisplay {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		gap: 6px;
		text-shadow: 2px 2px 14px rgb(0, 0, 0);
	}
	.clock {
		margin: 0;
		color: #ffffff;
		font-size: 76px;
		line-height: 1;
		letter-spacing: 3px;
		font-variant-numeric: tabular-nums;
	}
	.date {
		margin: 0;
		color: rgba(255, 255, 255, 0.72);
		font-family: 'Halvers';
		font-weight: 300;
		font-size: 15px;
		letter-spacing: 5px;
		text-transform: uppercase;
		text-shadow: 2px 2px 14px rgb(0, 0, 0);
	}
</style>
