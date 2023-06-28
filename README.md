# Postal code investigation using postcode-jp
## JSON data:
```
{
    "query": {
        "codes": [
            "1000001"
        ],
        "country": null
    },
    "results": {
        "1000001": [
            {
                "postal_code": "100-0001",
                "country_code": "JP",
                "latitude": "35.67490000",
                "longitude": "139.76190000",
                "city": "Chiyoda",
                "state": "Tokyo To",
                "city_en": "Chiyoda",
                "state_en": "Tokyo To",
                "state_code": "40",
                "province": "Chiyoda Ku",
                "province_code": "1864529"
            },
            {
                "postal_code": "1000-001",
                "country_code": "PT",
                "latitude": "38.71670000",
                "longitude": "-9.13330000",
                "city": "Lisboa",
                "state": "Lisboa",
                "city_en": "Lisboa",
                "state_en": "Lisboa",
                "state_code": "14",
                "province": "Lisboa",
                "province_code": "1106"
            }
        ]
    }
}
```
## Implementation steps:
Step 1: Create an account and login (https://app.zipcodebase.com)
Step 2: Install the Guzzle HTTP package
```
composer require guzzlehttp/guzzle
```
Step 3: Create route and controller
1. in routes/web.php
```
Route::get('/postal-code', [PostalCodeController::class, 'showForm'])->name('postal_code.form');
Route::get('/postal-code/lookup', [PostalCodeController::class, 'lookup'])->name('postal_code.lookup');
```
2. create controller:
```
php artisan make:controller PostalCodeController
```
3. in app/Http/controllers/PostalCodeController.php:
```
<?php

namespace App\Http\Controllers;

use GuzzleHttp\Client;
use Illuminate\Http\Request;

class PostalCodeController extends Controller
{
    public function showForm()
    {
        return view('postal_code');
    }

    public function lookup(Request $request)
    {
        //check remove dash
        $postcode = str_replace('-', '', $request->input('postcode'));

        $apiKey = 'your_APIKey';
        
        $client = new Client();
        
        $response = $client->get("https://app.zipcodebase.com/api/v1/search?codes={$postcode}", [
            'headers' => [
                'apikey' => $apiKey,
            ],
        ]);
        
        $data = json_decode($response->getBody(), true);
        
        $encodedData = json_encode($data, JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
        
        return view('postal_code', ['data' => $encodedData]);
    }
}

```
Step 4: Create lookup interface
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
    
    <form action="{{ route('postal_code.lookup') }}" method="GET">
        <label for="postcode">Enter Postal Code:</label>
        <input type="text" id="postcode" name="postcode" required>
        <button type="submit">Lookup</button>
    </form>
    
    @if(isset($data))
        <h2>Result:</h2>
        <pre>{{ $data }}</pre>
    @endif
</body>
</html>

```
----------------------------------
Document:  https://api-doc.postcode-jp.com/#api-v5
