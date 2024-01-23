 import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Scanner;
 
 /*.	
Abdallah Bouher - 2222025
1. displayWelcomeMessage
2.	getMachineType
3.	getMachineWeight
4.	getMachinePrice
5.	getWashType
6.	getAdditionalDetergent
7.	calculateDetergentCost
 
Saifullah Muhammad Hafizh - 2225505
8.	calculateExtraWeight
9.	calculateExtraWeightCharge
10.	getWashPrice
11.	getDurationOption
12.	calculateDurationCost
13.	calculateWashPrice

 Yusuf Mohammad Yunus - 2314467
14.	calculateLaundryCost
15.	convertToTokens
16.	displayCostBreakdown
17.	processTokenInput
18.	askForReceipt
19.	printReceiptToFile
20.	updateOptions
21.	main
 */

public class AutomaticLaundryMachine2 {
    
    static final double TOKEN_VALUE = 0.50;     // Constants for pricing and conversion
    static final double DURATION_RATE = 0.10;       // Price per minute for standard duration
    static final double ADDITIONAL_TIME_RATE = 0.15; // Price per minute for additional time
    static final double DETERGENT_PRICE = 0.02;     // Price for additional detergent per ml
    static final double[][] DURATION_COSTS = {  //array for duration cost
    {15, 1.0},
    {20, 2.0},
    {25, 3.0},
    {30, 4.0},
    {35, 5.0},
    {40, 6.0}}; 
    
    static final int[] MACHINE_TYPES = {1, 2, 3};
    static final double[][] MACHINE_INFO = { //array for Machines info, weight and price
    {7.0, 4.5},
    {10.0, 6.5},
    {14.0, 8.0}};
    
    // Main Method
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        displayWelcomeMessage();

        int machineType = getMachineType(scanner);
        double weight = getMachineWeight(machineType);
        double machinePrice = getMachinePrice(machineType);

        int washType = getWashType(scanner);
        double washPrice = getWashPrice(washType);

        double basePrice = machinePrice + washPrice;

        int durationOption = getDurationOption(scanner);
        double durationCost = calculateDurationCost(durationOption);

        int additionalDetergent = getAdditionalDetergent(scanner);
        double detergentCost = calculateDetergentCost(additionalDetergent);

        double washPriceBreakdown = calculateWashPrice(washType);
        double durationCostBreakdown = calculateDurationCost(durationOption);
        double detergentCostBreakdown = calculateDetergentCost(additionalDetergent);
        updateOptions(scanner);

        double totalCost = calculateLaundryCost(machinePrice, washPriceBreakdown, durationCostBreakdown, detergentCostBreakdown);
        int tokens = convertToTokens(totalCost);

        askForReceipt(scanner, machinePrice, washPriceBreakdown, durationCostBreakdown, detergentCostBreakdown, totalCost, tokens);

        displayCostBreakdown(machinePrice, washPriceBreakdown, durationCostBreakdown, detergentCostBreakdown, totalCost, tokens);

        // Ask the user for token input and process it
        processTokenInput(scanner, tokens);

