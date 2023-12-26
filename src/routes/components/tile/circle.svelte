<script lang="ts">
	export let propAngle: number;
    export let color: string;
	let strokeOffset = 0;
	let strokeDasharray = 0;
	let angle = propAngle;
	let circle: SVGCircleElement;

	let CIRCUMFERENCE: number = 2 * Math.PI * 72;

	$: {
		if (circle) {
			circle.style.stroke = color
			angle = propAngle;
		}
	}

	$: {
		strokeOffset = (1 / 4) * CIRCUMFERENCE;
		strokeDasharray = (angle / 360) * CIRCUMFERENCE;
	}

	$: {
		if (circle) {
			circle.style.strokeDasharray = `${strokeDasharray} ${CIRCUMFERENCE - strokeDasharray}`;
			circle.style.strokeDashoffset = `${strokeOffset}`;
		}
	}
</script>

<svg class="svg-cont">
	<circle
		bind:this={circle}
		class="inner-circle"
		stroke-width="8"
		fill="transparent"
		r="72"
		cx="90"
		cy="90"
	/>
</svg>

<style lang="sass">
    
.svg-cont
    width: 100%
    aspect-ratio: 1/1

    .inner-circle 
        transition: 0.9s stroke-dashoffset, 0.9s stroke-dasharray
        transform-origin: 50% 50%
        stroke-linecap: round
        stroke: white

</style>
