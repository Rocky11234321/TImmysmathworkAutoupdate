# Timmy’s Math Work — auto-update Web Bundle build

This project packages the supplied auto-update loader as the main document.
The loader requests the latest HTML from:

`https://cdn.jsdelivr.net/gh/Rocky11234321/TImmysmathworkAutoupdate@main/pureauratimmythingy-2.html`

## Important limitation

This preserves the remote auto-update behavior, but it does **not** make the remote page part of the bundle. Isolated Web Apps restrict remotely hosted executable code, so this loader is not a policy-compliant production IWA and may fail when run under IWA security rules. A valid signature does not remove that restriction.

## Create the signing key

Generate one Ed25519 key and keep it private. Do not regenerate it for updates:

```bash
openssl genpkey -algorithm ED25519 -out signing-key.pem
```

Add the key to the GitHub repository as an Actions secret named
`IWA_SIGNING_KEY_B64`:

```bash
base64 -w 0 signing-key.pem
```

Copy the output into the secret value. Never commit `signing-key.pem` to the repository.

## Build

Copy this project into a GitHub repository, add the secret, then run
**Actions → Build Signed Web Bundle → Run workflow**.

Download `timmys-math-work-swbn` from the workflow run's Artifacts section.

Keep the same signing key for every future build. Losing it means the app identity
changes and existing installations cannot receive normal updates.

## What this build does and does not do

- Produces a signed `.swbn` using the repository's persistent signing key.
- Packages the auto-update loader as `index.html`.
- The loader still fetches the current hosted HTML at runtime.
- It does not embed the remote HTML or make remote executable code IWA-compliant.
