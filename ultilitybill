import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.util.Random;
import java.util.Scanner;

class UtilityBillCalculator implements Runnable {
    protected int consumption;
    protected static final double TAX_RATE = 0.06;
    protected double billToPay;

    public UtilityBillCalculator(int consumption) {
        this.consumption = consumption;
    }

    public double getBillToPay() {
        return billToPay;
    }

    @Override
    public void run() {
        // This is a placeholder method, each subclass should provide its implementation.
    }
}

class WaterBillCalculator extends UtilityBillCalculator {
    public WaterBillCalculator(int consumption) {
        super(consumption);
    }

    @Override
    public void run() {
        synchronized (this) {
            // Adjust the calculation based on water billing logic
            if (consumption <= 500) {
                billToPay = consumption * 0.02;
            } else if (consumption <= 1000) {
                billToPay = 500 * 0.02 + (consumption - 500) * 0.04;
            } else {
                billToPay = 500 * 0.02 + 500 * 0.04 + (consumption - 1000) * 0.06;
            }

            // Apply static tax rate
            billToPay *= (1 + TAX_RATE);
        }
    }
}

class ElectricityBillCalculator extends UtilityBillCalculator {
    public ElectricityBillCalculator(int consumption) {
        super(consumption);
    }

    @Override
    public void run() {
        synchronized (this) {
            // Adjust the calculation based on electricity billing logic
            if (consumption < 201) {
                billToPay = consumption * 0.218;
            } else if (consumption < 301) {
                billToPay = 200 * 0.218 + (consumption - 200) * 0.334;
            } else if (consumption < 601) {
                billToPay = 200 * 0.218 + 100 * 0.334 + (consumption - 300) * 0.516;
            } else if (consumption >= 601) {
                billToPay = 200 * 0.218 + 300 * 0.334 + 100 * 0.516 + (consumption - 600) * 0.546;
            }

            // Apply static tax rate
            billToPay *= (1 + TAX_RATE);
        }
    }
}

class CurrentDateTimeFetcher implements Runnable {
    private LocalDateTime currentDateTime;

    public LocalDateTime getCurrentDateTime() {
        return currentDateTime;
    }

    @Override
    public void run() {
        synchronized (this) {
            currentDateTime = LocalDateTime.now();
        }
    }
}

class BillIdGenerator implements Runnable {
    private int billId;

    public int getBillId() {
        return billId;
    }

    @Override
    public void run() {
        synchronized (this) {
            Random random = new Random();
            billId = 10000 + random.nextInt(90000); // Generates a 5-digit random number
        }
    }
}

class CustomerId implements Runnable {
    private int customerId;

    public int getCustomerId() {
        return customerId;
    }

    @Override
    public void run() {
        synchronized (this) {
            Scanner scanner = new Scanner(System.in);
            while (true) {
                System.out.print("\nEnter the customer ID (8 digits): ");
                String input = scanner.nextLine();
                if (input.length() == 8 && input.matches("\\d+")) {
                    customerId = Integer.parseInt(input);
                    break;
                } else {
                    System.out.println("Invalid customer ID. Please enter an 8-digit number.");
                }
            }
        }
    }
}

class ProgramLoopInput implements Runnable {
    private boolean shouldLoop;

    public boolean shouldLoop() {
        return shouldLoop;
    }

    @Override
    public void run() {
        synchronized (this) {
            Scanner scanner = new Scanner(System.in);
            System.out.print("Do you want to loop the program? (Y/N): ");
            String input = scanner.nextLine();
            shouldLoop = input.equalsIgnoreCase("Y");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        do {
            CustomerId customerIdInput = new CustomerId();
            Thread customerIdThread = new Thread(customerIdInput);
            customerIdThread.start();

            try {
                customerIdThread.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            int customerId = customerIdInput.getCustomerId();

            System.out.print("Enter the type of utility (1 for water, 2 for electricity): ");
            Scanner scanner = new Scanner(System.in);
            int utilityType = scanner.nextInt();

            // Clear the scanner buffer
            scanner.nextLine();

            UtilityBillCalculator calculator;
            if (utilityType == 1) {
                System.out.print("Enter the consumption in gallons: ");
                int consumption = scanner.nextInt();
                calculator = new WaterBillCalculator(consumption);
            } else {
                System.out.print("Enter the consumption in kWh: ");
                int consumption = scanner.nextInt();
                calculator = new ElectricityBillCalculator(consumption);
            }

            Thread billThread = new Thread(calculator);
            billThread.start();

            try {
                billThread.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            double billAmount = calculator.getBillToPay();
            System.out.println("==============Utility Bill==============");
            System.out.println("Customer ID\t\t\t| " + customerId);
            System.out.println("Utility Type\t\t\t| " + (utilityType == 1 ? "Water" : "Electricity"));
            System.out.println("Consumption\t\t\t| " + calculator.consumption);
            System.out.println("Tax Rate\t\t\t| " + UtilityBillCalculator.TAX_RATE * 100 + "%");
            System.out.println("=======================================");
            System.out.printf("Bill to pay for current month\t| $%.2f%n", billAmount);
            System.out.println("---------------------------------------");

            CurrentDateTimeFetcher dateTimeFetcher = new CurrentDateTimeFetcher();
            Thread dateTimeThread = new Thread(dateTimeFetcher);
            dateTimeThread.start();

            try {
                dateTimeThread.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            LocalDateTime currentDateTime = dateTimeFetcher.getCurrentDateTime();
            DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
            String formattedDateTime = currentDateTime.format(formatter);
            System.out.println("Bill generated on\t\t| " + formattedDateTime);

            BillIdGenerator billIdGenerator = new BillIdGenerator();
            Thread billIdThread = new Thread(billIdGenerator);
            billIdThread.start();

            try {
                billIdThread.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            int billId = billIdGenerator.getBillId();
            System.out.println("Bill ID\t\t\t\t| " + billId);
            System.out.println("=======================================");

            ProgramLoopInput loopInput = new ProgramLoopInput();
            Thread loopThread = new Thread(loopInput);
            loopThread.start();

            try {
                loopThread.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            if (!loopInput.shouldLoop()) {
                break;
            }
        } while (true);

    }
}
