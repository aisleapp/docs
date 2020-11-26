# Aisle Webhooks
The Aisle API provides a way for merchants to create event based web applications through our webhook mechanism.

## Getting started
At this time, the initial setup of webhooks is not available through a self-serve merchanism and requires contacting our engineering team directly at api@aisle.city.

Once you have been given access to our webhook infrastructure, your organization will be issued a `client_token` and `client_secret` which you'll be able to use to verify requests you receive from us.

## Event Firehose
To begin receiving data, you will need to subscribe to our event firehose. This mechanism sends TCP POST requests to the HTTPS URL of your choosing.

Events from the firehose will be sent to you as JSON in the following format:

**Headers:**
`x-aisle-webhook-signature: {hmac}`

**Body:**
```json
{
    "id": "uuid",
    "event": "ticket_created",
    "broadcasted_at": "RFC3339 Date",
    "data": {
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

## Verifying requests
All requests you receive from us will contain a header called `x-aisle-webhook-signature`. You will use this header, along with the request body, to verify the authenticity of our requests. We use the HMAC mechanism for signing all firehose events.

Follow these steps to verify a firehose event:
* Create an HMAC using the **raw and untampered** json in the body as the data, and use your `client_secret` as the signature.
* Using the HMAC comparison function of the language you're using, compare it against the value of `x-aisle-webhook-signature`.

If the comparison succeeds, then the request is legitimate. Not only is the signature valid, but you can rest assured that the body of the request has not been tampered with.

## Timeouts
We currently have a connection timeout of 500ms and a response timeout of 1000ms. We will attempt to deliver your event a maximum of 2 times.
