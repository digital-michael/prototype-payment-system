# prototype-payment-system
Prototype a payment system


# Summary


# Design Goals
- multi-tenant
- scalable
- event driven
- container-ized
- auditable
- plugable payment 
- locality-sensitive business rules
- distributed transaction event logging and tracing
- dashboards: status, alerts, notiications
- 

# Diagram
## Placeholder
```mermaid
graph LR;

    clients-->Shopping_Cart;

    subgraph integration
       Shopping_Cart-->Javascript_Framework;
       Javascript_Framework-->API_Gateway_External;
    end

    API_Gateway_External-->API_Gateway_Invoicing;
    API_Gateway_External-->API_Gateway_Payments;
    API_Gateway_External-->API_Gateway_Inventory;
    API_Gateway_External-->API_Gateway_Shipping;
    API_Gateway_External-->API_Gateway_Returns;
    API_Gateway_External-->API_Gateway_Restocking;

    subgraph Invoicing    
       API_Gateway_Invoicing-->RESTFUL_Invoicing;
       RESTFUL_Invoicing-->SDK_Invoicing;
       SDK_Invoicing-->pre_order;
       SDK_Invoicing-->pre_payment;
       SDK_Invoicing-->order;
       SDK_Invoicing-->POS_payment;
       SDK_Invoicing-->invoice_service_first;
       SDK_Invoicing-->invoice_service_recurring;
       SDK_Invoicing-->invoice_service_terms;

    end

    subgraph Payments    
       API_Gateway_Payments-->RESTFUL_Payments;
       RESTFUL_Payments-->SDK_Payments;
       SDK_Payments-->Payment_Transaction_Request;
       Payment_Transaction_Request-->Screening;
       Screening-->Localized_Business_Logic_for_PTR_with_metadata;
       Localized_Business_Logic_for_PTR_with_metadata-->Processing_Queues;
       Processing_Queues-->Audit_Tables;
       Processing_Queues-->Inventor_Update;
       Localized_Business_Logic_for_PTR_with_metadata-->Client_Payment_Plugin;
       Client_Payment_Plugin-->PIC-1;
       Client_Payment_Plugin-->PIC-2;
       Client_Payment_Plugin-->PIC-3;

       Inventor_Update-->RESTFUL_Invoicing;
    end

    subgraph Monitors
      Reporting-->Processing_Queues;
      Reporting-->Audit_Tables;
      Reporting-->Inventor_Update;
      Fraud_Detection-->Screening;
      Fraud_Detection-->Processing_Queues;
      Fraud_Detection-->Client_Payment_Plugin;
      Process_Monitoring-->Processing_Queues;
      Process_Monitoring-->Audit_Tables;
      Process_Monitoring-->Inventor_Update;
      Dashboard_Alert_and_Notification-->Fraud_Detection;
      Dashboard_Alert_and_Notification-->Process_Monitoring;
    end

    subgraph Inventory    
       API_Gateway_Inventory-->RESTFUL_Inventory;
       RESTFUL_Inventory-->SDK_Inventory;
    end
    subgraph Shipping    
       API_Gateway_Shipping-->RESTFUL_Shipping;
       RESTFUL_Shipping-->SDK_Shipping;
    end
    subgraph Returns    
       API_Gateway_Returns-->RESTFUL_Returns;
       RESTFUL_Returns-->SDK_Returns;
    end
    subgraph Restocking    
       API_Gateway_Restocking-->RESTFUL_Restocking;
       RESTFUL_Restocking-->SDK_Restocking;
    end


```

# Components
- APIG: API Gateway
- PTR: Payment Transaction Request
- LBL: Localized Business Logic for PTR with metadata
- CPP: Client Payment Plugin
- PIC: Payment Integration Client
- PQ: Processing Queues
- AT: Audit Tables
- IU: Inventor Update
- FD: Fraud Detection
- DA: Dashboard Alert and Notification
