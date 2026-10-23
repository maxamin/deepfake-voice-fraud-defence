# Provenance, Watermarking and a Defence Programme

Detection asks "is this fake?" Provenance asks a better question: "can this content prove where it came from?" This chapter covers C2PA content credentials and watermarking, explains what they can and cannot do against fraud, and ends with a programme checklist and research directions.

## C2PA and content credentials

The Coalition for Content Provenance and Authenticity (C2PA) publishes an open specification for attaching signed provenance manifests to media. A manifest records claims — which device or software created the content, what edits were made — and is cryptographically bound to the content. "Content Credentials" is the user-facing name. Camera makers, editing software and some AI image generators can produce manifests, and some platforms display them.

What provenance gives you:

- **Positive proof** for content that carries a valid manifest from a trusted signer, such as an official corporate video or a press photo.
- **Edit history** for content that went through compliant tools.
- **AI disclosure** where generators attach manifests stating content is synthetic.

What it does not give you:

- **No proof of falsehood from absence.** Most media has no manifest; stripping metadata removes it.
- **No protection for live calls.** C2PA targets files and streams with manifests; ordinary phone and meeting audio carries none.
- **Trust depends on signers.** A valid signature from an unknown signer proves little.

## Watermarking

Invisible watermarks embed a signal in the content itself, which can survive some transformations that strip metadata. Examples include Google DeepMind's SynthID for generated content and Meta's AudioSeal for speech (San Roman et al., 2024). Watermarks help when cooperating generators embed them; fraudsters using open-source models without watermarks are unaffected. Treat watermarks as useful for platforms and newsrooms, and marginal for stopping a determined fraud group.

| Technique | Strength | Weakness | Fraud relevance |
|---|---|---|---|
| C2PA manifest | Cryptographic, rich history | Stripped easily; absent on most content | Verify official communications |
| Watermark | Survives some re-encoding | Only on cooperating generators; removable by attackers with effort | Low for targeted fraud |
| Detection | Works on any content | Brittle, base-rate problem | Risk signal |
| Process controls | Independent of media | Requires discipline | Primary defence |

## Using provenance in your organisation

1. Sign official media (executive videos, announcements) with content credentials so recipients can verify them.
2. Tell staff and customers that official video instructions about payments will never arrive outside defined channels, regardless of credentials.
3. Use provenance checks in investigations: a valid manifest is strong evidence; a missing one is neutral.

## Worked example

> **Execution status:** not executed in this book's CI; the commands and code below are illustrative. Verify locally in an authorised lab before relying on them, and check tool flags against your installed versions.

Inspect and create content credentials with the open-source `c2patool`.

1. Install `c2patool` (release binaries or `cargo install c2patool`).
2. Inspect a file that carries credentials, for example a sample from the C2PA public test files:

```bash
c2patool sample.jpg            # prints the manifest store as JSON
c2patool sample.jpg --detailed # lower-level detail
```

3. Strip metadata and inspect again to see that provenance disappears:

```bash
exiftool -all= -o stripped.jpg sample.jpg
c2patool stripped.jpg          # expected: no manifest is reported
```

4. Sign your own test image with a test certificate, following the c2patool documentation for the manifest definition file:

```bash
c2patool photo.jpg -m manifest.json -o signed.jpg
c2patool signed.jpg
```

5. Upload `signed.jpg` to the [Content Credentials verify site](https://contentcredentials.org/verify) and note how a test certificate is shown as untrusted.
6. Write down the lesson for your organisation: credentials authenticate content you choose to sign; they cannot flag an unsigned fake.

## Programme checklist

| Area | Control | Evidence |
|---|---|---|
| Governance | Named owner for impersonation fraud; risk register entry | Risk register |
| Payments | Call-back to number on file, dual approval, cooling-off for new beneficiaries | System configuration, logs |
| Help desk | No credential or MFA resets on voice alone | Ticket workflow |
| Executives | Challenge-response verification; reduced voice exposure; briefed assistants | Distribution records |
| Customers | Step-up authentication; voice biometrics only as one signal | Auth policy |
| Onboarding | Attestation, nonces, chip reading, injection testing | Test matrix |
| Detection | Evaluated on your channel, used as a risk signal | Evaluation report |
| Provenance | Official media signed; staff know absence proves nothing | Signing procedure |
| Training | Scenario-based, focused on process not artefacts | Tabletop records |
| Reporting | Easy way to report suspected impersonation; law enforcement contacts | Report channel |

Report incidents to law enforcement (for example the FBI's IC3 in the US, or Action Fraud in the UK) and to your bank quickly; fast reporting improves the chances of recalling transfers.

## Research directions

- Detectors that generalise across generators and survive telephone channels.
- Provenance for live audio and video calls, binding a stream to an authenticated device.
- Standardised injection-attack testing and certification for remote onboarding.
- Usable verification protocols for families and small businesses.
- Better public incident data, with consistent reporting of what was observed versus inferred.

## Common pitfalls

- Promoting provenance as a fraud filter, which teaches people that "no credentials" means fake and "credentials" means safe.
- Investing in detection before fixing payment and help-desk processes.
- Training that is a one-off video rather than repeated scenario practice.
- Slow internal escalation that wastes the window for recalling funds.

## Exercises

1. Run the c2patool walkthrough and write a one-paragraph explanation of provenance for your finance team.
2. Score your organisation against the checklist and pick the three weakest controls to fix this quarter.
3. Draft an internal "how we will contact you" statement from leadership that removes voice and video as an authority channel for payments.

## Further reading

- [C2PA specification](https://c2pa.org/specifications/) and [c2patool](https://github.com/contentauth/c2pa-rs).
- [Content Authenticity Initiative](https://contentauthenticity.org/).
- San Roman et al., "Proactive Detection of Voice Cloning with Localized Watermarking" (AudioSeal), ICML 2024.
- [Google DeepMind SynthID](https://deepmind.google/technologies/synthid/).
- Sibling book: **verifiable-ai-inference** for cryptographic attestation of AI outputs.
