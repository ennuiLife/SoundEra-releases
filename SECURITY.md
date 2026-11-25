# Security policy

SoundEra is a music player that logs in to your own music server, keeps a password, downloads and installs its own
updates, and reads files and pictures it did not make. We take reports of weaknesses in any of that seriously.

## Reporting a vulnerability

**Please do not open a public issue or discussion for a security problem.**

Report it privately instead, with GitHub's private vulnerability reporting:

1. Go to <https://github.com/ennuiLife/SoundEra-releases/security/advisories/new>
   (the **Security** tab of the repository, then **Report a vulnerability**).
2. Say what you found, how to trigger it, and what it lets someone do. A short description is enough to start;
   a small file, a command or steps that reproduce it make it much faster to fix.

Only the maintainer sees the report. You can take part in the fix and, if you want, be credited in the advisory.

### What to expect

| Step | Target |
|---|---|
| We confirm we received your report | within 7 days |
| We tell you whether we think it is a vulnerability, and how serious | within 14 days |
| A fix for a confirmed problem is released | as soon as it is ready; high severity first |
| We publish an advisory and credit you (if you wish) | after the fixed version is out |

SoundEra is maintained by one person, so these are targets, not guarantees. If you hear nothing within 7 days,
please write again on the same report.

We ask you to give us **90 days** from your report to release a fix before you share the details publicly, and we
will tell you if we need less or more. If the problem is already being used against people, tell us, and we will
move faster.

## Supported versions

Only the **latest release** is supported. SoundEra checks for updates when it starts and installs only updates that
carry a valid signature, so please check that you are on the newest version (Settings > Updates) before reporting.
Fixes are made for the newest version only; they are not back-ported.

## What is in scope

- The SoundEra program for Linux and Windows (including its AppImage, tarball and installer).
- How it handles **credentials**: the server password (kept in the system keyring), login tokens, addresses and logs.
- How it handles **data from a server or a file**: song and artist names, pictures, lyrics, downloads and the cache,
  the database and settings on disk.
- The **updater**: finding, downloading, verifying and installing a new version.
- The **release pipeline** (the build and signing workflow) and the published release files.

Examples of what we would like to hear about: a way to make SoundEra write or run something outside its own
folders, to install an update that was not signed by us, to read a password or token from a log or a file, to crash
it or use up all its memory or disk with a crafted file, picture or server answer, or to make it send your login to
another host.

## What is out of scope

- Problems in **your music server** (Navidrome, Subsonic and the like) or in the services SoundEra can ask when you
  switch them on (MusicBrainz, Wikipedia, TheAudioDB, LRCLIB): please report those to their own projects.
- Attacks that need **full control of your computer or account** already (for example, changing files in your own
  home folder while you are logged in as yourself).
- Using plain `http://` for a server: SoundEra warns about it, and what travels over it can be read on the way.
- Limits and slowness that are not a weakness (for example, GitHub limiting how often updates can be checked).
- Findings from automated scanners with no demonstrated effect.
- Missing hardening headers or settings on pages we do not run.

If you are not sure whether something counts, report it anyway.

## How SoundEra protects you

So you know what to test, and what you can check yourself:

- **Signed updates.** Every release file, and the `latest.json` file the program reads to look for updates, is signed
  with [minisign](https://jedisct1.github.io/minisign/). SoundEra carries the public key and installs nothing
  whose signature is wrong, whose signed version or name does not match the release, or whose version is not newer.
  Updates are fetched over HTTPS only.
- **Verify a download by hand.** The public key is
  `RWSM2klK8qRoiY5q1wgdJdLh7tiT7aUJG+EXRFPv09O7iHHErUz2zjQl`. With minisign installed:
  `minisign -Vm SoundEra-<version>-x86_64.AppImage -P RWSM2klK8qRoiY5q1wgdJdLh7tiT7aUJG+EXRFPv09O7iHHErUz2zjQl`
  (each release file has a `.minisig` file beside it).
- **Passwords.** The server password is kept in the system keyring (Secret Service, KDE Wallet or Windows
  Credential Manager), never in a settings file, the database or the cache. Requests to the server use a token and a
  salt, not the password, and web addresses, tokens and passwords are cut out of error messages and the log that
  COPY ERROR LOG puts on the clipboard.
- **Untrusted data.** Names, pictures, lyrics and downloads from a server are treated as hostile: file names made
  from them are cleaned, sizes and decoded picture dimensions are limited, and text is only ever shown, never run.
- **Your files.** SoundEra's data, settings and cache folders are readable by your account only (on Linux), and
  important files are written through a temporary file and renamed.
- **Releases.** The build workflow pins every action to a full commit, has read-only default permissions, and
  exposes the signing key only to the signing step.

## Safe harbour

If you make a good-faith effort to follow this policy (you test against your own copy and your own server, you do
not access other people's data, you do not disturb a service that is not yours, and you report to us privately),
we will not take legal action against you, and we will work with you to understand and fix the problem.

Thank you for helping keep SoundEra and the people who use it safe.
