# DashboardFest2Fun

Click here to see the app alive: https://dashboardfest2fun-production.up.railway.app/

## Overview
This document provides a comprehensive technical overview of the FestFunDashboard system, a full-stack web application designed for real-time event tracking and analytics with wristband-based participant monitoring. The system supports geographical visualization, zone tracking, and statistical analysis of event data stored in AWS DynamoDB.

Relevant source files
- [pom.xml](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/pom.xml)
- [src/main/frontend/views/@index.tsx](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx)
- [src/main/java/com/pedro/apps/config/DynamoDBConfig.java](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/config/DynamoDBConfig.java)
- [src/main/java/com/pedro/apps/events/EventRecord.java](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/events/EventRecord.java)

For information about specific user interface components, see [User Interface Components](https://deepwiki.com/pedroGEOGIScoding/FestFunDashboard/3-user-interface-components). For backend service implementation details, see [Backend Services](https://deepwiki.com/pedroGEOGIScoding/FestFunDashboard/4-backend-services). For deployment and configuration specifics, see [Development Environment](https://deepwiki.com/pedroGEOGIScoding/FestFunDashboard/5-development-environment) and [Configuration](https://deepwiki.com/pedroGEOGIScoding/FestFunDashboard/6-configuration).

## System Architecture

The FestFunDashboard follows a modern full-stack architecture with clear separation between frontend, backend, and data layers. The system is built on Spring Boot with Vaadin Hilla framework for type-safe client-server communication.

### High-Level Architecture

```
Data LayerBackend LayerGenerated API LayerFrontend LayerDashboardView Component
(@index.tsx)TrackingMapView Component
(tracking-map.tsx)MainLayout Component
(MainLayout.tsx)Vaadin React Components
(Grid, TextField, Button, etc.)EventRecordEndpoint
(Generated Client)TypeScript Types
(Generated from Java)EventRecordEndpoint
(@Endpoint)EventRecordRepository
(@Repository)EventRecordRepositoryImplDynamoDBConfig
(@Configuration)EventRecord
(@DynamoDbBean)EventRecord.Data
(@DynamoDbBean)DynamoDB Table
(TestingEvent_050)
```

Sources: [src/main/frontend/views/@index.tsx 1-643](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L1-L643) [src/main/java/com/pedro/apps/events/EventRecord.java 1-127](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/events/EventRecord.java#L1-L127) [src/main/java/com/pedro/apps/config/DynamoDBConfig.java 1-31](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/config/DynamoDBConfig.java#L1-L31) [pom.xml 1-173](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/pom.xml#L1-L173)

### Technology Stack

| Layer | Technology | Purpose |
| --- | --- | --- |
| Frontend | Vaadin 24.7.6 + React | UI components and client-side logic |
| Build System | Maven 3.9+ | Dependency management and build automation |
| Backend | Spring Boot 3.4.6 | Application framework and REST services |
| Database | AWS DynamoDB | NoSQL document storage |
| Java Runtime | Java 21 | Application runtime environment |
| Development | Vaadin Copilot | Enhanced development experience |

Sources: [pom.xml 14-16](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/pom.xml#L14-L16) [pom.xml 19-23](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/pom.xml#L19-L23) [pom.xml 25-35](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/pom.xml#L25-L35)

## Core Data Model

The system centers around event tracking with wristband identification, zone management, and temporal tracking. The data model is implemented using DynamoDB enhanced client annotations.

### EventRecord Entity Structure

```
"contains"EventRecord+String eventId+String operation+Data data+getEventId() : String+getOperation() : String+getData() : Data+setEventId(String)+setOperation(String)+setData(Data)Data+String bwId+String currentZone+Boolean preAssigned+Long timestamp+Integer totalBWIdQty+String type+double lat+double lon+getBwId() : String+getCurrentZone() : String+getPreAssigned() : Boolean+getTimestamp() : Long+getTotalBWIdQty() : Integer+getType() : String+getLat() : double+getLon() : doubleDynamoDB Partition Key: eventId
DynamoDB Sort Key: operationContains wristband tracking data
with geographical coordinates
```

Sources: [src/main/java/com/pedro/apps/events/EventRecord.java 8-39](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/events/EventRecord.java#L8-L39) [src/main/java/com/pedro/apps/events/EventRecord.java 40-103](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/events/EventRecord.java#L40-L103)

### DynamoDB Configuration

The system uses AWS DynamoDB with enhanced client support for type-safe operations:

```
DynamoDBConfig
(@Configuration)DynamoDbClient
(AWS SDK)DynamoDbEnhancedClient
(Enhanced SDK)EventRecordRepositoryImpl
(@Repository)TestingEvent_050
(DynamoDB Table)
```

Sources: [src/main/java/com/pedro/apps/config/DynamoDBConfig.java 11-31](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/config/DynamoDBConfig.java#L11-L31)

## Frontend Application Structure

The frontend is built with Vaadin Hilla, providing type-safe communication between React components and Spring Boot endpoints. The main dashboard provides comprehensive event analytics and filtering capabilities.

### Component Architecture

```
@index.tsx
(DashboardView)EventRecordEndpoint
(Generated Client)Vaadin Components
(Grid, TextField, Button, etc.)eventRecords: EventRecord[]
(State Management)filteredRecords: EventRecord[]
(Filtered Data)bwIdStats: BwIdStats[]
(Analytics Data)zoneTimeStats: ZoneTimeStats[]
(Zone Analytics)processBwIdStats()
calculateZoneTimeStats()
calculateOverallZoneTimeStats()EventRecordEndpoint
(Backend @Endpoint)Grid Component
(Data Display)TextField Components
(Filtering)Button Components
(Actions)ProgressBar
(Statistics Display)
```

Sources: [src/main/frontend/views/@index.tsx 15-65](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L15-L65) [src/main/frontend/views/@index.tsx 323-432](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L323-L432) [src/main/frontend/views/@index.tsx 66-280](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L66-L280)

### Key Interface Definitions

The frontend defines TypeScript interfaces that mirror the backend Java classes:

```
interface EventRecord {
  eventId?: string;
  operation?: string;
  data?: Data;
}

interface Data {
  bwId?: string;
  currentZone?: string;
  preAssigned?: boolean;
  timestamp?: number;
  totalBWIdQty?: number;
  type?: string;
  lat: number;
  lon: number;
}
```

Sources: [src/main/frontend/views/@index.tsx 17-33](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L17-L33)

## Core Application Features

### Event Analytics and Filtering

The system provides comprehensive analytics capabilities including:

- **Wristband Statistics**: Track unique wristband usage with percentage calculations
- **Zone Time Analysis**: Calculate time spent in different zones with visit counts
- **Real-time Filtering**: Filter by Event ID, Wristband ID, or both
- **Timestamp Sorting**: Ascending/descending chronological sorting

### Data Processing Functions

| Function | Purpose | Source |
| --- | --- | --- |
| `processBwIdStats()` | Calculate wristband usage statistics | [src/main/frontend/views/@index.tsx 66-98](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L66-L98) |
| `calculateZoneTimeStats()` | Analyze zone time for specific wristband | [src/main/frontend/views/@index.tsx 100-189](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L100-L189) |
| `calculateOverallZoneTimeStats()` | Calculate overall zone analytics | [src/main/frontend/views/@index.tsx 191-280](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L191-L280) |
| `fetchAllEvents()` | Load all event records from backend | [src/main/frontend/views/@index.tsx 323-341](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/frontend/views/@index.tsx#L323-L341) |

### Geographic Data Support

The system supports geographical tracking with latitude/longitude coordinates stored in the `Data` class:

```
EventRecordDatalat: double
lon: doublecurrentZone: String
type: StringbwId: String
timestamp: LongMap Visualization
(Future Feature)Zone Time AnalyticsWristband Statistics
```

Sources: [src/main/java/com/pedro/apps/events/EventRecord.java 96-102](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/events/EventRecord.java#L96-L102) [src/main/java/com/pedro/apps/events/EventRecord.java 58-63](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/events/EventRecord.java#L58-L63) [src/main/java/com/pedro/apps/events/EventRecord.java 50-56](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/src/main/java/com/pedro/apps/events/EventRecord.java#L50-L56)

## Build and Deployment

The application is packaged as a single executable JAR using Maven with Spring Boot plugin integration:

```
Maven Build
(pom.xml)Spring Boot Plugin
(Executable JAR)Vaadin Plugin
(Frontend Assets)Frontend Assets
(TypeScript/React)festfundashboard.jar
(Executable JAR)Docker Container
(Port 8080)Production Build
(-Pproduction)Optimized Assets
(Vite Bundle)
```

Sources: [pom.xml 90-136](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/pom.xml#L90-L136) [pom.xml 138-172](https://github.com/pedroGEOGIScoding/FestFunDashboard/blob/595e08b2/pom.xml#L138-L172)

The system is designed for deployment as a containerized application with external DynamoDB connectivity, supporting both development and production environments through Maven profiles.
