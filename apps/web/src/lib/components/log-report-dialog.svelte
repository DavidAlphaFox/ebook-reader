<script lang="ts">
  import DialogTemplate from '$lib/components/dialog-template.svelte';
  import Ripple from '$lib/components/ripple.svelte';
  import { buttonClasses } from '$lib/css-classes';
  import { logger } from '$lib/data/logger';
  import {
    theme$,
    fontSize$,
    fontFamilyGroupOne$,
    fontFamilyGroupTwo$,
    firstDimensionMargin$,
    secondDimensionMaxValue$,
    viewMode$,
    writingMode$,
    autoBookmark$,
    hideSpoilerImage$,
    hideFurigana$,
    furiganaStyle$,
    avoidPageBreak$,
    pageColumns$,
    autoPositionOnResize$,
    requestPersistentStorage$,
    multiplier$
  } from '$lib/data/store';

  export let title = 'Error';

  export let message: string;

  const encodedLog = encodeURIComponent(
    JSON.stringify(
      {
        userAgent: navigator.userAgent,
        viewport: {
          visualViewport: !!window.visualViewport,
          width: window.visualViewport?.width ?? window.innerWidth,
          height: window.visualViewport?.height ?? window.innerHeight
        },
        settings: {
          theme: theme$.getValue(),
          fontSize: fontSize$.getValue(),
          fontFamilyGroupOne: fontFamilyGroupOne$.getValue(),
          fontFamilyGroupTwo: fontFamilyGroupTwo$.getValue(),
          firstDimensionMargin: firstDimensionMargin$.getValue(),
          secondDimensionMaxValue: secondDimensionMaxValue$.getValue(),
          viewMode: viewMode$.getValue(),
          writingMode: writingMode$.getValue(),
          autoBookmark: autoBookmark$.getValue(),
          hideSpoilerImage: hideSpoilerImage$.getValue(),
          hideFurigana: hideFurigana$.getValue(),
          furiganaStyle: furiganaStyle$.getValue(),
          avoidPageBreak: avoidPageBreak$.getValue(),
          pageColumns: pageColumns$.getValue(),
          autoPositionOnResize: autoPositionOnResize$.getValue(),
          requestPersistentStorage: requestPersistentStorage$.getValue(),
          multiplier: multiplier$.getValue()
        },
        log: logger.history
      },
      null,
      2
    )
  );
  const downloadableLog = `data:text/json;charset=utf-8,${encodedLog}`;
</script>

<DialogTemplate>
  <svelte:fragment slot="header">{title}</svelte:fragment>
  <svelte:fragment slot="content">
    <p>{message}</p>
  </svelte:fragment>
  <svelte:fragment slot="footer">
    <a class={buttonClasses} href="https://github.com/ttu-ttu/ebook-reader" target="_blank">
      Open Repository
      <Ripple />
    </a>
    <a class={buttonClasses} href={downloadableLog} download="log.json">
      Download Report
      <Ripple />
    </a>
  </svelte:fragment>
</DialogTemplate>
