<script>
	import { dev } from '$app/environment';
	import { onMount } from 'svelte';
	import { isMobile, isPWA, showAd } from '$lib/store/app-stores';
	import { randomNumber } from '$lib/helpers/gacha/itemdrop-base';

	export let type = '';
	export let head = false;
	export let size = 'wide';

	const show = randomNumber(1, 2) === 1;

	const adSize = {
		square: 6247038092,
		wide: 6827309798
	};

	onMount(() => {
		if (type !== 'banner' || head || (type === 'banner' && !show)) return;
		try {
			// Google Ads
			(window.adsbygoogle = window.adsbygoogle || []).push({});
			// End Google Ads
		} catch (e) {
			console.error(e);
		}
	});

	let addcashLoaded = false;
	const loadAdcash = () => {
		if (addcashLoaded) return;
		addcashLoaded = true;

		const sc = document.createElement('script');
		sc.setAttribute('id', 'aclib');
		sc.setAttribute('type', 'text/javascript');
		sc.src = '//acscdn.com/script/aclib.js';
		document.head.appendChild(sc);
		sc.addEventListener('load', () => window.aclib.runAutoTag({ zoneId: 'v1xd6wvvpe' }));
	};

	const loadHeaderAds = () => {
		loadAdcash();
		// Ezoic
		window.ezstandalone = window.ezstandalone || {};
		window.ezstandalone.cmd = window.ezstandalone.cmd || [];
		window.ezstandalone.cmd.push(function () {
			window.ezstandalone.showAds();
		});
		// End Ezoic
	};

	$: if ($showAd && !dev && head && !type && !($isPWA && $isMobile)) loadHeaderAds();
</script>

{#if dev && type === 'banner' && show}
	<div class="row">
		<div class="banner" />
	</div>
{/if}

{#if $showAd && !dev}
	{#if type === 'banner' && show}
		<ins
			data-ad-slot={adSize[size]}
			class="adsbygoogle"
			style="display:block"
			data-ad-client="ca-pub-1874822310102113"
			data-ad-format="auto"
			data-full-width-responsive="true"
		/>
	{/if}

	{#if head && !type}
		<!-- Google Ad -->
		<script
			async
			src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?client=ca-pub-1874822310102113"
			crossorigin="anonymous"></script>
		<!-- Google Ad -->

		<!-- Ezoic -->
		<script src="https://the.gatekeeperconsent.com/cmp.min.js" data-cfasync="false"></script>
		<script src="https://the.gatekeeperconsent.com/ccpa/v2/standalone.js" async></script>
		<script async src="//www.ezojs.com/ezoic/sa.min.js"></script>
		<!-- Ezoic -->

		<!-- Autotag -->
		<!-- don't show autotag if PWA -->
		{#if !($isPWA && $isMobile)}
			<!-- <script
				data-cfasync="false"
				type="text/javascript"
				data-adel="atag"
				src="//acacdn.com/script/atg.js"
				czid="v1xd6wvvpe"></script> -->
		{/if}
		<!-- Autotag -->

		<!-- Desktop Only -->
		<!-- {#if !$isMobile} -->
		<!-- PopAds -->
		<!-- {#if randomNumber(1, 2) === 1}
				<script
					type="text/javascript"
					src="//pl17416355.profitablecpmgate.com/3e/70/98/3e7098724a8a6321d737e1bd39d9ffed.js"></script>
			{/if} -->
		<!-- PopAds -->

		<!-- In-Push Page -->
		<!-- <script
				type="text/javascript"
				src="//pl17419889.profitablecpmgate.com/fb/35/45/fb3545e433a3f40f53c83f80f24037ec.js"></script> -->
		<!-- In-Push Page -->
		<!-- {/if} -->
	{/if}
{/if}

<style>
	ins::-webkit-scrollbar,
	.row::-webkit-scrollbar {
		display: none;
	}

	.row {
		width: 100%;
		overflow: auto;
		display: flex;
	}

	ins {
		margin: auto;
		overflow: auto;
		max-width: 100%;
	}

	:global(ins.adsbygoogle[data-ad-status='unfilled']) {
		display: none !important;
	}

	.banner {
		margin: auto;
		width: 100%;
		max-width: 728px;
		height: 280px;
		aspect-ratio: 728/90;
		background-image: linear-gradient(40deg, #00aaff, pink 50%);
		background-size: cover;
	}
</style>
