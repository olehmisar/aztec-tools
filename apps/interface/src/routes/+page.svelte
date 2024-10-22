<script lang="ts">
  import { lib } from "$lib";
  import banner from "$lib/assets/banner.png";
  import { Ui } from "@repo/ui";
</script>

<Ui.GapContainer class="container">
  <section>
    <div class="prose mb-2">
      <h1>{lib.APP_NAME}</h1>
      <p>Useful utilities for Aztec developers.</p>
      <div class="text-center">
        <img src={banner} alt="banner" class="inline-block max-h-96" />
      </div>
    </div>
  </section>
</Ui.GapContainer>
