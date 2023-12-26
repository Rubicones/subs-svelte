<script context="module" lang="ts">
    export interface interval {
		n: number;
		interval: 'month' | 'week' | 'day';
	}
</script>

<script lang="ts">
	
	export let createNewTile: (
		title: string,
		lastPayday: number,
		cost: string,
		link: string,
		interval: any
	) => void;
	export let hideMenu: () => void;
	let datePicker: HTMLInputElement;
	let title: HTMLInputElement;
	let cost: HTMLInputElement;
	let link: HTMLInputElement;

	const interval: interval = { n: 1, interval: 'month' };

	const submitTileAdd = () => {
		createNewTile(
			title.value,
			new Date(datePicker.value).getTime(),
			cost.value,
			link.value,
			interval
		);
		hideMenu();
	};
</script>

<aside class="side-menu">
	<div class="cross" role="button" on:click={hideMenu} on:keydown={hideMenu} tabindex="0"></div>
	<div class="options-container">
		<div class="hiddenPart">
			<div class="label">Pick date of the last payment *</div>
			<input type="date" class="datepicker" bind:this={datePicker} />
			<div class="label">Title *</div>
			<input type="text" class="name-setter" bind:this={title} />
			<div class="label">Cost in USD *</div>
			<input type="number" class="name-setter" bind:this={cost} />
			<div class="label">Link</div>
			<input type="text" class="name-setter" bind:this={link} />
			<button class="add-submit-btn" on:click={submitTileAdd}> Add sub </button>
		</div>
	</div>
</aside>

<style lang="sass">
    .side-menu
        position: fixed
        bottom: 0
        right: 0
        width: 250px
        height: 100dvh
        &:after
            content: ""
            display: block
            position: absolute
            top: 0
            left: 0
            height: 100%
            width: 1px
            background-color: #c3deff82
            box-shadow: 0 1px 1px 0 #c3deff82


        .options-container    
            width: 100%
            height: 100%
            display: flex
            flex-direction: column
            justify-content: center
            align-items: center

            .label
                text-align: center
                font-size: 15px
                font-weight: 600
                margin-bottom: 10px

        .cross
            transform: rotate(45deg)
            position: absolute
            right: 0
            top: 0
            cursor: pointer
            &:after
                position: absolute
                content: ''
                display: block
                width: 30px
                height: 2px
                border-radius: 3px
                background-color: #676767
                transform: translate(-13.5px, 53.5px)

            
            &:before  
                position: absolute
                content: ''
                display: block
                transform: translate(-0.5px, 40.5px)
                width: 2px
                height: 30px
                border-radius: 3px
                background-color: #676767

        .hiddenPart
            // transform: translateY(100vh) 
            transition: transform 0.5s
            display: flex
            flex-direction: column
            align-items: center

            .datepicker, .name-setter
                border: 1px solid #c3deff
                height: 50px
                width: 85%
                border-radius: 5px
                text-align: center
                font-size: 15px
                margin-bottom: 20px
                box-sizing: border-box
                padding: 5px 5px 5px 5px

            .name-setter
                height: 35px

            .add-submit-btn
                height: 30px
                width: 55%  
                border: 1px solid #c3deff
                background-color: #b3d4fc
                border-radius: 5px
                color: white
                cursor: pointer
</style>
