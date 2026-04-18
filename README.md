# Lost-item-logger-system-for-college-campus-
The Lost Item Logger System is a digital platform designed to manage lost and found items on a college campus. Users can report lost or found items by providing details such as description, location, and date.

class Item:
    def __init__(self, item_id, name, location, date, desc):
        self.item_id = item_id
        self.name = name.lower()
        self.location = location
        self.date = date
        self.desc = desc

    def display(self, typ):
        print(f"\n[{typ}] ID:{self.item_id}")
        print("Name:", self.name)
        print("Location:", self.location)
        print("Date:", self.date)
        print("Description:", self.desc)


class Logger:
    def __init__(self):
        self.lost = []
        self.found = []

    # check duplicate ID
    def id_exists(self, item_id):
        for i in self.lost + self.found:
            if i.item_id == item_id:
                return True
        return False

    # ADD LOGIC
    def add(self, typ):
        item_id = input("Enter ID: ")

        if self.id_exists(item_id):
            print("ID already exists!")
            return

        name = input("Enter name: ")
        location = input("Enter location: ")
        date = input("Enter date: ")
        desc = input("Enter description: ")

        item = Item(item_id, name, location, date, desc)

        if typ == "Lost":
            self.lost.append(item)
            self.match(item, self.found, "Found")
        else:
            self.found.append(item)
            self.match(item, self.lost, "Lost")

        print("Item added successfully")

    # MATCH LOGIC
    def match(self, new_item, other_list, label):
        for item in other_list:
            if new_item.name in item.name or item.name in new_item.name:
                print("\nPossible Match Found!")
                item.display(label)
                new_item.display("Current")
                return

    # VIEW LOGIC
    def view(self, typ):
        items = self.lost if typ == "Lost" else self.found

        if not items:
            print("No items available")
            return

        for i in items:
            i.display(typ)

    # SEARCH LOGIC
    def search(self, typ):
        key = input("Enter name to search: ").lower()
        items = self.lost if typ == "Lost" else self.found

        found = False
        for i in items:
            if key in i.name:
                i.display(typ)
                found = True

        if not found:
            print("No matching item found")

    # DELETE LOGIC
    def delete(self, typ):
        item_id = input("Enter ID to delete: ")
        items = self.lost if typ == "Lost" else self.found

        for i in items:
            if i.item_id == item_id:
                items.remove(i)
                print("Item deleted successfully")
                return

        print("Item ID not found")


def main():
    log = Logger()

    while True:
        print("\n===== LOST & FOUND LOGGER =====")
        print("1. Add Lost Item")
        print("2. Add Found Item")
        print("3. View Lost Items")
        print("4. View Found Items")
        print("5. Search Lost")
        print("6. Search Found")
        print("7. Delete Lost")
        print("8. Delete Found")
        print("9. Exit")

        choice = input("Enter choice: ")

        if choice == '1':
            log.add("Lost")

        elif choice == '2':
            log.add("Found")

        elif choice == '3':
            log.view("Lost")

        elif choice == '4':
            log.view("Found")

        elif choice == '5':
            log.search("Lost")

        elif choice == '6':
            log.search("Found")

        elif choice == '7':
            log.delete("Lost")

        elif choice == '8':
            log.delete("Found")

        elif choice == '9':
            print("Exiting...")
            break

        else:
            print("Invalid choice")


main()
