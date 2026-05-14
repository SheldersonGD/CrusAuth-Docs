# License Information

CrusAuth requires a valid license key.

The license key is configured only on the Paper/Folia backend plugin. Proxy addons receive signed addon status from the backend through the secure bridge.

## Add the license key

Open:

```text
plugins/CrusAuth/config.yml
```

Set:

```yaml
license:
  key: "YOUR_LICENSE_KEY"
```

Restart the backend server.

## License validation

CrusAuth performs online license validation and caches successful validation for normal use.

The validation endpoint and internal product metadata are intentionally not exposed in customer configuration. Only `license.key` should be edited.

## Proxy addon licensing

The Velocity and BungeeCord modules are optional paid addons.

A license can allow:

- Paper/Folia core only;
- Paper/Folia core + Velocity addon;
- Paper/Folia core + BungeeCord addon;
- Paper/Folia core + both proxy addons.

If a proxy addon is installed but not included in the license, the proxy module will deny addon functionality.

## Offline license keys

Offline license keys may be provided for approved offline deployments.

Offline keys are intended for servers that cannot contact the validation service. They are not public demo keys and should not be shared.

## Files created by license validation

CrusAuth may create local license state files such as:

```text
license-cache.properties
offline-license-slots.properties
server-id.txt
```

Do not share these files publicly. They are tied to your installation and license state.

## Common license errors

### Missing license key

The plugin disables itself until a valid key is configured.

Fix:

```yaml
license:
  key: "YOUR_LICENSE_KEY"
```

Then restart the backend.

### Proxy addon is not enabled on this license

The installed proxy module is not included in the license.

Fix one of the following:

- purchase or activate the required addon;
- remove the proxy addon jar;
- verify that the Paper/Folia backend is using the correct license key;
- restart proxy and backend after activation.

### License server unavailable

Check:

- server internet access;
- DNS resolution;
- firewall rules;
- hosting provider outbound HTTP/HTTPS restrictions;
- whether the key was copied correctly.