        displayCostBreakdown(machinePrice, washPriceBreakdown, durationCostBreakdown, detergentCostBreakdown, totalCost, tokens);
    }


    // Display welcome message
    static void displayWelcomeMessage() {
        System.out.println("╔------------------------------------------╗");
        System.out.println("| Welcome to the Automatic Laundry Machine |");
        System.out.println("╚------------------------------------------╝\n");
        System.out.println("{Please note that all the INPUTS should \n  be in INTEGERS BETWEEN 0 & 1000}\n");
        System.out.println("╔-------------------╗");
        System.out.println("| 1 TOKEN = 0.50RM  |");
        System.out.println("╚-------------------╝\n");
    }
    
     // Method to check if input is an integer
    static boolean isInteger(String input) {
        try {
            Integer.parseInt(input);
            return true;
        } catch (NumberFormatException e) {
            return false;
        }
    }

    // Get machine type from user input
    // Get machine type from user input with integer validation
    static int getMachineType(Scanner scanner) {
        int typeChoice;
        do {
            System.out.println("╔--------------------------------------------------------╗");
            System.out.println("| (1) 7 kg(4.5RM)  | (2) 10 kg(6.5RM) | (3) 14 kg(8RM)   |");
            System.out.println("╚--------------------------------------------------------╝");
            System.out.print(" Enter The Machine Type (OPTION 1-3): ");
            String input = scanner.next();
            if (isInteger(input)) {
                typeChoice = Integer.parseInt(input);
                for (int i = 0; i < MACHINE_TYPES.length; i++) {
                    if (typeChoice == MACHINE_TYPES[i]) {
                        return typeChoice;
                    }
                }
                System.out.println("Invalid machine type. Please enter a valid option.");
            } else {
                System.out.println("Invalid input. Please enter a valid integer.");
            }
        } while (true);
    }


    // Get machine weight based on type
    static double getMachineWeight(int machineType) {
    for (int i = 0; i < MACHINE_TYPES.length; i++) {
        if (machineType == MACHINE_TYPES[i]) {
            return MACHINE_INFO[i][0];
        }
    }

    // Default to weight of option 1 if the input is invalid
    return MACHINE_INFO[0][0];
}

    // Get machine price based on type
    static double getMachinePrice(int machineType) {
    for (int i = 0; i < MACHINE_TYPES.length; i++) {
        if (machineType == MACHINE_TYPES[i]) {
            return MACHINE_INFO[i][1];
        }
    }

    // Default to price of option 1 if the input is invalid
    return MACHINE_INFO[0][1];
}

    // Get wash type from user input
    static int getWashType(Scanner scanner) {
    int washChoice;
    do {
        System.out.println(" ");
        System.out.println("╔---------------------------------------------------╗");
        System.out.println("| (1) Cold(Free)  | (2) Warm(1RM)  | (3) HOT(2RM)   |");
        System.out.println("╚---------------------------------------------------╝");
        System.out.print(" Enter The Wash Type (OPTION 1-3): ");
        String input = scanner.next();
        if (isInteger(input)) {
            washChoice = Integer.parseInt(input);
            if (washChoice >= 1 && washChoice <= 3) {
                return washChoice;
            } else {
                System.out.println("Invalid wash type. Please enter a valid option.");
            }
        } else {
            System.out.println("Invalid input. Please enter a valid integer.");
        }
    } while (true);
}

    // Get wash price based on type
    static double getWashPrice(int washType) {
        switch (washType) {
            case 1: return 0.0;
            case 2: return 1.0;
            case 3: return 2.0;
            default: return 0.0; // Default to 0 (Cold Wash cost no extra charge)
        }
    }

    // Get duration option from user input
    static int getDurationOption(Scanner scanner) {
        int durationChoice;
        do {
            System.out.println(" ");
            System.out.println("╔---------------------------------------╗");
            System.out.println("| (1) 15 mins(1RM)  | (2) 20 mins(2RM)  |");
            System.out.println("| (3) 25 mins(3RM)  | (4) 30 mins(4RM)  |");
            System.out.println("| (5) 35 mins(5RM)  | (6) 40 mins(6RM)  |");
            System.out.println("╚---------------------------------------╝");
            System.out.print(" Enter The Duration (OPTION 1-6): ");
            String input = scanner.next();
            if (isInteger(input)) {
                durationChoice = Integer.parseInt(input);
                if (durationChoice >= 1 && durationChoice <= 6) {
                    return durationChoice;
                } else {
                    System.out.println("Invalid duration option. Please enter a valid option.");
                }
            } else {
                System.out.println("Invalid input. Please enter a valid integer.");
            }
        } while (true);
    }
    
    // Calculate the cost of the selected duration
    static double calculateDurationCost(int durationOption) {
    double durationCost = DURATION_COSTS[durationOption - 1][1]; // Assuming durationOption is 1-indexed
    int durationInMinutes = (int) DURATION_COSTS[durationOption - 1][0];
    
    if (durationInMinutes > 30) {
        durationCost += (durationInMinutes - 30) * ADDITIONAL_TIME_RATE;
        System.out.println("Additional Time: " + (durationInMinutes - 30));
    }
    
    return durationCost;
}

    // Get additional detergent amount from user input
    static int getAdditionalDetergent(Scanner scanner) {
        int additionalDetergent;
        do {
            System.out.println(" ");
            System.out.println("╔-------------------------------╗");
            System.out.println("|  Example 100, 150, 200, 250   |");
            System.out.println("╚-------------------------------╝");
            System.out.println(" Every additional 50ml = +1RM: ");
            System.out.print(" Enter Additional Detergent (in ml): ");
            String input = scanner.next();
            if (isInteger(input)) {
                additionalDetergent = Integer.parseInt(input);
                return additionalDetergent;
            } else {
                System.out.println("Invalid input. Please enter a valid integer.");
            }
        } while (true);
    }

    // Calculate the cost of additional detergent
    static double calculateDetergentCost(int additionalDetergent) {
        return additionalDetergent * DETERGENT_PRICE;
    }
 
    // Calculate the breakdown of wash price
    static double calculateWashPrice(int washType) {
        return getWashPrice(washType);}
    
    // Calculate the breakdown of laundry cost
    static double calculateLaundryCost(double machinePrice, double washPrice, double durationCost, double detergentCost) {
    return machinePrice + washPrice + durationCost + detergentCost;
}

    // Function to convert cost to tokens
    static int convertToTokens(double cost) {
    return (int) (cost / TOKEN_VALUE);
}

    // Display the cost breakdown
    static void displayCostBreakdown(double machinePrice, double washPrice, double durationCost, double detergentCost, double totalCost, int tokens) {
    System.out.println(" ");
    System.out.printf("Total cost: RM %.2f\n", totalCost);
    System.out.println("Tokens needed: " + tokens + " tokens");
}
    // Convert the Rm into Token and print it out
