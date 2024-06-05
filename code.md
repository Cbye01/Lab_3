from datetime import date, datetime
import csv

class Person:
    def __init__(self, surname, first_name, birth_date_str, nickname=""):
        self.surname = surname
        self.first_name = first_name
        self.nickname = nickname
        year, month, day = map(int, birth_date_str.split('-'))
        self.birth_date = date(year, month, day)
    def get_age(self):
        today = date.today()
        age = today.year - self.birth_date.year - (
            (today.month, today.day) < (self.birth_date.month, self.birth_date.day)
        )
        return str(age)
    def get_fullname(self):
        return f"{self.surname} {self.first_name}"
def modifier(filename):
    with open(filename, 'r', newline='') as csvfile:
        reader = csv.DictReader(csvfile)
        fieldnames = reader.fieldnames + ['fullname', 'age']
        rows = []
        for row in reader:
            person = Person(
                surname=row['surname'],
                first_name=row['first_name'],
                birth_date_str=row['birth_date'],
                nickname=row.get('nickname', "")
            )
            row['fullname'] = person.get_fullname()
            row['age'] = person.get_age()
            rows.append(row)
    with open(filename, 'w', newline='') as csvfile:
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(rows)
