# Daily Reminder Email — n8n Workflow

A simple no-code automation built in **n8n** that sends a scheduled daily reminder email via Gmail. No servers, no cron jobs to maintain — n8n handles the trigger and delivery end to end.

## What it does

Every day at **10:00 AM**, the workflow fires automatically and sends a motivational study/reminder email to a fixed list of recipients — no manual step involved.

## Workflow

```
Every Day at 10 AM  --->  Send Daily Reminder
   (Schedule Trigger)         (Gmail node)
```

Two nodes, connected in sequence:

| Node | Type | Role |
|---|---|---|
| `Every Day at 10 AM` | Schedule Trigger | Fires once per day at a fixed time — no external trigger needed |
| `Send Daily Reminder` | Gmail (Send Message) | Sends the reminder email to the configured recipient list |

### Execution log

The screenshot below shows a successful run: the trigger fired, passed 1 item to the Gmail node, and the send succeeded in ~100ms.

<img width="1920" height="1280" alt="image" src="https://github.com/user-attachments/assets/d0fb032c-89a5-4f2a-967b-67cdfbd44bca" />


Output of the Gmail node confirms delivery — `id`, `labelIds: SENT`, and `threadId` are all returned by Gmail's API once the message goes out.

### Result in Gmail

![Email received](./screenshots/gmail-result.png

Sample message body sent by the workflow:
<img width="1920" height="1280" alt="image" src="https://github.com/user-attachments/assets/d415b225-be9f-4c25-bd02-1231e495a340" />


The footer note (`This email was sent automatically with n8n`) is n8n's default signature on Gmail-node messages.

## Setup (to replicate this)

1. Create a free [n8n.cloud](https://n8n.io) account, or self-host via Docker.
2. Add a **Schedule Trigger** node → set it to trigger daily at your preferred time.
3. Add a **Gmail** node → connect your Google account (OAuth) → action: *Send a message*.
4. Fill in the recipient(s), subject, and message body.
5. Connect the Schedule Trigger's output to the Gmail node's input.
6. Click **Execute workflow** to test, then **Publish/Activate** to run it live on schedule.

## Notes / learnings

- The Schedule Trigger removes the need for any external cron service — n8n keeps the workflow "awake" and fires it on schedule as long as it's published/active.
- The Gmail node's OAuth connection needs to be re-authorized if the underlying Google account credentials change.

## Tech stack

- [n8n](https://n8n.io) — workflow automation (no-code)
- Gmail API (via n8n's built-in Gmail node)
