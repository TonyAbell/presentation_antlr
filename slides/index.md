- title : ANTLR
- description : Introduction to ANTLR
- author : Tony Abell
- theme : night
- transition : default

***

### What is ANTLR?

***

### Motivation

Manage meta data of Feeder SQL Tables

---

## Meta

* Table Name
* Column Name
* Primary Key
* User ID
* Text Field
* Skip Column

---

## Example: Table | Column Name

    'tableA'
        columnA
        columnB
    'tableA'
        columnA
        columnB

---

## Example: User ID

    'tableA'
        *columnA
        columnB

---

## Example: Text Field

    'tableA'
        ^columnA
        columnB


---

## Example: Skip

    'tableA'
        !columnA
        columnB

---

## Example: Primary Key

    'tableA'
        @columnA
        columnB


---

## Example: All

    'User'
        @*UserId      // PK, User ID
        !^UserName    // Text, Skip
        *Manager_Id   // User ID  
        ^Status       // Text

***

### Gramer for DLS

    grammar Test;

    tables 	: table+ ;
    table 	: name cln+ ;
    name    : '\'' ID '\'' ;
    cln     : att ID ;
    att     : (SKIP | USERID | TEXT)* | ;

    ID      : [a-zA-Z0-9]+ ;
    SKIP    : [!];
    USERID  : [*];
    TEXT    : [^];

---

### Compile To Markdown

    'tableA'
    ------------
        !columnA
        columnB


---

### Compile To SQL

        /*
          Table Name:		  tableA
          Primary Keys:		columnB
          Parameter:
        */
        select columnB from tableA

---

### Compile To SQL

        /*
          Table Name:		  tableA
          Primary Keys:		columnB
          Parameter:	    Change Tracking Version
        */
        select columnB from
        from changetable(changes tableA,?) c
        left join sys.dm_tran_commit_table tct on
            c.sys_change_version = tct.commit_ts
        left join tableA t with (nolock) on  
            t.[columnB] = c.[columnB]


***

### Resources

* [ANTLR 3 Tutorial](http://javadude.com/articles/antlr3xtut/)
* [v4 Grammars](https://github.com/antlr/grammars-v4)
* [Wiki](http://en.wikipedia.org/wiki/ANTLR)
* [ANTLR By Example](http://www.alittlemadness.com/2006/06/05/antlr-by-example-part-1-the-language/)
* [ANTLR 4 / Maven Tutorial](http://www.alexecollins.com/antlr4-and-maven-tutorial/)
