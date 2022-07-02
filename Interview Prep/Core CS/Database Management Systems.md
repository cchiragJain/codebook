# Database Management Systems

**DBMS :** A database management system is a collection of software that provides you a quick way to access and modify the data. Ex. MySQL, Oracle.

## Evolution and Advantages of DBMS

The history of DBMS evolved in three primary phases:

- **File-based System :** In the file-based system we needed to write the user code to create, delete & update the files which will access the file system. This leads to a lot of issues like **redundancy(copies at a lot of places), inconsistent, difficult to access data(need to remember the exact location), conurrency issues(can't be accessed by multiple users at the same time).**
- **Relational DBMS :** Relational database means the data is stored as well as retrieved in the form of relations (tables). Each table has a predefined schema and data is retreived using SQL.

- **NoSQL :** Using these we don't need to define complex table like structures for everything.

## ER Model

Before using a database we need to design it.

ER Model is used to model the logical view of the system from the data perspective which consists of these components:

- **Entity :** An **Entity** may be an object with a physical existence - a particular person, car, house, or employee - or it may be an object with a conceptual existence - a company, a job, or a university course.

- **Entity Type :** An Entity is an object of **Entity Type** and set of all entities is called as **Entity Set**. . e.g.; E1 is an entity having Entity Type Student and set of all students is called Entity Set.

    <img src="Database%20Management%20Systems.assets/image-20220701132200013.png" alt="image-20220701132200013" style="zoom:50%;" />

- **Attributes :**  Attributes are the **properties which define the entity type**. For example, Roll_No, Name, DOB, Age, Address, Mobile_No are the attributes which defines entity type Student.

    <img src="Database%20Management%20Systems.assets/image-20220701132241830.png" alt="image-20220701132241830" style="zoom:67%;" />

    - **Composite Attribute : ** Contains many other attributes. Ex. name(first name, last name, middle name), address. 

        <img src="Database%20Management%20Systems.assets/image-20220701132713678.png" alt="image-20220701132713678" style="zoom:67%;" />

    - **Key Attribute -** The attribute which **uniquely identifies each entity** in the entity set is called key attribute.For example, Roll_No will be unique for each student. In ER diagram, key attribute is represented by an oval with underlying lines.

        <img src="Database%20Management%20Systems.assets/image-20220701133035398.png" alt="image-20220701133035398" style="zoom:67%;" />

    - **Multivalued Attribute -** An attribute consisting **more than one value** for a given entity. For example, Phone_No (can be more than one for a given student). In ER diagram, multivalued attribute is represented by double oval.

        <img src="Database%20Management%20Systems.assets/image-20220701133104644.png" alt="image-20220701133104644" style="zoom:67%;" />

    - **Derived Attribute -** An attribute which can be **derived from other attributes** of the entity type is known as derived attribute. e.g.; Age (can be derived from DOB). In ER diagram, derived attribute is represented by dashed oval.

        <img src="Database%20Management%20Systems.assets/image-20220701133423322.png" alt="image-20220701133423322" style="zoom:67%;" />

​	Ex. entity type Student can be : **(roll no, should have an underline(since key attribute))**

<img src="Database%20Management%20Systems.assets/image-20220701133459810.png" alt="image-20220701133459810" style="zoom: 80%;" />

- **Relationship Type :** A relationship type represents the **association between entity types**. For example,‘Enrolled in’ is a relationship type that exists between entity type Student and Course(mostly a verb). In ER diagram, relationship type is represented by a diamond and connecting the entities with lines.

    <img src="Database%20Management%20Systems.assets/image-20220701150418034.png" alt="image-20220701150418034" style="zoom: 80%;" />

    

### Degree of Relationship Set

- **Unary relationship : **When there is only entity set in the relation

    - Ex, a student is a friend of itself

        <img src="Database%20Management%20Systems.assets/image-20220701152141660.png" alt="image-20220701152141660" style="zoom:50%;" />

    - A person is married to a person

        <img src="Database%20Management%20Systems.assets/image-20220701152203677.png" alt="image-20220701152203677" style="zoom:67%;" />

    - An employee manages an employee

- **Binary Relationship(most common) :** When two entity set participate in a realtion.

    - Ex, a student attends a course

        <img src="Database%20Management%20Systems.assets/image-20220701152331187.png" alt="image-20220701152331187" style="zoom:67%;" />

    - A supplier supplies item

        <img src="Database%20Management%20Systems.assets/image-20220701152357875.png" alt="image-20220701152357875" style="zoom: 67%;" />

- **n-ary relationship(not common) :**  When there are n entities set participating in a relationship.

    - Ex, a proffesor, a student, a project is related to a project_guide.

        <img src="Database%20Management%20Systems.assets/image-20220701155340525.png" alt="image-20220701155340525" style="zoom:67%;" />

### Cardinality

The **number of times an entity of an entity set participates in a relationship** set is known as cardinality. Types : 

- **One to One :** When each entity in each entity set can take part **only once in the relationship**, the cardinality is one to one.

    - Ex, a driver can drive one car and a car can be driven by one driver.

    <img src="Database%20Management%20Systems.assets/image-20220701155836941.png" alt="image-20220701155836941" style="zoom:67%;" />

- **One to Many -** When entities in one entity set **can take part only once in the relationship set and entities in other entity sets can take part more than once in the relationship set,** cardinalities is one to many. Many to one also come under this category.

    - ex, a professor can teach many courses

        <img src="Database%20Management%20Systems.assets/image-20220701160658180.png" alt="image-20220701160658180" style="zoom: 67%;" />

    - ex, many employees can work for single department (many to one)

        <img src="Database%20Management%20Systems.assets/image-20220701160730627.png" alt="image-20220701160730627" style="zoom:67%;" />

- **Many to many -** When entities in all entity sets can **take part more than once in the relationship** cardinality is many to many.

    - ex, any number of students can study any number of subjects

        <img src="Database%20Management%20Systems.assets/image-20220701160838222.png" alt="image-20220701160838222" style="zoom:67%;" />

    - ex, customer can orders any products and any products can be ordered by any number of customers.

        <img src="Database%20Management%20Systems.assets/image-20220701160915208.png" alt="image-20220701160915208" style="zoom:67%;" />

### Participation Constraint

**Participation Constraint:** Participation Constraint is applied on the entity participating in the relationship set.

- **Total Participation -** Each entity in the entity set **must participate** in the relationship. Total participation is shown by a double line in ER diagram.

    - ex, if each student must attend a course then the participation of each student entity in the entity set will be total.

        <img src="Database%20Management%20Systems.assets/image-20220701164331915.png" alt="image-20220701164331915" style="zoom:67%;" />

    - ex, each employee entity must join department.

        <img src="Database%20Management%20Systems.assets/image-20220701164358288.png" alt="image-20220701164358288" style="zoom:67%;" />

- **Partial Participation -** The entity in the entity set **may or may NOT participat**e in the relationship. Partial is represented using the single line only.

    - ex, if some courses are not enrolled by any of the student, the participation of course will be partial. The student entity set has total participation and the course entity set has partial participation.

        <img src="Database%20Management%20Systems.assets/image-20220701164543232.png" alt="image-20220701164543232" style="zoom:67%;" />

### Weak Entitiy Set

- There are some entity type for which a key attribute can't be defined. They are weak entity type and are represented by double rectangle boxes. 

- **The participation of a weak entity type is always total.** 
- **The relationship between a weak entity type and its identifying strong entity type is called an identifying relationship** and it is represented by a double diamond.

ex 1 :  A school might have multiple classes and each class might have multiple sections. The section cannot be identified uniquely and hence they do not have any primary key (each class will have the same name of sections ). Though a class can be identified uniquely and the combination of a class and section is required to identify each section uniquely. Therefore the section is a weak entity and it shares total participation with the class which is the strong entity type.

<img src="Database%20Management%20Systems.assets/image-20220701164954406.png" alt="image-20220701164954406" style="zoom:67%;" />

## Keys(candidate, super, primary, composite)

Example suppose you have such a table

<img src="Database%20Management%20Systems.assets/image-20220702154108159.png" alt="image-20220702154108159" style="zoom:67%;" />

- **Candidate Key :** The minimal set of attribute that can uniquely identify a row is known as a candidate key.
    - Ex, Enroll no, Aadhaar no, pan no will be the candidate keys in this example since they all are unique to the person. 
    - Address, phone number can not be a candidate key in case of a Student Table since there could be siblings studying in the same school.
- **Primary Key :** There can be more than one candidate key in relation out of which one and the most suitable must be chosen as the primary key. For Example, Aadhaar No, as well as PAN No both, are candidate keys for relation Student but Enroll No seems to be most appropriate to be chosen as the primary key(only one out of many candidate keys). **Since primary key needs to be minmal**
- **Alternate Key :** The candidate keys other than the primary key is called an alternate key. 
- **Super Key :**  The set of attributes that can uniquely identify a tuple is known as Super Key. For Example, (Enrollment No, Student Name) can be used to identify a row etc.
    - **Adding zero or more attributes to candidate key generates super key.**
    - A candidate key is a superkey but vice versa is not true.

### How to identify Candidate Keys?

Using the Armstrong Axioms we can find candidate keys in a given funcitonal dependency. What we basically do is find the closure(what a particular attribute can determine).

**Armstrong Axioms :** 

- **Transitivity : ** 
    $$
    FD = \{A\ \rightarrow\ B,B\ \rightarrow\ C,C\ \rightarrow D\}
    \\
    \text{A is determing B, B is determining C using transitive property A should determine C}
    \\
    \text{Therefore, } A^+ = \{B,C\} \text{ (using transitive property)}
    \\
    \text{since C is also determing D therefore A determines D }
    \\
    \text{Therefore, } A^+ = \{B,C,D\} \text{ (using transitive property)}
    $$
     

- **Reflexive :**
    $$
    \text{Since A also determines D(using reflexive property)}
    \\
    A^+ = \{A,B,C,D\}
    $$
    Therfore A is a candidate key since it can determine all the other attributes of its table.

- **Augmentative :** If $X \rightarrow Y \text{ then } XZ\rightarrow YZ$ 

### Foreign Keys

This is a field in a table which uniquely identifies each row of another table. This field points to the primary key of another table and creates a kind of link between the tables.

<img src="Database%20Management%20Systems.assets/image-20220702171238565.png" alt="image-20220702171238565" style="zoom:80%;" />

Here The Order Table has Customer_ID as the foreign key which establishes a relation between the Order and Customer Table. The Customer_ID is the primary key for the Customer Table. This makes the work of access very easy and reliable. Suppose one needs to find the details about the customer who ordered an item of Rs. 2000 on 15-06-2019. Then we just need to get the Customer_ID from the Order Table based on the given conditions and then look up the Customer Table to get all the required details.

#### Referential Integrity

Referential integrity is a relational database concept, which states that table relationships must always be consistent. In other words, any foreign key field must agree with the primary key that is referenced by the foreign key. Thus, any primary key field changes must be applied to all foreign keys, or not at all. The same restriction also applies to foreign keys in that any updates (but not necessarily deletions) must be propagated to the primary parent key.

For example, suppose Table B has a foreign key that points to a field in Table A. Referential integrity would prevent you from adding a record to Table B that cannot be linked to Table A. In addition, the referential integrity rules might also specify that whenever you delete a record from Table A, any records in Table B that are linked to the deleted record will also be deleted. This is called cascading delete. Finally, the referential integrity rules could specify that whenever you modify the value of a linked field in Table A, all records in Table B that are linked to it will also be modified accordingly. This is called a cascading update.

## Database Normalisation

