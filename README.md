# Hospital-Management-System
import java.util.*;
import java.time.*;
import java.text.SimpleDateFormat;

// Main class to run the system
public class HospitalManagementSystem {
    public static void main(String[] args) {
        Hospital hospital = new Hospital("City General Hospital");
        hospital.initializeSampleData();
        hospital.run();
    }
}

// Main Hospital class
class Hospital {
    private String name;
    private List<Patient> patients;
    private List<Doctor> doctors;
    private List<Appointment> appointments;
    private List<MedicalRecord> medicalRecords;
    private List<Staff> staffMembers;
    private Inventory inventory;
    private BillingSystem billingSystem;
    private Scanner scanner;

    public Hospital(String name) {
        this.name = name;
        this.patients = new ArrayList<>();
        this.doctors = new ArrayList<>();
        this.appointments = new ArrayList<>();
        this.medicalRecords = new ArrayList<>();
        this.staffMembers = new ArrayList<>();
        this.inventory = new Inventory();
        this.billingSystem = new BillingSystem();
        this.scanner = new Scanner(System.in);
    }

    public void initializeSampleData() {
        // Add sample doctors
        doctors.add(new Doctor(1, "Dr. Smitha", "Dermitology", "123-456-7890", "smitha@hospital.com"));
        doctors.add(new Doctor(2, "Dr. Kiran", "Orthopaedic", "234-567-8901", "kiran@hospital.com"));
        
        // Add sample staff
        staffMembers.add(new Staff(1, "Nurse Ravi", "Nurse", "345-678-9012", 50000));
        staffMembers.add(new Staff(2, "Admin Anil", "Administrator", "456-789-0123", 45000));
        
        // Add sample patients
        patients.add(new Patient(1, "Gandhi", "1990-05-15", "M", "789-012-3456", "gandhi@email.com"));
        patients.add(new Patient(2, "Tata", "1985-10-22", "F", "890-123-4567", "tata@email.com"));
        
        // Add sample inventory
        inventory.addItem(new MedicalSupply(1, "Bandages", 100, 2.50, "2025-12-31"));
        inventory.addItem(new MedicalSupply(2, "Syringes", 50, 1.75, "2024-06-30"));
    }

