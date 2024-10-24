---
title: Step-by-Step Integration
parent: Getting Started
nav_order: 2
---

# Step-by-Step Integration

### Step 1: Install Stripe's SDK
Install Stripe's JavaScript library on your frontend:
```html 
<script src="https://js.stripe.com/v3"></script>
```

### Step 2: Add Stripe Elements to Your Checkout Page
Embed Stripe Elements in your frontend code to collect payment details securely:
```js
// Initialize Stripe
const stripe = Stripe('your-publishable-key');
const elements = stripe.elements();

// Create a card input field
const card = elements.create('card');
card.mount('#card-element');

// Handle form submission
const form = document.getElementById('payment-form');
form.addEventListener('submit', async (event) => {
  event.preventDefault();
  
  const { token, error } = await stripe.createToken(card);
  
  if (error) {
    // Display error to the user
    console.error(error.message);
  } else {
    // Pass token to your backend
    handleToken(token);
  }
});
```
### Step 3: Handle the Payment Token on Your Backend
Once the token is generated from Stripe Elements, it will be passed to your backend. Use this token to create a payment or a payment intent using Stripe's API.
```js
// Sample backend code for processing payment (Node.js example)

const stripe = require('stripe')('your-secret-key');

// Handle POST request from frontend
app.post('/charge', async (req, res) => {
  const { token, amount, insurancePolicyId } = req.body;
  
  try {
    // Create a charge or a payment intent
    const charge = await stripe.charges.create({
      amount: amount,
      currency: 'usd',
      source: token, // The token from Stripe Elements
      description: `Payment for insurance policy ${insurancePolicyId}`
    });

    // On success, call the Insurance API
    const insuranceResponse = await purchasePolicy(insurancePolicyId, charge.id);
    
    res.status(200).json({ success: true, insurance: insuranceResponse });
  } catch (error) {
    res.status(500).json({ success: false, error: error.message });
  }
});
```
### Step 4: Use Embrace Partner API to Complete Purchase
Once payment is confirmed, you’ll need to call our API to finalize the policy purchase. The endpoint you’ll use is `/purchase`

**Endpoint:** `POST /purchase`
- **URL:** `https://api.embracepetinsurance.com/api/v2/purchase`
- **Method:** POST
- **Headers:** Include your API key in the header
  
**Request Body:**

| Field                 | Type    | Description                                        |
|:----------------------|:--------|:---------------------------------------------------|
| `insurancePolicyId`   | string  | The ID of the insurance policy being purchased.    |
| `paymentConfirmation` | string  | The payment confirmation ID returned from Stripe.  |
| `amount`              | integer | The amount charged to the customer (in cents).     |

**Example Request:**
```json
{
  "insurancePolicyId": "12345",
  "paymentConfirmation": "ch_1Ftpzp2eZvKYlo2CDHcmgYfi",
  "amount": 5000
}
```

**Example Response:**
```json
{
  "policyNumber": "INS-987654321",
  "status": "active",
  "effectiveDate": "2024-10-23T00:00:00Z",
  "coverage": {
    "type": "auto",
    "premium": 5000,
    "term": "12 months"
  }
}
```