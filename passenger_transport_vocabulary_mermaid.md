# European Business Wallet Passenger Transport Vocabulary v0.1

Vocabulary for person transport entitlement, service, movement, boarding pass and transport event data in verifiable credentials and wallet-based transport use cases.

This Markdown file is ready for GitHub publication and contains an embedded Mermaid class diagram generated from the YAML vocabulary.

## Mermaid class diagram

```mermaid
classDiagram
direction LR
class PassengerTransportEntitlement
class PassengerTransportService
class PassengerTransportMovement
class BoardingPass
class PassengerTransportEvent
class Vehicle
PassengerTransportEntitlement : +entitlementIdentifier : Identifier
PassengerTransportEntitlement : +bookingReference : Identifier
PassengerTransportEntitlement : +holder : Person
PassengerTransportEntitlement : +issuer : Organization
PassengerTransportEntitlement : +forService : PassengerTransportService
PassengerTransportEntitlement : +forMovement : PassengerTransportMovement
PassengerTransportEntitlement : +fareType : Concept
PassengerTransportEntitlement : +travelClass : Concept
PassengerTransportEntitlement : +serviceLevel : Concept
PassengerTransportEntitlement : +price : MonetaryAmount
PassengerTransportEntitlement : +includedService : Concept
PassengerTransportEntitlement : +validFrom : dateTime
PassengerTransportEntitlement : +validUntil : dateTime
PassengerTransportEntitlement : +isFlexible : boolean
PassengerTransportEntitlement : +isRefundable : boolean
PassengerTransportEntitlement : +isChangeable : boolean
PassengerTransportEntitlement : +usageConditions : langString
PassengerTransportService : +serviceProvider : Organization
PassengerTransportService : +modeOfTransport : Concept
PassengerTransportService : +originLocation : Location
PassengerTransportService : +destinationLocation : Location
PassengerTransportService : +serviceStatus : Concept
PassengerTransportService : +serviceNumber : string
PassengerTransportService : +scheduledDepartureTime : dateTime
PassengerTransportService : +scheduledArrivalTime : dateTime
PassengerTransportMovement : +modeOfTransport : Concept
PassengerTransportMovement : +serviceNumber : string
PassengerTransportMovement : +scheduledDepartureTime : dateTime
PassengerTransportMovement : +scheduledArrivalTime : dateTime
PassengerTransportMovement : +isInstanceOfService : PassengerTransportService
PassengerTransportMovement : +operatingCarrier : Organization
PassengerTransportMovement : +departureLocation : Location
PassengerTransportMovement : +arrivalLocation : Location
PassengerTransportMovement : +vehicle : Vehicle
PassengerTransportMovement : +movementStatus : Concept
PassengerTransportMovement : +actualDepartureTime : dateTime
PassengerTransportMovement : +actualArrivalTime : dateTime
BoardingPass : +isBasedOnEntitlement : PassengerTransportEntitlement
BoardingPass : +isForMovement : PassengerTransportMovement
BoardingPass : +passenger : Person
BoardingPass : +boardingStatus : Concept
BoardingPass : +checkInStatus : Concept
BoardingPass : +cabinClass : Concept
BoardingPass : +validationStatus : Concept
BoardingPass : +seatNumber : string
BoardingPass : +coachNumber : string
BoardingPass : +departureGate : string
BoardingPass : +platform : string
BoardingPass : +boardingGroup : string
BoardingPass : +boardingTime : dateTime
BoardingPass : +gateClosingTime : dateTime
BoardingPass : +barcodeValue : string
PassengerTransportEvent : +eventType : Concept
PassengerTransportEvent : +eventStatus : Concept
PassengerTransportEvent : +eventLocation : Location
PassengerTransportEvent : +relatesToMovement : PassengerTransportMovement
PassengerTransportEvent : +relatesToEntitlement : PassengerTransportEntitlement
PassengerTransportEvent : +relatesToBoardingPass : BoardingPass
PassengerTransportEvent : +recordedBy : Organization
PassengerTransportEvent : +eventTime : dateTime
PassengerTransportEvent : +eventNote : langString
Vehicle : +vehicleIdentifier : Identifier
Vehicle : +vehicleType : Concept
Vehicle : +vehicleName : string
PassengerTransportEntitlement --> Identifier : entitlementIdentifier
PassengerTransportEntitlement --> Identifier : bookingReference
PassengerTransportEntitlement --> Person : holder
PassengerTransportEntitlement --> Organization : issuer
PassengerTransportEntitlement --> PassengerTransportService : forService
PassengerTransportEntitlement --> PassengerTransportMovement : forMovement
PassengerTransportEntitlement --> Concept : fareType
PassengerTransportEntitlement --> Concept : travelClass
PassengerTransportEntitlement --> Concept : serviceLevel
PassengerTransportEntitlement --> MonetaryAmount : price
PassengerTransportEntitlement --> Concept : includedService
PassengerTransportService --> Organization : serviceProvider
PassengerTransportService --> Concept : modeOfTransport
PassengerTransportMovement --> Concept : modeOfTransport
PassengerTransportService --> Location : originLocation
PassengerTransportService --> Location : destinationLocation
PassengerTransportService --> Concept : serviceStatus
PassengerTransportMovement --> PassengerTransportService : isInstanceOfService
PassengerTransportMovement --> Organization : operatingCarrier
PassengerTransportMovement --> Location : departureLocation
PassengerTransportMovement --> Location : arrivalLocation
PassengerTransportMovement --> Vehicle : vehicle
PassengerTransportMovement --> Concept : movementStatus
BoardingPass --> PassengerTransportEntitlement : isBasedOnEntitlement
BoardingPass --> PassengerTransportMovement : isForMovement
BoardingPass --> Person : passenger
BoardingPass --> Concept : boardingStatus
BoardingPass --> Concept : checkInStatus
BoardingPass --> Concept : cabinClass
BoardingPass --> Concept : validationStatus
PassengerTransportEvent --> Concept : eventType
PassengerTransportEvent --> Concept : eventStatus
PassengerTransportEvent --> Location : eventLocation
PassengerTransportEvent --> PassengerTransportMovement : relatesToMovement
PassengerTransportEvent --> PassengerTransportEntitlement : relatesToEntitlement
PassengerTransportEvent --> BoardingPass : relatesToBoardingPass
PassengerTransportEvent --> Organization : recordedBy
Vehicle --> Identifier : vehicleIdentifier
Vehicle --> Concept : vehicleType
```

