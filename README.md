# Asset Prequalification Service

The **Asset Prequalification** service evaluates and verifies the suitability of assets for participation in flexibility markets. Flexibility Service Providers (FSPs), referred to as **Nodes**, can register assets and group them into **Portfolios**, which are then reviewed by the **System Operator (SO)**. This ensures that only compliant and qualified assets are approved for operational use.

**Owner:** HEnEx  

---

## Features

- **Asset Registration** – FSPs can register energy assets with details like type, capacity, and location.  
- **Portfolio Management** – Assets can be grouped logically (e.g., by technology, project, or location).  
- **Review and Confirmation** – SOs review registered assets and confirm their eligibility.  
- **Notification System** – Alerts for asset/portfolio approval, rejection, or pending review.  
- **User Management** – Role-based access for FSPs, SOs, and operators.  

---

## Functional Requirements

1. **Asset Registration**
   - Register new assets with operational details.  
   - Optionally assign assets to portfolios.  
   - Each asset belongs to one portfolio at most.  

2. **Asset Review**
   - SOs review and approve/reject assets based on predefined criteria.  

3. **Notifications**
   - Notify FSPs about registration outcomes (asset-level or portfolio-level).  

4. **User Management**
   - Secure authentication and authorization for all roles.  

---

## Non-Functional Requirements

- **Performance**: Response time < 2s under normal load.  
- **Scalability**: Handle large numbers of assets and portfolios.  
- **Reliability**: 99.9% uptime, fault tolerance, automated recovery.  
- **Security**:  
  - Authentication: Bearer tokens.  
  - Role-based authorization.  
  - Data encryption at rest and in transit (AES-256, TLS).  
  - GDPR-compliant privacy handling.  

---

## API Endpoints

### Assets
- `GET /asset/list` – List all assets (filterable by status).  
- `POST /asset/company/:cmp_id` – Register new asset.  
- `PATCH /asset/:ast_id` – Update asset details.  
- `GET /asset/company/:cmp_id` – List company’s assets.  
- `GET /asset/:ast_id` – Get asset by ID.  
- `PATCH /asset/:ast_id/status` – Update asset status.  

### Portfolios
- `GET /portfolio/list` – List all portfolios.  
- `GET /portfolio/:prtf_id` – Get portfolio by ID.  
- `POST /portfolio` – Create new portfolio.  
- `DELETE /portfolio/:prtf_id` – Delete portfolio.  
- `PATCH /portfolio/:prtf_id` – Update portfolio.  
- `POST /portfolio/:prtf_id/assets/assign` – Assign assets to portfolio.  
- `POST /portfolio/:prtf_id/assets/unassign` – Remove assets from portfolio.  

See API specs for **examples, responses, and error handling**.  

---

## Data Model

### Entities
- **Companies** – Manage assets & portfolios; identified by VAT.  
- **Nodes** – Represent physical/logical grid connection points.  
- **Portfolios** – Group assets; belong to one Node and one Company.  
- **Assets** – Individual energy entities (generation/consumption/both), with metadata (capacity, type, metering ID).  

### Relationships
- Companies ↔ Portfolios (1:N).  
- Portfolios ↔ Assets (1:N).  
- Nodes ↔ Portfolios (1:N).  
- Nodes ↔ Assets (1:N).  

---

## Security & Privacy

- **Data Sensitivity**: Asset and portfolio data treated as critical infrastructure.  
- **Access Control**: Role-based (FSP, SO, admin).  
- **Audit Logs**: All user, asset, and portfolio actions logged.  
- **Encryption**: TLS/HTTPS and AES-256.  

---

## Integration & Dependencies

- **System dependencies**: PostgreSQL, FastAPI, Redis (notifications).  
- **Third-party integrations**: Keycloak for auth.  
- **IoT / HEDGE-IoT integration**: Future connection with edge devices and data space.  

---

- Implemented demo workflows using synthetic assets and portfolios.  
- Verified registration, grouping, and review processes.  
- Tested notification system with demo SO approvals.  
- Validated system response and scalability under simulated loads.  

---
