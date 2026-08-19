# Submitting the series

The series lives on branch `tb-2026.04-dev` in
[BeatLink/U-Boot](https://github.com/BeatLink/U-Boot), as the 13 commits
from `pinctrl: sunxi: add the A64's s_pwm function` through
`sunxi: pinephone: enter USB mass storage when volume up is held`:

    git log --oneline 961b7ecc11..a2b91909cc

The commits above that range are Tow-Boot's own fixes and are not part of
the submission. Neither is the last commit *in* it — the volume-up UMS
hook is Tow-Boot-only — which leaves patches 1-12 to send.

Cherry-pick the range onto current `next` and rebase before sending. The
last such rebase (2026-08-18, onto master of 2026-08-10) needed only
mechanical context fixes in patches 4, 8 and 12, and the result was
build-tested, checkpatch-clean, and byte-equivalent in content to the
build verified on the phone. `0000-cover-letter.patch` holds the reviewed
cover letter; regenerate its diffstat after rebasing.

## One-time setup

    git config sendemail.smtpServer smtp.gmail.com
    git config sendemail.smtpServerPort 587
    git config sendemail.smtpEncryption tls
    git config sendemail.smtpUser <the gmail address>

Gmail needs an app password (Google account -> Security -> App passwords).
Alternatively install `b4` (nixpkgs has it) and use `b4 send`.

## Send

From the U-Boot checkout carrying the rebased series, with the
format-patch output (cover letter + 12 patches) in `out/`:

    git send-email \
      --to u-boot@lists.u-boot-project.org \
      --cc "Andre Przywara <andre.przywara@arm.com>" \
      --cc "Anatolij Gustschin <ag.dev.uboot@gmail.com>" \
      --cc "Jaehoon Chung <jh80.chung@samsung.com>" \
      --cc "Peng Fan <peng.fan@nxp.com>" \
      --cc "Samuel Dionne-Riel <samuel@dionne-riel.com>" \
      out/00*.patch

Andre Przywara is the sunxi custodian and the main reviewer; Anatolij owns
drivers/video/; Jaehoon and Peng own drivers/power/. Samuel wrote the
Tow-Boot LRADC patch the button driver is derived from; CC him so he can
ack the attribution (Ondrej Jirman's address from the original patch bounced
domains before; his current one can be added if a reply thread surfaces it).

## After sending

- Watch https://patchwork.ozlabs.org/project/uboot/ for the series.
- Expect review rounds; resend as [PATCH v2 ...] with per-patch changelogs
  below the --- line, and keep the cover letter's diffstat regenerated.
- Rebase onto current next before each resend.