## Classes included

- `PassengerTransportEntitlement` — Passenger Transport Entitlement
- `PassengerTransportService` — Passenger Transport Service
- `PassengerTransportMovement` — Passenger Transport Movement
- `BoardingPass` — Boarding Pass
- `PassengerTransportEvent` — Passenger Transport Event
- `Vehicle` — Vehicle

## Property summary

### `PassengerTransportEntitlement`

- `entitlementIdentifier` → `ebwv:Identifier`
- `bookingReference` → `ebwv:Identifier`
- `holder` → `ebwv:Person`
- `issuer` → `org:Organization`
- `forService` → `PassengerTransportService`
- `forMovement` → `PassengerTransportMovement`
- `fareType` → `skos:Concept`
- `travelClass` → `skos:Concept`
- `serviceLevel` → `skos:Concept`
- `price` → `ebwv:MonetaryAmount`
- `includedService` → `skos:Concept`
- `validFrom` → `xsd:dateTime`
- `validUntil` → `xsd:dateTime`
- `isFlexible` → `xsd:boolean`
- `isRefundable` → `xsd:boolean`
- `isChangeable` → `xsd:boolean`
- `usageConditions` → `rdf:langString`

### `PassengerTransportService`

- `serviceProvider` → `org:Organization`
- `modeOfTransport` → `skos:Concept`
- `originLocation` → `ebwv:Location`
- `destinationLocation` → `ebwv:Location`
- `serviceStatus` → `skos:Concept`
- `serviceNumber` → `xsd:string`
- `scheduledDepartureTime` → `xsd:dateTime`
- `scheduledArrivalTime` → `xsd:dateTime`

### `PassengerTransportMovement`

- `modeOfTransport` → `skos:Concept`
- `serviceNumber` → `xsd:string`
- `scheduledDepartureTime` → `xsd:dateTime`
- `scheduledArrivalTime` → `xsd:dateTime`
- `isInstanceOfService` → `PassengerTransportService`
- `operatingCarrier` → `org:Organization`
- `departureLocation` → `ebwv:Location`
- `arrivalLocation` → `ebwv:Location`
- `vehicle` → `Vehicle`
- `movementStatus` → `skos:Concept`
- `actualDepartureTime` → `xsd:dateTime`
- `actualArrivalTime` → `xsd:dateTime`

### `BoardingPass`

- `isBasedOnEntitlement` → `PassengerTransportEntitlement`
- `isForMovement` → `PassengerTransportMovement`
- `passenger` → `ebwv:Person`
- `boardingStatus` → `skos:Concept`
- `checkInStatus` → `skos:Concept`
- `cabinClass` → `skos:Concept`
- `validationStatus` → `skos:Concept`
- `seatNumber` → `xsd:string`
- `coachNumber` → `xsd:string`
- `departureGate` → `xsd:string`
- `platform` → `xsd:string`
- `boardingGroup` → `xsd:string`
- `boardingTime` → `xsd:dateTime`
- `gateClosingTime` → `xsd:dateTime`
- `barcodeValue` → `xsd:string`

### `PassengerTransportEvent`

- `eventType` → `skos:Concept`
- `eventStatus` → `skos:Concept`
- `eventLocation` → `ebwv:Location`
- `relatesToMovement` → `PassengerTransportMovement`
- `relatesToEntitlement` → `PassengerTransportEntitlement`
- `relatesToBoardingPass` → `BoardingPass`
- `recordedBy` → `org:Organization`
- `eventTime` → `xsd:dateTime`
- `eventNote` → `rdf:langString`

### `Vehicle`

- `vehicleIdentifier` → `ebwv:Identifier`
- `vehicleType` → `skos:Concept`
- `vehicleName` → `xsd:string`