static void processTokenInput(Scanner scanner, int requiredTokens) {
    int inputTokens;
    do {
        System.out.print("Enter the number of tokens: ");
        String input = scanner.next();
        if (isInteger(input)) {
            inputTokens = Integer.parseInt(input);

            if (inputTokens == requiredTokens) {
                System.out.println("Thank You for using our laundry service!");
                return;
            } else if (inputTokens < requiredTokens) {
                requiredTokens -= inputTokens;
                System.out.println("Remaining tokens needed: " + requiredTokens);
            } else {
                int extraTokens = inputTokens - requiredTokens;
                System.out.println("Thank You for using our laundry service!");
                if (extraTokens > 1) {
                    System.out.println("Please Take Your Remaining Tokens: " + extraTokens);
                } else if (extraTokens == 1) {
                    System.out.println("Please Take Your Remaining Token: " + extraTokens);
                }
                break;
            }
        } else {
            System.out.println("Invalid input. Please enter a valid integer.");
            inputTokens = 0; // Set inputTokens to an invalid value to continue the loop
        }
    } while (requiredTokens > 0);
    System.out.println("");
}

// Method to ask the user if they want to print the receipt
static void askForReceipt(Scanner scanner, double machinePrice, double washPrice, double durationCost, double detergentCost, double totalCost, int tokens) {
    int choice = 0;
    String input;
    do {
        System.out.println("Do you want to print the receipt? (1 for YES, 2 for NO): ");
        input = scanner.next();
        if (isInteger(input)) {
            choice = Integer.parseInt(input);
            if (choice == 1) {
                try {
                    printReceiptToFile(machinePrice, washPrice, durationCost, detergentCost, totalCost, tokens);
                    System.out.println("Receipt has been printed to a file named: 'LaundryReceipt.txt' .");
                } catch (IOException e) {
                    System.out.println("Error printing receipt. Please try again.");
                }
            } else if (choice != 2) {
                System.out.println("Invalid choice. No receipt will be printed.");
            }
        } else {
            System.out.println("Invalid input. Please enter a valid integer.");
        }
    } while (!isInteger(input) || ((int) choice != 1 && choice != 2));
}

    // Method to print the receipt to a .txt file
    static void printReceiptToFile(double machinePrice, double washPrice, double durationCost, double detergentCost, double totalCost, int tokens) throws IOException {
        String fileName = "LaundryReceipt.txt";
        try (PrintWriter writer = new PrintWriter(new FileWriter(fileName))) {
            writer.printf("--------------------------------\n");
            writer.printf("Total cost breakdown:\n");
            writer.printf("Machine price: RM %.2f\n", machinePrice);
            writer.printf("Wash price: RM %.2f\n", washPrice);
            writer.printf("Duration cost: RM %.2f\n", durationCost);
            writer.printf("Detergent cost: RM %.2f\n", detergentCost);
            writer.printf("--------------------------------\n");
            writer.println(" ");
            writer.printf("Total cost: RM %.2f\n", totalCost);
            writer.println("Total Tokens " + tokens );
            writer.println("╔----------------------------------╗");
            writer.println("| THANK YOU FOR USING OUR SERVICE  |");
            writer.println("╚----------------------------------╝\n");
            
        }
}
    //Method to Update options
    static int updateOptions(Scanner scanner) {
    int choice;
    do {
        System.out.println("\nDo you want to update any option?");
        System.out.println("1. Machine Type");
        System.out.println("2. Wash Type");
        System.out.println("3. Duration");
        System.out.println("4. Additional Detergent");
        System.out.println("5. Finish Editing");
        System.out.print("Enter your choice (1-5): ");
        
        String input = scanner.next();
        if (isInteger(input)) {
            choice = Integer.parseInt(input);
            switch (choice) {
                case 1:
                    int newMachineType = getMachineType(scanner);
                    double newMachineWeight = getMachineWeight(newMachineType);
                    double newMachinePrice = getMachinePrice(newMachineType);
                    System.out.println("Machine Type updated to " + newMachineType + " - " + newMachineWeight + "kg");
                    break;
                case 2:
                    int newWashType = getWashType(scanner);
                    double newWashPrice = getWashPrice(newWashType);
                    System.out.println("Wash Type updated to " + newWashType);
                    break;
                case 3:
                    int newDurationOption = getDurationOption(scanner);
                    double newDurationCost = calculateDurationCost(newDurationOption);
                    System.out.println("Duration updated to Option " + newDurationOption);
                    break;
                case 4:
                    int newAdditionalDetergent = getAdditionalDetergent(scanner);
                    double newDetergentCost = calculateDetergentCost(newAdditionalDetergent);
                    System.out.println("Additional Detergent updated to " + newAdditionalDetergent + "ml");
                    break;
                case 5:
                    System.out.println("Finish Editing. Returning to main menu.");
                    break;
                default:
                    System.out.println("Invalid choice. Please enter a number between 1 and 5.");
            }
        } else {
            System.out.println("Invalid input. Please enter a valid integer.");
            choice = 0; // Set choice to an invalid value to continue the loop
        }
    } while (choice != 5);
    return choice;
    }
}
