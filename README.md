# Asset Prequalification Service

The **Asset Prequalification Service** is designed to evaluate and verify the suitability of assets for participation in flexibility markets.  
Flexibility Service Providers (FSPs), referred to as **Nodes**, are required to register their assets. Assets can be grouped into **Portfolios**, which are then reviewed by the **System Operator (SO)**.  

This ensures that only compliant and qualified assets are approved for operational use, reducing risks and ensuring regulatory compliance.  

**Owner:** HEnEx  

---

## Functional Requirements

1. **Asset Registration**
   - FSPs can register assets with details (type, capacity, location, operational parameters).  
   - Assets may optionally be assigned to a portfolio.  
   - Each asset can belong to only one portfolio, but assignment is not mandatory.  

2. **Asset Review and Confirmation**
   - SOs review and confirm assets based on predefined criteria and standards.  

3. **Notification System**
   - Notify FSPs about registration outcomes (approved, pending, rejected).  
   - Notifications can also be aggregated at the portfolio level.  

4. **User Management**
   - Secure, role-based access for FSPs and SOs.  

---

## Non-Functional Requirements

- **Performance**
  - Response time < 2s under normal load.  
  - Scales with increasing asset registrations and reviews.  

- **Reliability & Availability**
  - 99.9% uptime.  
  - Fault tolerance with recovery mechanisms.  

- **Security**
  - Authentication: Bearer tokens.  
  - Authorization: Role-based access control.  
  - Data encryption at rest (AES-256) and in transit (TLS).  
  - GDPR-compliant handling of sensitive data.  

---

## API Endpoints

All endpoints require headers:  
- `Content-Type: application/json`  
- `Authorization: Bearer <token>`  

---

### **Assets**

#### 1. List Assets  
- **URL:** `/asset/list`  
- **Method:** `GET`  
- **Description:** Retrieve all registered assets (filterable by status).  
- **Query Parameters:**  
  - `status` (optional): Filter by asset status.  

**Example Request:**  
```http
GET /asset/list?status=ACTIVE
```

**Response Example:**  
```json
[
  {
    "ast_id": 1,
    "ast_name": "Asset_name",
    "ast_description": "Asset In Node x",
    "ast_status": "ACTIVE",
    "ast_type": "PRODUCTION",
    "ast_created_at": "2025-05-01T10:00:00Z",
    "ast_updated_at": "2025-05-10T15:30:00Z"
  }
]
```

**Error Handling:**  
- 401 Unauthorized  
- 500 Internal Server Error  

---

#### 2. Register New Asset  
- **URL:** `/asset/company/:cmp_id`  
- **Method:** `POST`  
- **Description:** Register a new asset under a company.  

**Request Body:**  
```json
{
  "ast_name": "Asset_Name",
  "ast_description": "Asset In Node x",
  "ast_status": "ACTIVE",
  "ast_type": "CONSUMPTION"
}
```

**Response Example:**  
```json
{
  "ast_id": 15,
  "ast_name": "Asset_Name",
  "ast_description": "Asset In Node x",
  "ast_status": "ACTIVE",
  "ast_type": "CONSUMPTION",
  "cmp_id": 3,
  "ast_created_at": "2025-05-21T14:25:30Z",
  "ast_updated_at": "2025-05-21T14:25:30Z"
}
```

**Error Handling:**  
- 400 Bad Request  
- 401 Unauthorized  
- 500 Internal Server Error  

---

#### 3. Update Asset  
- **URL:** `/asset/:ast_id`  
- **Method:** `PATCH`  
- **Description:** Update details of an existing asset.  

**Request Body:**  
```json
{
  "ast_name": "New Name",
  "ast_description": "Updated asset description"
}
```

**Response Example:**  
```json
{
  "ast_id": 15,
  "ast_name": "New Asset Name",
  "ast_description": "Asset In Node x",
  "ast_status": "ACTIVE",
  "ast_type": "CONSUMPTION",
  "cmp_id": 3,
  "ast_created_at": "2025-05-21T14:25:30Z",
  "ast_updated_at": "2025-05-21T14:25:30Z"
}
```

**Error Handling:**  
- 400 Bad Request  
- 404 Not Found  
- 401 Unauthorized  
- 500 Internal Server Error  

---

#### 4. Get Company’s Assets  
- **URL:** `/asset/company/:cmp_id`  
- **Method:** `GET`  
- **Description:** Retrieve all assets of a company.  

**Example Request:**  
```http
GET /asset/company/5?status=ACTIVE
```

**Response Example:**  
```json
[
  {
    "ast_id": 1,
    "ast_name": "Asset_name",
    "ast_description": "Asset In Node x",
    "ast_status": "ACTIVE",
    "ast_type": "PRODUCTION",
    "ast_created_at": "2025-05-01T10:00:00Z",
    "ast_updated_at": "2025-05-10T15:30:00Z"
  }
]
```

**Error Handling:**  
- 401 Unauthorized  
- 500 Internal Server Error  

---

#### 5. Get Asset by ID  
- **URL:** `/asset/:ast_id`  
- **Method:** `GET`  
- **Description:** Retrieve a specific asset.  

**Example Request:**  
```http
GET /asset/1
```

**Response Example:**  
```json
{
  "ast_id": 1,
  "ast_name": "Asset_name",
  "ast_description": "Asset In Node x",
  "ast_status": "ACTIVE",
  "ast_type": "PRODUCTION",
  "ast_created_at": "2025-05-01T10:00:00Z",
  "ast_updated_at": "2025-05-10T15:30:00Z"
}
```

**Error Handling:**  
- 404 Not Found  
- 401 Unauthorized  
- 500 Internal Server Error  

---

#### 6. Update Asset Status  
- **URL:** `/asset/:ast_id/status`  
- **Method:** `PATCH`  
- **Description:** Update status of an asset.  

**Request Body:**  
```json
{
  "ast_status": "ACTIVE"
}
```

**Response Example:**  
```json
{
  "ast_id": 8,
  "ast_name": "Asset_name",
  "ast_description": "Asset In Node x",
  "ast_status": "ACTIVE",
  "ast_type": "PRODUCTION",
  "ast_created_at": "2025-05-01T10:00:00Z",
  "ast_updated_at": "2025-05-10T15:30:00Z"
}
```

**Error Handling:**  
- 400 Bad Request  
- 404 Not Found  
- 401 Unauthorized  
- 500 Internal Server Error  

---

### **Portfolios**

(Portfolios endpoints 7–13 would continue here with full details in the same expanded format)  

---

## Data Model

### Entities
- **Companies** – Manage multiple portfolios and assets; identified by VAT.  
- **Nodes** – Physical or logical grid connection points.  
- **Portfolios** – Group assets logically; belong to one company and node.  
- **Assets** – Energy entities (generation/consumption/both).  

### Relationships
- Companies ↔ Portfolios (1:N).  
- Portfolios ↔ Assets (1:N).  
- Nodes ↔ Portfolios (1:N).  
- Nodes ↔ Assets (1:N).  

---

## Security & Privacy

- **Data Sensitivity:** Asset and portfolio data classified as critical.  
- **Access Control:** Role-based (FSP, SO, admin).  
- **Audit Logs:** All actions logged.  
- **Encryption:** TLS/HTTPS and AES-256.  

---

## Integration & Dependencies

- **System dependencies:** PostgreSQL, FastAPI, Redis.  
- **Third-party integrations:** Keycloak.  
- **IoT / HEDGE-IoT integration:** Planned for future expansion.  

---
