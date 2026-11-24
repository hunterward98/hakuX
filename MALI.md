# hakuX Mali build

A build of this fork for Arm Mali GPUs, made for Halo 2 on the Anbernic RG405M (Unisoc T618, Mali-G52 MP2).

- `./gradlew assembleMali`: the release build installed beside the official app (com.rfandango.haku_x.mali,
  "hakuX Mali"), with a custom Vulkan driver packed from `android/app/src/mali/assets/drivers/` (malisx2,
  bmdhacks/malisx2, MIT: put the release zip there before building) and put in use once on a Mali device
  (`GpuDriverHelper.installBundledOnce`).
- `vcompact` quirk (vk/quirks.c, pb/pbdraw.c): sparse indexed draws are compacted for Mali drivers that aren't
  Arm's: malisx2 on Bifrost shades every vertex of a draw's index range (no IDVS). Halo 2's Cairo Station intro:
  -42% vertex shader threads, -10% GPU time, measured with Mali hardware counters.
- `HAKUX_HLE_FP16=1` (psh.c): combiner temporaries in mediump. No measurable gain on malisx2; off.
- Custom drivers load on Mali (kbase) since ce45c4cf (after 1.3.0).

The thermal fix that matters most on the RG405M is outside the app (Magisk module, root): see
https://github.com/hunterward98/hakuX (this fork) and the RG405M notes.
