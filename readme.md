## Console Lab

For this lab, we'd like you to strengthen your Rails console skills. This lab is going to be very familiar to the SQL lab, where we'll ask you to create a model and then write out the console commands you would use to execute these queries

### To Start

1. Create a model called Student, that has a first_name, last_name and age
2. Don't forget to run your migrations

### Tasks to create

1. Using the new/save syntax, create a student, first and last name and an age

	```
	student = Student.new
	student.first_name = "Sana"
	student.last_name = "Nasar"
	student.age = 22
	 ```
 
2. Save the student to the database

	```
	student.save
	```

3. Using the find/set/save syntax update the student's first name to taco

	```
	student = Student.find(7)
	student.first_name="Taco"
	student.save
	```
	
4. Delete the student (where first_name is taco)

	```
	student=Student.find_by(first_name:"Taco")
	student.destroy
	```
	
5. Validate that every Student's last name is unique

	```
	validates_uniqueness_of :last_name
	
	-or-
	
	validates :last_name, uniqueness: true
	
	```
	
6. Validate that every Student has a first and last name that is longer than 4 characters

	```
	validates :first_name, length: {minimum: 4}
	validates :last_name, length: {minimum: 4}
	```

7. Validate that every first and last name cannot be empty

	```
	validates_presence_of :first_name
	validates_presence_of :last_name
	```
	
8. Combine all of these individual validations into one validation (using validate and a hash)

	```
	validates :first_name, :last_name, {
										:presence => true, 
                                        :length => {minimum: 4}, 
                                        :uniqueness => true
                                        }
	``` 

9. Using the create syntax create a student named John Doe who is 33 years old

	```
	student = Student.create(:first_name => "John", :last_name => "Doe", :age => 33)	
	``` 

10. Show if this new student entry is valid

	```
	(Need to Review)
	Student.create(first_name: "Jonathan").valid?
	
	```

11. Show the number of errors for this student instance

	```
	(Need to Review)
	Student.create(first_name: "Jonathan").error?
	```

12. In one command, Change John Doe's name to Jonathan Doesmith

	```
	Student.find_by(first_name:"John", last_name:"Doe").update_attributes(:first_name =>"Jonathan", :last_name =>"Doesmith")
	```
 
13. Clear the errors array

	```
	(Need to Review)
	Student.create(first_name: "Jonathan").error?
	```

14. Save Jonathan Doesmith

	```
	Student.save
	```

15. Find all of the Students

	```
	Student.all
	```

16. Find the student with an ID of 128 and if it does not exist, make sure it returns nil and not an error

	```
	(Need to Review)
	Student.find_by_id(128)
	```

17. Find the first student in the table

	```
	Student.first
	```
18. Find the last student in the table

	```
	Student.last
	```

19. Find the student with the last name of Doesmith

	```
	Student.find_by(last_name:"Doesmith")
	```

20. Find all of the students and limit the search to 5 students, starting with the 2nd student and finally, order the students in alphabetical order

	```
	(Need to Review)
	order(students.last_name ASC)
		User.order(:age)
		User.order("age")
		User.order(age: :asc)
	limit (integer)
		User.limit(5)
	offset (integer)
		User.offset(2)
	
	```
	
21. Delete Jonathan Doesmith

	```
	students=Student.where(first_name: "Jonathan", last_name:"Doesmith")
	students.each {|student| student.destroy}
	
	-or-
	
	students=Student.where(first_name: "Jonathan", last_name:"Doesmith")
	students.each do |student| 
		student.destroy
	end

	```


### Bonus
1. Use the validates_format_of and regex to only validate names that consist of letters (no numbers or symbols) and start with a capital letter
	validates :first_name, :last_name, format: { with: /[A-Z]\w*/,
    message: "only allows letters" }
end 
	validates_format_of (pass in :with /[A-Z]\w*/

2. Write a custom validation to ensure that no one named Delmer Reed, Tim Licata, Anil Bridgpal or Elie Schoppik is included in the students table

```
FORBIDDEN_USERNAMES = ["Reed", "Licata", "Bridgpal", "Schoppik""]
validate :last_name_is_allowed

def username_is_allowed
    if FORBIDDEN_USERNAMES.include?(username)
        errors.add(:last_name, "this is a restricted username")
    end
end
```
