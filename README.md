#include <iostream>
#include <string>
#include <vector>
#include <fstream>
#include <algorithm>

using namespace std;

// Account class
class Account {
public:
    int accountNumber;
    string accountHolderName;
    double balance;

    Account(int accountNumber, string accountHolderName, double balance) {
        this->accountNumber = accountNumber;
        this->accountHolderName = accountHolderName;
        this->balance = balance;
    }
};

// ATM class
class ATM {
private:
    vector<Account> accounts;

public:
    // Load accounts from a file
    void loadAccounts(const string& filename) {
        ifstream file(filename);
        if (file.is_open()) {
            int accountNumber;
            string accountHolderName;
            double balance;
            while (file >> accountNumber >> accountHolderName >> balance) {
                accounts.push_back(Account(accountNumber, accountHolderName, balance));
            }
            file.close();
        } else {
            cout << "Error opening account file!" << endl;
        }
    }

    // Save accounts to a file
    void saveAccounts(const string& filename) {
        ofstream file(filename);
        if (file.is_open()) {
            for (Account& account : accounts) {
                file << account.accountNumber << " " << account.accountHolderName << " " << account.balance << endl;
            }
            file.close();
        } else {
            cout << "Error saving account file!" << endl;
        }
    }

    // Get account by account number
    Account* getAccount(int accountNumber) {
        for (Account& account : accounts) {
            if (account.accountNumber == accountNumber) {
                return &account;
            }
        }
        return nullptr;
    }

    // Display account balance
    void displayBalance(Account& account) {
        cout << "Account Holder: " << account.accountHolderName << endl;
        cout << "Account Number: " << account.accountNumber << endl;
        cout << "Balance: $" << account.balance << endl;
    }

    // Withdraw cash
    void withdrawCash(Account& account, double amount) {
        if (amount > 0 && amount <= account.balance) {
            account.balance -= amount;
            cout << "Withdrawal successful!" << endl;
            cout << "New balance: $" << account.balance << endl;
        } else {
            cout << "Insufficient funds or invalid withdrawal amount." << endl;
        }
    }

    // Deposit cash





    void depositCash(Account& account, double amount) {
        if (amount > 0) {
            account.balance += amount;
            cout << "Deposit successful!" << endl;
            cout << "New balance: $" << account.balance << endl;
        } else {
            cout << "Invalid deposit amount." << endl;
        }
    }

    // ATM operations menu
    void startATM() {
        int accountNumber;
        cout << "Welcome to the ATM!" << endl;
        cout << "Enter your account number: ";
        cin >> accountNumber;

        Account* account = getAccount(accountNumber);
        if (account != nullptr) {
            int choice;
            while (true) {
                cout << "\nATM Menu:" << endl;
                cout << "1. Display Balance" << endl;
                cout << "2. Withdraw Cash" << endl;
                cout << "3. Deposit Cash" << endl;
                cout << "4. Exit" << endl;
                cout << "Enter your choice: ";
                cin >> choice;

                switch (choice) {
                    case 1:
                        displayBalance(*account);
                        break;
                    case 2: {
                        double amount;
                        cout << "Enter withdrawal amount: $";
                        cin >> amount;
                        withdrawCash(*account, amount);
                        break;
                    }
                    case 3: {
                        double amount;
                        cout << "Enter deposit amount: $";
                        cin >> amount;
                        depositCash(*account, amount);
                        break;
                    }
                    case 4:
                        cout << "Thank you for using the ATM!" << endl;
                        saveAccounts("accounts.txt");
                        return;
                    default:
                        cout << "Invalid choice." << endl;
                }
            }
        } else {
            cout << "Account not found." << endl;
        }
    }
};

int main() {
    ATM atm;
    atm.loadAccounts("accounts.txt"); // Load accounts from a file
    atm.startATM();
    return 0;
}
