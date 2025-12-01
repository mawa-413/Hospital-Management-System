import java.util.*;
abstract class Person {
    private int id;
    private String name;
    private int age;

    public Person(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() { 
return id; 
}
    public String getName() {
 return name; 
}
    public int getAge() { 
return age; 
}

    public abstract void showInfo();
}

class Patient extends Person {
    private String disease;

    public Patient(int id, String name, int age, String disease) {
        super(id, name, age);
        this.disease = disease;
    }

    @Override
    public void showInfo() {
        System.out.println("Patient ID: " + getId());
        System.out.println("Name: " + getName());
        System.out.println("Age: " + getAge());
        System.out.println("Disease: " + disease);
    }
}



class Doctor extends Person {
    private String specialty;

    public Doctor(int id, String name, int age, String specialty) {
        super(id, name, age);
        this.specialty = specialty;
    }

    @Override
    public void showInfo() {
        System.out.println("Doctor ID: " + getId());
        System.out.println("Name: " + getName());
        System.out.println("Age: " + getAge());
        System.out.println("Specialty: " + specialty);
    }
}


class InventoryItem {
    private int itemId;
    private String name;
    private int quantity;
    private double price;

    public InventoryItem(int itemId, String name, int quantity, double price) {
        this.itemId = itemId;
        this.name = name;
        this.quantity = quantity;
        this.price = price;
    }

    public int getId() { 
return itemId; 
}
    public String getName() { 
return name; 
}
    public int getQuantity() { 
return quantity; 
}
    public double getPrice() { 
return price; 
}

    public void addStock(int q) { 
quantity += q; 
}

    public boolean reduceStock(int q) {
        if (q <= quantity) {
            quantity -= q;
            return true;
        }
        return false;
    }

    public void showItem() {
        System.out.println("Item ID: " + itemId + " | " + name + " | Qty: " + quantity + " | Price: " + price);
    }
}


class Appointment {
    private int apptId, patientId, doctorId;
    private String date;

    public Appointment(int apptId, int patientId, int doctorId, String date) {
        this.apptId = apptId;
        this.patientId = patientId;
        this.doctorId = doctorId;
        this.date = date;
    }

    public void show() {
        System.out.println("Appointment ID: " + apptId);
        System.out.println("Patient ID: " + patientId);
        System.out.println("Doctor ID: " + doctorId);
        System.out.println("Date: " + date);
    }
}







class Bill {
    private int billId;
    private int patientId;
    private double totalAmount = 0;
    private ArrayList<String> items = new ArrayList<>();

    public Bill(int billId, int patientId) {
        this.billId = billId;
        this.patientId = patientId;
    }

    public void addCharge(String name, double price) {
        items.add(name + " - " + price);
        totalAmount += price;
    }

    public void showBill() {
        System.out.println("Bill ID: " + billId + " | Patient ID: " + patientId);
        for (String s : items) System.out.println("  " + s);
        System.out.println("Total Amount: " + totalAmount);
    }
}


public class HospitalManagementSystem {

    static ArrayList<Patient> patients = new ArrayList<>();
    static ArrayList<Doctor> doctors = new ArrayList<>();
    static ArrayList<Appointment> appointments = new ArrayList<>();
    static ArrayList<InventoryItem> inventory = new ArrayList<>();
    static ArrayList<Bill> bills = new ArrayList<>();

    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {

        doctors.add(new Doctor(101, "Dr. Rahim", 45, "General"));
        doctors.add(new Doctor(102, "Dr. Sara", 38, "Child Specialist"));

        inventory.add(new InventoryItem(1, "Paracetamol", 50, 10));
        inventory.add(new InventoryItem(2, "Syringe", 100, 5));

        mainMenu();
    }

