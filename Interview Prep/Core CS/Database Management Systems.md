# Database Management Systems

**DBMS :** A database management system is a collection of software that provides you a quick way to access and modify the data. Ex. MySQL, Oracle.

## Evolution and Advantages of DBMS

The history of DBMS evolved in three primary phases:

- **File-based System :** In the file-based system we needed to write the user code to create, delete & update the files which will access the file system. This leads to a lot of issues like **redundancy(copies at a lot of places), inconsistent, difficult to access data(need to remember the exact location), conurrency issues(can't be accessed by multiple users at the same time).**
- **Relational DBMS :** Relational database means the data is stored as well as retrieved in the form of relations (tables). Each table has a predefined schema and data is retreived using SQL.

- **NoSQL : **Using these we don't need to define complex table like structures for everything.

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

    <img src="Database%20Management%20Systems.assets/image-20220701155836941.png" alt="image-20220701155836941" style="zoom:67%;" />

    
