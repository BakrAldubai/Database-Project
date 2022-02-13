CREATE DATABASE IF NOT EXISTS elearndb;

USE elearndb;

CREATE TABLE IF NOT EXISTS user(
    id INT NOT NULL AUTO_INCREMENT,
    user_name VARCHAR(50) NOT NULL,
    image VARCHAR(150),
    email VARCHAR(100) NOT NULL,
    user_password VARCHAR(150) NOT NULL,
    state TINYINT(4) NOT NULL DEFAULT 1,
    enroll_date DATE NOT NULL ,
    PRIMARY KEY (id)

);
CREATE TABLE IF NOT EXISTS permission(
id int NOT NULL AUTO_INCREMENT, 
permission_title varchar(50) NOT NULL,
PRIMARY KEY (id)
);

CREATE TABLE IF NOT EXISTS user_permissions(
user_id int NOT NULL,
permission_id int NOT NULL);

ALTER TABLE user_permissions
ADD FOREIGN KEY (user_id) REFERENCES user(id);

ALTER TABLE user_permissions
ADD FOREIGN KEY (permission_id) REFERENCES permission(id);

CREATE TABLE IF NOT EXISTS course(
    id INT NOT NULL AUTO_INCREMENT,
    course_name VARCHAR(50) NOT NULL,
    image VARCHAR(150),
    trainer_id INT NOT NULL,
    field VARCHAR(50) NOT NULL,
    publish_date DATE NOT NULL,
    course_state VARCHAR(10) NOT NULL DEFAULT 'running',
    code VARCHAR(100) NOT NULL,
    PRIMARY KEY(id),
    CONSTRAINT FK_course_trainer_id FOREIGN KEY(trainer_id) REFERENCES user(id)
);

CREATE TABLE IF NOT EXISTS studys(
    id int NOT NULL AUTO_INCREMENT,
    enroll_date DATE NOT NULL,
    student_id INT NOT NULL,
    course_id INT NOT NULL,
    PRIMARY KEY(id),
    CONSTRAINT FK_student_id FOREIGN KEY (student_id) REFERENCES user(id),
    CONSTRAINT FK_course_id FOREIGN KEY(course_id) REFERENCES course(id)
);


CREATE TABLE IF NOT EXISTS lesson(
    id INT NOT NULL AUTO_INCREMENT,
    course_id INT NOT NULL,
    publish_date DATE NOT NULL,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    drive_link TEXT NOT NULL,
    PRIMARY KEY(id),
    CONSTRAINT FK_course_lesson_id FOREIGN KEY(course_id) REFERENCES course(id)
);

CREATE TABLE IF NOT EXISTS comments(
    id INT NOT NULL AUTO_INCREMENT,
    comment_content TEXT NOT NULL ,
    user_id INT NOT NULL ,
    lesson_id INT NOT NULL ,
    publish_date date NOT NULL ,
    PRIMARY KEY(id),
    CONSTRAINT FK_lesson_id FOREIGN KEY (lesson_id) REFERENCES lesson(id),
    CONSTRAINT FK_commenter_id FOREIGN KEY (user_id) REFERENCES user(id)
);

insert into user(user_name,image,email,user_password,enroll_date) values
                  ('Abobakr Omar','1.png','eng.bakraldubai@gmail.com','774046471','2021-12-30'),
			    		('Alaa','2.png','eng.alaa@gmail.com','alaa123','2021-12-31'),
	            	('Maher','3.png','eng.maher@gmail.com','m123','2021-12-5');
	            	
	           	
SELECT * FROM USER;

insert into permission(permission_title) values
                  ('enroll for course'),
						('add course'),
						('add lesson'),
						('delete course'),
						('delete lesson'),
						('add comment'),
						('delete comment'),
						('update comment');
						
SELECT * FROM permission;

insert into user_permissions(user_id,permission_id) values
			(1,1),
			(1,6),
			(1,7),
			(1,8),
			(2,1),
			(2,6),
			(2,7),
			(2,8),
			(3,2),
			(3,3),
			(3,4),
			(3,5),
			(3,6),
			(3,7),
			(3,8);
			
SELECT * FROM user_permissions;
SELECT u.user_name , p.permission_title
FROM user_permissions up  INNER JOIN  user u 
								ON ( u.id = up.user_id)
								INNER JOIN permission p
								ON ( p.id = up.permission_id);
								
insert into course(course_name,image,trainer_id,field,publish_date,code) values
			('database','database.png',3,'programming','2022-1-1','db123');					
								

SELECT c.course_name,c.image,u.user_name,c.FIELD,c.publish_date,code FROM course c inner JOIN user u
ON c.trainer_id = u.id;



insert into studys(enroll_date,student_id ,course_id) values
			('2022-1-2',1,1),
			('2022-1-2',2,1);

SELECT u.user_name , c.course_name ,s.enroll_date
FROM studys s  INNER JOIN  user u 
								ON ( u.id = s.student_id)
								INNER JOIN course c
								ON ( c.id = s.course_id);
			
insert into lesson(course_id,publish_date,title,description,drive_link) values
			(1,'2022-1-4',"intro to database","in this lesson you will learn basics of mysql database","www.intro.com");
			
SELECT course_name,lesson.publish_date,title,description,drive_link FROM lesson
INNER JOIN course ON course_id = course.id;

SELECT * FROM lesson WHERE course_id = 1 ;

insert into comments(comment_content,user_id,lesson_id,publish_date) values
			('Hi Teacher',1,1,'2022-1-5'),
			('Hi Bakr',3,1,'2022-1-6');
			
SELECT l.title, u.user_name , c.comment_content , c.publish_date
FROM comments c INNER JOIN user u ON c.user_id=u.id 
INNER JOIN lesson l ON c.lesson_id = l.id
ORDER BY l.title,c.publish_date ;

