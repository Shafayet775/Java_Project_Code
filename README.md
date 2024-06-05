# Java_Project_Code
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class AirlineReservationSystem {
    // Flight class definition
    static class Flight {
        private String flightNumber;
        private String origin;
        private String destination;
        private int totalSeats;
        private int availableSeats;

        public Flight(String flightNumber, String origin, String destination, int totalSeats) {
            this.flightNumber = flightNumber;
            this.origin = origin;
            this.destination = destination;
            this.totalSeats = totalSeats;
            this.availableSeats = totalSeats;
        }

        public String getFlightNumber() {
            return flightNumber;
        }

        public String getOrigin() {
            return origin;
        }

        public String getDestination() {
            return destination;
        }

        public int getAvailableSeats() {
            return availableSeats;
        }

        public boolean bookSeat() {
            if (availableSeats > 0) {
                availableSeats--;
                return true;
            }
            return false;
        }

        @Override
        public String toString() {
            return "Flight " + flightNumber + " from " + origin + " to " + destination +
                   " | Available Seats: " + availableSeats;
        }
    }

    // Reservation class definition
    static class Reservation {
        private String passengerName;
        private Flight flight;

        public Reservation(String passengerName, Flight flight) {
            this.passengerName = passengerName;
            this.flight = flight;
        }

        public String getPassengerName() {
            return passengerName;
        }

        public Flight getFlight() {
            return flight;
        }

        @Override
        public String toString() {
            return "Reservation for " + passengerName + " on " + flight;
        }
    }

    // AirlineReservationSystem class definition
    private List<Flight> flights = new ArrayList<>();
    private List<Reservation> reservations = new ArrayList<>();

    public void addFlight(String flightNumber, String origin, String destination, int totalSeats) {
        flights.add(new Flight(flightNumber, origin, destination, totalSeats));
    }

    public List<Flight> getFlights() {
        return flights;
    }

    public Flight findFlight(String flightNumber) {
        for (Flight flight : flights) {
            if (flight.getFlightNumber().equalsIgnoreCase(flightNumber)) {
                return flight;
            }
        }
        return null;
    }

    public boolean bookFlight(String passengerName, String flightNumber) {
        Flight flight = findFlight(flightNumber);
        if (flight != null && flight.bookSeat()) {
            reservations.add(new Reservation(passengerName, flight));
            return true;
        }
        return false;
    }

    public List<Reservation> getReservations() {
        return reservations;
    }

    // Main method to run the application
    public static void main(String[] args) {
        AirlineReservationSystem system = new AirlineReservationSystem();
        system.addFlight("AA101", "New York", "Los Angeles", 200);
        system.addFlight("BA202", "London", "Paris", 100);
        system.addFlight("CA303", "Beijing", "Tokyo", 150);

        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("1. View Flights");
            System.out.println("2. Book Flight");
            System.out.println("3. View Reservations");
            System.out.println("4. Exit");

            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (choice) {
                case 1:
                    for (Flight flight : system.getFlights()) {
                        System.out.println(flight);
                    }
                    break;

                case 2:
                    System.out.println("Enter Passenger Name:");
                    String passengerName = scanner.nextLine();
                    System.out.println("Enter Flight Number:");
                    String flightNumber = scanner.nextLine();
                    if (system.bookFlight(passengerName, flightNumber)) {
                        System.out.println("Booking successful!");
                    } else {
                        System.out.println("Booking failed. Flight may be full or not found.");
                    }
                    break;

                case 3:
                    for (Reservation reservation : system.getReservations()) {
                        System.out.println(reservation);
                    }
                    break;

                case 4:
                    System.out.println("Exiting...");
                    scanner.close();
                    return;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
