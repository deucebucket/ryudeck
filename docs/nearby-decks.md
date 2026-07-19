# Using Nearby Decks

Nearby Decks copies game backup files between Ryudeck installs on the same local network. It is a
pull-only feature: the receiving Deck chooses a game and keeps its own local copy. There is no cloud
relay, live streaming, automatic mirroring, or remote deletion.

## Set up sharing

1. Connect both Steam Decks to the same local network and open Ryudeck on each one.
2. On each Deck, open **Settings > System > Network** and set a recognizable **Deck nickname**. Leave
   it blank to use the local Steam display name, then the device hostname.
3. Turn on **Share library on LAN** on both Decks during setup so they can discover each other.
4. Open **Nearby Decks** on both Decks. Select the other Deck, choose **Pair**, and confirm that the
   full friend code matches what the other Deck displays. Repeat on the other Deck.

Pairing is required and must be completed in both directions. An unpaired Deck cannot browse or pull
a library. After pairing, the receiving Deck can turn sharing back off; the sending Deck must leave
sharing on while its library is available.

Automatic discovery requires both Decks on the same broadcast LAN. A routed VPN, subnet router, or
separate VLAN can make the other network reachable without forwarding Nearby Decks discovery. This
release does not include a manual address or relay mode.

If an unpaired Deck tries to browse, the sender shows a pair-request notification telling its owner
to open Nearby Decks and compare codes. This notification does not approve the request by itself.

Sharing is off by default. Turning it off stops the sender from advertising or serving its library.

## Pull a game

1. On the receiving Deck, open **Nearby Decks** and select the paired sending Deck.
2. Select a game marked **Download**. Games already present in the receiver's library are marked
   **Installed**.
3. Leave Nearby Decks or continue using the launcher. The transfer remains active in the background,
   with progress and completion notifications. A transfer icon remains beside the battery indicator
   in the launcher while an upload or download is active.
4. Ryudeck verifies the completed file against the sender's signed content hash, moves it into the
   first writable configured game folder, and refreshes the receiving library.

Downloads run one at a time. The sender must keep Ryudeck open with sharing enabled until the copy
finishes. The sender warns before exit and blocks removal of a game or game folder involved in an
active transfer.

## Interrupted transfers

Ryudeck keeps an incomplete download as a partial file. If Wi-Fi drops, either Deck restarts, or the
transfer is canceled, downloading resumes from the saved offset when the sender becomes available
and the pull is retried. A failed integrity check discards the bad partial file instead of placing it
in the library.

## What is shared

v0.1.24 advertises the game files currently enumerated from the sender's configured game folders.
Each game has a **Share on LAN** control in its game settings; hiding it removes it from the next
shared-library listing without changing the local game.

Ryudeck does not offer keys, firmware, saves, configuration, profiles, or files outside configured
game folders. Separate update, DLC, and mod files are not advertised by v0.1.24. The receiving Deck
must already have its own keys and firmware.

## Trust and integrity

Pairing pins each Deck's local identity after you compare the displayed friend codes. Every library
request rejects unpaired Decks. Shared-library listings and content hashes are signed, and the
receiver checks the sender's pinned identity and the completed file before accepting it.

The transfer runs directly over the local network and is authenticated and integrity-checked; it is
not an encrypted tunnel. Use Nearby Decks on a trusted household network.

Deleting a game from either Deck never deletes it from the other. A game removed from the sender
simply stops appearing as available to pull.
