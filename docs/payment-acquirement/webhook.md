# Webhook

# Payment Intent Success

## Overview

This documentation details the `payment_intent.successed` webhook event. This webhook is triggered asynchronously when a blockchain payment has been fully confirmed on the network, thus confirming the successful payment of a customer.

Consumers should set up a `POST` endpoint to receive this payload and use it to fulfill orders or credit user accounts.

## Event Details

- **Event Type:** `payment_intent.successed`
- **HTTP Method:** `POST`
- **Content-Type:** `application/json`
- **Retry Policy:** We will not retry, please retry manually on the dashboard.

---

## Payload Schema

The webhook body contains a JSON object representing the payment transaction.

### Field Reference

| **Property** | **Type** | **Description** |
| --- | --- | --- |
| id | Number | Unique identifier for this webhook event/record. |
| paymentLinkId | Number | The ID of the Payment Link associated with this transaction. |
| receiveWalletAddress | String | The wallet address where the funds were received. |
| createTime | String (ISO 8601) | Timestamp when the payment record was created. |
| paidTime | String (ISO 8601) | Timestamp when the payment was confirmed on-chain. |
| status | Integer | The status of the payment. For this event, it will typically be 4. (See Status Codes below). |
| transactionHash | String | The blockchain transaction hash (txHash). |
| logIndex | Integer | The index of the event within the blockchain block. |
| chain | String | The blockchain network used (e.g., ethereum, polygon). |
| currency | String | The token symbol used for payment (e.g., USDC, USDT). |
| payAmount | String | The amount paid. Sent as a string to preserve decimal precision. |
| emailParam | String | The email address associated with the payer (if provided). |
| clientReferenceIdParam | String | The custom reference ID passed by the client during link creation. |

### Status Codes (`status`)

The `status` field represents the state of the transaction engine.

- `0`: **Pending** (Waiting for transaction)
- `1`: **User Marked as Paid** (Unverified)
- `2`: **Timeout** (Payment window expired)
- `4`: **Confirmed** (Transaction verified on-chain) (This webhook event will always be on this status)

---

## Example Payload

JSON
