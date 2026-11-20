# LineageOS 23.2 compatibility audit — SDM710 common

Status: **source-level audit only**. This branch is forked from `lineage-22.2`; it is **not yet build-compatible** with Android 16.

## Verified integration points

- `BoardConfigCommon.mk` sets `TARGET_KERNEL_SOURCE := kernel/xiaomi/sdm710` and uses `sdm670-perf_defconfig xiaomi/sdm710-common.config`. Preserve this vendor-derived Linux 4.9 path.
- `BoardConfigCommon.mk` includes `device/qcom/sepolicy_vndr/SEPolicy.mk`, `hardware/qcom-caf/common/vendor_framework_compatibility_matrix.xml` and `hardware/xiaomi/vintf/xiaomi_framework_compatibility_matrix.xml`.
- `sdm710.mk` inherits `hardware/qcom-caf/common/common.mk`.
- `Android.bp` imports `hardware/qcom-caf/common/libqti-perfd-client` and `hardware/xiaomi`.
- Legacy Qualcomm and Xiaomi vendor modules appear throughout `sdm710.mk`; their Android 16 module availability requires a real source checkout.

## Observed blocker

The base LineageOS `lineage-23.2` manifest does not declare the device-specific Qualcomm CAF source trees above. These cannot be assumed present after `repo sync`. Several old hardware paths also require verification against currently maintained upstream repositories. **Do not invent repository names or point them at incompatible branches to suppress this error.**

## Proposed next steps

1. Resolve the Qualcomm CAF common, SDM845 and vendor sepolicy source origins and SHAs.
2. Use a read-only source preflight and `repo list`/module lookup to distinguish missing checkout paths from actual Android 16 errors.
3. Extract proprietary vendor inputs from authorized stock firmware.
4. Compare HAL/Soong/VINTF/SELinux interfaces against `lineage-23.2`.
5. Make minimal fixes backed by actual `m`/Soong output; no blind removals of compatibility flags.

References: upstream `lineage-22.2` `BoardConfigCommon.mk`, `sdm710.mk`, `Android.bp`; official `LineageOS/android` `lineage-23.2` `default.xml`.
