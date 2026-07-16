# DBMS — Database Management System

A modular, SOLID-compliant Database Management System designed with a top-down architecture approach.
The system is decomposed into 8 core subsystems, each designed with clear interfaces, entities, and service classes following OOP best practices.

---

## Architecture Overview
```mermaid
mindmap
  root((DBMS Core))
    StorageEngine
    QueryProcessing
    TransactionConcurrency
    DatabaseObjectMetadata
    Security
    Administration
    BackupRecoveryLogging
    CommunicationConnectivity
```

### Layer 1 — System Domains

```mermaid
classDiagram
    direction TB
    
    class DBMS {
        <<Facade / Orchestrator>>
    }

    DBMS *-- CommunicationConnectivity
    DBMS *-- QueryProcessing
    DBMS *-- DatabaseObjectMetadata
    DBMS *-- StorageEngine
    DBMS *-- TransactionConcurrency
    DBMS *-- Security
    DBMS *-- Administration
    DBMS *-- BackupRecoveryLogging
    
    %% Thiết lập sự phụ thuộc (Dependencies)
    CommunicationConnectivity --> QueryProcessing : Gửi câu lệnh SQL
    CommunicationConnectivity --> Security : Yêu cầu Authentication
    
    QueryProcessing --> DatabaseObjectMetadata : Đọc Catalog, Schema
    QueryProcessing --> StorageEngine : Yêu cầu lấy/ghi dữ liệu
    QueryProcessing --> TransactionConcurrency : Yêu cầu Lock, Context
    QueryProcessing --> Security : Kiểm tra Authorization
    
    TransactionConcurrency --> StorageEngine : Khóa bản ghi/Page
    TransactionConcurrency --> BackupRecoveryLogging : Ghi Log (WAL)
    
    BackupRecoveryLogging --> StorageEngine : Restore/Flush Pages
    
    Administration --> Security : Audit Log, Quản lý User
    Administration --> BackupRecoveryLogging : Lên lịch Backup
    Administration --> QueryProcessing : Lấy Profiling, Metrics
    Administration --> StorageEngine : Chạy DBCC, Vacuum

```

---

## Design Principles

| Principle | Application |
|---|---|
| **SOLID** | Each class has a single responsibility; interfaces are segregated by functionality |
| **ISP** | Fat interfaces split into focused interfaces (e.g., `IFileReader`, `IFileWriter`, `IFileSynchronizer`) |
| **DIP** | High-level modules depend on abstractions, not concrete implementations |
| **Design Patterns** | Facade, Template Method, Strategy, Composite, Iterator used per module |

---

## Project Structure

```text
DBMS/
├── docs/
│   ├── architecture/       ← Layer 1-4 design diagrams
│   └── sequence_diagrams/  ← Key operation sequence diagrams
├── src/                    ← Implementation source code
└── tests/
    ├── unit/
    └── integration/
```

---

## Documentation

- [Layer 3 Component Breakdown](docs/architecture/layer3_components.md)
- [Storage Engine — File Manager (Detailed)](docs/04_class_detail_file_management.md)
