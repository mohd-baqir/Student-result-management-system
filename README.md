import java.util.*;

class Student {
    String id;
    String name;
    int[] marks;
    int total;
    double average;
    String grade;

    public Student(String id, String name, int[] marks) {
        this.id = id;
        this.name = name;
        this.marks = marks;
        calculateResults();
    }

    // Calculate total, average, and grade
    public void calculateResults() {
        total = 0;
        for (int mark : marks) {
            total += mark;
        }
        average = total / (double) marks.length;

        if (average >= 90) grade = "A+";
        else if (average >= 80) grade = "A";
        else if (average >= 70) grade = "B";
        else if (average >= 60) grade = "C";
        else if (average >= 50) grade = "D";
        else grade = "F";
    }

    public void displayStudent() {
        System.out.println("-------------------------------------------------");
        System.out.println("ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Marks: " + Arrays.toString(marks));
        System.out.println("Total: " + total);
        System.out.println("Average: " + average);
        System.out.println("Grade: " + grade);
        System.out.println("-------------------------------------------------");
    }
}

class StudentResultManagementSystem {
    static Scanner sc = new Scanner(System.in);
    static HashMap<String, Student> students = new HashMap<>();

    public static void main(String[] args) {
        int choice;
        do {
            System.out.println("\n==== Student Result Management System ====");
            System.out.println("1. Add Student Record");
            System.out.println("2. Update Student Record");
            System.out.println("3. Delete Student Record");
            System.out.println("4. Display Individual Result");
            System.out.println("5. Display All Students Results");
            System.out.println("6. Generate Report");
            System.out.println("7. Exit");
            System.out.print("Enter your choice: ");
            choice = sc.nextInt();

            switch (choice) {
                case 1 -> addStudent();
                case 2 -> updateStudent();
                case 3 -> deleteStudent();
                case 4 -> displayIndividual();
                case 5 -> displayAll();
                case 6 -> generateReport();
                case 7 -> System.out.println("Exiting... Thank you!");
                default -> System.out.println("Invalid choice! Try again.");
            }
        } while (choice != 7);
    }

    static void addStudent() {
        System.out.print("Enter Student ID: ");
        String id = sc.next();
        sc.nextLine(); // consume newline
        System.out.print("Enter Student Name: ");
        String name = sc.nextLine();
        System.out.print("Enter number of subjects: ");
        int n = sc.nextInt();
        int[] marks = new int[n];
        for (int i = 0; i < n; i++) {
            System.out.print("Enter marks for subject " + (i + 1) + ": ");
            marks[i] = sc.nextInt();
        }
        students.put(id, new Student(id, name, marks));
        System.out.println("Student added successfully!");
    }

    static void updateStudent() {
        System.out.print("Enter Student ID to update: ");
        String id = sc.next();
        if (!students.containsKey(id)) {
            System.out.println("Student not found!");
            return;
        }
        sc.nextLine(); // consume newline
        System.out.print("Enter new name: ");
        String name = sc.nextLine();
        System.out.print("Enter number of subjects: ");
        int n = sc.nextInt();
        int[] marks = new int[n];
        for (int i = 0; i < n; i++) {
            System.out.print("Enter marks for subject " + (i + 1) + ": ");
            marks[i] = sc.nextInt();
        }
        students.put(id, new Student(id, name, marks));
        System.out.println("Record updated successfully!");
    }

    static void deleteStudent() {
        System.out.print("Enter Student ID to delete: ");
        String id = sc.next();
        if (students.remove(id) != null) {
            System.out.println("Student record deleted successfully!");
        } else {
            System.out.println("Student not found!");
        }
    }

    static void displayIndividual() {
        System.out.print("Enter Student ID to view: ");
        String id = sc.next();
        Student s = students.get(id);
        if (s != null) {
            s.displayStudent();
        } else {
            System.out.println("Student not found!");
        }
    }

    static void displayAll() {
        if (students.isEmpty()) {
            System.out.println("No student records available!");
            return;
        }
        System.out.println("\n===== All Student Results =====");
        for (Student s : students.values()) {
            s.displayStudent();
        }
    }

    static void generateReport() {
        if (students.isEmpty()) {
            System.out.println("No records to generate report!");
            return;
        }
        double totalClassAverage = 0;
        for (Student s : students.values()) {
            totalClassAverage += s.average;
        }
        double classAverage = totalClassAverage / students.size();
        System.out.println("\n===== Class Report =====");
        System.out.println("Total Students: " + students.size());
        System.out.println("Class Average: " + classAverage);
        System.out.println("==============================");
    }
}
