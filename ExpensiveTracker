import java.io.*;
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.*;

class Transaction implements Serializable {
    private static final long serialVersionUID = 1L;
    private String type; // Income or Expense
    private String category;
    private double amount;
    private LocalDate date;
    private String description;

    public Transaction(String type, String category, double amount, LocalDate date, String description) {
        this.type = type;
        this.category = category;
        this.amount = amount;
        this.date = date;
        this.description = description;
    }

    public String getType() { return type; }
    public String getCategory() { return category; }
    public double getAmount() { return amount; }
    public LocalDate getDate() { return date; }
    public String getDescription() { return description; }

    @Override
    public String toString() {
        return String.format("%s | %-10s | %-10s | ₹%.2f | %s",
                date, type, category, amount, description);
    }
}

public class ExpenseTracker {
    private static final String DATA_FILE = "transactions.dat";
    private static ArrayList<Transaction> transactions = new ArrayList<>();
    private static Scanner sc = new Scanner(System.in);
    private static DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");

    public static void main(String[] args) {
        loadData();

        while (true) {
            System.out.println("\n===== EXPENSE TRACKER =====");
            System.out.println("1. Add Income");
            System.out.println("2. Add Expense");
            System.out.println("3. View Transactions");
            System.out.println("4. Generate Report");
            System.out.println("5. Search Transactions");
            System.out.println("6. Export to CSV");
            System.out.println("7. Exit");
            System.out.print("Choose an option: ");
            int choice = getIntInput();

            switch (choice) {
                case 1:
                    addTransaction("Income");
                    break;
                case 2:
                    addTransaction("Expense");
                    break;
                case 3:
                    viewTransactions();
                    break;
                case 4:
                    generateReport();
                    break;
                case 5:
                    searchTransactions();
                    break;
                case 6:
                    exportToCSV();
                    break;
                case 7:
                    saveData();
                    System.out.println("Goodbye!");
                    return;
                default:
                    System.out.println("Invalid choice!");
                    break;
            }
        }
    }

    // ---------- ADD TRANSACTION ----------
    private static void addTransaction(String type) {
        System.out.print("Enter category (Food, Rent, etc.): ");
        String category = sc.nextLine();
        System.out.print("Enter amount: ₹");
        double amount = getDoubleInput();
        System.out.print("Enter date (yyyy-MM-dd): ");
        LocalDate date = LocalDate.parse(sc.nextLine(), formatter);
        System.out.print("Enter description: ");
        String desc = sc.nextLine();

        transactions.add(new Transaction(type, category, amount, date, desc));
        saveData();
        System.out.println("✅ Transaction added successfully!");
    }

    // ---------- VIEW ALL TRANSACTIONS ----------
    private static void viewTransactions() {
        if (transactions.isEmpty()) {
            System.out.println("No transactions found.");
            return;
        }
        System.out.println("\nDATE       | TYPE       | CATEGORY   | AMOUNT | DESCRIPTION");
        System.out.println("------------------------------------------------------------");
        for (Transaction t : transactions) {
            System.out.println(t);
        }
    }

    // ---------- GENERATE REPORT ----------
    private static void generateReport() {
        System.out.println("1. Daily Report");
        System.out.println("2. Monthly Report");
        System.out.println("3. Yearly Report");
        System.out.print("Choose report type: ");
        int choice = getIntInput();

        System.out.print("Enter date (yyyy-MM-dd for daily / yyyy-MM for monthly / yyyy for yearly): ");
        String input = sc.nextLine();

        double totalIncome = 0, totalExpense = 0;

        for (Transaction t : transactions) {
            String dateStr = t.getDate().toString();

            if ((choice == 1 && dateStr.equals(input)) ||
                (choice == 2 && dateStr.startsWith(input)) ||
                (choice == 3 && dateStr.startsWith(input))) {

                if (t.getType().equalsIgnoreCase("Income"))
                    totalIncome += t.getAmount();
                else
                    totalExpense += t.getAmount();
            }
        }

        System.out.println("\n===== REPORT =====");
        System.out.println("Total Income : ₹" + totalIncome);
        System.out.println("Total Expense: ₹" + totalExpense);
        System.out.println("Net Balance  : ₹" + (totalIncome - totalExpense));
    }

    // ---------- SEARCH TRANSACTIONS ----------
    private static void searchTransactions() {
        System.out.print("Enter keyword to search (category/description): ");
        String keyword = sc.nextLine().toLowerCase();

        boolean found = false;
        for (Transaction t : transactions) {
            if (t.getCategory().toLowerCase().contains(keyword) ||
                t.getDescription().toLowerCase().contains(keyword)) {
                if (!found) {
                    System.out.println("\nResults:");
                    System.out.println("------------------------------------------------------------");
                    found = true;
                }
                System.out.println(t);
            }
        }

        if (!found) System.out.println("No matching transactions found.");
    }

    // ---------- EXPORT TO CSV ----------
    private static void exportToCSV() {
        try (PrintWriter writer = new PrintWriter(new File("transactions.csv"))) {
            writer.println("Date,Type,Category,Amount,Description");
            for (Transaction t : transactions) {
                writer.printf("%s,%s,%s,%.2f,%s%n",
                        t.getDate(), t.getType(), t.getCategory(), t.getAmount(), t.getDescription());
            }
            System.out.println("✅ Exported to transactions.csv successfully!");
        } catch (Exception e) {
            System.out.println("Error exporting data: " + e.getMessage());
        }
    }

    // ---------- FILE SAVE & LOAD ----------
    @SuppressWarnings("unchecked")
    private static void loadData() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(DATA_FILE))) {
            transactions = (ArrayList<Transaction>) ois.readObject();
        } catch (Exception ignored) {}
    }

    private static void saveData() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(DATA_FILE))) {
            oos.writeObject(transactions);
        } catch (Exception e) {
            System.out.println("Error saving data: " + e.getMessage());
        }
    }

    // ---------- INPUT HELPERS ----------
    private static int getIntInput() {
        while (true) {
            try {
                return Integer.parseInt(sc.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("Invalid input. Enter a number: ");
            }
        }
    }

    private static double getDoubleInput() {
        while (true) {
            try {
                return Double.parseDouble(sc.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("Invalid amount. Enter a number: ₹");
            }
        }
    }
}
