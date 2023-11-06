<script>
	import { fly } from 'svelte/transition';
	import { t } from 'svelte-i18n';
	import { customData, editID, editorMode as editMode } from '$lib/store/app-stores';
	import SvgIcon from '$lib/components/SVGIcon.svelte';
	import { playSfx } from '$lib/helpers/audio/audio';

	export let bannerName = '';
	export let character = '';
	export let charTitle = '';
	export let vision = '';
	export let editorMode = false;
	export let onBannerEdit = false;

	const highlightBannerName = (bannerName, vision) => {
		const name = bannerName || 'Banner Name';
		const splited = name.split(' ');
		return `<span class="${vision}-flat">${splited[0]}</span> ${splited.slice(1).join(' ')}`;
	};

	const editBanner = () => {
		playSfx();
		editID.set($customData.itemID);
		editMode.set(true);
	};
</script>

<div class="frame-content" class:editorMode class:onBannerEdit>
	<div class="top bg-{vision}">
		{$t('wish.banner.character-event')}
	</div>
	<h1 class="card-stroke" in:fly={{ x: 15, duration: 700 }}>
		<div class="wrap">
			{@html highlightBannerName(bannerName, vision)}
		</div>
	</h1>

	{#if $customData.status === 'owned' && !editorMode}
		<button class="edit" on:click={editBanner}>
			<i class="gi-pen" /> <span>Edit</span>
		</button>
	{/if}

	<div class="info">
		<div class="content">
			<div class="set card-stroke">
				{$t('wish.banner.probIncreased')}
			</div>
			<div class="desc bg-{vision}" style="opacity: 90%;">
				<div class="icon">
					<i class="gi-primo-star" />
				</div>
				<div class="text">
					{$t('wish.banner.wishDescription')}
				</div>
			</div>
			<div class="note card-stroke">
				{$t('wish.banner.eventNote')}
				{$t('wish.banner.viewDetails')}
			</div>
		</div>
	</div>

	<div class="character">
		<div class="char-name">
			<span>
				{character || 'Character Name'}
			</span>
			<span class="up">{$t('wish.banner.up')}</span>

			<div class="textbg">
				<div class="icon-vision {vision} filter-drop">
					<SvgIcon name={vision} />
				</div>
				<div class="stars">
					{#each Array(5) as _}
						<i class="gi-star" />
					{/each}
				</div>
			</div>
		</div>

		<div class="char-title">
			{charTitle || 'Character Title'}
		</div>
	</div>
</div>

<style>
	.frame-content {
		width: 100%;
		aspect-ratio: 1080/533;
		position: absolute;
		color: #565654;
		display: block;
		font-size: calc(1.8 / 100 * var(--content-width));
		line-height: 130%;
		bottom: 0;
		left: 0;
		z-index: +10;
	}
	.frame-content.onBannerEdit {
		pointer-events: none;
		opacity: 0.5;
	}

	h1 :global(span) {
		display: block;
	}

	h1,
	.frame-content > div {
		text-align: left;
		position: absolute;
	}
	h1 {
		bottom: 67%;
		left: 0;
		margin: 0 4%;
		line-height: 125%;
		font-size: calc(4.5 / 100 * var(--content-width));
		height: calc(0.23 * var(--content-height));
		display: flex;
		align-items: center;
	}

	:global(.zh-CN) h1 {
		font-size: calc(7 / 100 * var(--content-width));
	}

	:global(.ja-JP) h1 {
		max-width: 45%;
		font-size: calc(6 / 100 * var(--content-width));
		line-height: 100%;
	}

	.editorMode h1 {
		bottom: 70%;
	}

	.edit {
		position: absolute;
		top: 0;
		right: 0;
		color: rgba(255, 255, 255, 0.8);
		background-color: rgba(0, 0, 0, 0.5);
		padding: 1.5% 2.5%;
		display: flex;
		align-items: center;
		line-height: 0;
		border-radius: 2%;
		font-size: calc(1.7 / 100 * var(--content-width));
		opacity: 0;
		transition: all 0.25s;
	}
	.edit i {
		transform: translateX(-50%);
	}
	.frame-content:hover .edit {
		opacity: 1;
	}
	.edit:active {
		transform: scale(0.9);
	}
	.edit:hover {
		background-color: rgba(0, 0, 0, 0.8);
	}

	.top {
		color: #fff;
		padding: 0.3% 1.4%;
		border-bottom-left-radius: 2rem;
		border-top-left-radius: 2rem;
		border-bottom-right-radius: 4rem;
		top: 0;
		left: 0;
		transform: translate(-3%, -15%);
	}

	.info {
		left: 0;
		top: 40%;
		width: 40%;
		height: 45%;
		display: block;
		padding-left: 4%;
	}

	.editorMode .info {
		top: 32.5%;
	}

	.content {
		position: relative;
	}

	.info .content::after {
		content: '';
		display: block;
		width: calc(0.55 / 100 * var(--content-width));
		height: 100%;
		background-color: #565654;
		position: absolute;
		left: calc(-3.045 / 100 * var(--content-width));
		top: 0;
	}

	.set {
		font-size: calc(2.4 / 100 * var(--content-width));
	}

	.desc {
		color: #fff;
		min-height: calc(9 / 100 * var(--content-height));
		display: flex;
		align-items: center;
		margin: calc(0.7 / 100 * var(--content-width)) 0;
	}

	.icon {
		display: flex;
		justify-content: center;
		align-items: center;
		padding: calc(1 / 100 * var(--content-width));
		font-size: calc(1.1 / 100 * var(--content-width));
	}

	.desc .text {
		width: calc(32.5 / 100 * var(--content-width));
		padding: calc(0.3 / 100 * var(--content-width));
	}

	.character {
		--text-width: calc(30 / 100 * var(--content-width));
		width: calc(30 / 100 * var(--content-width));
		left: 54%;
		bottom: 10%;
	}

	.character .char-name {
		color: #fff;
		display: inline-block;
		-webkit-text-stroke: 0.02rem #565654;
		text-shadow: 0 0 0.15rem #d2c69c;
		line-height: 100%;
		position: relative;
		font-size: calc(11 / 100 * var(--text-width));
		white-space: nowrap;
	}

	:global(.zh-CN) .character .char-name {
		font-size: calc(17 / 100 * var(--text-width));
	}
	:global(.ja-JP) .character .char-name {
		font-size: calc(15 / 100 * var(--text-width));
	}

	.char-name span {
		filter: drop-shadow(0 0.3rem 0.5rem #000);
	}

	.char-name .up {
		color: #fff664;
		-webkit-text-stroke: 0.05rem #e7a12e;
		font-size: calc(2 / 100 * var(--content-width));
		filter: drop-shadow(0 0.3rem 0.5rem #fff);
		position: absolute;
		top: 0;
		right: 0;
		transform: translateX(100%) translateY(-50%);
		text-transform: uppercase;
		text-shadow: 0 0 0.4rem #f79c09;
	}

	.character .char-title {
		color: #cfbc99;
		background-color: #39425d;
		padding: 1% 2%;
		white-space: nowrap;
		width: fit-content;
	}

	.textbg {
		background-color: rgba(0, 0, 0, 0.55);
		width: calc(100% + (0.15 * var(--content-height)));
		transform: translateX(calc(7.5 / 100 * var(--content-height) * -1));
		height: calc(9 / 100 * var(--content-height));
		margin-top: calc(-3 / 100 * var(--content-height));
		filter: drop-shadow(0 0 calc(0.02 * var(--content-height)) #000);
		position: relative;
		z-index: -2;
	}

	.stars {
		position: absolute;
		left: calc(7.5 / 100 * var(--content-height));
		bottom: -10%;
	}

	.gi-star {
		color: #f7cf33;
		display: inline-block;
		font-size: calc(3.5 / 100 * var(--content-height));
	}

	.icon-vision {
		width: calc(9 / 100 * var(--content-height));
		transform: translate(-45%, -40%);
		position: absolute;
		left: 0;
		top: 0;
	}
	.icon-vision :global(svg) {
		width: 100%;
		height: 100%;
	}
</style>