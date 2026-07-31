# status.json: broadcasting a notice to everybody

`status.json` in this repository is how you tell every copy of VDR Workbench something in real time:
the gateway is down, a data room is being re-synced, do not start a run this afternoon. Edit this one
file and it reaches everybody the next time they open the app.

Added in v1.4.3. Earlier versions ignore it.

## To show a notice

```json
{
  "message": "The AI gateway is being maintained this afternoon. Runs started now may fail.",
  "level": "yellow"
}
```

It appears as a panel laid over the top of the app, under the screen's own title. It does not push
anything down and it does not stop anyone working.

## Colours

`level` picks the colour. Leave it out and you get green.

| `level` | Colour | Reads as | Use it for |
|---|---|---|---|
| `green` | Green | **Notice** | Something to know. Maintenance is finished, a room has finished re-syncing. |
| `yellow` | Amber | **Please note** | Something that might affect their work. A service is degraded, a run may fail. |
| `red` | Red | **Important** | Stop and read. Do not start a run today, a data room is incomplete. |

The older names `info`, `warn` and `critical` still work and map to green, yellow and red. Anything
unrecognised shows green rather than losing the notice.

## To clear it

Set the message back to empty. Do not delete the file.

```json
{ "message": "" }
```

## Fields

| Field | Required | What it does |
|---|---|---|
| `message` | yes | The text shown. An empty message means no notice. |
| `level` | no | `green` (default), `yellow` or `red`. Colour only, and it never changes the wording. |
| `id` | no | So dismissing it is remembered. Leave it out and it is derived from the text, which means **editing the wording brings the notice back for everyone**, including people who dismissed the previous one. Set an id by hand only if you want to change the wording *without* re-showing it. |
| `until` | no | `YYYY-MM-DD`. The notice stops showing after this date, so a planned outage can be scheduled and forgotten. |
| `link` | no | An http(s) link shown as a button. Any other scheme is ignored. |
| `link_text` | no | The button's label. Defaults to "Open". |

## Things worth knowing

**It is dismissible.** Each person can close a notice, and it stays closed for them until the text or
the `id` changes. There is no way to force a notice to stay on screen, deliberately: this is for
information, and the update prompt already exists for anything that must block.

**It cannot break the app.** The fetch is separate from everything the app needs to start. An
unreachable file, a 404, malformed JSON or a missing message all result in no notice and no error.
That is why it is safe to edit live.

**Propagation takes up to about five minutes.** GitHub's raw host caches, and the query-string cache
buster the app sends does not defeat it. Use `gh api repos/Alton-Aviation-Consultancy/VDRWorkbench-Public/contents/status.json`
to see what is genuinely committed, since the raw URL can serve a stale copy for a few minutes after a
push.

**Keep it short.** It renders as one banner across the top of the app.
