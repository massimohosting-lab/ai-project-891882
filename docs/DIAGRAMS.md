# Diyagramlar

```json
{
  "diagrams": [
    {
      "id": "tekstil-erd",
      "name": "Tekstil ERP Veritabanı ERD",
      "type": "erDiagram",
      "mermaid_code": "erDiagram\n    COMPANY ||--o{ DEPARTMENT : has\n    DEPARTMENT ||--o{ EMPLOYEE : contains\n    EMPLOYEE ||--o{ PRODUCTION_ORDER : creates\n    EMPLOYEE ||--o{ ATTENDANCE : records\n    \n    CUSTOMER ||--o{ ORDER : places\n    ORDER ||--o{ ORDER_ITEM : contains\n    ORDER ||--o{ INVOICE : generates\n    ORDER ||--o{ DELIVERY_NOTE : creates\n    \n    PRODUCTION_ORDER ||--o{ PRODUCTION_TRACKING : tracks\n    PRODUCTION_ORDER ||--o{ MATERIAL_CONSUMPTION : consumes\n    MACHINE ||--o{ PRODUCTION_TRACKING : uses\n    SHIFT ||--o{ PRODUCTION_TRACKING : during\n    \n    SUPPLIER ||--o{ RAW_MATERIAL : supplies\n    RAW_MATERIAL ||--o{ STOCK_MOVEMENT : moves\n    WAREHOUSE ||--o{ STOCK_MOVEMENT : stores\n    RAW_MATERIAL ||--o{ LOT : batched\n    \n    PRODUCT ||--o{ ORDER_ITEM : ordered\n    PRODUCT ||--o{ QUALITY_TEST : tested\n    QUALITY_STANDARD ||--o{ QUALITY_TEST : defines\n    CUSTOMER ||--o{ COMPLAINT : files\n    \n    ACCOUNT ||--o{ ACCOUNTING_ENTRY : records\n    COST_CENTER ||--o{ ACCOUNTING_ENTRY : categorizes"
    },
    {
      "id": "tekstil-flowchart",
      "name": "Tekstil ERP İş Akışı",
      "type": "flowchart",
      "mermaid_code": "flowchart TD\n    A[Müşteri Siparişi] --> B{Stok Kontrolü}\n    B -->|Yeterli| C[Sipariş Onayı]\n    B -->|Yetersiz| D[Üretim Planlaması]\n    \n    C --> E[Teslimat Planı]\n    D --> F[İş Emri Oluşturma]\n    F --> G[Hammadde Temini]\n    G --> H{Hammadde Var mı?}\n    H -->|Hayır| I[Tedarikçi Siparişi]\n    H -->|Evet| J[Üretim Başlatma]\n    I --> K[Hammadde Girişi]\n    K --> J\n    \n    J --> L[Üretim Takibi]\n    L --> M[Kalite Kontrol]\n    M --> N{Kalite OK?}\n    N -->|Hayır| O[Hatalı Ürün İşlemi]\n    N -->|Evet| P[Mamul Stok Girişi]\n    \n    O --> Q[İyileştirme Aksiyonu]\n    Q --> J\n    P --> E\n    E --> R[Sevkiyat]\n    R --> S[Fatura Kesimi]\n    S --> T[Muhasebe Kaydı]\n    T --> U[Sipariş Kapanışı]"
    },
    {
      "id": "tekstil-sequence",
      "name": "Sipariş İşleme Sequence Diyagramı",
      "type": "sequenceDiagram",
      "mermaid_code": "sequenceDiagram\n    participant C as Customer\n    participant CRM as CRM Module\n    participant OM as Order Management\n    participant SM as Stock Management\n    participant PM as Production Module\n    participant QC as Quality Control\n    participant ACC as Accounting\n    \n    C->>CRM: Sipariş Talebi\n    CRM->>OM: Sipariş Oluştur\n    OM->>SM: Stok Kontrolü\n    SM-->>OM: Stok Durumu\n    \n    alt Stok Yeterli\n        OM->>ACC: Fatura Oluştur\n        ACC-->>C: Fatura Gönder\n    else Stok Yetersiz\n        OM->>PM: Üretim Emri\n        PM->>SM: Hammadde Kontrolü\n        SM-->>PM: Hammadde Durumu\n        \n        alt Hammadde Yeterli\n            PM->>PM: Üretim Başlat\n        else Hammadde Yetersiz\n            PM->>SM: Tedarikçi Siparişi\n            SM-->>PM: Hammadde Temin\n            PM->>PM: Üretim Başlat\n        end\n        \n        PM->>QC: Kalite Kontrol\n        QC-->>PM: Kalite Onayı\n        PM->>SM: Mamul Stok Girişi\n        SM->>OM: Stok Güncelleme\n        OM->>ACC: Fatura Oluştur\n        ACC-->>C: Fatura Gönder\n    end\n    \n    OM->>C: Teslimat Bilgisi"
    },
    {
      "id": "tekstil-c4-context",
      "name": "Tekstil ERP C4 Context Diyagramı",
      "type": "flowchart",
      "mermaid_code": "flowchart TD\n    subgraph \"Tekstil Şirketi\"\n        U1[Üretim Müdürü]\n        U2[Stok Sorumlusu]\n        U3[Satış Temsilcisi]\n        U4[Muhasebe Uzmanı]\n        U5[Kalite Kontrol]\n    end\n    \n    subgraph \"Tekstil ERP Sistemi\"\n        CORE[ERP Core System]\n        \n        subgraph \"Production Module\"\n            PM[Üretim Takibi]\n            WO[İş Emri Yönetimi]\n        end\n        \n        subgraph \"Inventory Module\"\n            IM[Stok Yönetimi]\n            WM[Depo Yönetimi]\n        end\n        \n        subgraph \"Sales Module\"\n            OM[Sipariş Yönetimi]\n            CM[CRM]\n        end\n        \n        subgraph \"Finance Module\"\n            AM[Muhasebe]\n            REP[Raporlama]\n        end\n        \n        subgraph \"Quality Module\"\n            QC[Kalite Kontrol]\n        end\n    end\n    \n    subgraph \"External Systems\"\n        EXT1[Tedarikçi Sistemleri]\n        EXT2[E-Ticaret Platformları]\n        EXT3[Muhasebe Yazılımları]\n        EXT4[Banka Sistemleri]\n    end\n    \n    U1 --> PM\n    U2 --> IM\n    U3 --> OM\n    U4 --> AM\n    U5 --> QC\n    \n    CORE --> PM\n    CORE --> IM\n    CORE --> OM\n    CORE --> AM\n    CORE --> QC\n    \n    IM --> EXT1\n    OM --> EXT2\n    AM --> EXT3\n    AM --> EXT4"
    },
    {
      "id": "tekstil-state-machine",
      "name": "Sipariş Durum Makinesi",
      "type": "stateDiagram-v2",
      "mermaid_code": "stateDiagram-v2\n    [*] --> OrderReceived: Müşteri Siparişi\n    \n    OrderReceived --> StockCheck: Stok Kontrolü\n    \n    StockCheck --> OrderConfirmed: Stok Yeterli\n    StockCheck --> ProductionPlanning: Stok Yetersiz\n    \n    ProductionPlanning --> MaterialCheck: Hammadde Kontrolü\n    \n    MaterialCheck --> ProductionStarted: Hammadde Yeterli\n    MaterialCheck --> MaterialOrdering: Hammadde Yetersiz\n    \n    MaterialOrdering --> MaterialReceived: Tedarikçi Teslimatı\n    MaterialReceived --> ProductionStarted: Üretim Başlatma\n    \n    ProductionStarted --> InProduction: Üretim Devam Ediyor\n    InProduction --> QualityControl: Üretim Tamamlandı\n    \n    QualityControl --> QualityApproved: Kalite OK\n    QualityControl --> ProductionStarted: Kalite Reddedildi\n    \n    QualityApproved --> ReadyForDelivery: Mamul Stokta\n    OrderConfirmed --> ReadyForDelivery: Direkt Sevkiyat\n    \n    ReadyForDelivery --> Shipped: Sevkiyat Yapıldı\n    Shipped --> Invoiced: Fatura Kesildi\n    Invoiced --> PaymentReceived: Ödeme Alındı\n    PaymentReceived --> OrderCompleted: Sipariş Tamamlandı\n    \n    OrderCompleted --> [*]\n    \n    state ProductionStarted {\n        [*] --> SetupMachine\n        SetupMachine --> Running\n        Running --> QualityCheck\n        QualityCheck --> Running: Devam Et\n        QualityCheck --> [*]: Tamamlandı\n    }"
    }
  ]
}
```