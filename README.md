<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>فاتورة مبيعات ذكية</title>

<style>
    * {
        box-sizing: border-box;
        font-family: Tahoma, Arial, sans-serif;
    }

    body {
        margin: 0;
        background: #f2f4f7;
        color: #222;
    }

    .container {
        width: 95%;
        max-width: 1200px;
        margin: 25px auto;
        background: #fff;
        padding: 25px;
        border-radius: 15px;
        box-shadow: 0 5px 25px rgba(0,0,0,0.08);
    }

    .header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 20px;
        border-bottom: 2px solid #222;
        padding-bottom: 15px;
        margin-bottom: 20px;
    }

    .header h1 {
        margin: 0;
        font-size: 28px;
    }

    .invoice-info {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 10px;
        margin-bottom: 20px;
    }

    .invoice-info input {
        width: 100%;
        padding: 11px;
        border: 1px solid #ccc;
        border-radius: 7px;
        font-size: 15px;
    }

    .buttons {
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
        margin-bottom: 20px;
    }

    button {
        border: none;
        border-radius: 7px;
        padding: 11px 18px;
        cursor: pointer;
        font-size: 15px;
        font-weight: bold;
    }

    .add-btn {
        background: #198754;
        color: white;
    }

    .reset-btn {
        background: #dc3545;
        color: white;
    }

    .print-btn {
        background: #0d6efd;
        color: white;
    }

    button:hover {
        opacity: 0.85;
    }

    .table-wrapper {
        overflow-x: auto;
    }

    table {
        width: 100%;
        border-collapse: collapse;
        min-width: 900px;
    }

    th {
        background: #212529;
        color: white;
        padding: 12px 8px;
    }

    td {
        border: 1px solid #ddd;
        padding: 7px;
        text-align: center;
    }

    td input {
        width: 100%;
        padding: 9px;
        border: 1px solid #ccc;
        border-radius: 5px;
        text-align: center;
        font-size: 14px;
    }

    .delete-btn {
        background: #dc3545;
        color: white;
        padding: 8px 12px;
    }

    .total-box {
        margin-top: 20px;
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 12px;
    }

    .total-card {
        padding: 18px;
        border-radius: 10px;
        text-align: center;
        background: #f8f9fa;
        border: 1px solid #ddd;
    }

    .total-card span {
        display: block;
        font-size: 14px;
        margin-bottom: 8px;
        color: #666;
    }

    .total-card strong {
        font-size: 22px;
    }

    .grand-total {
        background: #e7f1ff;
    }

    .paid-total {
        background: #e8f7ee;
    }

    .remaining-total {
        background: #fff0f0;
    }

    .footer {
        margin-top: 25px;
        padding-top: 15px;
        border-top: 1px solid #ddd;
        text-align: center;
        color: #777;
    }

    @media (max-width: 700px) {
        .container {
            width: 100%;
            margin: 0;
            border-radius: 0;
            padding: 12px;
        }

        .header {
            flex-direction: column;
            text-align: center;
        }

        .invoice-info {
            grid-template-columns: 1fr;
        }

        .total-box {
            grid-template-columns: 1fr;
        }
    }

    @media print {
        body {
            background: white;
        }

        .container {
            width: 100%;
            max-width: none;
            margin: 0;
            box-shadow: none;
            border-radius: 0;
        }

        .buttons {
            display: none;
        }

        .delete-column {
            display: none;
        }

        .delete-cell {
            display: none;
        }

        input {
            border: none !important;
            background: transparent !important;
        }

        .total-card {
            break-inside: avoid;
        }
    }
</style>
</head>

<body>

<div class="container">

    <div class="header">
        <div>
            <h1>فاتورة مبيعات</h1>
            <p>نظام فواتير ذكي</p>
        </div>

        <div>
            <strong>رقم الفاتورة:</strong>
            <span id="invoiceNumber"></span>
            <br>
            <strong>التاريخ:</strong>
            <span id="invoiceDate"></span>
        </div>
    </div>

    <div class="invoice-info">
        <input type="text" id="customerName" placeholder="اسم العميل">
        <input type="text" id="customerPhone" placeholder="رقم الهاتف">
    </div>

    <div class="buttons">
        <button class="add-btn" onclick="addProduct()">
            + إضافة منتج
        </button>

        <button class="reset-btn" onclick="resetInvoice()">
            تصفير الفاتورة
        </button>

        <button class="print-btn" onclick="window.print()">
            طباعة الفاتورة
        </button>
    </div>

    <div class="table-wrapper">

        <table id="invoiceTable">

            <thead>
                <tr>
                    <th>#</th>
                    <th>اسم المنتج</th>
                    <th>الكمية</th>
                    <th>السعر</th>
                    <th>الإجمالي</th>
                    <th>المدفوع</th>
                    <th>المتبقي</th>
                    <th class="delete-column">حذف</th>
                </tr>
            </thead>

            <tbody id="productBody">
            </tbody>

            <tfoot>
                <tr>
                    <th colspan="4">الإجمالي العمودي</th>

                    <th id="verticalTotal">
                        0.00
                    </th>

                    <th id="verticalPaid">
                        0.00
                    </th>

                    <th id="verticalRemaining">
                        0.00
                    </th>

                    <th class="delete-column">-</th>
                </tr>
            </tfoot>

        </table>

    </div>

    <!-- الملخص الأفقي -->
    <div class="total-box">

        <div class="total-card grand-total">
            <span>إجمالي الفاتورة</span>
            <strong id="grandTotal">0.00</strong>
        </div>

        <div class="total-card paid-total">
            <span>إجمالي المدفوع</span>
            <strong id="grandPaid">0.00</strong>
        </div>

        <div class="total-card remaining-total">
            <span>إجمالي المتبقي</span>
            <strong id="grandRemaining">0.00</strong>
        </div>

    </div>

    <div class="footer">
        شكرًا لتعاملكم معنا
    </div>

