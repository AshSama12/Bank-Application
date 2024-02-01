# Bank-Application
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

class NegativeAmountException extends Exception {
    public NegativeAmountException(String message) {
        super(message);
    }
}

class Accounts {
    private double balance;
    private int accountNumber;

    public Accounts(double initialBalance, int accNumber) {
        balance = initialBalance;
        accountNumber = accNumber;
    }

    public int getAccountNumber() {
        return accountNumber;
    }

    public double getBalance() {
        return balance;
    }

    public void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException("Insufficient balance");
        }
        balance -= amount;
    }

    public void credit(double amount) throws NegativeAmountException {
        if (amount < 0) {
            throw new NegativeAmountException("Negative amount");
        }
        balance += amount;
    }
}

public class BankApplication {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);
        List<Accounts> customerAccounts = new ArrayList<>();

        try {
            System.out.print("Enter the number of accounts for the customer: ");
            int numAccounts = scanner.nextInt();
            for (int i = 0; i < numAccounts; i++) {
                System.out.print("Enter account number: ");
                int accNumber = scanner.nextInt();
                Accounts account = new Accounts(0.0, accNumber); // Assuming initial balance is 0.0
                customerAccounts.add(account);
            }

            System.out.println("-----BANK AMS------");

            while (true) {
                System.out.print("Enter your account number: ");
                int accountNumber = scanner.nextInt();
                scanner.nextLine(); // Consume the newline left by nextInt()

                Accounts selectedAccount = null;
                for (Accounts account : customerAccounts) {
                    if (account.getAccountNumber() == accountNumber) {
                        selectedAccount = account;
                        break;
                    }
                }

                if (selectedAccount == null) {
                    System.out.println("!!Invalid account number!!");
                    System.out.print("Do you want to continue (N/Y): ");
                    String continueChoice = scanner.nextLine();
                    if (continueChoice.equalsIgnoreCase("N")) {
                        break;
                    }
                    continue;
                }

                System.out.print("Do you want to withdraw or credit?(w/c): ");
                String choice = scanner.nextLine();

                if (choice.equalsIgnoreCase("w")) {
                    System.out.print("Amount: ");
                    double amount = scanner.nextDouble();
                    scanner.nextLine(); // Consume the newline left by nextDouble()

                    try {
                        selectedAccount.withdraw(amount);
                        System.out.println("Current Balance: " + selectedAccount.getBalance());
                    } catch (InsufficientBalanceException e) {
                        System.out.println("!!Insufficient balance!!");
                    }
                } else if (choice.equalsIgnoreCase("c")) {
                    System.out.print("Amount: ");
                    double amount = scanner.nextDouble();
                    scanner.nextLine(); // Consume the newline left by nextDouble()

                    try {
                        selectedAccount.credit(amount);
                        System.out.println("Current balance of your account: " + selectedAccount.getBalance());
                    } catch (NegativeAmountException e) {
                        System.out.println("!!Negative amount!!");
                    }
                }

                System.out.print("Do you want to continue (N/Y): ");
                String continueChoice = scanner.nextLine();
                if (continueChoice.equalsIgnoreCase("N")) {
                    break;
                }
            }

            System.out.println("Thank you for banking with us....");
        } finally {
            scanner.close();
        }
    }
}

