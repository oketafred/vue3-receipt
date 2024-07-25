<template>
    <div>
        <button @click="printReceipt">Print Receipt</button>
        <iframe ref="printFrame" style="display: none;"></iframe>
    </div>
</template>

<script setup>
import { ref } from 'vue';

const receiptData = ref({
    storeName: "Vue Mart",
    date: new Date().toLocaleDateString(),
    items: [
        { name: "Vue.js Cookbook", price: 39.99 },
        { name: "JavaScript: The Good Parts", price: 29.99 },
        { name: "CSS Secrets", price: 34.99 },
        { name: "TypeScript in 50 Lessons", price: 44.99 }
    ],
    get total() {
        return this.items.reduce((sum, item) => sum + item.price, 0);
    }
});

const printFrame = ref(null);

const generateReceiptHTML = () => {
    return `
      <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Deposit Receipt</title>
    <style>
        body, html {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
            min-height: 100vh;
        }
        .receipt {
            background-color: white;
            padding: 20px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
            width: 100%;
            box-sizing: border-box;
            max-width: 100%;
        }
        h1 {
            text-align: center;
            margin-bottom: 20px;
        }
        .logo {
            text-align: center;
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 20px;
        }
        .logo-circle {
            display: inline-block;
            width: 100px;
            height: 100px;
            border-radius: 50%;
            border: 2px solid black;
            line-height: 100px;
        }
        .customer-info, .transaction-details {
            margin-bottom: 20px;
        }
        .field {
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
        }
        .label {
            font-weight: bold;
        }
        .signature {
            margin-top: 30px;
            text-align: center;
        }
        .footer {
            margin-top: 20px;
            text-align: center;
            font-style: italic;
        }
        @media (min-width: 768px) {
            .receipt {
                padding: 40px;
            }
            .field {
                display: flex;
                justify-content: flex-start;
            }
            .label {
                width: 150px;
                margin-right: 10px;
            }
        }
        @media print {
            @page {
                margin: 0;
            }
            body {
                margin: 0;
            }
            .receipt {
                box-shadow: none;
            }
        }
    </style>
</head>
<body>
    <div class="receipt">
        <h1>DEPOSIT RECEIPT</h1>
        <div class="logo">
            <div class="logo-circle">SACCO</div>
        </div>
        <div class="customer-info">
            <div class="field">
                <span class="label">Customer ID:</span>
                <span class="value">xxx 401</span>
            </div>
            <div class="field">
                <span class="label">Customer name:</span>
                <span class="value">xxx xxxx</span>
            </div>
            <div class="field">
                <span class="label">Acc No:</span>
                <span class="value">xxxx301</span>
            </div>
            <div class="field">
                <span class="label">Acc Name:</span>
                <span class="value">Voluntary Saving</span>
            </div>
            <div class="field">
                <span class="label">Deposit Type:</span>
                <span class="value">Cash</span>
            </div>
        </div>
        <div class="transaction-details">
            <h2>Transaction Details</h2>
            <div class="field">
                <span class="label">Trax ID:</span>
                <span class="value"></span>
            </div>
            <div class="field">
                <span class="label">Trax Amount:</span>
                <span class="value"></span>
            </div>
            <div class="field">
                <span class="label">Trax Fees:</span>
                <span class="value"></span>
            </div>
            <div class="field">
                <span class="label">Total Amount:</span>
                <span class="value"></span>
            </div>
            <div class="field">
                <span class="label">Description:</span>
                <span class="value"></span>
            </div>
            <div class="field">
                <span class="label">Trax Date & Time:</span>
                <span class="value"></span>
            </div>
            <div class="field">
                <span class="label">Status:</span>
                <span class="value"></span>
            </div>
        </div>
        <div class="field">
            <span class="label">You were served by:</span>
            <span class="value">(Teller's 247)</span>
        </div>
        <div class="signature">
            <div class="label">Signature:</div>
            <div class="value">_________________</div>
        </div>
        <div class="footer">
            Thank you for saving with us<br>
            Powered by Mobii
        </div>
    </div>
</body>
</html>
    `;
};

const printReceipt = () => {
    const iframe = printFrame.value;
    const iframeWindow = iframe.contentWindow || iframe;
    const iframeDocument = iframe.contentDocument || iframeWindow.document;

    iframeDocument.write(generateReceiptHTML());
    iframeDocument.close();

    iframeWindow.focus();
    iframeWindow.print();
};
</script>