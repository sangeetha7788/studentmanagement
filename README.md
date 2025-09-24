# studentmanagement
import java.util.*;


class Person {
    protected int id;
    protected String name;

    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public void displayInfo() {
        System.out.println("ID: " + id + ", Name: " + name);
    }
}

class Student extends Person {
    private String course;
    private int marks;

    public Student(int id, String name, String course, int marks) {
        super(id, name);
        this.course = course;
        this.marks = marks;
    }

    public String getCourse() { return course; }
    public void setCourse(String course) { this.course = course; }

    public int getMarks() { return marks; }
    public void setMarks(int marks) { this.marks = marks; }

    @Override
    public void displayInfo() {
        System.out.println("Student ID: " + id + ", Name: " + name +
                ", Course: " + course + ", Marks: " + marks);
    }

    public void displayInfo(boolean showGrades) {
        displayInfo();
        if (showGrades) {
            System.out.println("Grade: " + calculateGrade());
        }
    }

    private String calculateGrade() {
        if (marks >= 90) return "A";
        else if (marks >= 75) return "B";
        else if (marks >= 60) return "C";
        else return "F";
    }
}


class Teacher extends Person {
    private String subject;

    public Teacher(int id, String name, String subject) {
        super(id, name);
        this.subject = subject;
    }

    @Override
    public void displayInfo() {
        System.out.println("Teacher ID: " + id + ", Name: " + name +
                ", Subject: " + subject);
    }
}


interface DatabaseOperations {
    void addStudent(Student student);
    void updateStudent(int id, Student updatedStudent);
    void deleteStudent(int id);
    List<Student> viewStudents();
}


class StudentDatabase implements DatabaseOperations {
    private Map<Integer, Student> studentMap = new HashMap<>();

    @Override
    public void addStudent(Student student) {
        studentMap.put(student.id, student);
        System.out.println("Student added successfully.");
    }

    @Override
    public void updateStudent(int id, Student updatedStudent) {
        if (studentMap.containsKey(id)) {
            studentMap.put(id, updatedStudent);
            System.out.println("Student record updated.");
        } else {
            System.out.println("Student not found.");
        }
    }

    @Override
    public void deleteStudent(int id) {
        if (studentMap.remove(id) != null) {
            System.out.println("student deleted");
        }
             else {
            System.out.println("Student not found.");
        }
    }

    @Override
    public List<Student> viewStudents() {
        return new ArrayList<>(studentMap.values());
    }
}


public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        StudentDatabase db = new StudentDatabase();

        while (true) {
            System.out.println("\n--- Student Database Management System ---");
            System.out.println("1. Add Student");
            System.out.println("2. View All Students");
            System.out.println("3. Update Student");
            System.out.println("4. Delete Student");
            System.out.println("5. Exit");
            System.out.print("Enter choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1 -> {
                    System.out.print("Enter ID: ");
                    int id = scanner.nextInt();
                    scanner.nextLine();
                    System.out.print("Enter Name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter Course: ");
                    String course = scanner.nextLine();
                    System.out.print("Enter Marks: ");
                    int marks = scanner.nextInt();

                    Student student = new Student(id, name, course, marks);
                    db.addStudent(student);
                }
                case 2 -> {
                    List<Student> students = db.viewStudents();
                    if (students.isEmpty()) {
                        System.out.println("No student records found.");
                    } else {
                        for (Student s : students) {
                            s.displayInfo(true);
                        }
                    }
                }
                case 3 -> {
                    System.out.print("Enter ID to update: ");
                    int id = scanner.nextInt();
                    scanner.nextLine();
                    System.out.print("Enter New Name: ");
                    String name = scanner.nextLine();
                    System.out.print("Enter New Course: ");
                    String course = scanner.nextLine();
                    System.out.print("Enter New Marks: ");
                    int marks = scanner.nextInt();

                    Student updated = new Student(id, name, course, marks);
                    db.updateStudent(id, updated);
                }
                case 4 -> {
                    System.out.print("Enter ID to delete: ");
                    int id = scanner.nextInt();
                    db.deleteStudent(id);
                }
                case 5 -> {
                    System.out.println("Exiting system.");
                    return;
                }
                default -> System.out.println("Invalid choice.");
            }
        }
    }
}

