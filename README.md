import java.time.LocalTime;
import java.util.*;

class Reservation {
    String name, from, to, trainName, classType, date;
    int trainNo;
    String pnr;

    Reservation(String name, int trainNo, String trainName, String classType, String date, String from, String to, String pnr) {
        this.name = name;
        this.trainNo = trainNo;
        this.trainName = trainName;
        this.classType = classType;
        this.date = date;
        this.from = from;
        this.to = to;
        this.pnr = pnr;
    }

    void display() {
        System.out.println("PNR: " + pnr);
        System.out.println("Name: " + name);
        System.out.println("Train No: " + trainNo);
        System.out.println("Train Name: " + trainName);
        System.out.println("Class: " + classType);
        System.out.println("Date of Journey: " + date);
        System.out.println("From: " + from + " To: " + to);
    }
}

public class OnlineReservationSystem {

    static Scanner sc = new Scanner(System.in);
    static Map<String, String> users = new HashMap<>();
    static List<Reservation> reservations = new ArrayList<>();
    private static String put;

    public static void main(String[] args) {
        // Predefined login
        users.put("Akash9008", "AKASHDAS@999a");
        users.put("Dip006", "DIP@9090d");
        System.out.println("=== Welcome To IRCTC Train Reservation System ===");

        int option;
        do {
            System.out.println("\n1. Sign Up\n2. Login\n3. Exit");
            System.out.print("Enter your choice: ");
            option = sc.nextInt();
            sc.nextLine(); // consume newline

            switch (option) {
                case 1 -> signUp();
                case 2 -> {
                    if (login()) {
                        System.out.println("Login Successful!");
                        System.out.println(getGreeting() + "!");
                        System.out.println("Welcome to Our online Reservation System");
                        int choice;
                        do {
                            System.out.println("\n1. Make Reservation\n2. Cancel Reservation\n3. Exit");
                            System.out.print("Enter your choice: ");
                            choice = sc.nextInt();
                            sc.nextLine(); // consume newline

                            switch (choice) {
                                case 1 -> makeReservation();
                                case 2 -> cancelReservation();
                                case 3 -> System.out.println("Thank you for using our Reservation System. Have a good day.");
                                default -> System.out.println("Invalid choice!");
                            }
                        } while (choice != 3);
                    } else {
                        System.out.println("Login failed!");
                    }
                }
                case 3 -> System.out.println("Exiting...");
                default -> System.out.println("Invalid choice!");
            }
        } while (option != 3);
    }

    public static void signUp() {
        System.out.print("Enter new Login ID: ");
        String newId = sc.nextLine();
        if (users.containsKey(newId)) {
            System.out.println("User already exists in our system. Please try a different ID.");
            return;
        }
        System.out.print("Enter new Password: ");
        String newPass = sc.nextLine();
        users.put(newId, newPass);
        System.out.println("Sign Up successful! You can now log in.");
    }

    public static boolean login() {
        System.out.print("Please Enter Your Login ID: ");
        String id = sc.nextLine();
        System.out.print("Please Your Enter Password: ");
        String pass = sc.nextLine();

        return users.containsKey(id) && users.get(id).equals(pass);
    }

    public static void makeReservation() {
        System.out.print("Enter Your Name: ");
        String name = sc.nextLine();

        System.out.print("Enter Train Number: ");
        int trainNo = sc.nextInt();
        sc.nextLine();

        String trainName = getTrainName(trainNo);

        System.out.print("Enter Class Type (e.g., Sleeper, AC): ");
        String classType = sc.nextLine();

        System.out.print("Enter Date of Journey (dd-mm-yyyy): ");
        String date = sc.nextLine();

        System.out.print("Enter From: ");
        String from = sc.nextLine();

        System.out.print("Enter To: ");
        String to = sc.nextLine();

        Random rand = new Random();
        String pnr = "PNR" + (100000 + rand.nextInt(900000));

        Reservation r = new Reservation(name, trainNo, trainName, classType, date, from, to, pnr);
        reservations.add(r);

        System.out.println("\nReservation Successful!");
        System.out.println("\nYour Journey Details:");
        r.display();
    }

    public static void cancelReservation() {
        System.out.print("Enter PNR to Cancel: ");
        String pnr = sc.nextLine();

        boolean found = false;
        Iterator<Reservation> iterator = reservations.iterator();
        while (iterator.hasNext()) {
            Reservation r = iterator.next();
            if (r.pnr.equalsIgnoreCase(pnr)) {
                found = true;
                System.out.println("\n=== Your Reservation Details ===");
                r.display(); // Show full details before confirmation

                System.out.print("Confirm cancellation (yes/no)? ");
                String confirm = sc.nextLine();
                if (confirm.equalsIgnoreCase("yes")) {
                    iterator.remove();
                    System.out.println("Reservation Cancelled Successfully.");
                } else {
                    System.out.println("Cancellation Aborted.");
                }
                break;
            }
        }
        if (!found) {
            System.out.println("No reservation found with PNR: " + pnr);
        }
    }

    public static String getTrainName(int trainNo) {
        // Dummy data for example
        switch (trainNo) {
            case 101: return "Rajdhani Express";
            case 102: return "Shatabdi Express";
            case 103: return "Duronto Express";
            case 104: return "Jan Shatabdi Express";
            case 105: return "Garib Rath Express";
            case 106: return "Express Mail";
            case 107: return "Superfast Express";
            case 108: return "Intercity Express";
            case 109: return "Passenger Train";
            case 110: return "Local Train";
            case 111: return "Metro Express";
            case 112: return "Luxury Express";
            case 113: return "Hill Station Express";
            case 114: return "Coastal Express";
            case 115: return "Heritage Express";
            case 116: return "Freight Express";
            case 117: return "Tourist Express";
            case 118: return "Luxury Sleeper Express";
            case 119: return "High-Speed Express";
            case 120: return "Night Express";
            case 121: return "Suburban Express";
            case 122: return "Regional Express";
            case 123: return "Interstate Express";
            case 124: return "City Express";
            case 125: return "Express Cargo";
            default: return "Generic Express";
        }
    }

    public static String getGreeting() {
        LocalTime now = LocalTime.now();
        int hour = now.getHour();
        if (hour >= 5 && hour < 12) {
            return "Good Morning";
        } else if (hour >= 12 && hour < 17) {
            return "Good Afternoon";
        } else if (hour >= 17 && hour < 21) {
            return "Good Evening";
        } else {
            return "Good Night";
        }
    }
}
