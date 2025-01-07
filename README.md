# res

#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Structure for a menu item
struct MenuItem {
    string name;
    double price;
};

// Structure for an order
struct Order {
    string customerName;
    vector<MenuItem> orderedItems;
    double totalAmount = 0;
};

// Class to handle restaurant operations
class RestaurantManagementSystem {
private:
    vector<MenuItem> menu;
    vector<Order> orders;

public:
    // Function to add menu item
    void addMenuItem(const string& name, double price) {
        MenuItem item;
        item.name = name;
        item.price = price;
        menu.push_back(item);
    }

    // Function to list all menu items
    void displayMenu() {
        cout << "\n---- Menu ----\n";
        for (int i = 0; i < menu.size(); i++) {
            cout << i + 1 << ". " << menu[i].name << " - $" << menu[i].price << endl;
        }
    }

    // Function to take order from the customer
    void takeOrder() {
        string customerName;
        vector<MenuItem> orderItems;
        double totalAmount = 0;

        cout << "\nEnter customer name: ";
        cin.ignore();
        getline(cin, customerName);

        displayMenu();

        int choice;
        cout << "\nEnter item numbers to order (enter 0 to finish):\n";
        while (true) {
            cin >> choice;
            if (choice == 0) break;
            if (choice > 0 && choice <= menu.size()) {
                orderItems.push_back(menu[choice - 1]);
                totalAmount += menu[choice - 1].price;
            } else {
                cout << "Invalid choice. Try again.\n";
            }
        }

        Order newOrder;
        newOrder.customerName = customerName;
        newOrder.orderedItems = orderItems;
        newOrder.totalAmount = totalAmount;

        orders.push_back(newOrder);
        cout << "\nOrder placed successfully!\n";
    }

    // Function to display all orders
    void displayOrders() {
        cout << "\n---- All Orders ----\n";
        for (int i = 0; i < orders.size(); i++) {
            cout << "\nOrder #" << i + 1 << " (Customer: " << orders[i].customerName << ")\n";
            for (auto& item : orders[i].orderedItems) {
                cout << item.name << " - $" << item.price << endl;
            }
            cout << "Total Bill: $" << orders[i].totalAmount << endl;
        }
    }

    // Function to generate a bill for a specific order
    void generateBill() {
        int orderId;
        cout << "\nEnter Order ID to generate the bill: ";
        cin >> orderId;

        if (orderId > 0 && orderId <= orders.size()) {
            Order order = orders[orderId - 1];
            cout << "\n---- Bill for Order #" << orderId << " ----\n";
            cout << "Customer: " << order.customerName << endl;
            for (auto& item : order.orderedItems) {
                cout << item.name << " - $" << item.price << endl;
            }
            cout << "Total: $" << order.totalAmount << endl;
        } else {
            cout << "Invalid Order ID.\n";
        }
    }

    // Function to remove a menu item (by index)
    void removeMenuItem() {
        int choice;
        cout << "\nEnter menu item number to remove: ";
        cin >> choice;

        if (choice > 0 && choice <= menu.size()) {
            menu.erase(menu.begin() + choice - 1);
            cout << "Menu item removed successfully.\n";
        } else {
            cout << "Invalid item number.\n";
        }
    }
};

int main() {
    RestaurantManagementSystem rms;

    int option;

    do {
        cout << "\n---- Restaurant Management System ----\n";
        cout << "1. Add Menu Item\n";
        cout << "2. Display Menu\n";
        cout << "3. Take Order\n";
        cout << "4. Display Orders\n";
        cout << "5. Generate Bill\n";
        cout << "6. Remove Menu Item\n";
        cout << "7. Exit\n";
        cout << "Enter option: ";
        cin >> option;

        switch (option) {
        case 1: {
            string name;
            double price;
            cout << "Enter menu item name: ";
            cin.ignore();
            getline(cin, name);
            cout << "Enter menu item price: ";
            cin >> price;
            rms.addMenuItem(name, price);
            break;
        }
        case 2:
            rms.displayMenu();
            break;
        case 3:
            rms.takeOrder();
            break;
        case 4:
            rms.displayOrders();
            break;
        case 5:
            rms.generateBill();
            break;
        case 6:
            rms.removeMenuItem();
            break;
        case 7:
            cout << "Exiting the system...\n";
            break;
        default:
            cout << "Invalid option, try again.\n";
        }

    } while (option != 7);

    return 0;
}
