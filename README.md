# Bomberman · Gamira client

Client integration patches for the [Godot Multiplayer Bomber demo](https://github.com/godotengine/godot-demo-projects/tree/master/networking/multiplayer_bomber),
with platform sign-in, Gamira matchmaking and connection to an allocated game server.

## Apply the patch

Run these commands from a new working directory. Git and tar are required.

```bash
git clone https://github.com/Gamira-Tech/bomberman.git bomberman-patches
git clone https://github.com/godotengine/godot-demo-projects.git godot-upstream
mkdir -p bomberman-client/multiplayer-bomber/project
git -C godot-upstream archive b4eff8de9d7ba5a4f1a2dea8bae60f28816b7eea \
  networking/multiplayer_bomber | tar -x --strip-components=2 \
  -C bomberman-client/multiplayer-bomber/project
git -C godot-upstream show b4eff8de9d7ba5a4f1a2dea8bae60f28816b7eea:LICENSE.md \
  > bomberman-client/multiplayer-bomber/LICENSE
cd bomberman-client
git apply --check ../bomberman-patches/patches/0001-gamira-integration.patch
git apply ../bomberman-patches/patches/0001-gamira-integration.patch
```

The upstream revision and patch checksum are recorded in [upstream.json](upstream.json).

## Configure and run

Edit `multiplayer-bomber/project/client.cfg`:

```ini
[services]
gamira_url="https://gamira.example.com"
platform_url="https://platform.example.com"
```

Use the authentication platform's base URL without `/api/v1`. Environment variables
`GAMIRA_API` and `PLATFORM_URL` can override these settings. Configuration holds
public service URLs; sign-in obtains the player's token at runtime.

With Godot 4.6 or later installed:

```bash
godot --path multiplayer-bomber/project/
```

Sign in, then choose **Find Match**. The client uses game mode `gm_bomber`,
configured in Gamira. Game servers and the two services are operated separately.
Patch reconstruction, Godot import and headless startup have been checked.

## Credits

Original demo: Godot Engine contributors, MIT. See [CREDITS.md](CREDITS.md) for
font and other acknowledgements. Preserve upstream licence notices with the
reconstructed client.

## Licence

Gamira’s client integration changes are licensed under [MIT](LICENSE). Original
upstream copyright and licence notices are preserved in [NOTICE.md](NOTICE.md)
and [LICENSES/](LICENSES/). Third-party assets and dependencies retain their own terms.
