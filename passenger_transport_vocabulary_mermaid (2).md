# European Business Wallet Passenger Transport Vocabulary v0.1

Vocabulary for person transport entitlement, service, movement, boarding pass and transport event data in verifiable credentials and wallet-based transport use cases.

This Markdown file is ready for GitHub publication and contains a Mermaid class diagram generated from the YAML vocabulary.

## Mermaid class diagram

```mermaid
classDiagram
direction LR
class PassengerTransportEntitlement {
  +entitlementIdentifier: Identifier
  +bookingReference: Identifier
  +holder: Person
  +issuer: Organization
  +forService: PassengerTransportService
  +forMovement: PassengerTransportMovement
  +fareType: Concept
  +travelClass: Concept
  +serviceLevel: Concept
  +price: MonetaryAmount
  +includedService: Concept
  +validFrom: dateTime
  +validUntil: dateTime
  +isFlexible: boolean
  +isRefundable: boolean
  +isChangeable: boolean
  +usageConditions: langString
}
class PassengerTransportService {
  +serviceProvider: Organization
  +modeOfTransport: Concept
  +originLocation: Location
  +destinationLocation: Location
  +serviceStatus: Concept
  +serviceNumber: string
  +scheduledDepartureTime: dateTime
  +scheduledArrivalTime: dateTime
}
class PassengerTransportMovement {
  +modeOfTransport: Concept
  +serviceNumber: string
  +scheduledDepartureTime: dateTime
  +scheduledArrivalTime: dateTime
  +isInstanceOfService: PassengerTransportService
  +operatingCarrier: Organization
  +departureLocation: Location
  +arrivalLocation: Location
  +vehicle: Vehicle
  +movementStatus: Concept
  +actualDepartureTime: dateTime
  +actualArrivalTime: dateTime
}
class BoardingPass {
  +isBasedOnEntitlement: PassengerTransportEntitlement
  +isForMovement: PassengerTransportMovement
  +passenger: Person
  +boardingStatus: Concept
  +checkInStatus: Concept
  +cabinClass: Concept
  +validationStatus: Concept
  +seatNumber: string
  +coachNumber: string
  +departureGate: string
  +platform: string
  +boardingGroup: string
  +boardingTime: dateTime
  +gateClosingTime: dateTime
  +barcodeValue: string
}
class PassengerTransportEvent {
  +eventType: Concept
  +eventStatus: Concept
  +eventLocation: Location
  +relatesToMovement: PassengerTransportMovement
  +relatesToEntitlement: PassengerTransportEntitlement
  +relatesToBoardingPass: BoardingPass
  +recordedBy: Organization
  +eventTime: dateTime
  +eventNote: langString
}
class Vehicle {
  +vehicleIdentifier: Identifier
  +vehicleType: Concept
  +vehicleName: string
}
class Concept
class Identifier
class Location
class MonetaryAmount
class Organization
class Person
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
