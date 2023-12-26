<script lang="ts">
	import Tile from './components/tile/tile.svelte';
	import SideMenu from './components/sideMenu.svelte';
	import type { tileProps } from './components/tile/tile.svelte';

	let tiles: {
		value: tileProps;
		component: typeof Tile;
	}[] = [];
	let isMenuOpen = false;

	const createNewTile = (
		title: string,
		lastPayday: number,
		cost: string,
		link: string,
		interval: any
	) => {
		let props: tileProps = {
			title: title,
			cost: cost,
			date: lastPayday,
			interval: interval,
			id: tiles.length,
			link: link
		};
		tiles = [
			...tiles,
			{
				value: props,
				component: Tile
			}
		];
	};
</script>

<header>
	<h1 class="title">Subs</h1>
	<h2 class="subtitle">Manage your subscribitions</h2>
</header>

<div class="container">
	<div class="tiles-container">
		{#each tiles as tile}
			<svelte:component this={tile.component} {...tile.value} />
		{/each}

		<div
			class="add-new"
			role="button"
			tabindex="0"
			on:keydown={() => {
				isMenuOpen = true;
			}}
			on:click={() => {
				isMenuOpen = true;
			}}
		/>
	</div>
	{#if isMenuOpen}
		<SideMenu
			{createNewTile}
			hideMenu={() => {
				isMenuOpen = false;
			}}
		/>
	{/if}
</div>

<style lang="sass">
    @import url('https://fonts.googleapis.com/css2?family=Inter:wght@200;300;400&display=swap')
    
    header
        margin-bottom: 5vh
        font-family: 'Inter', sans-serif

        h1
            font-size: 5vw
            font-weight: 300

        h2
            font-size: 1.5vw
            font-weight: 200
            color: #616060
            margin-top: 10px

        h1, h2
            margin-left: 10%
            margin-bottom: 0

        .title
            font-weight: 400
        .subtitle
            font-weight: 200

    .container
        display: flex
        flex-direction: row
        height: 75dvh
        font-family: 'Inter', sans-serif

        .tiles-container
            display: flex
            flex-direction: row
            flex-wrap: wrap
            gap: 20px
            width: calc(100vw - 270px)
            height: 260px
            float: left
            box-sizing: border-box
            padding: 0 0 0 10%

            .add-new
                position: relative
                height: 240px
                width: 180px
                cursor: pointer

                &:after
                    position: absolute
                    content: ''
                    display: block
                    width: 30px
                    height: 3px
                    border-radius: 3px
                    background-color: #676767
                    transform: translate(80px, 110.5px)

                
                &:before  
                    position: absolute
                    content: ''
                    display: block
                    transform: translate(93.5px, 97.5px)
                    width: 3px
                    height: 30px
                    border-radius: 3px
                    background-color: #676767
</style>
