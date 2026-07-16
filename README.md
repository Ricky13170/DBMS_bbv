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
    Security
    DatabaseObjectMetadata
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
    CommunicationConnectivity --> QueryProcessing : Gửi SQL
    CommunicationConnectivity --> Security : Yêu cầu Authentication
    QueryProcessing --> DatabaseObjectMetadata : Đọc Catalog
    QueryProcessing --> StorageEngine : Đọc/Ghi dữ liệu
    QueryProcessing --> TransactionConcurrency : Lock & Context
    QueryProcessing --> Security : Check Authorization
    TransactionConcurrency --> StorageEngine : Khóa bản ghi (Page)
    TransactionConcurrency --> BackupRecoveryLogging : Ghi WAL
    BackupRecoveryLogging --> StorageEngine : Restore/Flush
    Administration --> Security : Audit Log
    Administration --> BackupRecoveryLogging : Lên lịch Backup
    Administration --> QueryProcessing : Metrics
    Administration --> StorageEngine : DBCC, Vacuum
```

---

## Design Principles

| Principle | Application |
|---|---|
| **SOLID** | Each class has a single responsibility; interfaces are segregated by functionality |
| **ISP** | Fat interfaces split into focused interfaces (e.g., `IFileReader`, `IFileWriter`, `IFileSynchronizer`) |
| **DIP** | High-level modules depend on abstractions, not concrete implementations |
| **Design Patterns** | Facade, Template Method, Strategy, Composite, Iterator used per module |
