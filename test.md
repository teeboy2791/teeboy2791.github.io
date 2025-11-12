```mermaid
graph LR
A[User/Device] --> B{NAC Server};
B --> C{Authentication Server};
C --> B;
B --> D{Policy Server};
D --> B;
B --> E{Endpoint Compliance Check};
E --> B;
B --> F{Network Access Device (Switch/Router/Firewall)};
F --> G{Authorized Network Resources};
F --> H{Quarantine/Remediation};
B -- Deny Access --> H;
B -- Allow Access --> F;
subgraph "Authentication"
C;
end
subgraph "Policy Enforcement"
D;
end
subgraph "Compliance"
E;
end
```
