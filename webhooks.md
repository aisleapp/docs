# Aisle Webhooks
The Aisle API provides a way for merchants to create event based web applications through our webhook mechanism.

## Getting started
At this time, the initial setup of webhooks is not available through a self-serve merchanism and requires contacting our engineering team directly at api@aisle.city.

Once you have been given access to our webhook infrastructure, your organization will be issued a `client_token` and `client_secret` which you'll be able to use to verify requests you receive from us.

## Event firehose
To begin receiving data, you will need to subscribe to our event firehose. This mechanism sends HTTPS POST requests to the URL of your choosing.

An sample event from the firehose will be sent to you as JSON in the following format:

```json
{
    "event": "ticket_created",
    "broadcasted_at": "RFC3339 Date",
    "body": {
      "id": "uuid",
      "store_id": "uuid",
      "user_id": "uuid",
      "user": {
        "id": "uuid",
        "first_name": "string",
        "last_name": "string",
        "phone_number": "E164 Phone Format"
      },
      "created_at": "RFC3339 Date",
      "updated_at": "RFC3339 Date"
    }
}
```

### Events available
We currently broadcast the following events:
* `ticket_created`: Customer has been added to your waitlist.
* `ticket_cancelled`: Customer has cancelled their own place in line.
* `ticket_removed`: Customer has been removed by an employee of your store.
* `ticket_called`: It's the customer's turn and they've been called.
* `ticket_served`: The customer has entered the store.