    static void mainMenu() {
        while (true) {
            System.out.println("\n=== Hospital Management System ===");
            System.out.println("1. Add Patient");
            System.out.println("2. Show Patients");
            System.out.println("3. Show Doctors");
            System.out.println("4. Book Appointment");
            System.out.println("5. Inventory Menu");
            System.out.println("6. Billing Menu");
            System.out.println("7. Reports");
            System.out.println("8. Exit");
            System.out.print("Choose: ");

            int choice = sc.nextInt();
            sc.nextLine();

            switch (choice) {
                case 1 -> addPatient();
                case 2 -> showPatients();
                case 3 -> showDoctors();
                case 4 -> bookAppointment();
                case 5 -> inventoryMenu();
                case 6 -> billingMenu();
                case 7 -> reportMenu();
                case 8 -> { System.out.println("Bye!"); return; }
                default -> System.out.println("Invalid!");
            }
        }
    }

    static void addPatient() {
        System.out.print("ID: ");
        int id = sc.nextInt(); sc.nextLine();
        System.out.print("Name: ");
        String name = sc.nextLine();
        System.out.print("Age: ");
        int age = sc.nextInt(); sc.nextLine();
        System.out.print("Disease: ");
        String dis = sc.nextLine();

        patients.add(new Patient(id, name, age, dis));
        System.out.println("Patient Added!");
    }

    static void showPatients() {
        patients.forEach(p -> { p.showInfo(); System.out.println("------"); });
    }

    static void showDoctors() {
        doctors.forEach(d -> { d.showInfo(); System.out.println("------"); });
    }


    static void bookAppointment() {
        System.out.print("Appt ID: ");
        int apid = sc.nextInt();
        System.out.print("Patient ID: ");
        int pid = sc.nextInt();
        System.out.print("Doctor ID: ");
        int did = sc.nextInt();
        sc.nextLine();
        System.out.print("Date: ");
        String date = sc.nextLine();

        appointments.add(new Appointment(apid, pid, did, date));
        System.out.println("Appointment booked!");
    }

    static void inventoryMenu() {
        while (true) {
            System.out.println("\n=== Inventory ===");
            System.out.println("1. Show Items");
            System.out.println("2. Add Stock");
            System.out.println("3. Back");

            int c = sc.nextInt();
            if (c == 1) inventory.forEach(InventoryItem::showItem);
            else if (c == 2) addStock();
            else break;
        }
    }

    static void addStock() {
        System.out.print("Item ID: ");
        int id = sc.nextInt();
        System.out.print("Qty to add: ");
        int qty = sc.nextInt();

        for (InventoryItem item : inventory) {
            if (item.getId() == id) {
                item.addStock(qty);
                System.out.println("Stock updated!");
                return;
            }
        }
        System.out.println("Item not found!");
    }


    static void billingMenu() {
        System.out.print("Bill ID: ");
        int bid = sc.nextInt();
        System.out.print("Patient ID: ");
        int pid = sc.nextInt();

        Bill bill = new Bill(bid, pid);

        while (true) {
            System.out.println("\n1. Add medicine/charge");
            System.out.println("2. Finish & Show Bill");
            int c = sc.nextInt();

            if (c == 1) {
                System.out.println("Inventory:");
                inventory.forEach(InventoryItem::showItem);

                System.out.print("Item ID: ");
                int id = sc.nextInt();
                System.out.print("Quantity: ");
                int qty = sc.nextInt();

                for (InventoryItem item : inventory) {
                    if (item.getId() == id) {
                        if (item.reduceStock(qty)) {
                            bill.addCharge(item.getName(), qty * item.getPrice());
                        } else {
                            System.out.println("Not enough stock!");
                        }
                    }
                }
            } else {
                bill.showBill();
                bills.add(bill);
                break;
            }
        }
    }


    static void reportMenu() {
        System.out.println("\n=== Reports ===");
        System.out.println("Total Patients: " + patients.size());
        System.out.println("Total Doctors: " + doctors.size());
        System.out.println("Total Appointments: " + appointments.size());
        System.out.println("Total Bills Generated: " + bills.size());
    }
}


