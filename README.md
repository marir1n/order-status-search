# order-status-search
git clone https://github.com/marir1n/order-status-search.git
cd order-status-search
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Order Status Search</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Order Status Search</h1>
    <form id="searchForm">
        <label for="accountName">Account Name:</label>
        <input type="text" id="accountName" name="accountName" required>
        <button type="submit">Search</button>
    </form>
    <div id="results"></div>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 20px;
}

form {
    margin-bottom: 20px;
}

#results {
    margin-top: 20px;
}
document.getElementById('searchForm').addEventListener('submit', function(event) {
    event.preventDefault();
    const accountName = document.getElementById('accountName').value;
    fetch(`/search?accountName=${encodeURIComponent(accountName)}`)
        .then(response => response.json())
        .then(data => {
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = '';
            if (data.orders && data.orders.length > 0) {
                data.orders.forEach(order => {
                    const orderDiv = document.createElement('div');
                    orderDiv.innerHTML = `
                        <h2>Order ID: ${order.id}</h2>
                        <p>Status: ${order.status}</p>
                        <p>Payment: ${order.payment}</p>
                        <p>Items: ${order.items.join(', ')}</p>
                    `;
                    resultsDiv.appendChild(orderDiv);
                });
            } else {
                resultsDiv.innerHTML = '<p>No orders found for this account.</p>';
            }
        });
});
