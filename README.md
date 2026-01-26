# me3 Protocol (me.json)

The **me3 Protocol** (`me.json`) is a minimal standard for portable personal websites.

It treats your online identity as a **"Digital Business Card"**—a single JSON file that makes you discoverable by both humans and AI agents, without locking you into a specific platform.

## 1. The Specification

### File Location & Discovery
To be compliant, your `me.json` file must be hosted at one of the following locations (in order of priority):

1.  **Primary**: `https://yourdomain.com/me.json`
2.  **Fallback**: `https://yourdomain.com/.well-known/me`

### Transport & Security
*   **HTTPS Only**: The file must be served over a secure connection.
*   **CORS (Cross-Origin Resource Sharing)**: You **MUST** serve the file with the following header:
    ```http
    Access-Control-Allow-Origin: *
    ```
    **Why?** This ensures that AI agents running in browsers (e.g., Chrome extensions, web-based assistants) can read your identity file even if they are running on a different domain. Without this, your digital business card is invisible to the tools that help people find you.

### Content Type
The file should be served with `application/json` content type.

## 2. The Schema

Your `me.json` defines who you are and where to find you. It is strictly typed to ensure compatibility across all readers.

### Core Structure (`Me3Profile`)

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `version` | `string` | **Yes** | Protocol version (currently "0.1"). |
| `name` | `string` | **Yes** | Your display name. |
| `handle` | `string` | No | Your preferred username/handle. |
| `bio` | `string` | No | Short bio (max 500 chars). |
| `avatar` | `string` | No | URL to your profile picture. |
| `banner` | `string` | No | URL to a header/banner image. |
| `links` | `object` | No | Social links (website, github, twitter, etc.). |
| `buttons` | `array` | No | Primary actions (e.g., "Book Call", "Subscribe"). |
| `pages` | `array` | No | Custom content pages. |

### Example

```json
{
  "version": "0.1",
  "name": "Jane Doe",
  "handle": "janedoe",
  "bio": "Building the open web. Creative Director at Studio X.",
  "avatar": "https://example.com/jane.jpg",
  "links": {
    "website": "https://janedoe.com",
    "twitter": "janedoe",
    "github": "janedoe"
  },
  "buttons": [
    {
      "text": "Hire Me",
      "url": "https://cal.com/janedoe",
      "style": "primary"
    }
  ]
}
```

## 3. What it is NOT

*   **NOT Authentication**: `me.json` is public data. It does not handle logins, passwords, or private keys.
*   **NOT a Social Network**: There is no "feed", no "likes", and no central server. You own your data.
*   **NOT a Platform**: You can host this file on GitHub Pages, Vercel, WordPress, or your own server.

## 4. Usage

### For Developers
You can use this package to validate `me.json` files in your applications.

```bash
npm install me3-protocol
```

```typescript
import { validateProfile, parseMe3Json } from 'me3-protocol'

// Validate an object
const result = validateProfile(myProfileData)

// Parse and validate a string
const result = parseMe3Json(jsonString)

if (!result.valid) {
  console.error(result.errors)
}
```

### JSON Schema
A standard JSON Schema is available in [schema.json](./schema.json) for non-TypeScript implementations.
