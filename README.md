import java.util.Scanner;

public class CinemaBooking {

    static final int ROWS = 5;
    static final int COLS = 6;

    static char[][] seats = new char[ROWS][COLS];
    static String[] movieNames = {"Superman", "Avatar", "Minecraft", "Inside Out", "F1"};
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        initSeats();
        int choice;

        do {
            showMenu();
            choice = readInt("Enter your choice: ");

            switch (choice) {
                case 1:
                    displaySeats();
                    break;
                case 2:
                    bookSeat();
                    break;
                case 3:
                    cancelBooking();
                    break;
                case 4:
                    showMovies();
                    break;
                case 5:
                    showSeatCounts();
                    break;
                case 0:
                    System.out.println("Thank you for visiting the cinema. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        } while (choice != 0);

        scanner.close();
    }

    // Fill every seat with 'O' (available)
    static void initSeats() {
        for (int i = 0; i < ROWS; i++) {
            for (int j = 0; j < COLS; j++) {
                seats[i][j] = 'O';
            }
        }
    }

    static void showMenu() {
        System.out.println("\n===== CINEMA TICKET BOOKING =====");
        System.out.println("1. Display Seats");
        System.out.println("2. Book Seat");
        System.out.println("3. Cancel Booking");
        System.out.println("4. Show all movies");
        System.out.println("5. Show number of available and booked seats");
        System.out.println("0. Exit");
    }

    // Reads an integer safely; returns -1 if the input is not a number
    static int readInt(String prompt) {
        System.out.print(prompt);
        if (scanner.hasNextInt()) {
            return scanner.nextInt();
        }
        scanner.next(); // discard bad input
        return -1;
    }

    static void displaySeats() {
        System.out.println("\n        SCREEN");
        System.out.print("     ");
        for (int j = 1; j <= COLS; j++) {
            System.out.print(j + " ");
        }
        System.out.println();
        for (int i = 0; i < ROWS; i++) {
            System.out.print("Row " + (i + 1) + " ");
            for (int j = 0; j < COLS; j++) {
                System.out.print(seats[i][j] + " ");
            }
            System.out.println();
        }
        System.out.println("(O = Available, X = Booked)");
    }

    // Returns true if the seat code (e.g. 12) is a valid row/column combination
    static boolean isValidSeat(int code) {
        if (code < 0) {
            return false;
        }
        int row = code / 10;
        int col = code % 10;
        return row >= 1 && row <= ROWS && col >= 1 && col <= COLS;
    }

    static void bookSeat() {
        int code = readInt("Enter seat number (row then seat, e.g. 12): ");
        if (!isValidSeat(code)) {
            System.out.println("Invalid Seat.");
            return;
        }
        int row = code / 10 - 1;
        int col = code % 10 - 1;

        if (seats[row][col] == 'X') {
            System.out.println("Sorry, this seat is already booked.");
        } else {
            seats[row][col] = 'X';
            System.out.println("Seat " + code + " booked successfully.");
        }
    }

    static void cancelBooking() {
        int code = readInt("Enter seat number to cancel (e.g. 12): ");
        if (!isValidSeat(code)) {
            System.out.println("Invalid Seat.");
            return;
        }
        int row = code / 10 - 1;
        int col = code % 10 - 1;

        if (seats[row][col] == 'O') {
            System.out.println("This seat is not booked, nothing to cancel.");
        } else {
            seats[row][col] = 'O';
            System.out.println("Booking for seat " + code + " cancelled.");
        }
    }

    static void showMovies() {
        System.out.println("\nNow Showing:");
        for (int i = 0; i < movieNames.length; i++) {
            System.out.println((i + 1) + ". " + movieNames[i]);
        }
    }

    static void showSeatCounts() {
        int available = 0;
        int booked = 0;

        System.out.print("Booked seats: ");
        StringBuilder bookedList = new StringBuilder();
        StringBuilder availableList = new StringBuilder();

        for (int i = 0; i < ROWS; i++) {
            for (int j = 0; j < COLS; j++) {
                String code = "" + (i + 1) + (j + 1);
                if (seats[i][j] == 'X') {
                    booked++;
                    bookedList.append(code).append(" ");
                } else {
                    available++;
                    availableList.append(code).append(" ");
                }
            }
        }

        System.out.println(booked == 0 ? "none" : bookedList.toString().trim());
        System.out.println("Available seats: " + (available == 0 ? "none" : availableList.toString().trim()));
        System.out.println("Total available: " + available);
        System.out.println("Total booked: " + booked);
    }
}
