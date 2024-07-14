Task 1: Create a table named teachers and insert 8 rows
CREATE TABLE teachers (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    subject VARCHAR(50),
    experience INT,
    salary DECIMAL(10, 2)
);

INSERT INTO teachers (name, subject, experience, salary) VALUES
('John Doe', 'Math', 5, 50000),
('Jane Smith', 'English', 8, 60000),
('Michael Johnson', 'Science', 3, 45000),
('Emily Davis', 'History', 6, 55000),
('David Brown', 'Physics', 4, 48000),
('Lisa Wilson', 'Biology', 7, 58000),
('Robert Lee', 'Chemistry', 2, 42000),
('Sarah Miller', 'Geography', 9, 65000);
Task 2: Create a before insert trigger to check salary
DELIMITER //

CREATE TRIGGER before_insert_teacher
BEFORE INSERT ON teachers
FOR EACH ROW
BEGIN
    IF NEW.salary < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Salary cannot be negative';
    END IF;
END//

DELIMITER ;
Task 3: Create an after insert trigger to log insertions
First, create a table teacher_log to store log entries:

CREATE TABLE teacher_log (
    log_id INT AUTO_INCREMENT PRIMARY KEY,
    teacher_id INT,
    action VARCHAR(50),
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
Now, create the after insert trigger:

DELIMITER //

CREATE TRIGGER after_insert_teacher
AFTER INSERT ON teachers
FOR EACH ROW
BEGIN
    INSERT INTO teacher_log (teacher_id, action)
    VALUES (NEW.id, 'Inserted');
END//

DELIMITER ;
Task 4: Create a before delete trigger to check experience
DELIMITER //

CREATE TRIGGER before_delete_teacher
BEFORE DELETE ON teachers
FOR EACH ROW
BEGIN
    IF OLD.experience > 10 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Cannot delete teacher with experience greater than 10 years';
    END IF;
END//

DELIMITER ;
Task 5: Create an after delete trigger to log deletions
Create the after delete trigger:
DELIMITER //

CREATE TRIGGER after_delete_teacher
AFTER DELETE ON teachers
FOR EACH ROW
BEGIN
    INSERT INTO teacher_log (teacher_id, action)
    VALUES (OLD.id, 'Deleted');
END//

DELIMITER ;

