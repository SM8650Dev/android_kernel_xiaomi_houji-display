# Houji CN N3 panel identification

The matched OS3.0.307 CN tree selects the N3 42/0d/0a panel and declares
`mi,panel-id = <0x4e33 0x420d0a>`. MiCode only recognized the earlier
`0x00004e3300420d00` identifier, so boot logged the actual ID as unknown.

The existing N3 ESD rule accepts status bytes below the configured threshold
(0x08). Unknown panels use exact equality instead. The healthy panel returns
0x00, which was incorrectly rejected every five seconds, producing PANEL_DEAD
and a visible off/on recovery cycle. Add the known CN identifier to N3_PANEL_PA
so the existing family rule and other N3 behavior are selected. Preserve the
legacy ID and the other panel mappings; ESD checks remain enabled.

Evidence: ordinary userdebug boot with SELinux enforcing, source 6.1.25 kernel,
matched CN DTB/DTBO, and working TCM2/THP input. Runtime validation of the alias
is performed with the rebuilt display module and normal startup images.
