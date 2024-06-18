# MovieTicketBooking

A OOD Design practice project

## System Requirements

- It should be able to list the cities where affiliate cinemas are located.
- Each cinema can have multiple halls and each hall can run one movie show at a time.
- Each Movie will have multiple shows.
- Customers should be able to search movies by their title, language, genre, release date, and city name.
- Once the customer selects a movie, the service should display the cinemas running that movie and its available shows.
- The customer should be able to select a show at a particular cinema and book their tickets.
- The service should show the customer the seating arrangement of the cinema hall. The customer should be able to select multiple seats according to their preference.
- The customer should be able to distinguish between available seats and booked ones.
- The system should send notifications whenever there is a new movie, as well as when a booking is made or canceled.
- Customers of our system should be able to pay with credit cards or cash.
- The system should ensure that no two customers can reserve the same seat.
- Customers should be able to add a discount coupon to their payment.

## FLow Analysis

- Customer
  - Customers should be able to search movies by their title, language, genre, release date, and city name.
  - Once the customer selects a movie, the service should display the cinemas running that movie and its available shows.
  - The customer should be able to select a show at a particular cinema and book their tickets.
  - The service should show the customer the seating arrangement of the cinema hall. The customer should be able to select multiple seats according to their preference.
  - Customers of our system should be able to pay with credit cards or cash.
  - Customers should be able to add a discount coupon to their payment.

- Cinema
  - It should be able to list the cities where affiliate cinemas are located.
  - Each cinema can have multiple halls and each hall can run one movie show at a time.
  - Each Movie will have multiple shows.
  - The service should show the customer the seating arrangement of the cinema hall. The customer should be able to select multiple seats according to their preference.
  
- System
  - The system should send notifications whenever there is a new movie, as well as when a booking is made or canceled.
  - The system should ensure that no two customers can reserve the same seat.

### Components

- Cinemas
  - Halls
    - Shows
      - Seating
- Tickets
- Customers
- Movies
  - Title
  - Language
  - Genre
  - Release Data
  - City Name
- Notifications
- Payment
  - Coupon

#### Cinema

``` ruby
class Cinema
    has_many: halls
    belongs_to: city
end

# IST
class Hall
    has_many: shows
    has_one: seating_map
end

class LargeHall < Hall; end

class SmallHall < Hall; end

class SeatingMap
    has_many: seats
end

class Seat
    attr_accessor: isOccupied
end

class Show
    belongs_to: hall
    belongs_to: movie
    has_one: seating_map

    validate_presence_of :start_time
end

class Reservation
    belongs_to: show
    has_one: payment

    enum status: [:initialized, :reserved, :cancelled; :expired]
end

class Payment
    attr_accessor :paid
    
    def pay_by_credit; end
    def pay_by_debit; end

    def apply_coupon; end

    def generateTicket
        return if !paid

        ticket = TicketFactory.buildTicket(self.reservation)
        Notification.notify(ticket) 
    end
end

class Ticket
    belongs_to: reservation

end

class Movie
    has_one: title
    has_one: language
    has_one: genre
    has_one: release_date
    has_one: city_name

    has_many: shows
end





```
