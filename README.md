# EasyOffset API

EasyOffset provides a simple API for e-commerce platforms to integrate carbon offsetting into their checkout flow. Allow your customers to make environmentally conscious purchases by calculating and offsetting their carbon footprint.

## Quick Start

```bash
# Example using curl
curl -X POST https://api.easyoffset.net/v1/calculate \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "items": [{
      "product_type": "electronics",
      "weight": 0.5,
      "shipping_distance": 1000
    }]
  }'
```

## Authentication

All API requests require an API key. Get your API key by [signing up](https://easyoffset.net/signup).

Include the API key in your requests:
```
Authorization: Bearer YOUR_API_KEY
```

## API Endpoints

### Calculate Carbon Footprint

Calculate the carbon footprint for a shopping cart.

```http
POST https://api.easyoffset.net/v1/calculate
```

**Request Body:**
```json
{
  "items": [
    {
      "product_type": "string",      // Product category
      "weight": number,              // Weight in kg
      "shipping_distance": number    // Distance in km
    }
  ]
}
```

**Response:**
```json
{
  "calculation_id": "calc_123abc",
  "carbon_footprint": {
    "total_kg_co2": 25.5,
    "breakdown": {
      "products": 15.5,
      "shipping": 10.0
    }
  },
  "offset_options": [
    {
      "offset_id": "off_456def",
      "project_name": "Amazon Rainforest Conservation",
      "price": {
        "amount": 2.99,
        "currency": "USD"
      }
    }
  ]
}
```

### Create Offset Purchase

Purchase carbon offsets for a calculated footprint.

```http
POST https://api.easyoffset.net/v1/offsets
```

**Request Body:**
```json
{
  "calculation_id": "string",
  "offset_id": "string",
  "customer_email": "string",
  "order_reference": "string"
}
```

**Response:**
```json
{
  "offset_purchase_id": "purchase_789ghi",
  "status": "confirmed",
  "certificate_url": "https://easyoffset.net/certificates/789ghi"
}
```

### Retrieve Offset Certificate

Get the certificate for a completed offset purchase.

```http
GET https://api.easyoffset.net/v1/certificates/{offset_purchase_id}
```

**Response:**
```json
{
  "certificate_id": "cert_012jkl",
  "issue_date": "2024-04-24T12:00:00Z",
  "kg_co2_offset": 25.5,
  "project_details": {
    "name": "Amazon Rainforest Conservation",
    "location": "Brazil",
    "type": "Forest Conservation"
  },
  "pdf_url": "https://easyoffset.net/certificates/012jkl.pdf"
}
```

## Error Handling

The API uses standard HTTP response codes:
- `200` Success
- `400` Bad Request
- `401` Unauthorized
- `404` Not Found
- `429` Too Many Requests
- `500` Server Error

Error responses include a message:
```json
{
  "error": {
    "code": "invalid_request",
    "message": "Invalid calculation_id provided"
  }
}
```

## Rate Limits

- 1000 requests per minute per API key
- Rate limit info in response headers:
  - `X-RateLimit-Limit`
  - `X-RateLimit-Remaining`
  - `X-RateLimit-Reset`

## SDKs

Official SDKs:
- [Node.js](https://github.com/easyoffset/node-sdk)
- [Python](https://github.com/easyoffset/python-sdk)
- [PHP](https://github.com/easyoffset/php-sdk)

## Support

- [API Status](https://easyoffset.net/status)
- [Documentation](https://easyoffset.net/docs)
- Email: api@easyoffset.net

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
