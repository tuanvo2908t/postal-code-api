# Postal code investigation using Japan Postal code

## JSON data:
```
{
  "prefecture": "東京都",
  "city": "渋谷区",
  "area": "道玄坂",
  "street": ""
}
```

## Implementation steps:
Step 1: Install:
```
npm install japan-postal-code
```
Step 2: Create resource/js/postalcode.js
```
window.postal_code = require('japan-postal-code');
```
Step 3: Create lookup interface and hanlde
1. Create resources/views/postal_code.blade.php
2. In resources/views/postal_code.blade.php:
```
<!DOCTYPE html>
<html>
<head>
    <title>Postal Code Lookup</title>
</head>
<body>
    <h1>Postal Code Lookup</h1>
    
    <form id="lookupForm">
        <label for="postcode">Enter Postal Code:</label>
        <input type="text" id="postcode" name="postcode" required>
        <button type="submit">Lookup</button>
    </form>
    
    <div id="resultContainer" style="display: none;">
        <h2>Result:</h2>
        <pre id="result"></pre>
    </div>
    
    <script src="{{ asset('js/postalcode.js') }}"></script>
    <script>
        // Wait for the DOM to load
        document.addEventListener('DOMContentLoaded', function() {
            const lookupForm = document.getElementById('lookupForm');
            const resultContainer = document.getElementById('resultContainer');
            const result = document.getElementById('result');
            
            lookupForm.addEventListener('submit', function(event) {
                event.preventDefault(); // Prevent form submission
                
                const postcode = document.getElementById('postcode').value;
                postal_code.get(postcode, function(address) {
                    // Display the result
                    result.textContent = JSON.stringify(address, null, 2);
                    resultContainer.style.display = 'block';
                });
            });
        });
    </script>
</body>
</html>


```
----------------------------------
Document:  https://www.npmjs.com/package/japan-postal-code
