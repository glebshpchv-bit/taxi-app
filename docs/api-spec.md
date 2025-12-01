Here's a YAML-formatted OpenAPI 3.0 specification for the core endpoints of a "taxi ordering service." This specification outlines the primary functionalities for users to request, track, and manage their taxi rides, as well as retrieve fare estimates.

```yaml
openapi: 3.0.0
info:
  title: Taxi Ordering Service API
  description: |
    API for users to order, track, and manage taxi rides.
    This service allows users to:
    - Get price and time estimates for potential rides.
    - Request new taxi rides with specified pickup, destination, and payment methods.
    - View their ride history and details of specific rides.
    - Cancel active or pending rides.
  version: 1.0.0
servers:
  - url: https://api.taxiapp.com/v1
    description: Production server
  - url: http://localhost:8080/v1
    description: Development server
security:
  - bearerAuth: [] # All core endpoints require authentication
tags:
  - name: Estimates
    description: Operations for getting ride price and time estimates before booking.
  - name: Rides
    description: Operations related to ordering and managing taxi rides.
paths:
  /estimate:
    get:
      tags:
        - Estimates
      summary: Get a ride price and time estimate
      description: Provides an estimated price and time for a potential ride based on pickup and destination locations.
      operationId: getRideEstimate
      parameters:
        - in: query
          name: pickupLat
          schema:
            type: number
            format: float
          required: true
          description: Latitude of the pickup location.
          example: 34.0522
        - in: query
          name: pickupLon
          schema:
            type: number
            format: float
          required: true
          description: Longitude of the pickup location.
          example: -118.2437
        - in: query
          name: destinationLat
          schema:
            type: number
            format: float
          required: true
          description: Latitude of the destination location.
          example: 34.0000
        - in: query
          name: destinationLon
          schema:
            type: number
            format: float
          required: true
          description: Longitude of the destination location.
          example: -118.5000
      responses:
        '200':
          description: Successful estimate retrieval.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Estimate'
        '400':
          $ref: '#/components/responses/BadRequest'
        '500':
          $ref: '#/components/responses/ServerError'

  /rides:
    post:
      tags:
        - Rides
      summary: Request a new taxi ride
      description: Creates a new ride request with specified pickup, destination, and payment method.
      operationId: createRide
      requestBody:
        description: Ride request details
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RideRequest'
      responses:
        '201':
          description: Ride successfully created.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Ride'
        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '500':
          $ref: '#/components/responses/ServerError'
    get:
      tags:
        - Rides
      summary: Get user's ride history
      description: Retrieves a list of all rides (past and active) for the authenticated user.
      operationId: getUserRides
      parameters:
        - in: query
          name: status
          schema:
            type: string
            enum: [pending, accepted, en_route_to_pickup, in_progress, completed, cancelled, no_drivers_found]
          description: Filter rides by their current status.
          example: completed
        - in: query
          name: limit
          schema:
            type: integer
            format: int32
            default: 20
            minimum: 1
            maximum: 100
          description: Maximum number of rides to return.
        - in: query
          name: offset
          schema:
            type: integer
            format: int32
            default: 0
            minimum: 0
          description: Offset for pagination.
      responses:
        '200':
          description: A list of rides.
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Ride'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '500':
          $ref: '#/components/responses/ServerError'

  /rides/{rideId}:
    parameters:
      - in: path
        name: rideId
        schema:
          type: string
          format: uuid
        required: true
        description: Unique identifier of the ride.
        example: "1a2b3c4d-5e6f-7890-1234-567890abcdef"
    get:
      tags:
        - Rides
      summary: Get details of a specific ride
      description: Retrieves detailed information about a single ride by its ID.
      operationId: getRideDetails
      responses:
        '200':
          description: Detailed ride information.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Ride'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'
        '500':
          $ref: '#/components/responses/ServerError'
    delete:
      tags:
        - Rides
      summary: Cancel a ride
      description: Allows the user to cancel an active or pending ride.
      operationId: cancelRide
      responses:
        '200':
          description: Ride successfully cancelled.
          content:
            application/json:
              schema:
                type: object
                properties:
                  rideId:
                    type: string
                    format: uuid
                    description: The ID of the cancelled ride.
                  status:
                    type: string
                    enum: [cancelled]
                    description: The new status of the ride.
        '401':
          $ref: '#/components/responses/Unauthorized'
        '404':
          $ref: '#/components/responses/NotFound'
        '409':
          description: Conflict - Ride cannot be cancelled (e.g., driver already arrived or ride completed).
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Error'
        '500':
          $ref: '#/components/responses/ServerError'

components:
  schemas:
    Location:
      type: object
      required:
        - latitude
        - longitude
      properties:
        latitude:
          type: number
          format: double
          minimum: -90
          maximum: 90
          description: Latitude coordinate.
        longitude:
          type: number
          format: double
          minimum: -180
          maximum: 180
          description: Longitude coordinate.
      example:
        latitude: 34.0522
        longitude: -118.2437

    PaymentMethod:
      type: string
      enum:
        - cash
        - credit_card
        - apple_pay
        - google_pay
      description: The method used to pay for the ride.

    RideRequest:
      type: object
      required:
        - pickupLocation
        - destination
        - paymentMethod
      properties:
        pickupLocation:
          $ref: '#/components/schemas/Location'
          description: The starting point of the ride.
        destination:
          $ref: '#/components/schemas/Location'
          description: The desired destination of the ride.
        paymentMethod:
          $ref: '#/components/schemas/PaymentMethod'
          description: The chosen payment method for the ride.
      example:
        pickupLocation:
          latitude: 34.0522
          longitude: -118.2437
        destination:
          latitude: 34.0000
          longitude: -118.5000
        paymentMethod: credit_card

    RideStatus:
      type: string
      enum:
        - pending # Awaiting driver assignment
        - searching_driver # Actively looking for a driver
        - accepted # Driver assigned, on their way to pickup
        - en_route_to_pickup # Driver is en route to pickup location
        - arriving # Driver is near pickup location
        - in_progress # Passenger is in the car
        - completed # Ride finished successfully
        - cancelled # Ride cancelled by user or driver
        - no_drivers_found # No drivers available for the request
      description: Current status of the ride.

    DriverInfo:
      type: object
      properties:
        driverId:
          type: string
          format: uuid
          description: Unique identifier for the driver.
        name:
          type: string
          description: Driver's name.
        rating:
          type: number
          format: float
          minimum: 1
          maximum: 5
          description: Driver's average rating.
        phoneNumber:
          type: string
          pattern: '^\\+\\d{10,15}$' # E.g., +15551234567
          description: Driver's contact number (masked or full based on privacy policy).
      example:
        driverId: "a1b2c3d4-e5f6-7890-1234-567890abcdef"
        name: "John Doe"
        rating: 4.8
        phoneNumber: "+15551234567"

    VehicleInfo:
      type: object
      properties:
        vehicleId:
          type: string
          format: uuid
          description: Unique identifier for the vehicle.
        make:
          type: string
          description: Vehicle manufacturer.
        model:
          type: string
          description: Vehicle model.
        licensePlate:
          type: string
          description: Vehicle license plate number.
        color:
          type: string
          description: Vehicle color.
      example:
        vehicleId: "f9e8d7c6-b5a4-3210-fedc-ba9876543210"
        make: "Toyota"
        model: "Camry"
        licensePlate: "XYZ123"
        color: "Silver"

    Ride:
      type: object
      required:
        - rideId
        - status
        - pickupLocation
        - destination
        - createdAt
      properties:
        rideId:
          type: string
          format: uuid
          description: Unique identifier for the ride.
        status:
          $ref: '#/components/schemas/RideStatus'
        pickupLocation:
          $ref: '#/components/schemas/Location'
        destination:
          $ref: '#/components/schemas/Location'
        driverInfo:
          $ref: '#/components/schemas/DriverInfo'
          nullable: true
          description: Details of the assigned driver, if any. Present once a driver accepts.
        vehicleInfo:
          $ref: '#/components/schemas/VehicleInfo'
          nullable: true
          description: Details of the assigned vehicle, if any. Present once a driver accepts.
        currentDriverLocation:
          $ref: '#/components/schemas/Location'
          nullable: true
          description: Current real-time location of the assigned driver.
        estimatedPrice:
          type: number
          format: float
          description: Estimated price of the ride.
          nullable: true # Could be null if not yet calculated or if a specific pricing model is used
        actualPrice:
          type: number
          format: float
          description: Actual final price of the ride after completion.
          nullable: true
        estimatedArrivalTime:
          type: string
          format: date-time
          description: Estimated time of arrival at the destination.
          nullable: true
        createdAt:
          type: string
          format: date-time
          description: Timestamp when the ride was requested.
        updatedAt:
          type: string
          format: date-time
          description: Timestamp of the last update to the ride status.
          nullable: true
      example:
        rideId: "1a2b3c4d-5e6f-7890-1234-567890abcdef"
        status: accepted
        pickupLocation:
          latitude: 34.0522
          longitude: -118.2437
        destination:
          latitude: 34.0000
          longitude: -118.5000
        driverInfo:
          driverId: "a1b2c3d4-e5f6-7890-1234-567890abcdef"
          name: "John Doe"
          rating: 4.8
          phoneNumber: "+15551234567"
        vehicleInfo:
          vehicleId: "f9e8d7c6-b5a4-3210-fedc-ba9876543210"
          make: "Toyota"
          model: "Camry"
          licensePlate: "XYZ123"
          color: "Silver"
        currentDriverLocation:
          latitude: 34.0400
          longitude: -118.2500
        estimatedPrice: 25.50
        estimatedArrivalTime: "2023-10-27T10:30:00Z"
        createdAt: "2023-10-27T10:00:00Z"
        updatedAt: "2023-10-27T10:05:00Z"

    Estimate:
      type: object
      required:
        - estimatedPrice
        - estimatedTimeMinutes
      properties:
        estimatedPrice:
          type: number
          format: float
          description: The estimated cost of the ride in the local currency.
        estimatedTimeMinutes:
          type: integer
          description: The estimated travel time in minutes.
      example:
        estimatedPrice: 25.50
        estimatedTimeMinutes: 15

    Error:
      type: object
      required:
        - code
        - message
      properties:
        code:
          type: string
          description: A unique error code.
          example: RIDE_NOT_FOUND
        message:
          type: string
          description: A human-readable error message.
          example: The requested ride could not be found.

  responses:
    Unauthorized:
      description: Authentication required or invalid token.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: AUTH_REQUIRED
            message: Authentication token is missing or invalid.
    BadRequest:
      description: Invalid request parameters or body.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: INVALID_INPUT
            message: Missing required fields or invalid data format.
    NotFound:
      description: The requested resource was not found.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: RESOURCE_NOT_FOUND
            message: The requested resource does not exist.
    ServerError:
      description: Internal server error.
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/Error'
          example:
            code: INTERNAL_SERVER_ERROR
            message: An unexpected error occurred on the server.

  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
      description: JWT token for user authentication.
```