# ImageGrid

ImageGrid is optional. Million Dollar Script works without it and uses local rendering whenever ImageGrid is disconnected, unavailable, or over quota.

## Configure ImageGrid

Install and activate the **Million Dollar Script ImageGrid** extension first. Until the extension is active, **Million Dollar Script > Settings > Rendering** shows an ImageGrid prompt with install, activation, and service links instead of API and quota fields.

After the extension is active, open **Million Dollar Script > Settings > Rendering**.

Set:

- **ImageGrid API URL**
- **ImageGrid API Key**
- **Local Render Threshold MP**
- quota limits for grid megapixels, source megapixels, tile estimate, storage, and processing credits

When the API URL or key is blank, Million Dollar Script stays in local fallback mode.

## Account Status

Open **Million Dollar Script > Extensions** to see the ImageGrid status panel. The standalone ImageGrid settings page is linked from the Million Dollar Script dashboard menu after the extension is active; it does not add another WordPress sidebar submenu.

The panel shows:

- current status
- plan/account label
- whether the API is configured
- fallback mode
- quota values
- latest render job status for recent grids
- account, billing, reconnect, and configure links

The panel does not run a remote network check on every page load. Use the REST account test endpoint or support tooling when you need to verify a live token.

## Connect Or Reconnect

Use **Connect ImageGrid** when setting up a new account. Use **Reconnect** after a token is rotated, revoked, or copied from another site.

After reconnecting:

1. Save the API URL and API key in **Settings > Rendering**.
2. Open **Extensions** and confirm the panel shows the API as configured.
3. Run a render preflight for a large grid.
4. Submit a render job and confirm the latest job status changes.

## Billing

Use **Billing portal** from the ImageGrid panel. Billing is hosted outside WordPress; Million Dollar Script only stores the connection settings and quota values used to decide when remote rendering should be attempted.

## Fallback Behavior

Million Dollar Script falls back to local rendering when:

- ImageGrid is not configured
- the grid is below the local threshold
- quota checks fail
- the remote submission filter returns an error
- the ImageGrid API request fails

Fallback jobs are stored as local render jobs so support can see why remote rendering was not used.

## Support Checklist

For ImageGrid support requests, collect:

- WordPress site URL
- Million Dollar Script version
- ImageGrid API URL configured in settings
- whether the API key is configured
- current quota values
- latest render job provider, status, and error message
- whether local fallback rendered correctly
