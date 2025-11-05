# Filflo Supply Chain Observability API Documentation

<div align="center">

**Filflo** - Smart Supply Chain Management Platform

*Real-time visibility and control over your entire supply chain*

</div>

---

**Version:** 1.0.0  
**Base URL:** `https://api.filflo.in/v1`  
**Authentication:** Bearer Token (JWT)  
**Company:** Filflo Technologies  
**Website:** https://filflo.in

---

## About Filflo

Filflo is a comprehensive supply chain management platform that provides end-to-end visibility and control over your inventory, orders, fulfillment, and logistics operations. Our API enables seamless integration with your existing systems to unlock powerful observability and automation capabilities.

---

## Quick Start Guide

### 1. Get Your API Credentials
Contact the Filflo team at team@filflo.in or log into your Filflo dashboard at https://app.filflo.in to generate your API credentials.

### 2. Authenticate
```bash
curl -X POST https://api.filflo.in/v1/auth/token \
  -H "Content-Type: application/json" \
  -d '{
    "client_id": "your_client_id",
    "client_secret": "your_client_secret",
    "grant_type": "client_credentials"
  }'
```

### 3. Make Your First API Call
```bash
curl -X GET https://api.filflo.in/v1/products \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### 4. Explore Our SDKs
Filflo provides official SDKs for popular programming languages:
- **Node.js:** `npm install @filflo/api-client`
- **Python:** `pip install filflo-sdk`
- **PHP:** `composer require filflo/api-client`
- **Java:** Available on Maven Central

---

## Key Features

🚀 **Real-time Observability** - Monitor your entire supply chain in real-time  
📊 **Comprehensive Analytics** - Deep insights into sales, inventory, and operations  
🔗 **Seamless Integration** - Easy integration with existing ERP and marketplace systems  
🔔 **Smart Webhooks** - Real-time event notifications for critical operations  
📱 **Multi-channel Support** - Unified API for Zepto, Blinkit, Swiggy, Amazon & more  
🏭 **Multi-warehouse** - Manage inventory across multiple warehouses  
📦 **End-to-end Tracking** - Complete visibility from PO to delivery  
🎯 **Quality Management** - Built-in quality control and batch tracking  
💰 **Dynamic Pricing** - Flexible rate cards and margin management  
🔐 **Enterprise Security** - Bank-grade security with JWT authentication

## Use Cases

### E-commerce Operations
- Sync orders from multiple marketplaces (Zepto, Blinkit, Swiggy, Amazon)
- Real-time inventory updates across channels
- Automated order fulfillment and tracking

### Supply Chain Management
- Complete procurement workflow from PO to GRN
- Supplier performance monitoring
- Quality control and batch tracking

### Business Intelligence
- Real-time sales and revenue analytics
- Inventory optimization insights
- Fulfillment performance metrics

### Financial Operations
- Automated invoice generation
- GST-compliant documentation
- Credit note management

---

## Environments

Filflo provides multiple environments for development and production:

| Environment | Base URL | Purpose |
|------------|----------|---------|
| **Production** | `https://api.filflo.in/v1` | Live production environment |
| **Sandbox** | `https://sandbox-api.filflo.in/v1` | Testing and development |
| **Staging** | `https://staging-api.filflo.in/v1` | Pre-production testing |

**Note:** Use sandbox environment for testing. Sandbox data is reset weekly.

---

## Table of Contents