    public void run() {
        System.out.println("Welcome to " + name + " Management System");
        
        while (true) {
            System.out.println("\nMain Menu:");
            System.out.println("1. Patient Registration");
            System.out.println("2. Appointment Scheduling");
            System.out.println("3. Electronic Health Records");
            System.out.println("4. Billing and Invoicing");
            System.out.println("5. Inventory Management");
            System.out.println("6. Staff Management");
            System.out.println("7. Exit");
            
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline
            
            switch (choice) {
                case 1:
                    patientRegistration();
                    break;
                case 2:
                    appointmentScheduling();
                    break;
                case 3:
                    electronicHealthRecords();
                    break;
                case 4:
                    billingAndInvoicing();
                    break;
                case 5:
                    inventoryManagement();
                    break;
                case 6:
                    staffManagement();
                    break;
                case 7:
                    System.out.println("Exiting system. Goodbye!");
                    return;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }

    // Patient Registration Module
    private void patientRegistration() {
        System.out.println("\nPatient Registration");
        
        System.out.print("Enter patient ID: ");
        int id = scanner.nextInt();
        scanner.nextLine();
        
        System.out.print("Enter full name: ");
        String name = scanner.nextLine();
        
        System.out.print("Enter date of birth (YYYY-MM-DD): ");
        String dob = scanner.nextLine();
        
        System.out.print("Enter gender (M/F/O): ");
        String gender = scanner.nextLine();
        
        System.out.print("Enter phone number: ");
        String phone = scanner.nextLine();
        
        System.out.print("Enter email: ");
        String email = scanner.nextLine();
        
        Patient patient = new Patient(id, name, dob, gender, phone, email);
        patients.add(patient);
        
        System.out.println("Patient registered successfully!");
    }

    // Appointment Scheduling Module
    private void appointmentScheduling() {
        System.out.println("\nAppointment Scheduling");
        
        System.out.print("Enter patient ID: ");
        int patientId = scanner.nextInt();
        scanner.nextLine();
        
        Patient patient = findPatientById(patientId);
        if (patient == null) {
            System.out.println("Patient not found.");
            return;
        }
        
        System.out.println("Available Doctors:");
        for (Doctor doctor : doctors) {
            System.out.println(doctor.getId() + ": " + doctor.getName() + " (" + doctor.getSpecialization() + ")");
        }
        
        System.out.print("Enter doctor ID: ");
        int doctorId = scanner.nextInt();
        scanner.nextLine();
        
        Doctor doctor = findDoctorById(doctorId);
        if (doctor == null) {
            System.out.println("Doctor not found.");
            return;
        }
        
        System.out.print("Enter appointment date (YYYY-MM-DD): ");
        String dateStr = scanner.nextLine();
        
        System.out.print("Enter appointment time (HH:MM): ");
        String timeStr = scanner.nextLine();
        
        System.out.print("Enter reason for visit: ");
        String reason = scanner.nextLine();
        
        String appointmentDateTime = dateStr + " " + timeStr;
        Appointment appointment = new Appointment(appointments.size() + 1, patientId, doctorId, appointmentDateTime, reason);
        appointments.add(appointment);
        
        System.out.println("Appointment scheduled successfully!");
        System.out.println("Appointment ID: " + appointment.getId());
    }

    // Electronic Health Records Module
    private void electronicHealthRecords() {
        System.out.println("\nElectronic Health Records");
        System.out.println("1. View Patient Records");
        System.out.println("2. Add Medical Record");
        System.out.print("Enter your choice: ");
        
        int choice = scanner.nextInt();
        scanner.nextLine();
        
        if (choice == 1) {
            System.out.print("Enter patient ID: ");
            int patientId = scanner.nextInt();
            scanner.nextLine();
            
            List<MedicalRecord> patientRecords = getMedicalRecordsForPatient(patientId);
            if (patientRecords.isEmpty()) {
                System.out.println("No records found for this patient.");
            } else {
                System.out.println("\nMedical Records for Patient ID " + patientId + ":");
                for (MedicalRecord record : patientRecords) {
                    System.out.println(record);
                }
            }
        } else if (choice == 2) {
            System.out.print("Enter patient ID: ");
            int patientId = scanner.nextInt();
            scanner.nextLine();
            
            Patient patient = findPatientById(patientId);
            if (patient == null) {
                System.out.println("Patient not found.");
                return;
            }
            
            System.out.print("Enter doctor ID: ");
            int doctorId = scanner.nextInt();
            scanner.nextLine();
            
            Doctor doctor = findDoctorById(doctorId);
            if (doctor == null) {
                System.out.println("Doctor not found.");
                return;
            }
            
            System.out.print("Enter diagnosis: ");
            String diagnosis = scanner.nextLine();
            
            System.out.print("Enter treatment: ");
            String treatment = scanner.nextLine();
            
            System.out.print("Enter prescription: ");
            String prescription = scanner.nextLine();
            
            System.out.print("Enter notes: ");
            String notes = scanner.nextLine();
            
            MedicalRecord record = new MedicalRecord(
                medicalRecords.size() + 1,
                patientId,
                doctorId,
                LocalDate.now().toString(),
                diagnosis,
                treatment,
                prescription,
                notes
            );
            
            medicalRecords.add(record);
            System.out.println("Medical record added successfully!");
        } else {
            System.out.println("Invalid choice.");
        }
    }

    // Billing and Invoicing Module
    private void billingAndInvoicing() {
        System.out.println("\nBilling and Invoicing");
        System.out.println("1. Generate Bill");
        System.out.println("2. View Payment History");
        System.out.print("Enter your choice: ");
        
        int choice = scanner.nextInt();
        scanner.nextLine();
        
        if (choice == 1) {
            System.out.print("Enter patient ID: ");
            int patientId = scanner.nextInt();
            scanner.nextLine();
            
            Patient patient = findPatientById(patientId);
            if (patient == null) {
                System.out.println("Patient not found.");
                return;
            }
            
            System.out.print("Enter service description: ");
            String description = scanner.nextLine();
            
            System.out.print("Enter amount: ");
            double amount = scanner.nextDouble();
            scanner.nextLine();
            
            Bill bill = new Bill(billingSystem.getBills().size() + 1, patientId, description, amount, LocalDate.now().toString(), "Pending");
            billingSystem.addBill(bill);
            
            System.out.println("Bill generated successfully!");
            System.out.println("Bill ID: " + bill.getId());
        } else if (choice == 2) {
            System.out.print("Enter patient ID: ");
            int patientId = scanner.nextInt();
            scanner.nextLine();
            
            List<Bill> patientBills = billingSystem.getBillsForPatient(patientId);
            if (patientBills.isEmpty()) {
                System.out.println("No bills found for this patient.");
            } else {
                System.out.println("\nBills for Patient ID " + patientId + ":");
                for (Bill bill : patientBills) {
                    System.out.println(bill);
                }
            }
        } else {
            System.out.println("Invalid choice.");
        }
    }

    // Inventory Management Module
    private void inventoryManagement() {
        System.out.println("\nInventory Management");
        System.out.println("1. View Inventory");
        System.out.println("2. Add New Item");
        System.out.println("3. Update Item Quantity");
        System.out.print("Enter your choice: ");
        
        int choice = scanner.nextInt();
        scanner.nextLine();
        
        if (choice == 1) {
            System.out.println("\nCurrent Inventory:");
            for (MedicalSupply item : inventory.getItems()) {
                System.out.println(item);
            }
        } else if (choice == 2) {
            System.out.print("Enter item ID: ");
            int id = scanner.nextInt();
            scanner.nextLine();
            
            System.out.print("Enter item name: ");
            String name = scanner.nextLine();
            
            System.out.print("Enter quantity: ");
            int quantity = scanner.nextInt();
            scanner.nextLine();
            
            System.out.print("Enter unit price: ");
            double price = scanner.nextDouble();
            scanner.nextLine();
            
            System.out.print("Enter expiration date (YYYY-MM-DD): ");
            String expiryDate = scanner.nextLine();
            
            MedicalSupply item = new MedicalSupply(id, name, quantity, price, expiryDate);
            inventory.addItem(item);
            
            System.out.println("Item added to inventory successfully!");
        } else if (choice == 3) {
            System.out.print("Enter item ID: ");
            int id = scanner.nextInt();
            scanner.nextLine();
            
            MedicalSupply item = inventory.findItemById(id);
            if (item == null) {
                System.out.println("Item not found.");
                return;
            }
            
            System.out.print("Enter new quantity: ");
            int newQuantity = scanner.nextInt();
            scanner.nextLine();
            
            item.setQuantity(newQuantity);
            System.out.println("Quantity updated successfully!");
        } else {
            System.out.println("Invalid choice.");
        }
    }

    // Staff Management Module
    private void staffManagement() {
        System.out.println("\nStaff Management");
        System.out.println("1. View Staff");
        System.out.println("2. Add New Staff Member");
        System.out.println("3. Update Staff Salary");
        System.out.print("Enter your choice: ");
        
        int choice = scanner.nextInt();
        scanner.nextLine();
        
        if (choice == 1) {
            System.out.println("\nStaff Members:");
            for (Staff staff : staffMembers) {
                System.out.println(staff);
            }
        } else if (choice == 2) {
            System.out.print("Enter staff ID: ");
            int id = scanner.nextInt();
            scanner.nextLine();
            
            System.out.print("Enter full name: ");
            String name = scanner.nextLine();
            
            System.out.print("Enter position: ");
            String position = scanner.nextLine();
            
            System.out.print("Enter phone number: ");
            String phone = scanner.nextLine();
            
            System.out.print("Enter salary: ");
            double salary = scanner.nextDouble();
            scanner.nextLine();
            
            Staff staff = new Staff(id, name, position, phone, salary);
            staffMembers.add(staff);
            
            System.out.println("Staff member added successfully!");
        } else if (choice == 3) {
            System.out.print("Enter staff ID: ");
            int id = scanner.nextInt();
            scanner.nextLine();
            
            Staff staff = findStaffById(id);
            if (staff == null) {
                System.out.println("Staff member not found.");
                return;
            }
            
            System.out.print("Enter new salary: ");
            double newSalary = scanner.nextDouble();
            scanner.nextLine();
            
            staff.setSalary(newSalary);
            System.out.println("Salary updated successfully!");
        } else {
            System.out.println("Invalid choice.");
        }
    }

    // Helper methods
    private Patient findPatientById(int id) {
        for (Patient patient : patients) {
            if (patient.getId() == id) {
                return patient;
            }
        }
        return null;
    }

    private Doctor findDoctorById(int id) {
        for (Doctor doctor : doctors) {
            if (doctor.getId() == id) {
                return doctor;
            }
        }
        return null;
    }

    private Staff findStaffById(int id) {
        for (Staff staff : staffMembers) {
            if (staff.getId() == id) {
                return staff;
            }
        }
        return null;
    }

    private List<MedicalRecord> getMedicalRecordsForPatient(int patientId) {
        List<MedicalRecord> result = new ArrayList<>();
        for (MedicalRecord record : medicalRecords) {
            if (record.getPatientId() == patientId) {
                result.add(record);
            }
        }
        return result;
    }
}

// Patient class
class Patient {
    private int id;
    private String name;
    private String dateOfBirth;
    private String gender;
    private String phoneNumber;
    private String email;

    public Patient(int id, String name, String dateOfBirth, String gender, String phoneNumber, String email) {
        this.id = id;
        this.name = name;
        this.dateOfBirth = dateOfBirth;
        this.gender = gender;
        this.phoneNumber = phoneNumber;
        this.email = email;
    }

    // Getters and setters
    public int getId() { return id; }
    public String getName() { return name; }
    public String getDateOfBirth() { return dateOfBirth; }
    public String getGender() { return gender; }
    public String getPhoneNumber() { return phoneNumber; }
    public String getEmail() { return email; }

    @Override
    public String toString() {
        return "Patient ID: " + id + ", Name: " + name + ", DOB: " + dateOfBirth + 
               ", Gender: " + gender + ", Phone: " + phoneNumber + ", Email: " + email;
    }
}

// Doctor class
class Doctor {
    private int id;
    private String name;
    private String specialization;
    private String phoneNumber;
    private String email;

    public Doctor(int id, String name, String specialization, String phoneNumber, String email) {
        this.id = id;
        this.name = name;
        this.specialization = specialization;
        this.phoneNumber = phoneNumber;
        this.email = email;
    }

    // Getters and setters
    public int getId() { return id; }
    public String getName() { return name; }
    public String getSpecialization() { return specialization; }
    public String getPhoneNumber() { return phoneNumber; }
    public String getEmail() { return email; }

    @Override
    public String toString() {
        return "Doctor ID: " + id + ", Name: " + name + ", Specialization: " + specialization + 
               ", Phone: " + phoneNumber + ", Email: " + email;
    }
}

// Staff class
class Staff {
    private int id;
    private String name;
    private String position;
    private String phoneNumber;
    private double salary;

    public Staff(int id, String name, String position, String phoneNumber, double salary) {
        this.id = id;
        this.name = name;
        this.position = position;
        this.phoneNumber = phoneNumber;
        this.salary = salary;
    }

    // Getters and setters
    public int getId() { return id; }
    public String getName() { return name; }
    public String getPosition() { return position; }
    public String getPhoneNumber() { return phoneNumber; }
    public double getSalary() { return salary; }
    public void setSalary(double salary) { this.salary = salary; }

    @Override
    public String toString() {
        return "Staff ID: " + id + ", Name: " + name + ", Position: " + position + 
               ", Phone: " + phoneNumber + ", Salary: $" + String.format("%.2f", salary);
    }
}

// Appointment class
class Appointment {
    private int id;
    private int patientId;
    private int doctorId;
    private String dateTime;
    private String reason;
    private String status; // Scheduled, Completed, Cancelled

    public Appointment(int id, int patientId, int doctorId, String dateTime, String reason) {
        this.id = id;
        this.patientId = patientId;
        this.doctorId = doctorId;
        this.dateTime = dateTime;
        this.reason = reason;
        this.status = "Scheduled";
    }

    // Getters
    public int getId() { return id; }
    public i