</div>


<script>

let productCount = 0;


/* =========================
   إنشاء رقم الفاتورة
========================= */

function generateInvoiceNumber() {

    const number =
        Date.now().toString().slice(-8);

    document.getElementById("invoiceNumber").textContent = number;
}


/* =========================
   التاريخ
========================= */

function setDate() {

    const today = new Date();

    const date =
        today.toLocaleDateString("ar-YE");

    document.getElementById("invoiceDate")
        .textContent = date;
}


/* =========================
   إضافة منتج
========================= */

function addProduct(
    name = "",
    quantity = 1,
    price = 0,
    paid = 0
) {

    productCount++;

    const tbody =
        document.getElementById("productBody");

    const row =
        document.createElement("tr");

    row.innerHTML = `

        <td class="number">
            ${productCount}
        </td>

        <td>
            <input
                type="text"
                class="product-name"
                placeholder="اسم المنتج"
                value="${name}"
            >
        </td>

        <td>
            <input
                type="number"
                class="quantity"
                min="0"
                step="0.01"
                value="${quantity}"
                oninput="calculateInvoice()"
            >
        </td>

        <td>
            <input
                type="number"
                class="price"
                min="0"
                step="0.01"
                value="${price}"
                oninput="calculateInvoice()"
            >
        </td>

        <td class="row-total">
            0.00
        </td>

        <td>
            <input
                type="number"
                class="paid"
                min="0"
                step="0.01"
                value="${paid}"
                oninput="calculateInvoice()"
            >
        </td>

        <td class="row-remaining">
            0.00
        </td>

        <td class="delete-cell delete-column">
            <button
                class="delete-btn"
                onclick="deleteProduct(this)"
            >
                حذف
            </button>
        </td>

    `;

    tbody.appendChild(row);

    calculateInvoice();
}


/* =========================
   حذف منتج
========================= */

function deleteProduct(button) {

    const row =
        button.closest("tr");

    row.remove();

    renumberRows();

    calculateInvoice();
}


/* =========================
   إعادة ترقيم المنتجات
========================= */

function renumberRows() {

    const rows =
        document.querySelectorAll("#productBody tr");

    productCount = rows.length;

    rows.forEach((row, index) => {

        row.querySelector(".number")
            .textContent = index + 1;

    });
}


/* =========================
   حساب الفاتورة
========================= */

function calculateInvoice() {

    const rows =
        document.querySelectorAll("#productBody tr");

    let grandTotal = 0;
    let grandPaid = 0;
    let grandRemaining = 0;


    rows.forEach(row => {

        const quantity =
            parseFloat(
                row.querySelector(".quantity").value
            ) || 0;

        const price =
            parseFloat(
                row.querySelector(".price").value
            ) || 0;

        let paid =
            parseFloat(
                row.querySelector(".paid").value
            ) || 0;


        /* إجمالي المنتج */

        const total =
            quantity * price;


        /* منع المدفوع من تجاوز قيمة المنتج */

        if (paid > total) {

            paid = total;

            row.querySelector(".paid").value =
                paid.toFixed(2);
        }


        /* المتبقي */

        const remaining =
            total - paid;


        /* عرض القيم */

        row.querySelector(".row-total")
            .textContent =
            total.toFixed(2);

        row.querySelector(".row-remaining")
            .textContent =
            remaining.toFixed(2);


        /* الإجماليات */

        grandTotal += total;

        grandPaid += paid;

        grandRemaining += remaining;

    });


    /* =========================
       الإجماليات العمودية
    ========================= */

    document.getElementById("verticalTotal")
        .textContent =
        grandTotal.toFixed(2);

    document.getElementById("verticalPaid")
        .textContent =
        grandPaid.toFixed(2);

    document.getElementById("verticalRemaining")
        .textContent =
        grandRemaining.toFixed(2);


    /* =========================
       الإجماليات الأفقية
    ========================= */

    document.getElementById("grandTotal")
        .textContent =
        grandTotal.toFixed(2);

    document.getElementById("grandPaid")
        .textContent =
        grandPaid.toFixed(2);

    document.getElementById("grandRemaining")
        .textContent =
        grandRemaining.toFixed(2);
}


/* =========================
   تصفير الفاتورة
========================= */

function resetInvoice() {

    const confirmReset =
        confirm(
            "هل أنت متأكد من تصفير الفاتورة؟"
        );

    if (!confirmReset) {
        return;
    }

    document.getElementById("productBody")
        .innerHTML = "";

    productCount = 0;

    document.getElementById("customerName")
        .value = "";

    document.getElementById("customerPhone")
        .value = "";

    generateInvoiceNumber();

    calculateInvoice();

    addProduct();
}


/* =========================
   تشغيل النظام
========================= */

generateInvoiceNumber();

setDate();

addProduct();

</script>

</body>
</html>
