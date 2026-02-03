# 🍱 Cloud-Kitchen Order Management API
> **Industrial Level:** Production-Ready | **Architecture:** Object-Oriented | **Format:** JSON

This repository contains the technical specification for a **Cloud-Kitchen Order System**. It acts as the "Universal Translator" between a high-performance Java backend and external Delivery Partner applications.

---

## 🛠 System Architecture
The API is designed around a **State-Driven Model**. Instead of sending raw strings, the system exports "Snapshots" of the order's current state from the Java environment.

### **Data Blueprint (Object Definition)**

| Field | Type | Description |
| :--- | :--- | :--- |
| `order_id` | `String` | Unique alphanumeric transaction key. |
| `order_time` | `ISO-8601` | Universal timestamp of order creation. |
| `customer` | `Object` | Nested object containing `name` and `tier`. |
| `items` | `Array` | A collection of `Item` objects with custom instructions. |
| `payment_status`| `Boolean` | Final gate for order release (True/False). |

---

## 🟢 Success Scenario: "The Happy Path"
*This JSON is generated when an order is successfully processed and paid.*



```json
{
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
    "payment_status": true}

---

## 🔴 Error Scenario: "Payment Pending"
*Triggered when the payment boolean is false. The API communicates the "Recovery Action" to the client.*

> **Critical Note:** When `payment_status` is `false`, the delivery partner app must halt the driver's dispatch and trigger the payment collection UI.

```json
{
  "status": "error",
  "error_code": "PAYMENT_REQUIRED",
  "message": "Payment pending. Do not release order for delivery.",
  "details": {
    "order_id": "ORD-776",
    "pending_amount": 410
  }
}
  }
}