1. [Authentication](#authentication)
2. [Products API](#products-api)
3. [Inventory API](#inventory-api)
4. [Orders & Fulfillment API](#orders--fulfillment-api)
5. [Customers API](#customers-api)
6. [Suppliers API](#suppliers-api)
7. [Purchase Orders & GRN API](#purchase-orders--grn-api)
8. [Invoices & Credit Notes API](#invoices--credit-notes-api)
9. [Transporters & Logistics API](#transporters--logistics-api)
10. [Quality Management API](#quality-management-api)
11. [Rate Cards & Pricing API](#rate-cards--pricing-api)
12. [Analytics & Reports API](#analytics--reports-api)
13. [Webhooks](#webhooks)
14. [Rate Limits](#rate-limits)
15. [Error Codes](#error-codes)

---

## Authentication

All API requests require authentication using a Bearer token in the Authorization header.

### Get Access Token

```http
POST /auth/token
Content-Type: application/json

{
  "client_id": "your_client_id",
  "client_secret": "your_client_secret",
  "grant_type": "client_credentials"
}
```

**Response:**
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Using the Token

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## Products API

### List All Products

```http
GET /products
```

**Query Parameters:**
- `category` (string): Filter by product category
- `sku_code` (string): Search by SKU code
- `zepto_sku` (string): Filter by Zepto SKU
- `blinkit_sku` (string): Filter by Blinkit SKU
- `swiggy_sku` (string): Filter by Swiggy SKU
- `amazon_sku` (string): Filter by Amazon SKU
- `page` (integer): Page number (default: 1)
- `limit` (integer): Items per page (default: 50, max: 100)
- `include_inventory` (boolean): Include current inventory levels (default: false)

**Response:**
```json
{
  "data": [
    {
      "product_name": "A2 Hallikar Cow Ghee - 1000mL (CLOSED)",
      "product_sku_code": "FPCL-GHEE-HLKR-GJAR-1LTR",
      "category": "GHEE - Ghee Products",
      "marketplace_skus": {
        "zepto": "45CD34E6-FFCC-4E43-83F5-A203B62934A1",
        "blinkit": "10105175",
        "swiggy": "870796",
        "amazon": "SF-TKKR-HRLK-1LTR",
        "easyecom": "FPCL-GHEE-HRLK-1LTR"
      },
      "identifiers": {
        "ean_code": "8906136780019",
        "hsn_code": "04059020",
        "asin_code": "B08KJG9VLJ"
      },
      "pricing": {
        "price_per_unit": 2025,
        "tax_slab": "TAX-05"
      },
      "packaging": {
        "case_size": 1,
        "weight": null,
        "dimensions": null
      },
      "inventory_thresholds": {
        "lower_threshold": null,
        "upper_threshold": null
      },
      "current_inventory": {
        "current_stock": 32,
        "average_age_days": 11
      }
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 150,
    "total_pages": 3
  }
}
```

### Get Product by SKU

```http
GET /products/{sku_code}
```

**Response:**
```json
{
  "product_name": "A2 Hallikar Cow Ghee - 1000mL (CLOSED)",
  "product_sku_code": "FPCL-GHEE-HLKR-GJAR-1LTR",
  "category": "GHEE - Ghee Products",
  "marketplace_skus": {
    "zepto": "45CD34E6-FFCC-4E43-83F5-A203B62934A1",
    "blinkit": "10105175",
    "swiggy": "870796",
    "amazon": "SF-TKKR-HRLK-1LTR"
  },
  "identifiers": {
    "ean_code": "8906136780019",
    "hsn_code": "04059020",
    "asin_code": "B08KJG9VLJ"
  },
  "pricing": {
    "price_per_unit": 2025,
    "tax_slab": "TAX-05"
  },
  "current_inventory": {
    "current_stock": 32,
    "warehouses": [
      {
        "warehouse": "JWL Store",
        "quantity": 32,
        "average_age_days": 11
      }
    ]
  },
  "recent_transactions": {
    "last_purchase_order": {
      "po_number": "WELL/30102025/6474",
      "date": "2025-10-30",
      "quantity": 1500,
      "supplier": "Wellness shots"
    },
    "recent_sales": [
      {
        "order_id": "P2551659",
        "customer": "Zepto Private Limited - Telangana",
        "quantity": 80,
        "date": "2025-11-03"
      }
    ]
  }
}
```

### Get Product Categories

```http
GET /products/categories
```

**Response:**
```json
{
  "categories": [
    {
      "category_name": "GHEE - Ghee Products",
      "product_count": 12,
      "total_inventory_value": 450000
    },
    {
      "category_name": "OILL - Oil Products",
      "product_count": 25,
      "total_inventory_value": 780000
    }
  ]
}
```

---

## Inventory API

### Get Inventory Levels

```http
GET /inventory
```

**Query Parameters:**
- `sku_code` (string): Filter by SKU
- `category` (string): Filter by category
- `warehouse` (string): Filter by warehouse
- `stock_status` (string): Filter by status - `in_stock`, `low_stock`, `out_of_stock`, `negative_stock`
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "product_name": "A2 Hallikar Cow Ghee - 1000mL (CLOSED)",
      "product_sku_code": "FPCL-GHEE-HLKR-GJAR-1LTR",
      "category": "GHEE - Ghee Products",
      "current_stock": 32,
      "stock_status": "in_stock",
      "thresholds": {
        "lower_threshold": 0,
        "upper_threshold": 0
      },
      "average_age_days": 11,
      "warehouses": [
        {
          "warehouse": "JWL Store",
          "quantity": 32,
          "last_updated": "2025-11-05T06:08:00Z"
        }
      ]
    }
  ],
  "summary": {
    "total_skus": 150,
    "in_stock": 120,
    "low_stock": 20,
    "out_of_stock": 8,
    "negative_stock": 2
  },
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 150,
    "total_pages": 3
  }
}
```

### Get Inventory by SKU

```http
GET /inventory/{sku_code}
```

**Response:**
```json
{
  "product_name": "A2 Hallikar Cow Ghee - 1000mL (CLOSED)",
  "product_sku_code": "FPCL-GHEE-HLKR-GJAR-1LTR",
  "category": "GHEE - Ghee Products",
  "current_stock": 32,
  "stock_status": "in_stock",
  "thresholds": {
    "lower_threshold": 0,
    "upper_threshold": 0
  },
  "average_age_days": 11,
  "warehouses": [
    {
      "warehouse": "JWL Store",
      "quantity": 32,
      "location": "A-12-3",
      "last_updated": "2025-11-05T06:08:00Z"
    }
  ],
  "stock_movements": [
    {
      "date": "2025-11-04",
      "type": "inbound",
      "quantity": 50,
      "reference": "GRN-12345",
      "balance_after": 82
    },
    {
      "date": "2025-11-05",
      "type": "outbound",
      "quantity": -50,
      "reference": "INV-AFT-HR26-001848",
      "balance_after": 32
    }
  ],
  "aging_analysis": {
    "0_30_days": 20,
    "31_60_days": 12,
    "61_90_days": 0,
    "over_90_days": 0
  }
}
```

### Get Low Stock Alerts

```http
GET /inventory/alerts/low-stock
```

**Response:**
```json
{
  "alerts": [
    {
      "product_name": "Black Mustard Oil - 2L HDPE (CLOSED)",
      "product_sku_code": "CP09-OILL-BLMS-HDPE-2LTR",
      "current_stock": 0,
      "lower_threshold": 10,
      "days_until_stockout": 0,
      "recommended_order_quantity": 50,
      "priority": "critical"
    }
  ],
  "summary": {
    "critical_alerts": 5,
    "warning_alerts": 15,
    "total_alerts": 20
  }
}
```

---

## Orders & Fulfillment API

### List Orders

```http
GET /orders
```

**Query Parameters:**
- `po_number` (string): Filter by PO number
- `customer_id` (string): Filter by customer
- `order_type` (string): Filter by order type - `Zepto`, `Blinkit`, `Swiggy`, `Amazon`, `JWL`
- `order_status` (string): Filter by status - `pending`, `approved`, `in_transit`, `delivered`, `cancelled`
- `dispatch_status` (boolean): Filter by dispatch status
- `delivery_status` (boolean): Filter by delivery status
- `date_from` (date): Start date (YYYY-MM-DD)
- `date_to` (date): End date (YYYY-MM-DD)
- `warehouse` (string): Filter by warehouse
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "order_id": "P2551659",
      "po_number": "P2551659",
      "invoice_number": "AFT-HR26-001848",
      "order_type": "Zepto",
      "order_status": "in_transit",
      "warehouse": "JWL Store",
      "customer": {
        "customer_id": "B2BCUST-XXX",
        "customer_name": "Zepto Private Limited - Telangana",
        "gst_number": "36AAICK4821A1ZW",
        "shipping_address": "HYD-DRY-MH2-KANDLAKOYA"
      },
      "dates": {
        "po_date": "2025-11-03T14:52:00Z",
        "po_expiry_date": "2025-12-18T23:59:00Z",
        "punch_date": "2025-11-03T16:23:33Z",
        "approved_date": "2025-11-04T09:59:03Z",
        "invoice_date": "2025-11-04T17:13:00Z",
        "dispatch_date": "2025-11-05T11:35:33Z",
        "delivery_date": null
      },
      "financial": {
        "total_taxable_value": 41933.33,
        "total_amount_with_tax": 44030.00,
        "currency": "INR"
      },
      "logistics": {
        "mode_of_transport": null,
        "carrier": "Omkar Logistics",
        "awb_number": "292770760",
        "dispatch_status": true,
        "delivery_status": false
      },
      "items_summary": {
        "total_items": 3,
        "total_quantity_ordered": 112,
        "total_quantity_approved": 112,
        "total_quantity_fulfilled": 92,
        "fulfillment_rate": 82.14
      },
      "proof_documents": {
        "has_po_proof": true,
        "po_proof_url": "https://filflo-s3.blr1.cdn.digitaloceanspaces.com/anveshan/proof-of-po/1762167319298-po-690889ad5e53b11f3e0d0207.pdf",
        "has_delivery_proof": false,
        "delivery_proof_url": null,
        "has_grn_proof": false,
        "grn_proof_url": null,
        "has_dispatch_proof": false,
        "dispatch_proof_url": null
      }
    }
  ],
  "summary": {
    "total_orders": 1250,
    "total_value": 5500000,
    "by_status": {
      "pending": 45,
      "approved": 120,
      "in_transit": 85,
      "delivered": 950,
      "cancelled": 50
    }
  },
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 1250,
    "total_pages": 25
  }
}
```

### Get Order Details

```http
GET /orders/{order_id}
```

**Response:**
```json
{
  "order_id": "P2551659",
  "po_number": "P2551659",
  "invoice_number": "AFT-HR26-001848",
  "order_type": "Zepto",
  "order_status": "in_transit",
  "warehouse": "JWL Store",
  "customer": {
    "customer_id": "B2BCUST-XXX",
    "customer_name": "Zepto Private Limited - Telangana",
    "gst_number": "36AAICK4821A1ZW",
    "billing_address": "Full billing address...",
    "shipping_address": "HYD-DRY-MH2-KANDLAKOYA"
  },
  "dates": {
    "po_date": "2025-11-03T14:52:00Z",
    "po_expiry_date": "2025-12-18T23:59:00Z",
    "punch_date": "2025-11-03T16:23:33Z",
    "approved_date": "2025-11-04T09:59:03Z",
    "invoice_date": "2025-11-04T17:13:00Z",
    "dispatch_date": "2025-11-05T11:35:33Z",
    "delivery_date": null
  },
  "line_items": [
    {
      "sku_code": "FPCL-OILL-SNFW-PETT-1LTR",
      "product_name": "Sunflower Oil - 1000mL PET Bottle (CLOSED)",
      "quantities": {
        "ordered_cases": 80,
        "approved_cases": 80,
        "fulfilled_cases": 80,
        "ordered_units": 80,
        "approved_units": 80,
        "fulfilled_units": 80,
        "grn_units": null
      },
      "pricing": {
        "item_taxable_value": 25333.33,
        "item_value_with_tax": 26600.00
      },
      "fulfillment": {
        "approval_remarks": "No change",
        "fulfillment_remarks": null,
        "grn_remarks": null,
        "order_vs_fulfilled_percentage": 100.00,
        "approved_vs_fulfilled_percentage": 100.00
      }
    }
  ],
  "financial": {
    "total_taxable_value": 41933.33,
    "total_amount_with_tax": 44030.00,
    "currency": "INR",
    "payment_terms": null
  },
  "logistics": {
    "mode_of_transport": null,
    "carrier": "Omkar Logistics",
    "awb_number": "292770760",
    "eway_bill_number": null,
    "dispatch_status": true,
    "delivery_status": false,
    "tracking_url": null
  },
  "proof_documents": {
    "has_po_proof": true,
    "po_proof_url": "https://filflo-s3.blr1.cdn.digitaloceanspaces.com/anveshan/proof-of-po/1762167319298-po-690889ad5e53b11f3e0d0207.pdf",
    "has_delivery_proof": false,
    "delivery_proof_url": null,
    "has_grn_proof": false,
    "grn_proof_url": null,
    "has_dispatch_proof": false,
    "dispatch_proof_url": null
  }
}
```

### Get Order Fulfillment Status

```http
GET /orders/{order_id}/fulfillment
```

**Response:**
```json
{
  "order_id": "P2551659",
  "po_number": "P2551659",
  "overall_status": "in_transit",
  "fulfillment_metrics": {
    "total_skus": 3,
    "fully_fulfilled_skus": 1,
    "partially_fulfilled_skus": 2,
    "unfulfilled_skus": 0,
    "overall_fulfillment_rate": 82.14
  },
  "timeline": [
    {
      "event": "Order Placed",
      "timestamp": "2025-11-03T14:52:00Z",
      "status": "completed"
    },
    {
      "event": "Order Approved",
      "timestamp": "2025-11-04T09:59:03Z",
      "status": "completed"
    },
    {
      "event": "Invoice Generated",
      "timestamp": "2025-11-04T17:13:00Z",
      "status": "completed",
      "reference": "AFT-HR26-001848"
    },
    {
      "event": "Dispatched",
      "timestamp": "2025-11-05T11:35:33Z",
      "status": "completed",
      "carrier": "Omkar Logistics",
      "awb": "292770760"
    },
    {
      "event": "In Transit",
      "timestamp": "2025-11-05T12:00:00Z",
      "status": "in_progress"
    },
    {
      "event": "Delivered",
      "timestamp": null,
      "status": "pending"
    }
  ],
  "line_items_status": [
    {
      "sku_code": "FPCL-OILL-SNFW-PETT-1LTR",
      "product_name": "Sunflower Oil - 1000mL PET Bottle (CLOSED)",
      "ordered": 80,
      "approved": 80,
      "fulfilled": 80,
      "fulfillment_percentage": 100.00,
      "status": "fully_fulfilled"
    },
    {
      "sku_code": "FPCL-M300-BLMS-PETT-1LTR",
      "product_name": "Black Mustard Oil - 1000mL PET Bottle (CLOSED)",
      "ordered": 16,
      "approved": 16,
      "fulfilled": 12,
      "fulfillment_percentage": 75.00,
      "status": "partially_fulfilled",
      "remarks": "Partial Shipment Requested"
    }
  ]
}
```

---

## Customers API

### List Customers

```http
GET /customers
```

**Query Parameters:**
- `customer_id` (string): Filter by customer ID
- `customer_name` (string): Search by name
- `rate_card` (string): Filter by rate card
- `status` (string): Filter by status - `active`, `inactive`
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "customer_id": "B2BCUST-346",
      "customer_name": "Zepto Private Limited-BLR-DRY-MH-Sumadhura2",
      "business_name": "Zepto Private Limited-BLR-DRY-MH-Sumadhura2",
      "gst_number": "N/A",
      "contact": {
        "phone": "N/A",
        "email": null
      },
      "addresses": {
        "billing_address": "Block-B, Sumadhura Logistics Park, 11, Hobli, Doddanehalli...",
        "shipping_address": "Block-B, Sumadhura Logistics Park, 11, Hobli, Doddanehalli..."
      },
      "preferences": {
        "rate_card_reference": "Zepto Margin ( 30 %)",
        "document_type_preference": "invoice"
      },
      "created_on": "2025-10-23",
      "remarks": null
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 200,
    "total_pages": 4
  }
}
```

### Get Customer Details

```http
GET /customers/{customer_id}
```

**Response:**
```json
{
  "customer_id": "B2BCUST-346",
  "customer_name": "Zepto Private Limited-BLR-DRY-MH-Sumadhura2",
  "business_name": "Zepto Private Limited-BLR-DRY-MH-Sumadhura2",
  "gst_number": "N/A",
  "contact": {
    "phone": "N/A",
    "email": null
  },
  "addresses": {
    "billing_address": "Block-B, Sumadhura Logistics Park...",
    "shipping_address": "Block-B, Sumadhura Logistics Park..."
  },
  "preferences": {
    "rate_card_reference": "Zepto Margin ( 30 %)",
    "document_type_preference": "invoice"
  },
  "delivery_locations": [
    {
      "location_code": "BLR-DRY-MH-Sumadhura2",
      "status": "Active",
      "created_on": "2025-10-23",
      "updated_on": "2025-10-23"
    }
  ],
  "order_history": {
    "total_orders": 45,
    "total_value": 1250000,
    "average_order_value": 27777.78,
    "last_order_date": "2025-11-05"
  },
  "financial_summary": {
    "total_invoiced": 1250000,
    "total_paid": 1100000,
    "outstanding": 150000,
    "credit_limit": 500000
  },
  "top_products": [
    {
      "sku_code": "FPCL-OILL-SNFW-PETT-1LTR",
      "product_name": "Sunflower Oil - 1000mL PET Bottle",
      "total_quantity": 500,
      "total_value": 150000
    }
  ],
  "created_on": "2025-10-23",
  "remarks": null
}
```

### Get Customer Delivery Locations

```http
GET /customers/{customer_id}/delivery-locations
```

**Response:**
```json
{
  "customer_id": "B2BCUST-346",
  "customer_name": "Zepto Private Limited-BLR-DRY-MH-Sumadhura2",
  "delivery_locations": [
    {
      "location_code": "BLR-DRY-MH-Sumadhura2",
      "status": "Active",
      "address": "Block-B, Sumadhura Logistics Park...",
      "created_on": "2025-10-23",
      "updated_on": "2025-10-23",
      "recent_deliveries": 15,
      "last_delivery_date": "2025-11-05"
    }
  ]
}
```

---

## Suppliers API

### List Suppliers

```http
GET /suppliers
```

**Query Parameters:**
- `supplier_id` (string): Filter by supplier ID
- `name` (string): Search by name
- `status` (string): Filter by status - `active`, `inactive`
- `city` (string): Filter by city
- `state` (string): Filter by state
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "supplier_id": "SUP-1",
      "name": "V S NATURALS AGRO FOODS",
      "contact": {
        "email": "vsagro74@gmail.com",
        "phone": "9049008874",
        "point_of_contact": "Somnath",
        "poc_phone": "9049008874",
        "poc_email": "N/A"
      },
      "tax_info": {
        "gst_number": "27AAVFV3380R1ZY",
        "pan_number": "AAVFV3380R"
      },
      "address": {
        "address_line": "Bhandure Industrial estate, Bajarng Nagar, Satpur",
        "city": "Nashik",
        "state": "Maharashtra",
        "pincode": "422007"
      },
      "bank_details": {
        "bank_name": "N/A",
        "account_holder": "N/A",
        "account_number": "N/A",
        "ifsc_code": "N/A",
        "branch": "N/A"
      },
      "status": "active",
      "website": "N/A",
      "remarks": "N/A"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 50,
    "total_pages": 1
  }
}
```

### Get Supplier Details

```http
GET /suppliers/{supplier_id}
```

**Response:**
```json
{
  "supplier_id": "SUP-1",
  "name": "V S NATURALS AGRO FOODS",
  "contact": {
    "email": "vsagro74@gmail.com",
    "phone": "9049008874",
    "point_of_contact": "Somnath",
    "poc_phone": "9049008874",
    "poc_email": "N/A"
  },
  "tax_info": {
    "gst_number": "27AAVFV3380R1ZY",
    "pan_number": "AAVFV3380R"
  },
  "address": {
    "address_line": "Bhandure Industrial estate, Bajarng Nagar, Satpur",
    "city": "Nashik",
    "state": "Maharashtra",
    "pincode": "422007"
  },
  "bank_details": {
    "bank_name": "N/A",
    "account_holder": "N/A",
    "account_number": "N/A",
    "ifsc_code": "N/A",
    "branch": "N/A"
  },
  "purchase_history": {
    "total_purchase_orders": 125,
    "total_value": 5500000,
    "average_po_value": 44000,
    "last_po_date": "2025-10-30"
  },
  "top_products_supplied": [
    {
      "sku_code": "FPCL-HONY-WDFT-GJAR-01KG",
      "product_name": "Wild Forest Honey -1kg",
      "total_quantity": 5000,
      "total_value": 975000
    }
  ],
  "performance_metrics": {
    "on_time_delivery_rate": 92.5,
    "quality_acceptance_rate": 98.2,
    "average_lead_time_days": 7
  },
  "status": "active",
  "website": "N/A",
  "remarks": "N/A"
}
```

---

## Purchase Orders & GRN API

### List Purchase Orders

```http
GET /purchase-orders
```

**Query Parameters:**
- `po_number` (string): Filter by PO number
- `supplier_name` (string): Filter by supplier
- `status` (string): Filter by status - `Pending`, `Completed`, `Partially Received`
- `product_category` (string): Filter by product category
- `date_from` (date): Start date (YYYY-MM-DD)
- `date_to` (date): End date (YYYY-MM-DD)
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "po_number": "WELL/30102025/6474",
      "po_date": "2025-10-30",
      "status": "Pending",
      "supplier_name": "Wellness shots",
      "product": {
        "category": "HONY - Raw Honey",
        "name": "Wild Forest Honey -1kg",
        "code": "FPCL-HONY-WDFT-GJAR-01KG",
        "uom": "units"
      },
      "pricing": {
        "po_rate_with_tax": 199.5,
        "po_rate_without_tax": 190.00,
        "tax_rate": "5%",
        "tax_amount": 9.50
      },
      "quantities": {
        "po_quantity": 1500,
        "invoice_quantity": null,
        "grn_quantity": null,
        "grn_boxes": null
      },
      "grn_details": {
        "grn_number": null,
        "grn_date": null,
        "supplier_invoice_number": null,
        "batch_number": null,
        "manufacturing_date": null,
        "expiry_date": null,
        "quantity_received": null,
        "received_date": null
      },
      "terms": {
        "delivery_lead_time": null,
        "freight_charges": null
      },
      "remarks": null
    }
  ],
  "summary": {
    "total_pos": 450,
    "total_value": 8500000,
    "by_status": {
      "pending": 125,
      "completed": 280,
      "partially_received": 45
    }
  },
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 450,
    "total_pages": 9
  }
}
```

### Get Purchase Order Details

```http
GET /purchase-orders/{po_number}
```

**Response:**
```json
{
  "po_number": "WELL/30102025/6474",
  "po_date": "2025-10-30",
  "status": "Pending",
  "supplier_name": "Wellness shots",
  "supplier_details": {
    "supplier_id": "406568000089209001",
    "contact": {
      "email": "npathak@wellnessshot.in",
      "phone": "9811946546",
      "poc": "Neeraj kumar Pathak"
    },
    "address": "C-157, Hosiery Complex, Phase - 2, Noida, UP - 201305"
  },
  "product": {
    "category": "HONY - Raw Honey",
    "name": "Wild Forest Honey -1kg",
    "code": "FPCL-HONY-WDFT-GJAR-01KG",
    "uom": "units"
  },
  "pricing": {
    "po_rate_with_tax": 199.5,
    "po_rate_without_tax": 190.00,
    "tax_rate": "5%",
    "tax_amount": 9.50,
    "total_value_without_tax": 285000.00,
    "total_value_with_tax": 299250.00
  },
  "quantities": {
    "po_quantity": 1500,
    "invoice_quantity": null,
    "grn_quantity": null,
    "grn_boxes": null,
    "pending_quantity": 1500
  },
  "grn_history": [],
  "terms": {
    "delivery_lead_time": null,
    "freight_charges": null,
    "payment_terms": null
  },
  "remarks": null,
  "audit_trail": [
    {
      "action": "PO Created",
      "timestamp": "2025-10-30T10:00:00Z",
      "user": "procurement_team"
    }
  ]
}
```

### List GRNs (Goods Receipt Notes)

```http
GET /grns
```

**Query Parameters:**
- `grn_number` (string): Filter by GRN number
- `po_number` (string): Filter by PO number
- `supplier_name` (string): Filter by supplier
- `date_from` (date): Start date
- `date_to` (date): End date
- `product_code` (string): Filter by product
- `batch_number` (string): Filter by batch
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "grn_number": "GRN-2025-001234",
      "grn_date": "2025-11-02",
      "po_number": "RAIN/29102025/3213",
      "supplier_name": "Rainier Foods and Health Pvt Ltd",
      "supplier_invoice_number": "SUP-INV-12345",
      "product": {
        "code": "FPCL-ATTA-KPLI-POUC-01KG",
        "name": "Khapli Atta 1 Kg",
        "category": "ATTA - Flours & Grains"
      },
      "quantities": {
        "po_quantity": 480,
        "received_quantity": 480,
        "accepted_quantity": 480,
        "rejected_quantity": 0,
        "number_of_boxes": 40
      },
      "batch_details": {
        "batch_number": "BATCH-20251102-001",
        "manufacturing_date": "2025-10-15",
        "expiry_date": "2026-04-15"
      },
      "received_date": "2025-11-02",
      "quality_status": "Approved",
      "remarks": null
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 280,
    "total_pages": 6
  }
}
```

### Get GRN Details

```http
GET /grns/{grn_number}
```

**Response:**
```json
{
  "grn_number": "GRN-2025-001234",
  "grn_date": "2025-11-02",
  "received_date": "2025-11-02T10:30:00Z",
  "po_reference": {
    "po_number": "RAIN/29102025/3213",
    "po_date": "2025-10-29",
    "po_quantity": 480
  },
  "supplier": {
    "name": "Rainier Foods and Health Pvt Ltd",
    "invoice_number": "SUP-INV-12345",
    "invoice_date": "2025-11-01"
  },
  "product": {
    "code": "FPCL-ATTA-KPLI-POUC-01KG",
    "name": "Khapli Atta 1 Kg",
    "category": "ATTA - Flours & Grains",
    "uom": "units"
  },
  "quantities": {
    "ordered": 480,
    "received": 480,
    "accepted": 480,
    "rejected": 0,
    "variance": 0,
    "number_of_boxes": 40,
    "units_per_box": 12
  },
  "batch_details": {
    "batch_number": "BATCH-20251102-001",
    "manufacturing_date": "2025-10-15",
    "expiry_date": "2026-04-15",
    "shelf_life_days": 182
  },
  "quality_inspection": {
    "status": "Approved",
    "tested_by": "QC Team",
    "test_date": "2025-11-02",
    "test_report_url": null,
    "remarks": "Quality standards met"
  },
  "warehouse_location": {
    "warehouse": "Main Warehouse",
    "location": "A-15-2",
    "received_by": "warehouse_staff"
  },
  "pricing": {
    "rate_per_unit": 127.05,
    "total_value": 61000.00
  },
  "remarks": null,
  "documents": {
    "supplier_invoice": "https://...",
    "quality_report": null,
    "packing_list": null
  }
}
```

---

## Invoices & Credit Notes API

### List Invoices

```http
GET /invoices
```

**Query Parameters:**
- `invoice_number` (string): Filter by invoice number
- `po_number` (string): Filter by PO number
- `customer_id` (string): Filter by customer
- `warehouse` (string): Filter by warehouse
- `invoice_status` (string): Filter by status - `Invoiced`, `Partially Paid`, `Paid`, `Cancelled`
- `voucher_type` (string): Filter by type - `Tax Invoice`, `Delivery Note`
- `date_from` (date): Start date
- `date_to` (date): End date
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "invoice_number": "DC-25/26-0000254",
      "voucher_type": "Delivery Note",
      "invoice_date": "2025-11-05T11:28:23Z",
      "warehouse": "Anveshan Manesar",
      "po_details": {
        "po_number": "24092025_JWL_ColdStore_101",
        "po_date": "2025-11-05",
        "order_type": "JWL"
      },
      "customer": {
        "customer_name": "Anveshan Farm Tech (JWL Cold Store Pvt Ltd.)",
        "gst_number": "06AATCA2081M1Z9",
        "shipping_address": "Khasra no 70/1/2/2, FARRUKHNAGAR...",
        "billing_address": "Khasra no 70/1/2/2, FARRUKHNAGAR...",
        "state": "Haryana",
        "city": "FARRUKHNAGAR",
        "pincode": "122506"
      },
      "financial": {
        "total_taxable_value": 50160.00,
        "total_amount_with_tax": 52668.00,
        "currency": "INR"
      },
      "invoice_status": "Invoiced",
      "order_status": "invoiced",
      "irn_details": {
        "irn_number": "N/A",
        "irn_date": "2025-11-05T11:28:23Z",
        "acknowledgement_number": "N/A",
        "acknowledgement_date": "2025-11-05T11:28:23Z"
      },
      "logistics": {
        "mode_of_transport": null,
        "awb_number": null,
        "eway_bill_number": null
      },
      "items_count": 2,
      "dispatch_date": null,
      "delivery_date": null
    }
  ],
  "summary": {
    "total_invoices": 850,
    "total_value": 12500000,
    "by_status": {
      "invoiced": 450,
      "partially_paid": 250,
      "paid": 100,
      "cancelled": 50
    }
  },
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 850,
    "total_pages": 17
  }
}
```

### Get Invoice Details

```http
GET /invoices/{invoice_number}
```

**Response:**
```json
{
  "invoice_number": "DC-25/26-0000254",
  "voucher_type": "Delivery Note",
  "invoice_date": "2025-11-05T11:28:23Z",
  "warehouse": "Anveshan Manesar",
  "po_details": {
    "po_number": "24092025_JWL_ColdStore_101",
    "po_date": "2025-11-05",
    "po_expiry_date": null,
    "punch_date": "2025-11-05T11:12:23Z",
    "order_id": "24092025_JWL_ColdStore_101",
    "order_type": "JWL"
  },
  "customer": {
    "customer_name": "Anveshan Farm Tech (JWL Cold Store Pvt Ltd.)",
    "gst_number": "06AATCA2081M1Z9",
    "shipping_address": "Khasra no 70/1/2/2, 3,8,9,10/1,11/2, 12,13/1, HAILY MANDI ROAD, FARRUKHNAGAR, KHERA KHURRAMPUR",
    "billing_address": "Khasra no 70/1/2/2, 3,8,9,10/1,11/2, 12,13/1, HAILY MANDI ROAD, FARRUKHNAGAR, KHERA KHURRAMPUR",
    "state": "Haryana",
    "city": "FARRUKHNAGAR",
    "pincode": "122506"
  },
  "line_items": [
    {
      "sku_code": "CP32-GHEE-HLKR-GJAR-150M",
      "sku_name": "A2 Hallikar Cow Ghee - 150mL ( CLOSED ) - CASEPACK",
      "quantities": {
        "ordered": 7,
        "approved": 7,
        "fulfilled": 6,
        "grn_quantity": null
      },
      "pricing": {
        "mrp": 9120,
        "mrp_without_tax": 8685.71,
        "selling_price": 3648.00,
        "selling_price_without_tax": 3474.29,
        "margin_percentage": 60
      },
      "tax": {
        "gst_rate": "5%",
        "taxable_amount": 20845.71,
        "igst_amount": 0.00,
        "sgst_amount": 521.14,
        "cgst_amount": 521.14,
        "total_tax": 1042.28
      },
      "total_item_value": 21888.00,
      "fulfillment": {
        "approval_remarks": null,
        "fulfillment_remarks": "Partial Shipment Requested",
        "grn_remarks": null
      }
    },
    {
      "sku_code": "OP18-GHEE-HLKR-GJAR-150M",
      "sku_name": "A2 Hallikar Cow Ghee - 150mL (OPEN) - CASEPACK",
      "quantities": {
        "ordered": 15,
        "approved": 15,
        "fulfilled": 15,
        "grn_quantity": null
      },
      "pricing": {
        "mrp": 5130,
        "mrp_without_tax": 4885.71,
        "selling_price": 2052.00,
        "selling_price_without_tax": 1954.29,
        "margin_percentage": 60
      },
      "tax": {
        "gst_rate": "5%",
        "taxable_amount": 29314.29,
        "igst_amount": 0.00,
        "sgst_amount": 732.86,
        "cgst_amount": 732.86,
        "total_tax": 1465.72
      },
      "total_item_value": 30780.00,
      "fulfillment": {
        "approval_remarks": null,
        "fulfillment_remarks": "No change",
        "grn_remarks": null
      }
    }
  ],
  "financial_summary": {
    "total_taxable_value": 50160.00,
    "total_igst": 0.00,
    "total_sgst": 1253.99,
    "total_cgst": 1253.99,
    "total_tax": 2508.00,
    "total_amount_with_tax": 52668.00,
    "currency": "INR"
  },
  "invoice_status": "Invoiced",
  "order_status": "invoiced",
  "irn_details": {
    "irn_number": "N/A",
    "irn_date": "2025-11-05T11:28:23Z",
    "acknowledgement_number": "N/A",
    "acknowledgement_date": "2025-11-05T11:28:23Z"
  },
  "logistics": {
    "mode_of_transport": null,
    "awb_number": null,
    "eway_bill_number": null
  },
  "dates": {
    "invoice_date": "2025-11-05T11:28:23Z",
    "dispatch_date": null,
    "delivery_date": null
  },
  "proof_documents": {
    "has_po_proof": true,
    "po_proof_url": "https://filflo-s3.blr1.cdn.digitaloceanspaces.com/anveshan/proof-of-po/1762321415605-po-690ae3bfc043b15155376f35.csv",
    "has_delivery_proof": false,
    "delivery_proof_url": null,
    "has_grn_proof": false,
    "grn_proof_url": null,
    "has_dispatch_proof": false,
    "dispatch_proof_url": null
  }
}
```

### List Credit Notes

```http
GET /credit-notes
```

**Query Parameters:**
- `credit_note_number` (string): Filter by credit note number
- `original_invoice_number` (string): Filter by original invoice
- `customer_id` (string): Filter by customer
- `warehouse` (string): Filter by warehouse
- `status` (string): Filter by status
- `date_from` (date): Start date
- `date_to` (date): End date
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "credit_note_number": "CN-2025-001234",
      "credit_note_date": "2025-11-03",
      "original_invoice": {
        "invoice_number": "AFT-HR26-001848",
        "invoice_date": "2025-11-04"
      },
      "warehouse": "JWL Store",
      "voucher_type": "Credit Note",
      "punch_date": "2025-11-03T16:00:00Z",
      "customer": {
        "customer_name": "Zepto Private Limited - Telangana",
        "gst_number": "36AAICK4821A1ZW",
        "shipping_address": "HYD-DRY-MH2-KANDLAKOYA",
        "billing_address": "HYD-DRY-MH2-KANDLAKOYA",
        "state": "Telangana",
        "city": "Hyderabad",
        "pincode": "500001"
      },
      "financial": {
        "total_credit_note_taxable_value": 2000.00,
        "total_credit_note_amount_with_tax": 2100.00,
        "currency": "INR"
      },
      "status": "Approved",
      "irn_details": {
        "irn_number": "IRN123456789",
        "irn_date": "2025-11-03",
        "acknowledgement_number": "ACK123456",
        "acknowledgement_date": "2025-11-03"
      },
      "reason": "Product damage during transit"
    }
  ],
  "summary": {
    "total_credit_notes": 45,
    "total_value": 125000
  },
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 45,
    "total_pages": 1
  }
}
```

---

## Transporters & Logistics API

### List Transporters

```http
GET /transporters
```

**Response:**
```json
{
  "data": [
    {
      "transporter_id": "29AAICA3918J1ZE",
      "transporter_name": "Amazon Freight --",
      "created_by": "Main Admin",
      "updated_by": "N/A",
      "active_shipments": 25,
      "total_shipments": 450,
      "on_time_delivery_rate": 94.5
    },
    {
      "transporter_id": "36AADCG2096A1ZY",
      "transporter_name": "Gati solution pvt ltd",
      "created_by": "Main Admin",
      "updated_by": "Main Admin",
      "active_shipments": 15,
      "total_shipments": 320,
      "on_time_delivery_rate": 89.2
    }
  ]
}
```

### Get Transporter Performance

```http
GET /transporters/{transporter_id}/performance
```

**Query Parameters:**
- `date_from` (date): Start date
- `date_to` (date): End date

**Response:**
```json
{
  "transporter_id": "36AADCG2096A1ZY",
  "transporter_name": "Gati solution pvt ltd",
  "performance_period": {
    "from": "2025-10-01",
    "to": "2025-11-05"
  },
  "metrics": {
    "total_shipments": 85,
    "on_time_deliveries": 76,
    "delayed_deliveries": 8,
    "failed_deliveries": 1,
    "on_time_delivery_rate": 89.41,
    "average_delay_days": 1.5
  },
  "shipment_breakdown": {
    "by_status": {
      "in_transit": 15,
      "delivered": 68,
      "delayed": 1,
      "returned": 1
    },
    "by_destination_state": {
      "Maharashtra": 35,
      "Karnataka": 25,
      "Tamil Nadu": 15,
      "Telangana": 10
    }
  },
  "cost_analysis": {
    "total_freight_cost": 125000,
    "average_cost_per_shipment": 1470.59
  }
}
```

### Track Shipment

```http
GET /shipments/{awb_number}/track
```

**Response:**
```json
{
  "awb_number": "292770760",
  "order_id": "P2551659",
  "carrier": "Omkar Logistics",
  "current_status": "in_transit",
  "origin": {
    "warehouse": "JWL Store",
    "city": "Manesar",
    "state": "Haryana"
  },
  "destination": {
    "customer": "Zepto Private Limited - Telangana",
    "address": "HYD-DRY-MH2-KANDLAKOYA",
    "city": "Hyderabad",
    "state": "Telangana"
  },
  "dates": {
    "dispatch_date": "2025-11-05T11:35:33Z",
    "expected_delivery_date": "2025-11-07T18:00:00Z",
    "actual_delivery_date": null
  },
  "tracking_events": [
    {
      "timestamp": "2025-11-05T11:35:33Z",
      "status": "Dispatched",
      "location": "JWL Store, Manesar",
      "description": "Shipment dispatched from warehouse"
    },
    {
      "timestamp": "2025-11-05T14:20:00Z",
      "status": "In Transit",
      "location": "Delhi Hub",
      "description": "Shipment in transit to Delhi hub"
    },
    {
      "timestamp": "2025-11-05T18:45:00Z",
      "status": "In Transit",
      "location": "Delhi Hub",
      "description": "Shipment sorted at Delhi hub"
    }
  ],
  "shipment_details": {
    "total_items": 3,
    "total_boxes": 5,
    "total_weight": "45 kg",
    "invoice_value": 44030.00
  }
}
```

---

## Quality Management API

### List Quality Reports

```http
GET /quality-reports
```

**Query Parameters:**
- `batch_number` (string): Filter by batch number
- `product_code` (string): Filter by product
- `tested_by` (string): Filter by tester
- `status` (string): Filter by status - `Approved`, `Rejected`, `Pending`
- `date_from` (date): Start date
- `date_to` (date): End date
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "batch_number": "596680",
      "product": {
        "product_code": "FPCL-OILL-BLMS-PETT-1LTR",
        "product_name": "Black Mustard Oil"
      },
      "tested_by": "Sandip@Anveshan.Farm",
      "test_date": "2025-10-25",
      "quantities": {
        "received_quantity": 20295,
        "approved_quantity": 20295,
        "rejected_quantity": 0,
        "approval_rate": 100.00
      },
      "quality_status": "Approved",
      "test_parameters": {
        "appearance": "Pass",
        "odor": "Pass",
        "taste": "Pass",
        "ffa": "0.2%",
        "moisture": "0.05%"
      },
      "remarks": null
    }
  ],
  "summary": {
    "total_batches_tested": 125,
    "approved_batches": 122,
    "rejected_batches": 3,
    "overall_approval_rate": 97.6
  },
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 125,
    "total_pages": 3
  }
}
```

### Get Quality Report by Batch

```http
GET /quality-reports/{batch_number}
```

**Response:**
```json
{
  "batch_number": "596680",
  "product": {
    "product_code": "FPCL-OILL-BLMS-PETT-1LTR",
    "product_name": "Black Mustard Oil",
    "category": "OILL - Oil Products"
  },
  "supplier": {
    "supplier_name": "V S NATURALS AGRO FOODS",
    "supplier_id": "SUP-1"
  },
  "tested_by": "Sandip@Anveshan.Farm",
  "test_date": "2025-10-25T10:00:00Z",
  "quantities": {
    "received_quantity": 20295,
    "approved_quantity": 20295,
    "rejected_quantity": 0,
    "approval_rate": 100.00
  },
  "quality_status": "Approved",
  "test_parameters": {
    "physical_tests": {
      "appearance": "Pass",
      "color": "Amber yellow",
      "odor": "Pass",
      "taste": "Pass"
    },
    "chemical_tests": {
      "ffa_percent": 0.2,
      "moisture_percent": 0.05,
      "peroxide_value": 2.5,
      "iodine_value": 98.5
    },
    "microbiological_tests": {
      "total_plate_count": "< 1000 cfu/ml",
      "yeast_mold": "< 10 cfu/ml",
      "e_coli": "Negative",
      "salmonella": "Negative"
    }
  },
  "batch_details": {
    "manufacturing_date": "2025-10-15",
    "expiry_date": "2026-10-15",
    "batch_size": 20295
  },
  "grn_reference": {
    "grn_number": "GRN-2025-001200",
    "po_number": "RAIN/29102025/3213"
  },
  "documents": {
    "test_report_pdf": "https://...",
    "coa_certificate": "https://..."
  },
  "remarks": "All parameters within acceptable range",
  "approved_by": "Quality Manager",
  "approved_date": "2025-10-25T15:30:00Z"
}
```

### Get Quality Metrics

```http
GET /quality-metrics
```

**Query Parameters:**
- `date_from` (date): Start date
- `date_to` (date): End date
- `supplier_id` (string): Filter by supplier
- `product_category` (string): Filter by category

**Response:**
```json
{
  "period": {
    "from": "2025-10-01",
    "to": "2025-11-05"
  },
  "overall_metrics": {
    "total_batches_tested": 125,
    "approved_batches": 122,
    "rejected_batches": 3,
    "approval_rate": 97.6,
    "average_test_turnaround_hours": 4.5
  },
  "by_supplier": [
    {
      "supplier_id": "SUP-1",
      "supplier_name": "V S NATURALS AGRO FOODS",
      "batches_tested": 45,
      "approved": 45,
      "rejected": 0,
      "approval_rate": 100.0
    }
  ],
  "by_product_category": [
    {
      "category": "OILL - Oil Products",
      "batches_tested": 60,
      "approved": 58,
      "rejected": 2,
      "approval_rate": 96.67
    }
  ],
  "rejection_reasons": [
    {
      "reason": "High FFA content",
      "count": 2,
      "percentage": 66.67
    },
    {
      "reason": "Off odor",
      "count": 1,
      "percentage": 33.33
    }
  ]
}
```

---

## Rate Cards & Pricing API

### List Rate Cards

```http
GET /rate-cards
```

**Query Parameters:**
- `rate_card_name` (string): Filter by rate card name
- `rate_card_type` (string): Filter by type
- `product_sku` (string): Filter by product SKU
- `page` (integer): Page number
- `limit` (integer): Items per page

**Response:**
```json
{
  "data": [
    {
      "rate_card_name": "JWL-CP-60%",
      "rate_card_type": "undefined",
      "product_name": "Groundnut Oil 3L HDPE CAN (OPEN) Case of 6",
      "product_sku_code": "OP06-OILL-GRND-HDPE-3LTR",
      "price": 3000,
      "currency": "INR",
      "created_on": "2025-11-05"
    },
    {
      "rate_card_name": "Zepto Margin ( 30 %)",
      "rate_card_type": "margin_based",
      "applicable_customers": [
        "Zepto Private Limited - BLR",
        "Zepto Private Limited - Telangana"
      ],
      "margin_percentage": 30,
      "products_count": 150
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 500,
    "total_pages": 10
  }
}
```

### Get Rate Card Details

```http
GET /rate-cards/{rate_card_name}
```

**Response:**
```json
{
  "rate_card_name": "Zepto Margin ( 30 %)",
  "rate_card_type": "margin_based",
  "margin_percentage": 30,
  "applicable_customers": [
    {
      "customer_id": "B2BCUST-346",
      "customer_name": "Zepto Private Limited-BLR-DRY-MH-Sumadhura2"
    },
    {
      "customer_id": "B2BCUST-342",
      "customer_name": "Zepto Private Limited - LKO-DRY-MH-SOHRAMAU"
    }
  ],
  "product_pricing": [
    {
      "product_sku_code": "FPCL-GHEE-HLKR-GJAR-1LTR",
      "product_name": "A2 Hallikar Cow Ghee - 1000mL (CLOSED)",
      "base_price": 2025,
      "rate_card_price": 1417.50,
      "margin": 30.00,
      "effective_date": "2025-01-01"
    }
  ],
  "created_on": "2025-01-01",
  "last_updated": "2025-10-15",
  "status": "Active"
}
```

### Get Product Pricing by Customer

```http
GET /pricing/customer/{customer_id}
```

**Query Parameters:**
- `product_sku` (string): Specific product SKU (optional)
- `category` (string): Filter by category (optional)

**Response:**
```json
{
  "customer_id": "B2BCUST-346",
  "customer_name": "Zepto Private Limited-BLR-DRY-MH-Sumadhura2",
  "rate_card": "Zepto Margin ( 30 %)",
  "pricing": [
    {
      "product_sku_code": "FPCL-GHEE-HLKR-GJAR-1LTR",
      "product_name": "A2 Hallikar Cow Ghee - 1000mL (CLOSED)",
      "category": "GHEE - Ghee Products",
      "base_price": 2025,
      "customer_price": 1417.50,
      "margin_percentage": 30.00,
      "gst_rate": "5%",
      "price_with_tax": 1488.38,
      "minimum_order_quantity": 1,
      "currency": "INR"
    }
  ]
}
```

---

## Analytics & Reports API

### Get Dashboard Summary

```http
GET /analytics/dashboard
```

**Query Parameters:**
- `date_from` (date): Start date
- `date_to` (date): End date
- `warehouse` (string): Filter by warehouse (optional)

**Response:**
```json
{
  "period": {
    "from": "2025-11-01",
    "to": "2025-11-05"
  },
  "sales": {
    "total_orders": 125,
    "total_revenue": 5500000,
    "average_order_value": 44000,
    "order_growth_percentage": 12.5,
    "revenue_growth_percentage": 15.3
  },
  "fulfillment": {
    "total_orders_fulfilled": 95,
    "fulfillment_rate": 76.0,
    "on_time_delivery_rate": 89.5,
    "average_fulfillment_time_hours": 24
  },
  "inventory": {
    "total_skus": 150,
    "in_stock_skus": 120,
    "low_stock_skus": 20,
    "out_of_stock_skus": 8,
    "negative_stock_skus": 2,
    "total_inventory_value": 12500000,
    "inventory_turnover_ratio": 4.2
  },
  "procurement": {
    "total_purchase_orders": 45,
    "total_po_value": 2500000,
    "pending_pos": 25,
    "grns_processed": 30,
    "average_po_processing_time_days": 5
  },
  "quality": {
    "batches_tested": 35,
    "approval_rate": 97.1,
    "rejected_batches": 1
  },
  "top_customers": [
    {
      "customer_name": "Zepto Private Limited",
      "total_orders": 45,
      "total_value": 1250000
    }
  ],
  "top_products": [
    {
      "product_name": "Sunflower Oil - 1000mL PET Bottle",
      "sku_code": "FPCL-OILL-SNFW-PETT-1LTR",
      "total_quantity": 2500,
      "total_value": 625000
    }
  ]
}
```

### Get Sales Analytics

```http
GET /analytics/sales
```

**Query Parameters:**
- `date_from` (date): Start date
- `date_to` (date): End date
- `group_by` (string): Group by - `day`, `week`, `month`, `customer`, `product`, `category`
- `warehouse` (string): Filter by warehouse
- `order_type` (string): Filter by order type

**Response:**
```json
{
  "period": {
    "from": "2025-10-01",
    "to": "2025-11-05"
  },
  "summary": {
    "total_orders": 450,
    "total_revenue": 12500000,
    "total_items_sold": 15000,
    "average_order_value": 27777.78,
    "unique_customers": 85
  },
  "by_day": [
    {
      "date": "2025-11-01",
      "orders": 25,
      "revenue": 650000,
      "items_sold": 450
    },
    {
      "date": "2025-11-02",
      "orders": 30,
      "revenue": 780000,
      "items_sold": 520
    }
  ],
  "by_customer": [
    {
      "customer_id": "B2BCUST-346",
      "customer_name": "Zepto Private Limited",
      "orders": 125,
      "revenue": 3500000,
      "percentage_of_total": 28.0
    }
  ],
  "by_product_category": [
    {
      "category": "GHEE - Ghee Products",
      "orders": 150,
      "revenue": 4500000,
      "items_sold": 3000,
      "percentage_of_total": 36.0
    },
    {
      "category": "OILL - Oil Products",
      "orders": 200,
      "revenue": 5000000,
      "items_sold": 6000,
      "percentage_of_total": 40.0
    }
  ],
  "by_order_type": [
    {
      "order_type": "Zepto",
      "orders": 200,
      "revenue": 5500000,
      "percentage": 44.0
    },
    {
      "order_type": "Blinkit",
      "orders": 150,
      "revenue": 4000000,
      "percentage": 32.0
    }
  ]
}
```

### Get Inventory Analytics

```http
GET /analytics/inventory
```

**Query Parameters:**
- `warehouse` (string): Filter by warehouse
- `category` (string): Filter by category
- `stock_status` (string): Filter by stock status

**Response:**
```json
{
  "summary": {
    "total_skus": 150,
    "total_inventory_value": 12500000,
    "in_stock_skus": 120,
    "low_stock_skus": 20,
    "out_of_stock_skus": 8,
    "negative_stock_skus": 2,
    "average_inventory_age_days": 25
  },
  "by_category": [
    {
      "category": "GHEE - Ghee Products",
      "total_skus": 25,
      "in_stock": 20,
      "inventory_value": 4500000,
      "average_age_days": 18
    },
    {
      "category": "OILL - Oil Products",
      "total_skus": 40,
      "in_stock": 35,
      "inventory_value": 5000000,
      "average_age_days": 22
    }
  ],
  "by_warehouse": [
    {
      "warehouse": "JWL Store",
      "total_skus": 120,
      "inventory_value": 10000000,
      "utilization_percentage": 78
    },
    {
      "warehouse": "Anveshan Manesar",
      "total_skus": 80,
      "inventory_value": 2500000,
      "utilization_percentage": 65
    }
  ],
  "aging_analysis": {
    "0_30_days": {
      "skus": 80,
      "value": 7500000,
      "percentage": 60.0
    },
    "31_60_days": {
      "skus": 40,
      "value": 3500000,
      "percentage": 28.0
    },
    "61_90_days": {
      "skus": 20,
      "value": 1000000,
      "percentage": 8.0
    },
    "over_90_days": {
      "skus": 10,
      "value": 500000,
      "percentage": 4.0
    }
  },
  "fast_moving_products": [
    {
      "product_name": "Sunflower Oil - 1000mL PET Bottle",
      "sku_code": "FPCL-OILL-SNFW-PETT-1LTR",
      "current_stock": 250,
      "average_daily_sales": 35,
      "days_to_stockout": 7
    }
  ],
  "slow_moving_products": [
    {
      "product_name": "Olive Oil 250ml Glass Bottle",
      "sku_code": "FPCL-OILL-OLIV-GBOT-250M",
      "current_stock": 150,
      "average_daily_sales": 2,
      "inventory_age_days": 65
    }
  ]
}
```

### Get Supplier Performance Analytics

```http
GET /analytics/suppliers
```

**Query Parameters:**
- `date_from` (date): Start date
- `date_to` (date): End date
- `supplier_id` (string): Filter by supplier

**Response:**
```json
{
  "period": {
    "from": "2025-10-01",
    "to": "2025-11-05"
  },
  "summary": {
    "total_suppliers": 50,
    "total_purchase_orders": 250,
    "total_po_value": 12500000,
    "average_po_value": 50000
  },
  "top_suppliers": [
    {
      "supplier_id": "SUP-1",
      "supplier_name": "V S NATURALS AGRO FOODS",
      "total_pos": 45,
      "total_value": 2500000,
      "on_time_delivery_rate": 95.5,
      "quality_approval_rate": 100.0,
      "average_lead_time_days": 5
    },
    {
      "supplier_id": "406568000089209001",
      "supplier_name": "Wellness shots",
      "total_pos": 35,
      "total_value": 1800000,
      "on_time_delivery_rate": 88.5,
      "quality_approval_rate": 97.1,
      "average_lead_time_days": 7
    }
  ],
  "performance_metrics": {
    "average_on_time_delivery_rate": 92.3,
    "average_quality_approval_rate": 97.8,
    "average_lead_time_days": 6.5
  },
  "by_product_category": [
    {
      "category": "GHEE - Ghee Products",
      "suppliers": 8,
      "total_pos": 85,
      "total_value": 4200000
    }
  ]
}
```

### Get Fulfillment Analytics

```http
GET /analytics/fulfillment
```

**Query Parameters:**
- `date_from` (date): Start date
- `date_to` (date): End date
- `warehouse` (string): Filter by warehouse
- `carrier` (string): Filter by carrier

**Response:**
```json
{
  "period": {
    "from": "2025-10-01",
    "to": "2025-11-05"
  },
  "summary": {
    "total_orders": 450,
    "orders_fulfilled": 380,
    "orders_in_transit": 50,
    "orders_delivered": 320,
    "fulfillment_rate": 84.4,
    "on_time_delivery_rate": 89.5,
    "average_fulfillment_time_hours": 24,
    "average_delivery_time_days": 3.2
  },
  "by_warehouse": [
    {
      "warehouse": "JWL Store",
      "orders_processed": 300,
      "fulfillment_rate": 85.0,
      "average_processing_time_hours": 22
    },
    {
      "warehouse": "Anveshan Manesar",
      "orders_processed": 150,
      "fulfillment_rate": 83.3,
      "average_processing_time_hours": 26
    }
  ],
  "by_carrier": [
    {
      "carrier": "Omkar Logistics",
      "shipments": 180,
      "on_time_deliveries": 165,
      "on_time_rate": 91.7,
      "average_delivery_time_days": 3.0
    },
    {
      "carrier": "Gati solution pvt ltd",
      "shipments": 120,
      "on_time_deliveries": 105,
      "on_time_rate": 87.5,
      "average_delivery_time_days": 3.5
    }
  ],
  "by_destination_state": [
    {
      "state": "Maharashtra",
      "orders": 150,
      "fulfillment_rate": 88.0,
      "average_delivery_time_days": 2.5
    },
    {
      "state": "Karnataka",
      "orders": 120,
      "fulfillment_rate": 85.0,
      "average_delivery_time_days": 3.0
    }
  ],
  "bottlenecks": [
    {
      "stage": "Approval to Invoice",
      "average_time_hours": 12,
      "orders_delayed": 45
    },
    {
      "stage": "Invoice to Dispatch",
      "average_time_hours": 18,
      "orders_delayed": 35
    }
  ]
}
```

### Export Report

```http
POST /analytics/export
```

**Request Body:**
```json
{
  "report_type": "sales" | "inventory" | "fulfillment" | "suppliers" | "custom",
  "format": "csv" | "xlsx" | "pdf",
  "date_from": "2025-10-01",
  "date_to": "2025-11-05",
  "filters": {
    "warehouse": "JWL Store",
    "customer_id": "B2BCUST-346",
    "order_type": "Zepto"
  },
  "email_to": "team@filflo.in"
}
```

**Response:**
```json
{
  "report_id": "RPT-2025-001234",
  "status": "processing",
  "estimated_completion_time": "2025-11-05T12:30:00Z",
  "download_url": null,
  "message": "Report generation in progress. You will receive an email when it's ready."
}
```

### Get Report Status

```http
GET /analytics/export/{report_id}
```

**Response:**
```json
{
  "report_id": "RPT-2025-001234",
  "status": "completed",
  "generated_at": "2025-11-05T12:25:00Z",
  "download_url": "https://api.filflo.in/reports/RPT-2025-001234.xlsx",
  "expires_at": "2025-11-12T12:25:00Z",
  "file_size_bytes": 2547890
}
```

---

## Webhooks

Subscribe to real-time events in your supply chain.

### Available Events

- `order.created` - New order placed
- `order.approved` - Order approved
- `order.dispatched` - Order dispatched
- `order.delivered` - Order delivered
- `order.cancelled` - Order cancelled
- `inventory.low_stock` - Stock below threshold
- `inventory.out_of_stock` - Stock depleted
- `invoice.created` - New invoice generated
- `grn.created` - New GRN created
- `quality.batch_approved` - Batch approved
- `quality.batch_rejected` - Batch rejected
- `po.created` - New PO created
- `po.completed` - PO fully received

### Create Webhook

```http
POST /webhooks
```

**Request Body:**
```json
{
  "url": "https://your-domain.com/webhook-endpoint",
  "events": ["order.created", "order.dispatched", "inventory.low_stock"],
  "secret": "your_webhook_secret",
  "active": true
}
```

**Response:**
```json
{
  "webhook_id": "wh_1234567890",
  "url": "https://your-domain.com/webhook-endpoint",
  "events": ["order.created", "order.dispatched", "inventory.low_stock"],
  "secret": "your_webhook_secret",
  "active": true,
  "created_at": "2025-11-05T10:00:00Z"
}
```

### Webhook Payload Example

```json
{
  "event": "order.created",
  "timestamp": "2025-11-05T14:52:00Z",
  "data": {
    "order_id": "P2551659",
    "po_number": "P2551659",
    "customer_id": "B2BCUST-XXX",
    "customer_name": "Zepto Private Limited - Telangana",
    "total_value": 44030.00,
    "items_count": 3,
    "warehouse": "JWL Store"
  }
}
```

---

## Rate Limits

- **Standard Tier:** 1000 requests per hour
- **Premium Tier:** 5000 requests per hour
- **Enterprise Tier:** Custom limits

Rate limit headers included in every response:
```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 995
X-RateLimit-Reset: 1699200000
```

---

## Error Codes

### HTTP Status Codes

- `200 OK` - Request successful
- `201 Created` - Resource created successfully
- `400 Bad Request` - Invalid request parameters
- `401 Unauthorized` - Missing or invalid authentication
- `403 Forbidden` - Insufficient permissions
- `404 Not Found` - Resource not found
- `429 Too Many Requests` - Rate limit exceeded
- `500 Internal Server Error` - Server error
- `503 Service Unavailable` - Service temporarily unavailable

### Error Response Format

```json
{
  "error": {
    "code": "INVALID_PARAMETER",
    "message": "The parameter 'date_from' is invalid. Expected format: YYYY-MM-DD",
    "field": "date_from",
    "timestamp": "2025-11-05T10:00:00Z"
  }
}
```

### Common Error Codes

- `AUTHENTICATION_FAILED` - Authentication credentials invalid
- `AUTHORIZATION_FAILED` - Insufficient permissions
- `INVALID_PARAMETER` - Invalid request parameter
- `RESOURCE_NOT_FOUND` - Requested resource not found
- `RATE_LIMIT_EXCEEDED` - Too many requests
- `VALIDATION_ERROR` - Request validation failed
- `INTERNAL_ERROR` - Internal server error

---

## Pagination

All list endpoints support pagination with the following parameters:

- `page` (integer): Page number (default: 1)
- `limit` (integer): Items per page (default: 50, max: 100)

**Response includes pagination metadata:**
```json
{
  "data": [...],
  "pagination": {
    "page": 1,
    "limit": 50,
    "total_items": 450,
    "total_pages": 9,
    "has_next": true,
    "has_previous": false
  }
}
```

---

## Filtering & Sorting

### Filtering

Most endpoints support filtering via query parameters. Refer to individual endpoint documentation for available filters.

### Sorting

Use the `sort` parameter with field name and direction:

```
GET /orders?sort=po_date:desc
GET /products?sort=product_name:asc
```

Multiple sort fields:
```
GET /orders?sort=po_date:desc,total_value:asc
```

---

## Best Practices

1. **Use appropriate endpoints** - Use specific endpoints (e.g., `/inventory/{sku}`) instead of listing all and filtering client-side
2. **Cache responses** - Cache static data like product catalogs to reduce API calls
3. **Use webhooks** - Subscribe to Filflo webhooks for real-time updates instead of polling
4. **Batch requests** - Use batch endpoints where available to reduce API calls
5. **Handle rate limits** - Implement exponential backoff when rate limited
6. **Error handling** - Implement robust error handling and retry logic with Filflo error codes
7. **Secure your credentials** - Never expose Filflo API keys in client-side code
8. **Use pagination** - Always use pagination for large datasets
9. **Monitor usage** - Track your API usage via the Filflo dashboard at https://app.filflo.in/api/usage
10. **Test in sandbox** - Use our sandbox environment at https://sandbox-api.filflo.in/v1 for testing

---

## SDK Examples

### Node.js Example
```javascript
const Filflo = require('@filflo/api-client');

const client = new Filflo({
  clientId: 'your_client_id',
  clientSecret: 'your_client_secret'
});

// Get products
const products = await client.products.list({
  category: 'GHEE - Ghee Products',
  limit: 50
});

// Get inventory
const inventory = await client.inventory.getBySku('FPCL-GHEE-HLKR-GJAR-1LTR');
```

### Python Example
```python
from filflo import FilfloClient

client = FilfloClient(
    client_id='your_client_id',
    client_secret='your_client_secret'
)

# Get orders
orders = client.orders.list(
    order_status='in_transit',
    date_from='2025-11-01'
)

# Track shipment
tracking = client.shipments.track('292770760')
```

---

## Changelog

### Version 1.0.0 (November 5, 2025)
- ✨ Initial release of Filflo Observability API
- 🎯 Complete Products, Inventory, and Orders APIs
- 📦 Purchase Orders and GRN management
- 💰 Invoices and Credit Notes APIs
- 🚚 Transporters and Logistics tracking
- 🔬 Quality Management APIs
- 💵 Rate Cards and Pricing APIs
- 📊 Comprehensive Analytics and Reporting
- 🔔 Webhook support for real-time notifications
- 📚 Full API documentation with examples

### Coming Soon
- 🔄 Batch operations API
- 🤖 AI-powered demand forecasting
- 📱 Mobile SDK (iOS & Android)
- 🔍 Advanced search with Elasticsearch
- 📈 Custom dashboard widgets API
- 🌐 Multi-language support

---

## Support

For API support and assistance, Filflo offers multiple channels:

### Contact Us
- **Email:** team@filflo.in
- **Sales:** shubham@filflo.in
- **Phone:** +91-XXXX-XXXXXX (9 AM - 6 PM IST, Mon-Fri)

### Resources
- **API Documentation:** https://docs.filflo.in
- **Developer Portal:** https://developers.filflo.in
- **Status Page:** https://status.filflo.in
- **GitHub:** https://github.com/filflo
- **Postman Collection:** https://www.postman.com/filflo

### Dashboard Access
- **Production:** https://app.filflo.in
- **Sandbox:** https://sandbox.filflo.in

### Community
- **Discord:** https://discord.gg/filflo
- **LinkedIn:** https://linkedin.com/company/filflo
- **Twitter:** @filflo_in

### Response Times
- **Critical Issues:** < 2 hours
- **Standard Support:** < 24 hours
- **General Inquiries:** < 48 hours

---

**Last Updated:** November 5, 2025  
**Version:** 1.0.0  
**Powered by Filflo**  
**© 2025 Filflo Technologies. All rights reserved.**
