# Case-Study-Java
Java Case Study On Hospital Management System
import java.io.*;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

class Patient {
    int id;
    String patientName;
    String patientAddress;
    String disease;
    String date;

    public Patient() {
        this.date = getCurrentDate();
    }

    private String getCurrentDate() {
        SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
        return dateFormat.format(new Date());
    }
}

class Doctor {
    int id;
    String name;
    String address;
    String specialize;
    String date;

    public Doctor() {
        this.date = getCurrentDate();
    }

    private String getCurrentDate() {
        SimpleDateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
        return dateFormat.format(new Date());
    }
}

public class HospitalManagementSystem {
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        int choice;
        while (true) {
            clearConsole();
            System.out.println("<== Hospital Management System ==>");
            System.out.println("1. Admit Patient");
            System.out.println("2. Patient List");
            System.out.println("3. Discharge Patient");
            System.out.println("4. Add Doctor");
            System.out.println("5. Doctors List");
            System.out.println("0. Exit\n");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 0:
                    System.exit(0);
                    break;
                case 1:
                    admitPatient();
                    break;
                case 2:
                    patientList();
                    break;
                case 3:
                    dischargePatient();
                    break;
                case 4:
                    addDoctor();
                    break;
                case 5:
                    doctorList();
                    break;
                default:
                    System.out.println("Invalid Choice...\n");
            }
            System.out.print("\nPress Enter To Continue...");
            scanner.nextLine(); // Consume the newline character left by nextInt()
            scanner.nextLine(); // Wait for Enter key
        }
    }

    private static void admitPatient() {
        Patient patient = new Patient();
        FileWriter fileWriter = null;
        try {
            fileWriter = new FileWriter("patient.txt", true);
            BufferedWriter bufferedWriter = new BufferedWriter(fileWriter);

            System.out.print("Enter Patient id: ");
            patient.id = scanner.nextInt();

            System.out.print("Enter Patient name: ");
            scanner.nextLine(); // Consume the newline character left by nextInt()
            patient.patientName = scanner.nextLine();

            System.out.print("Enter Patient Address: ");
            patient.patientAddress = scanner.nextLine();

            System.out.print("Enter Patient Disease: ");
            patient.disease = scanner.nextLine();

            System.out.println("\nPatient Added Successfully");
            bufferedWriter.write(patient.id + "," + patient.patientName + "," + patient.patientAddress + ","
                    + patient.disease + "," + patient.date + "\n");
            bufferedWriter.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void patientList() {
        clearConsole();
        System.out.println("<== Patient List ==>\n");
        System.out.printf("%-10s %-30s %-30s %-20s %s\n", "Id", "Patient Name", "Address", "Disease", "Date");
        System.out.println(
                "----------------------------------------------------------------------------------------------------------");

        try {
            BufferedReader bufferedReader = new BufferedReader(new FileReader("patient.txt"));
            String line;
            while ((line = bufferedReader.readLine()) != null) {
                String[] parts = line.split(",");
                System.out.printf("%-10s %-30s %-30s %-20s %s\n", parts[0], parts[1], parts[2], parts[3], parts[4]);
            }
            bufferedReader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void dischargePatient() {
        int id, f = 0;
        clearConsole();
        System.out.println("<== Discharge Patient ==>\n");
        System.out.print("Enter Patient id to discharge: ");
        id = scanner.nextInt();

        try {
            BufferedReader bufferedReader = new BufferedReader(new FileReader("patient.txt"));
            BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter("temp.txt"));

            String line;
            while ((line = bufferedReader.readLine()) != null) {
                String[] parts = line.split(",");
                int patientId = Integer.parseInt(parts[0]);

                if (id == patientId) {
                    f = 1;
                } else {
                    bufferedWriter.write(line + "\n");
                }
            }
            bufferedReader.close();
            bufferedWriter.close();

            if (f == 1) {
                System.out.println("\nPatient Discharged Successfully.");
            } else {
                System.out.println("\nRecord Not Found !");
            }

            File patientFile = new File("patient.txt");
            if (patientFile.delete()) {
                System.out.println("Patient file deleted successfully.");
            } else {
                System.out.println("Failed to delete the patient file.");
            }

            File tempFile = new File("temp.txt");
            if (tempFile.renameTo(patientFile)) {
                System.out.println("File renamed successfully");
            } else {
                System.out.println("Failed to rename the file");
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    private static void addDoctor() {
        Doctor doctor = new Doctor();
        FileWriter fileWriter = null;
        try {
            fileWriter = new FileWriter("doctor.txt", true);
            BufferedWriter bufferedWriter = new BufferedWriter(fileWriter);

            System.out.print("Enter Doctor id: ");
            doctor.id = scanner.nextInt();

            System.out.print("Enter Doctor Name: ");
            scanner.nextLine();
            doctor.name = scanner.nextLine();

            System.out.print("Enter Doctor Address: ");
            doctor.address = scanner.nextLine();

            System.out.print("Doctor Specialize in: ");
            doctor.specialize = scanner.nextLine();

            System.out.println("Doctor Added Successfully\n");
            bufferedWriter.write(doctor.id + "," + doctor.name + "," + doctor.address + "," + doctor.specialize + ","
                    + doctor.date + "\n");
            bufferedWriter.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void doctorList() {
        clearConsole();
        System.out.println("<== Doctor List ==>\n");
        System.out.printf("%-10s %-30s %-30s %-30s %s\n", "id", "Name", "Address", "Specialize", "Date");
        System.out.println(
                "-------------------------------------------------------------------------------------------------------------------");

        try {
            BufferedReader bufferedReader = new BufferedReader(new FileReader("doctor.txt"));
            String line;
            while ((line = bufferedReader.readLine()) != null) {
                String[] parts = line.split(",");
                System.out.printf("%-10s %-30s %-30s %-30s %s\n", parts[0], parts[1], parts[2], parts[3], parts[4]);
            }
            bufferedReader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void clearConsole() {
        try {
            final String os = System.getProperty("os.name");
            if (os.contains("Windows")) {
                new ProcessBuilder("cmd", "/c", "cls").inheritIO().start().waitFor();
            } else {
                System.out.print("\033[H\033[2J");
                System.out.flush();
            }
        } catch (final Exception e) {
            e.printStackTrace();
        }
    }
}
