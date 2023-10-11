# Title: EX-Query con GROUP BY

```sql
-- 1. Contare quanti iscritti ci sono stati ogni anno

SELECT COUNT(*) `numb_iscritti`, YEAR(`enrolment_date`) `year`
FROM `students`
GROUP BY (`year`);
```

```sql
-- 2. Contare gli insegnanti che hanno l'ufficio nello stesso edificio

SELECT COUNT(`office_address`) `tot_teachers_office_same_building`, `office_address`
FROM `teachers`
GROUP BY (`office_address`);
```

```sql
-- 3. Calcolare la media dei voti di ogni appello "d'esame"

SELECT `exam_id`,AVG(`vote`)
FROM `exam_student`
GROUP BY (`exam_id`);
```

```sql
-- 4. Contare quanti corsi di laurea ci sono per ogni dipartimento

SELECT COUNT(`department_id` ) `tot_degree_courses_for_each_department`
FROM `degrees`
GROUP BY (`department_id`);
```

# Title: EX-Query con JOIN

```sql
-- 1. Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

SELECT `s`.`name` `student_name`, `s`.`surname` `student_surname`, `d`.`name` `degree_couse`
FROM `students` `s`
INNER JOIN `degrees` `d`
ON `s`.`degree_id`=`d`.`id`
WHERE `d`.`name`="Corso di Laurea in Economia";
```

```sql
-- 2. Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

SELECT `degrees`.`name` `degrees`,`departments`.`name` `departments`
FROM `degrees`
INNER JOIN `departments`
ON `degrees`.`department_id`=`departments`.`id`
WHERE `degrees`.`name` LIKE 'Corso di Laurea Magistrale%'
AND `departments`.`name`= "Dipartimento di Neuroscienze";
```

```sql
-- 3. Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)
SELECT `c`.`name` `courses_name`, `t`.`name` `teacher_name`,`t`.`surname` `teacher_surname`, `t`.`id`
FROM `courses` `c`
INNER JOIN `course_teacher` `c_t`
ON `c`.`id`=`c_t`.`course_id`

INNER JOIN `teachers` `t`
ON `c_t`.`teacher_id`=`t`.`id`

WHERE `t`.`name`="Fulvio"
AND `t`.`surname`="Amato";

-- or WHERE `t`.`id`=44;
```

```sql
-- 4. Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

SELECT `students`.`surname` `student_surname`, `students`.`name` `student_name`, `degrees`.`name` `degree_name`,`departments`.`name` `department_name`
FROM `students`

INNER JOIN `degrees`
ON `students`.`degree_id`=`degrees`.`id`

INNER JOIN `departments`
ON `degrees`.`department_id`=`departments`.`id`

ORDER BY `student_surname` ASC, `student_name` ASC;
```

```sql
-- 5. Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

SELECT `degrees`.`name` `degree_name`, `courses`.`name` `course_name`, `teachers`.`name` `teacher_name`, `teachers`.`surname` `teacher_surname`
FROM `degrees`

INNER JOIN `courses`
ON `degrees`.`id`=`courses`.`degree_id`

INNER JOIN `course_teacher`
ON `courses`.`id`=`course_teacher`.`course_id`

INNER JOIN `teachers`
ON `course_teacher`.`teacher_id`=`teachers`.`id`;
```

```sql
-- 6. Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

SELECT  COUNT(`teachers`.`surname`) `numb_teacher`, `teachers`.`surname` `teacher_surname`, `teachers`.`name` `teacher_name`, `departments`.`name` `department_name`
FROM `teachers`

INNER JOIN `course_teacher`
ON `teachers`.`id`=`course_teacher`.`teacher_id`

INNER JOIN `courses`
ON `course_teacher`.`course_id`=`courses`.`id`

INNER JOIN `degrees`
ON `courses`.`degree_id`=`degrees`.`id`

INNER JOIN `departments`
ON `degrees`.`department_id`=`departments`.`id`

WHERE `departments`.`name`="Dipartimento di Matematica"

GROUP BY  `teachers`.`name`, `teachers`.`surname`;
```

```sql
-- 7. BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.

SELECT COUNT(*),`students`.`id` `id_stud`,`courses`.`id` `id_course`, `courses`.`name` `course_name` , `students`.`name` `student_name`, `students`.`surname` `student_surname`,  MAX(`exam_student`.`vote`) `vote_max`
FROM `students`

INNER JOIN `exam_student`
ON `students`.`id`=`exam_student`.`student_id`

INNER JOIN `exams`
ON `exam_student`.`exam_id`=`exams`.`id`

JOIN `courses`
ON `exams`.`course_id`=`courses`.`id`

GROUP BY `students`.`id`,`courses`.`id`

HAVING `vote_max`>=18;
```
