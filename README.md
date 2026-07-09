#include <iostream>
#include <fstream>
#include <string>
#include <vector>

using namespace std;

class BankAccount {
private:
    int accountNumber;
    string accountHolder;
    double balance;

public:
    BankAccount(int accNum = 0, string name = "", double bal = 0.0) 
        : accountNumber(accNum), accountHolder(name), balance(bal) {}

    // Getters and Setters
    int getAccountNumber() { return accountNumber; }
    void deposit(double amount) { balance += amount; }
    bool withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }
    double getBalance() { return balance; }

    // File I/O
    void saveToFile() {
        ofstream outFile("accounts.txt", ios::app);
        outFile << accountNumber << " " << accountHolder << " " << balance << endl;
        outFile.close();
    }
};

// Helper functions for file management
void updateFile(int targetAcc, double newBalance) {
    ifstream inFile("accounts.txt");
    ofstream tempFile("temp.txt");
    int acc; string name; double bal;
    while (inFile >> acc >> name >> bal) {
        if (acc == targetAcc) bal = newBalance;
        tempFile << acc << " " << name << " " << bal << endl;
    }
    inFile.close(); tempFile.close();
    remove("accounts.txt");
    rename("temp.txt", "accounts.txt");
}

int main() {
    int choice, accNum;
    string name;
    double amount;

    while (true) {
        cout << "\n--- Bank Management System ---\n";
        cout << "1. Create Account\n2. Deposit\n3. Withdraw\n4. Check Balance\n5. Exit\n";
        cout << "Selection: "; cin >> choice;

        if (choice == 5) break;

        switch (choice) {
            case 1:
                cout << "Acc Num, Name, Initial Deposit: ";
                cin >> accNum >> name >> amount;
                BankAccount(accNum, name, amount).saveToFile();
                break;
            case 2:
                cout << "Acc Num and Amount: "; cin >> accNum >> amount;
                // Logic: Search file, calculate, then updateFile()
                cout << "Deposit processed.\n";
                break;
            case 3:
                cout << "Acc Num and Amount: "; cin >> accNum >> amount;
                // Logic: Search file, check balance, then updateFile()
                cout << "Withdrawal processed.\n";
                break;
            case 4:
                cout << "Enter Acc Num: "; cin >> accNum;
                cout << "Balance is: $XXXX.XX\n";
                break;
        }
    }
    return 0;
}
