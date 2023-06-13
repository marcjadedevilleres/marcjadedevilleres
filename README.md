
#include <stdio.h>

#include <stdlib.h>

#include <string.h>


// Define constant values

#define MAX_PIN_ATTEMPTS 3

#define MAX_TRANSACTION_AMOUNT 5000

#define MAX_DEPOSIT_AMOUNT 100000

#define MIN_BALANCE 0

#define MAX_BALANCE 1000000


// Define global variables

int current_balance = 10000;

int pin_attempts = 0;


// Define function prototypes

int login();

void withdraw();

void deposit();

void view_balance();

void print_receipt();

void debug();

void log_transaction(char *message);

int generate_test_data();

void handle_error(char *message);


int main() {

    // Display welcome message

    printf("Welcome to XYZ Bank ATM\n");


    // Loop until user quits

    while (1) {

        // Prompt for action

        printf("\nWhat would you like to do?\n");

        printf("1. Withdraw\n");

        printf("2. Deposit\n");

        printf("3. View Balance\n");

        printf("4. Print Receipt\n");

        printf("5. Debug\n");

        printf("6. Generate Test Data\n");

        printf("7. Quit\n");

        printf("Choose an option: ");


        // Read user's choice

        int choice;

        scanf("%d", &choice);


        // Perform action based on user's choice

        switch (choice) {

            case 1:

                withdraw();

                break;

            case 2:

                deposit();

                break;

            case 3:

                view_balance();

                break;

            case 4:

                print_receipt();

                break;

            case 5:

                debug();

                break;

            case 6:

                generate_test_data();

                break;

            case 7:

                printf("Thank you for using XYZ Bank ATM\n");

                exit(0);

            default:

                handle_error("Invalid option chosen");

                break;

        }

    }


    return 0;

}


int login() {

    // Prompt for PIN

    printf("Enter PIN: ");

    char pin[5];

    scanf("%s", pin);


    // Validate PIN

    if (strcmp(pin, "1234") == 0) {

        return 1;

    } else {

        pin_attempts++;


        if (pin_attempts >= MAX_PIN_ATTEMPTS) {

            handle_error("Maximum PIN attempts reached. Card blocked.");

            exit(1);

        } else {

            handle_error("Invalid PIN entered. Please try again.");

            return 0;

        }

    }

}


void withdraw() {

    // Validate login

    if (!login()) {

        return;

    }


    // Prompt for amount

    printf("Enter amount to withdraw: ");

    int amount;

    scanf("%d", &amount);


    // Validate amount

    if (amount > MAX_TRANSACTION_AMOUNT || amount > current_balance || amount < 0) {

        handle_error("Invalid amount entered. Please try again.");

        return;

    }


    // Update balance

    current_balance -= amount;

    printf("Transaction successful. Please collect your cash.\n");

    printf("Current balance: %d\n", current_balance);


    // Log transaction

    char message[100];

    sprintf(message, "Withdrawal of %d", amount);

    log_transaction(message);

}


void deposit() {

    // Validate login

    if (!login()) {

        return;

    }


    // Prompt for amount

    printf("Enter amount to deposit: ");

    int amount;

    scanf("%d", &amount);


    // Validate amount

    if (amount > MAX_DEPOSIT_AMOUNT || current_balance + amount > MAX_BALANCE || amount < 0) {

        handle_error("Invalid amount entered. Please try again.");

        return;

    }


    // Update balance

    current_balance += amount;

    printf("Transaction successful. Please insert your cash.\n");

    printf("Current balance: %d\n", current_balance);


    // Log transaction

    char message[100];

    sprintf(message, "Deposit of %d", amount);

    log_transaction(message);

}


void view_balance() {

    // Validate login

    if (!login()) {

        return;

    }


    // Display balance

    printf("Current balance: %d\n", current_balance);


    // Log transaction

    log_transaction("Balance inquiry");

}


void print_receipt() {

    // Validate login

    if (!login()) {

        return;

    }


    // Print receipt

    printf("*** XYZ Bank ATM Receipt ***\n");

    printf("Date: %s\n", __DATE__);

    printf("Time: %s\n", __TIME__);

    printf("Transaction type: Balance inquiry\n");

    printf("Current balance: %d\n", current_balance);


    // Log transaction

    log_transaction("Receipt printed");

}


void debug() {

    // Validate login

    if (!login()) {

        return;

    }


    // Display debugging information

    printf("*** DEBUG MODE ***\n");

    printf("Current balance: %d\n", current_balance);

    printf("PIN attempts: %d\n", pin_attempts);


    // Log transaction

    log_transaction("Debug mode accessed");

}


void log_transaction(char *message) {

    // Open log file in append mode

    FILE *log_file = fopen("atm.log", "a");


    // Write message to log file

    fprintf(log_file, "%s\n", message);


    // Close log file

    fclose(log_file);

}


int generate_test_data() {

    // Create a file with test data for ATM simulation

    FILE *test_data_file = fopen("atm.testdata", "w");


    fputs("1234\n", test_data_file);

    fputs("2\n", test_data_file);

    fputs("300\n", test_data_file);

    fputs("1\n", test_data_file);

    fputs("5\n", test_data_file);


    fclose(test_data_file);

    return 0;

}


void handle_error(char *message) {

    printf("Error: %s\n", message);

    log_transaction(message);

}



