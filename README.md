# DaySchedule MCP Server

Connect Claude, ChatGPT, Cursor, VS Code and other MCP clients to your [DaySchedule](https://dayschedule.com) account. Ask your AI assistant to check open time slots, book or reschedule appointments, block days off, and set up new event types.

This is a hosted remote server. There is nothing to install or run.

```
https://api.dayschedule.com/mcp
```

- Transport: Streamable HTTP
- Authentication: OAuth 2.1 (sign in with your DaySchedule account), or an API key
- Docs: [DaySchedule MCP server guide](https://dayschedule.com/docs/t/dayschedule-mcp-server-ai-appointment-booking-and-scheduling/570)

## What you can do

Here are some prompts you can try once you're connected:

- "What bookings do I have this week? Flag any that are still pending."
- "Find a free 30-minute slot next Tuesday afternoon on my Discovery Call event and book it for jane@acme.com."
- "Move my 3pm with Rahul tomorrow to Thursday at the same time."
- "I'm out on December 24 and 25. Block those days on my default schedule."
- "Create a 45-minute round robin Sales Demo event using my Sales Team schedule."
- "Mark yesterday's 11am booking as a no-show."

The assistant can only see and change what your DaySchedule account can access. If you're on a team account, that includes your team members' bookings.

## Connect

### Claude

1. In [Claude](https://claude.ai), open Settings, then Connectors, and select Add custom connector.
2. Enter `DaySchedule` as the name and `https://api.dayschedule.com/mcp` as the URL.
3. Select Connect, sign in to DaySchedule, and approve access.


### Claude Code

```bash
claude mcp add --transport http dayschedule https://api.dayschedule.com/mcp
```

Then run `/mcp` inside Claude Code and sign in to DaySchedule.

### Cursor

Add this to `~/.cursor/mcp.json`. Cursor opens the sign-in page the first time it connects.

```json
{
  "mcpServers": {
    "dayschedule": {
      "url": "https://api.dayschedule.com/mcp"
    }
  }
}
```

### VS Code

Add this to `.vscode/mcp.json` in your workspace:

```json
{
  "servers": {
    "dayschedule": {
      "type": "http",
      "url": "https://api.dayschedule.com/mcp"
    }
  }
}
```

### ChatGPT and other clients

Any MCP client that supports remote servers with OAuth can connect using the server URL above. In ChatGPT, add it as a custom connector.

### Using an API key

Some clients don't support OAuth. For those, create a key in DaySchedule under Settings > API Keys and send it as a bearer token:

```json
{
  "mcpServers": {
    "dayschedule": {
      "url": "https://api.dayschedule.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

An API key has full access to your account. Don't commit it to source control. If a key is exposed, revoke it in Settings > API Keys and create a new one.

## Tools

Event types are called "resource types" in the API. They cover [1:1 meetings](https://dayschedule.com/products/1-1-meeting-software), [group events](https://dayschedule.com/products/group-scheduling-software), [round robin](https://dayschedule.com/products/round-robin-scheduling-software) and [webinars](https://dayschedule.com/products/webinar-software).

| Tool | What it does | Access |
|---|---|---|
| `get_resource_types` | List event types | Read |
| `get_resource_type` | Get one event type with its booking settings | Read |
| `create_resource_type` | Create an event type | Write |
| `update_resource_type` | Update an event type | Write |
| `delete_resource_type` | Delete an event type | Destructive |
| `get_schedules` | List availability schedules | Read |
| `get_schedule` | Get a schedule with its working hours and rules | Read |
| `get_default_schedule` | Get the account's default schedule | Read |
| `create_schedule` | Create a schedule | Write |
| `update_schedule` | Change working hours, time zone, or block specific dates | Write |
| `get_available_slots` | Find open time slots for an event type in a date range | Read |
| `get_pages` | List booking pages | Read |
| `create_page` | Create a booking page | Write |
| `get_bookings` | List bookings, filtered by status, event type, invitee email or date range | Read |
| `get_booking` | Get one booking with invitees | Read |
| `create_booking` | Book an appointment | Write |
| `reschedule_booking` | Move a booking to a new time | Destructive |
| `cancel_booking` | Cancel a booking | Destructive |
| `confirm_booking` | Confirm a pending booking | Write |
| `mark_booking_noshow` | Mark a booking as a no-show | Write |
| `add_booking_invitee` | Add an attendee to a booking | Write |

Every tool sets MCP annotations (`readOnlyHint`, `destructiveHint`), so clients can ask you to confirm before anything is changed or deleted. Creating, rescheduling and cancelling bookings send invitees the same notifications as the DaySchedule app.

## Security and privacy

- OAuth uses PKCE. Access tokens expire after one hour, and refresh tokens are rotated on every use.
- To revoke access, open DaySchedule and go to Settings > Developers > Connected Apps, or remove the connector in your AI client.
- How we handle data accessed through connectors is covered in the [privacy policy](https://dayschedule.com/privacy). See also our [terms](https://dayschedule.com/terms) and [GDPR](https://dayschedule.com/gdpr) pages.

## Related

- [DaySchedule REST API](https://dayschedule.com/docs/api) and the [Node.js library](https://github.com/dayschedule/dayschedule-api)
- [Booking widget](https://github.com/dayschedule/dayschedule-widget) for embedding a booking calendar on your website
- [Webhooks](https://dayschedule.com/docs/t/webhook-integration/278) and [Zapier](https://dayschedule.com/docs/t/zapier-integration/235) integrations
- [All integrations](https://dayschedule.com/integrations), including Zoom, Google Meet, Microsoft Teams, Stripe and PayPal

DaySchedule is used for [interview scheduling](https://dayschedule.com/solutions/interview-scheduling-software), [doctor appointments](https://dayschedule.com/solutions/doctors-scheduling-software), [consulting](https://dayschedule.com/solutions/consulting-scheduling-software), [sales calls](https://dayschedule.com/solutions/sales-service-scheduling-software), [salons](https://dayschedule.com/solutions/salons-scheduling-software) and [more](https://dayschedule.com/solutions). See [pricing](https://dayschedule.com/pricing) to get started for free.

## Support

Email [support@dayschedule.com](mailto:support@dayschedule.com) or use the [contact page](https://dayschedule.com/contact). Feature requests go on the [public roadmap](https://github.com/dayschedule/public-roadmap).

## License

The contents of this repository are released under the [MIT License](LICENSE). The DaySchedule service is subject to the [DaySchedule terms](https://dayschedule.com/terms).
