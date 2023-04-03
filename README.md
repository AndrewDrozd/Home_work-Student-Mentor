class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}
# оценка лекторов
    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in lecturer.courses_attached and course in self.courses_in_progress:
            if course in lecturer.lect_grades:
                lecturer.lect_grades[course] += [grade]
            else:
                lecturer.lect_grades[course] = [grade]
        else:
            return 'Ошибка'
# средний балл студента    
    def average_g(self):
        count = 0
        len_gr = 0
        for i in self.grades.values():
            count += sum(i)
            len_gr += len(i)
        fin_average = round(count / len_gr, 2)
        return fin_average
# средний балл по одному студенту по конкрет предмету
    def aver_one_student(self, course):
        sum_1 = 0
        aver_1 = 0
        for name in self.grades.keys():
            if name == course:
                sum_1 += sum(self.grades[name])
                aver_1 += len(self.grades[name])
        aver_all_grades = round(sum_1 / aver_1, 2)
        return aver_all_grades

# завершенные курсы 
    def fin_cours(self):
        for i in self.finished_courses:
            return i
# сравнение средних баллов студентов
    def __lt__(self, other):
        if not isinstance(other, Student):
            print('В студентах не числится')
            return
        return self.average_g() < other.average_g()
# анкета студента 
    def __str__(self):
        res = (f'Имя: {self.name}\n'
               f'Фамилия: {self.surname}\n'
               f'Средняя оценка за домашние задания: {self.average_g()}\n'
            f'Курсы в процессе изучения: {", ".join(self.courses_in_progress)}\n'
            f'Завершенные курсы: {self.fin_cours()}')
        return res

class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.lect_grades = {}
        self.list_lectures = []
# средний балл лектора    
    def aver_rait(self):
        count_rait = 0
        len_rait = 0
        for i in self.lect_grades.values():
            count_rait += sum(i)
            len_rait += len(i)
        average_rait = round(count_rait / len_rait, 2)
        return average_rait
    
# средний балл лектора по конкрет предмету
    def aver_one_lect(self, course):
        sum_1 = 0
        aver_1 = 0
        for name in self.lect_grades.keys():
            if name == course:
                sum_1 += sum(self.lect_grades[name])
                aver_1 += len(self.lect_grades[name])
        aver_all_grades_cours = round(sum_1 / aver_1, 2)
        return aver_all_grades_cours

# сравнение средних баллов лекторов
    def __lt__(self, other):
        if not isinstance(other, Lecturer):
            print('В лекторах не числится')
            return
        return self.aver_rait() < other.aver_rait()    

# анкета лектора        
    def __str__(self):
        res = f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {self.aver_rait()}'
        return res 

class Reviewer(Mentor):
# оценки студентам  
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'
# анкета ревьювера          
    def __str__(self):
        res = f'Имя: {self.name}\nФамилия: {self.surname}'
        return res    

# студенты
student_ek = Student('Евпатий', 'Коловрат', 'male')
student_ek.courses_in_progress += ['Git', 'Python']
student_ek.finished_courses += ['Введение в программирование']

student_sb = Student('Шура', 'Балаганов', 'male')
student_sb.courses_in_progress += ['Git', 'Python']
student_sb.finished_courses += ['Введение в программирование']

# лекторы
lecturer_1 = Lecturer('Arnold', 'Schwarz')
lecturer_1.courses_attached += ['Python', 'Git']
lecturer_2 = Lecturer('Haruki', 'Muracami')
lecturer_2.courses_attached += ['Python', 'Git']

# ревьюверы
reviewer_1 = Reviewer('Jim', 'Carry')
reviewer_2 = Reviewer('Jane', 'Fonda')
reviewer_1.courses_attached += ['Python', 'Git']
reviewer_2.courses_attached += ['Python', 'Git']
# оценки студентам
reviewer_1.rate_hw(student_ek, 'Python', 8)
reviewer_1.rate_hw(student_ek, 'Git', 4)
reviewer_2.rate_hw(student_sb, 'Python', 10)
reviewer_2.rate_hw(student_sb, 'Git', 9)

# оценки лекторам
student_ek.rate_lecturer(lecturer_1, 'Python', 8)
student_ek.rate_lecturer(lecturer_1, 'Git', 7)
student_sb.rate_lecturer(lecturer_2, 'Git', 6)
student_sb.rate_lecturer(lecturer_2, 'Python', 4)

list_students = [student_ek, student_sb]
list_lectures = [lecturer_1, lecturer_2]
list_reviewers = []  

# средняя оценка по всем студентам по 1 предмету
def aver_all_student(list_students, course):
    raiting_sum = 0
    numb_student = 0
    for i in list_students:
        rait_one = i.aver_one_student(course)
        raiting_sum += rait_one
        numb_student += 1
    all_stud_average = raiting_sum / numb_student
    return all_stud_average
    
# средняя оценка по всем лекторам по 1 предмету
def aver_all_lecturer(list_lectures, course):
    raiting_sum = 0
    numb_lect = 0
    for i in list_lectures:
        rait_one = i.aver_one_lect(course)
        raiting_sum += rait_one
        numb_lect += 1
    all_lect_average = raiting_sum / numb_lect
    return all_lect_average

print(student_ek)
print(student_sb)
print(reviewer_1)
print(reviewer_2)
print(lecturer_1)
print(lecturer_2)
print(student_ek < student_sb)
print(lecturer_1 < lecturer_2)
print(student_ek.aver_one_student('Python'))
print(student_sb.aver_one_student('Python'))
print(aver_all_student(list_students, 'Python'))
print(aver_all_student(list_students, 'Git'))
print(lecturer_1.aver_one_lect('Python'))
print(lecturer_2.aver_one_lect('Git'))
print(aver_all_lecturer(list_lectures, 'Python'))
print(aver_all_lecturer(list_lectures, 'Git'))  