# cloud-kitchen-api-docs

##Project Title: Cloud-Kitchen Order Management 

##APIOverview: This API acts as the bridge between the kitchen's internal Java-based order processing engine and the external delivery partner's application. 

It handles order state, customer tiering, and complex item instructions.Data Model (The "Object" Definition)The system processes orders as high-level objects. 

Below is the breakdown of the fields returned in an order 

snapshot.FieldType

###Description:

order_idString - Unique alphanumeric identifier for the transaction.
order_timeTimestamp - ISO 8601 formatted time of order creation.
customerObject - Nested object containing name and membership tier.
itemsArray - A list of individual food items and their specific instructions.
payment_statusBoolean - True if payment is captured; False if pending. 

Example: Successful Order ResponseWhen an order is successfully processed by the Java backend, the following JSON snapshot is generated:

JSON{
  "name": "order",
  "details": {
    "order_id": "ORD-776",
    "order_time": "2026-02-03T07:57:11.123Z",
    "customer": {
      "name": "Rajesh",
      "tier": "Premium"
    },
    "items": [
      {
        "item_name": "Paneer Tikka",
        "price": 350,
        "instructions": ["No Onions", "Extra Lemon"]
      },
      {
        "item_name": "Garlic Naan",
        "price": 60,
        "instructions": []
      }
    ],
    "total_amount": 410,
    "payment_status": true
  }
}
Error State: Payment PendingIf the payment_status returns false, the delivery partner's app must trigger a "Payment Collection" flow.JSON{
  "status": "error",
  "error_code": "PAYMENT_REQUIRED",
  "message": "Payment pending. Do not release order for delivery.",
  "details": {
    "order_id": "ORD-776",
    "pending_amount": 410
  }
}
