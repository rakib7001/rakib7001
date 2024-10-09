<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>bKash Payment Integration</title>

    <!-- Styles for the form and buttons -->
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f0f0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }

        .payment-container {
            background-color: #fff;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            max-width: 400px;
            width: 100%;
        }

        h2 {
            text-align: center;
            color: #333;
        }

        label {
            font-size: 14px;
            color: #555;
            margin-bottom: 10px;
            display: block;
        }

        input[type="number"] {
            width: 100%;
            padding: 10px;
            margin-bottom: 20px;
            border: 1px solid #ccc;
            border-radius: 5px;
            font-size: 16px;
        }

        button {
            background-color: #e2136e;
            color: white;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            width: 100%;
            font-size: 16px;
        }

        button:hover {
            background-color: #b50f53;
        }

        .info {
            text-align: center;
            font-size: 12px;
            color: #888;
            margin-top: 10px;
        }

        .error {
            color: red;
            text-align: center;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>

    <div class="payment-container">
        <h2>bKash Payment</h2>

        <form id="bkashPaymentForm">
            <label for="amount">Enter Amount:</label>
            <input type="number" id="amount" name="amount" placeholder="Enter amount" required>

            <div id="error-message" class="error"></div>

            <button type="button" id="payWithBkashBtn">Pay with bKash</button>
        </form>

        <div class="info">
            <p>Powered by bKash</p>
        </div>
    </div>

    <!-- Include bKash JS library -->
    <script src="https://scripts.bka.sh/versions/1.2.0-beta/bKash-checkout-sandbox.js"></script>

    <script>
        let paymentID = '';

        bKash.init({
            paymentMode: 'checkout',
            paymentRequest: function () {
                const amount = document.getElementById('amount').value;
                const errorMessage = document.getElementById('error-message');

                if (amount > 0) {
                    errorMessage.textContent = '';
                    fetch('your_payment_initialization_url', {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({ amount: amount })
                    })
                    .then(response => response.json())
                    .then(data => {
                        paymentID = data.paymentID;
                        bKash.create().onSuccess(paymentID);
                    })
                    .catch(error => {
                        console.error('Error:', error);
                        errorMessage.textContent = 'Payment initialization failed. Please try again.';
                    });
                } else {
                    errorMessage.textContent = 'Please enter a valid amount.';
                }
            },
            onClose: function () {
                alert('Payment window closed.');
            }
        });

        document.getElementById('payWithBkashBtn').addEventListener('click', function () {
            bKash.execute();
        });
    </script>
</body>
</html>
scripts.bka.sh
Sent
Write to Habiba Islam Rakib
