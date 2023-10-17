### YAML == YAML Ain't Markup Language

- data format used to exchange data
- Similar to XML & JSON
- You can store only data, and not commands.
- Case Sensitive
- use yaml parser to avoid error

### Data Serialisation :

- share a particular object and data in one single format
- other data serialization languages : 1. XML 2. JSON


```yml
# Key Datatype :
- "apple" : "I am a red fruit"
- 1 : "this is kunal"

---     -> used to seperate documents
# List Datatype
- apple
- banana
---
# Block Style
- new delhi
- mumbai
- gujrat
---
# Euivalent to above block style but in single line
cities: [new delhi, mumbai]
---
{mango:"yellow", age: 56}

# String Variables
myself: Pushkar Mishra

# Multi-line
bio : |
hey i am yo
i am lllllllllll

# write a single line in multi line
message: >
this will
all be
in one single line

# Datatypes
number: 5432
marks: 98.76
bool: No #n,N,false,False,FALSE
#same for true -> yes,y,Y

#Specify the type
zero: !!int 0
positiveNum: !!int 45
negativeNum: !!int -45
binaryNum: !!int 0b11001
octalNum: !!int 06574
hexa: !!int 0x45
commaValue: !!int +540_000 # 540,000
exponential numbers: 6.023E56

# floating point numbers
marks: !!float 56.89
infinite: !!float .inf
not a num: .nan

# similarly for !!bool !!str
# null
surname: !!null Null # or null NULL ~
~: this is a null key

# dates and time
date: !!timestamp 2002-12-14
india time: 2001-12-15T02:59:43.10 +5:30
no time zone: 2001-12-15T02:59:43.10


# Advanced Datatypes :
student: !!seq
marks
- name
- roll_no
# like this also
cities: [new delhi, mumbai]
# some of the keys of the seq will be empty
# sparse seq
sparse seq:
- hey
- how
- 
- NULL
- sup


# nested sequence
- 
	- mango
	- apple
	- banana
-
	- marks
	- roll num
	- date


# Maps Datatype
#key
!!map

#nested mappings: map within an map
name: PM
role:
	age: 45
	job: student

# same as
name: PM
role: {age: 45, job: student}

# pair
pair example: !!pairs
- job: student
- job: teacher

pair example: !!pairs[job:student,job:teacher]
# set - allow only unique values
names: !!set
	? Kunal
	? PM

# dictionary !!omap
people: !!omap
	- Kunal:
		name: KK
		age: 24
		height: 2
	- PM:
		name: Pushkar
		age: 18
		height: 3


# Reusing properties with anchors
likings: &likes
	fav: orange
	dislike: junk

person1:
	name: PM
	<<: *likes

# equivalent to 
person1:
	name: PM
	fav: orange
	dislike: junk

# you overide too
person1:
	name: PM
	<<: *likes
	dislike: pizza
```


### Tools to prevent k8s misconfigurations
- datree
- monokle(by kubeshop)
- Lens

