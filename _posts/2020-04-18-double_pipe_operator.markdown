---
layout: post
title:      "Double pipe operator"
date:       2020-04-19 03:12:51 +0000
permalink:  double_pipe_operator
---


In ruby operators, these are conditional assignments such as ||= I see the first time. What is it?

= is an assignment operator

|| is "double-pipe" which represents "OR"

so what could ||= "double-pipe-equal" be?

a += b translates to a = a + b

a ||= b can be considered to translates to a || a = b buta ||= b is entirely different ideas and are implemented entirely differently. 
Actually a ||= b is equal to a = a || b

It means if a is undefined or false, then evaluateb  and set toa the result. If the left-hand side of the || comparison is true, there's no need to check the right-hand side.
Here are examples;

```
a = nil
b = 20
a = b
a # => 20
```

The code above, a is set to nil and b is assigned to a. If we use double-pipe equal below

```
a = nil
b = 20
a ||= b
a # => 20
```

the result is the same.
When a is set to the true value, a is overwritten by b.

```
a = 2
b = 20
a = b
a # => 20
```

However, using a ||= b, a can keep its original value.

```
a = 2
b = 20
a ||= b
a # => 2
```

||= is useful when we want to add values to the key that is already existing. The following code is what we achieved without ||=.

```
class School
attr_writer :add_student, :grade, :sort

def initialize(school_name)
@name = school_name
@roster = {}
end

def add_student(student_name, grade)
@student_name = student_name
@grade = grade

if @roster.include? (@grade)
@roster[@grade] << @student_name
else
@roster[@grade] = [] 
@roster[@grade] << @student_name
end
end

def roster
@roster
end

end
```


With ||=, the code will be ;

```
class School
attr_writer :add_student, :grade, :sort

def initialize(school_name)
@name = school_name
@roster = {}
end

def add_student(student_name, grade)
@student_name = student_name
@grade = grade
@roster[@grade] ||= [] 
@roster[@grade] << @student_name
end

def roster
@roster
end

end
```

It will cut the conditional

```
if @roster.include? (@grade)
@roster[@grade] << @student_name
else
```
