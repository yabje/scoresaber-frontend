<script>
   import { onMount } from 'svelte';

   import { dev } from '$app/env';

   import { userData } from '$lib/stores/global-store';

   import permissions from '$lib/utils/permissions';
   $: showAd = true;
   onMount(() => {
      try {
         window.adsbygoogle.push({});
      } catch (ex) {
         console.error(ex);
      }
      if (!window.canRunAds) {
         showAd = false;
      }
      userData.subscribe((u) => {
         if (u) {
            if (
               permissions.checkPermissionNumber(u.permissions, permissions.security.SUPPORTER) ||
               permissions.checkPermissionNumber(u.permissions, permissions.groups.ALL_STAFF)
            ) {
               showAd = false;
            }
         }
      });
   });
</script>

{#if showAd}
   <div class="ad">
      <ins
         class="adsbygoogle"
         style="display:inline-block;width:100%;height:208px"
         data-ad-client="ca-pub-9829527932347716"
         data-ad-slot="3006730349"
         data-adtest={dev ? 'on' : 'off'}
      >
         <div class="ad-subtext">
            Ads help keep ScoreSaber alive, if you'd like to remove ads, consider supporting us on <a
               target="_blank"
               rel="external"
               href="https://patreon.com/scoresaber"
            >
               Patreon</a
            > ❤️
         </div></ins
      >
   </div>
{/if}

<style>
   .ad-subtext {
      color: var(--textColor);
      text-decoration: none !important;
   }
   @media only screen and (max-width: 769px) {
      .ad {
         display: none;
      }
   }
</style>
