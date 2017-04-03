**Project Description**
Generate your CRUD Stored procedures. Write your own custom stored procedrues. Now, generate generate a complete data access layer that is Data Bindable.

* Generate basic CRUD stored procedures.
* Call any stored procedure from a .NET applications without manually writting ADO.NET. 
* Have the compiler check your code instead of putting everything in strings.
* Incorporate your custom stored procedures.
* Generate data objects for OO and Data Binding.
## Click here for a seriese of articles on this project: 
[ZacksFiasco - Build a complete Stored Procedure based Data Access Layer using Code Generation](http://Blog.ZacksFiasco.com/2007/12/build-complete-stored-procedure-based.html)
* [Part 1: Intro to Code Generation and Generating CRUD Stored procedures](http://Blog.ZacksFiasco.com/2007/12/build-complete-stored-procedure-based.html)
* [Part 2: Modifying the script to use OUTPUT clause for Computed Columns](http://Blog.ZacksFiasco.com/2008/01/build-complete-stored-procedure-based.html)
* Part 3: Generating a C# DAL and Consuming Meta-Data
* Part 4: Using the DAL in projects
Call any stored procedure. 

Generate your stored procedures or write them yourself in TSQL:
{{
-- <MetaData entityName="Table1" commandType="Insert"/>
CREATE PROCEDURE [api](api).[Table1Insert](Table1Insert)
(
	@Table1Id uniqueidentifier,
	@Col1 varchar(50),
	@Col2 int = NULL,
	@rowversion timestamp = NULL OUTPUT
)
AS
BEGIN
	SET NOCOUNT ON
	

	INSERT INTO [dbo](dbo).[Table1](Table1)
	(
		[Table1Id](Table1Id),
		[Col1](Col1),
		[Col2](Col2)
	)
	VALUES
	(
		@Table1Id,
		@Col1,
		@Col2
	)
	
	RETURN @@Error
END }}

Call it from C# like this:
{{
using(DataAccess da = new DataAccess("MyConnectionStringName"))
{
    // call it like this
    byte[]() rowversion = null;
    Guid? table1id = Guid.NewGuid();

    da.Table1Insert(table1id, "blah", 3, ref rowversion);

    // or like this
    Table1Obj t1 = new Table1Obj();

    t1.Table1Id = Guid.NewGuid();
    t1.Col1 = "blah2";
    t1.Col2 = 123;
    
    da.InsertTable1(t1);

    // select like this
    IDataReader reader = null;
    da.Table1SelectAll(ref reader);

    while(reader.Read())
    {
        Console.WriteLine("ID={0}", (Guid)reader["Table1Id"](_Table1Id_));
    }
    reader.Dispose();

    // or like this
    List<Table1Obj> list = da.SelectObjTable1All();

    foreach(Table1Obj obj in list)
    {
        Console.WriteLine("obj.Table1Id = {0}", obj.Table1Id);
    }
} }}
This project uses [MyGeneration](http://www.mygenerationsoftware.com/) scripts and other tools to create a complete DAL.


