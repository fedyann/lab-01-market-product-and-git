## Product Choice
Product name: Wildberries

Link to the product's website: https://www.wildberries.ru/

Short description of the product: 

Wildberries (Russian: Уайлдберрис, Uayldberris) is the largest Russian online retailer. Wildberries sells 37,000 brands of clothing, shoes, cosmetics, household products, children's goods, electronics, books, jewelry, food and much more.

## Main components
![Wildberries Component Diagram](./diagrams/out/wildberries/component-diagram/Component-Diagram.svg)

![Wildberries Component Diagram code](./diagrams/src/wildberries/architecture-component.puml)

Selected components:
1. Catalog & Search Service – Manages product catalog, provides full-text search and filtering via Elasticsearch.
2. Cart & Checkout Service – Handles the shopping cart, calculates totals, and initiates the checkout process.
3. Order Management (OMS) – Manages the order lifecycle from creation to fulfillment, including integration with warehouse systems.
4. Logistics & Supply Chain – Optimizes delivery routes, manages warehouse inventory, and integrates with third-party logistics partners (3PL).
5. Notification Service – Sends notifications to users via SMS, push notifications, and email about order statuses and promotions.
   

## Data flow
![Wildberries Sequence Diagram](./diagrams/out/wildberries/sequence-diagram/Sequence-Diagram.svg)

![Wildberries Sequence Diagram code](./diagrams/src/wildberries/architecture-sequence.puml)

Selected group: "Checkout (Reservation & Payment)"

Description:
When a user clicks "Pay Now", the system creates an order, reserves items in the warehouse for 15 minutes, initiates payment through a banking gateway with 3DS support, and after successful payment, asynchronously triggers the order assembly process in the warehouse.

Components interacting:
1. Cart & Checkout Service → Order Management (OMS) – transfer of cart data for order creation
2. Order Management → Inventory Service – reservation of stock items (Reserve Stock)
3. Order Management → Payment Service – initiation of payment through banking gateway
4. Payment Service → Bank/Acquirer – processing of 3DS authentication
5. Kafka Event Bus → WMS (Warehouse) – asynchronous transmission of order for assembly after payment

## Deployment