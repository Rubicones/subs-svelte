<script context="module" lang="ts">
	export interface tileProps {
		title: string;
		cost: string;
		date: number;
		interval: interval;
		link: string;
		id: number;
	}
</script>

<script lang="ts">
	import { onMount } from 'svelte';
	import dayjs from 'dayjs';

	import type interval from '../sideMenu.svelte';

	import Circle from './circle.svelte';

	export let title: string;
	export let cost: string;
	export let date: number;
	export let interval: interval;
	export let link: string;
	export let id: number;

	let today = dayjs()
	let daysLeftLabel: HTMLElement;
	let daysLeft = -2;
	let angle = 0;
	let color = "green";

	$: {
		if (angle) {
			if (angle > 180) color = 'green';
			else if (angle > 90) color = '#f7cf3b';
			else color = 'red';
		}
		if (daysLeftLabel)
			daysLeftLabel.style.color = color
	}

	$: {
		switch (interval.interval) {
			case 'month':
				angle = (daysLeft * 360) / (today.daysInMonth() * interval.n);
				break;
			case 'week':
				angle = (daysLeft * 360) / (7 * interval.n);
				break;
			case 'day':
				angle = (daysLeft * 360) / interval.n;
				break;
		}
	}

	onMount(() => {
		let today = dayjs();
		let payday = dayjs(date);
		
		if (daysLeft === -2) {
			payday = payday.add(interval.n, interval.interval)
			daysLeft = payday.diff(today, 'day');
		}
	});
</script>

<div class="tileDropShadow">
	<div class="tile" data-id={id}>
		<div class="tile-inner">
			<div class="tile-front">
				<Circle propAngle={angle} color={color}/>
				<span class="days-left" bind:this={daysLeftLabel}>
					{daysLeft}
				</span>
				<span class="name">{title}</span>
			</div>
			<div class="tile-back">
				<div class="trash-btn">
					<!-- <FontAwesomeIcon
						icon={faTrash}
						size="2xs"
						onClick={() => {
							deleteTile(id);
						}}
					/> -->
				</div>

				<div class="edit-btn">
					<!-- <FontAwesomeIcon
						icon={faPencil}
						size="2xs"
						onClick={() => {
							openMenu(id);
						}}
					/> -->
				</div>
				<span class="cost-header">Cost</span>
				<span class="cost">{cost}$</span>
				<span class="date-header"> Next payment </span>
				<span class="date"> </span>
				<span class="date-header">Link</span>
				<span class="date">
					<a href={link} class="link">
						{link}
					</a>
				</span>
			</div>
		</div>
	</div>
</div>

<style lang="sass">
.tileDropShadow
    position: relative
    width: 180px
    height: 240px
    box-shadow: 0 0 30px 15px #e1e0e0
    background-color: #e1e0e0dc
    border-radius: 40px


    .tile
        position: absolute
        top: 0
        left: 0
        .tile-inner
            text-align: center
            transition: transform 0.8s
            transform-style: preserve-3d
            position: relative
            width: 180px
            height: 240px
            background-color: #faf8f8
            border: 1px solid #d5d4d4d7
            text-align: center
            perspective: 1000px
            border-radius: 40px



        .tile-front, .tile-back
            position: absolute
            width: 100%
            height: 100%
            -webkit-backface-visibility: hidden
            backface-visibility: hidden



        
        .tile-back
            transform: rotateY(180deg)
            display: flex
            flex-direction: column
            padding: 15%
            box-sizing: border-box
            justify-content: center
            font-size: 25px

            .cost-header, .date-header
                font-size: 15px
                font-weight: 600

            .cost
                color: green
                
            .cost, .date
                margin-bottom: 10px
                font-weight: 300
            
            .edit-btn
                height: 20px
                width: 20px
                position: absolute
                top: 20px
                right: 20px
            
            .trash-btn
                height: 20px
                width: 20px
                position: absolute
                top: 20px
                left: 20px

            .link
                text-decoration: none


        .name
            display: flex
            justify-content: center
            align-items: center
            height: 50px
            font-size: 25px
            transform: translateY(-55px)
            letter-spacing: 1px
            padding: 0 10px 0 10px

        .days-left
            display: block
            transform: translateY(-115px)
            font-size: 40px
            font-family: 'Roboto', sans-serif
            font-weight: 500
            color: red


        &:hover
            .tile-inner
                transform: rotateY(180deg)



</style>
