---
title: File I/O and Sorting
tags: 
 - C#
 - console
 - StreamReader
 - Sort()
 - Bubble sort algorithm
description: How to read a datafile and sorting and comparing between the bubble sort algorithm and sort() method of List class
---

# Read a data file and sorting

This article will show you how to read text files and sort data from them. There are several methods for aligning data, however solutions by using Bubble Sort and List's Sort() method were described.
    
### Comparing process speed regarding sort algorithm(BigO notaion)
According to [docs.microsoft](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1.sort?view=net-6.0), List.sort() uses Array.Sort, which uses the introspective sort in the following manner:

 1. It is using an insertion sort algorithm if the partition size is less than or equal to 16 elements.

 2. It uses a Heapsort method if the number of partitions exceeds 2 log n, where n is the range of the input array.

 3. Otherwise, a Quicksort algorithm is used.

 |Sort|Time|Space|Stable|
 |---|---|---|---|
 |Bubble sort|O(n^2)|Constant|Stable|
 |Heap Sort|O(n*log(n))|Constant|Instable|
 |QuickSort|O(n*log(n))|Constant|Stable|

**To summarise, it is more efficient to using List.Sort(), which uses a variety of algorithms in some cases.**

Bubble Short is simple to implement but inefficient due to its O(n2) time complexity.

Heap Sort has an O(nlogn) time complexity, which is fast and doesn't require any additional memory. It is, however, a little slower than other alignments and does not guarantee stability, depending on the quality of the data.

Quick Sort is implemented by dividing by reference value and has a time complexity of O(nlogn) in the best case and O(nn) in the worst case.

# The approach the Bubble Sort
## Contents(employees.txt)
```
Bruce Wayne,    123456, 25.88, 35.50
Clark Kent,     232344, 25.88, 38.75
Diana Prince,   657659, 27.62, 30.25
Hal Jordan,     989431, 23.14, 44.25
Barry Allan,    342562, 25.12, 25.50
Arthur Curry,   565603, 21.09, 23.75
John Jones,     812984, 18.99, 32.75
Dinah Lance,    342988, 18.99, 34.50
Oliver Queen,   340236, 17.45, 41.25
Ray Palmer,     120985, 24.75, 40.00
Ronald Raymond, 239824, 16.43, 35.00
Carter Hall,    657123, 19.34, 42.75
Shayera Hol,    761742, 16.73, 38.50
```

## Employee.cs

```c#
public Employee()
{
}
/// <summary>
/// Four-argument constructor for Employee
/// </summary>
/// <param name="name">Employee name</param>
/// <param name="number">Employee number</param>
/// <param name="rate">Hourly rate of pay</param>
/// <param name="hours">Hours worked in a week</param>
public Employee(string name, int number, decimal rate, double hours)
{
    /* This is the better way to set data in a class - use the accessor methods.
    * That way, if their implementation changes, the constructor doesn't need to
    * be edited as well.
    */
    SetName(name);
    SetNumber(number);
    SetRate(rate);
    SetHours(hours);

    gross = GetGross();
}

// The following Get methods allow for private data retrieval - in the future we'll use properties

/// <summary>
/// Gross pay get method
/// </summary>
/// <returns>Gross pay for the week</returns>
public decimal GetGross() => (hours < 40) ? (decimal)hours * rate : (40.0m * rate) + (((decimal)hours - 40.0m) * rate * 1.5m);

/// <summary>
/// Hours get method
/// </summary>
/// <returns>Hours worked this week</returns>
public double GetHours() => hours;

/// <summary>
/// Name get method
/// </summary>
/// <returns>Employee name</returns>
public string GetName() => name;

/// <summary>
/// Number get method
/// </summary>
/// <returns>Employee number</returns>
public int GetNumber() => number;

/// <summary>
/// Rate get method
/// </summary>
/// <returns>Hourly rate of pay</returns>
public decimal GetRate() => rate;

/// <summary>
/// Employee display method - in the future, we'll override the ToString method of Object
/// </summary>
public override String ToString() => $"{GetName(),-20}  {GetNumber():D5}  {GetRate(),6:C}  {GetHours():#0.00}  {GetGross(),9:C}";

// The following Set methods allow for private data modification - in the future we'll use properties

/// <summary>
///  Hours set method
/// </summary>
/// <param name="hours">Hours worked this week</param>
public void SetHours(double hours) => this.hours = hours;

/// <summary>
/// Name set method
/// </summary>
/// <param name="name">Employee name</param>
public void SetName(string name) => this.name = name;

/// <summary>
/// Number set method
/// </summary>
/// <param name="number">Employee number</param>
public void SetNumber(int number) => this.number = number;

/// <summary>
/// 
/// </summary>
/// <param name="rate">Hourly rate of pay</param>
public void SetRate(decimal rate) => this.rate = rate;

}
```
## Main.cs (Using bubble sort algorithm)
```c#
class Main
{
    /// <summary>
    /// The main method reads the data file, populates the Employee array and provides a menu of sort options.
    /// </summary>
    /// <param name="args">Command line arguments are not used in this program</param>
    static void Main(string[] args)
    {
        int count;                                      // Keep track of how many employees are instantiated
        bool loop = true;                               // A loop control variable
        string input;                                   // The user's menu option pick as a string
        int choice;                                     // The user's menu option pick as an integer
        Employee[] employees;                           // The array of Employee objects

        // Read the data file to build the Employee array and find out how many there are
        Read(out employees, out count);

        // Keep the menu loop running so the user can sort several times
        while (loop)
        {
            // Display the menu and retrieve the user's choice
            input = Menu();

            // Based on the user's entry, sort using the appropriate option
            if (Int32.TryParse(input, out choice))
            {
                switch (choice)
                {
                    // Sort by employee name - ascending
                    case 1:
                        Sort(employees, count, 1);
                        break;

                    // Sort by employee ID number - ascending
                    case 2:
                        Sort(employees, count, 2);
                        break;

                    // Sort by hourly rate - descending
                    case 3:
                        Sort(employees, count, 3);
                        break;

                    // Sort by weekly hours - descending
                    case 4:
                        Sort(employees, count, 4);
                        break;

                    // Sort by gross pay - descending
                    case 5:
                        Sort(employees, count, 5);
                        break;

                    // Exit the program
                    case 6:
                        loop = false;
                        break;

                    // Trap invalid selections to try again
                    default:
                        Console.WriteLine("\n*** Invalid Choice - Try Again ***\n");
                        break;
                }

                // Display the table when a valid choice is made, otherwise display an error
                if (choice >= 0 && choice <= 5)
                    DisplayTable(employees, count);
            }
            else
                Console.WriteLine("\n*** Invalid Choice - Try Again ***\n");
        }
    }

    /// <summary>
    /// This method displays the entire table, including column headers
    /// </summary>
    /// <param name="employees">The array of employees</param>
    /// <param name="count">How many employees are in use</param>
    private static void DisplayTable(Employee[] employees, int count)
    {
        Console.Clear();
        Console.WriteLine("Employee              Number    Rate  Hours  Gross Pay           Nick's Company");
        Console.WriteLine("====================  ======  ======  =====  =========           --------------");

        // Display each employee in the array
        for (int i = 0; i < count; i++)
          Console.WriteLine(employees[i]); 
        Console.WriteLine();
    }

    /// <summary>
    /// This method displays the menu options to the user and returns their selection
    /// </summary>
    /// <returns>The user's menu selection</returns>
    private static string Menu()
    {
        Console.WriteLine("1. Sort by Employee Name");
        Console.WriteLine("2. Sort by Employee Number");
        Console.WriteLine("3. Sort by Employee Pay Rate");
        Console.WriteLine("4. Sort by Employee Hours");
        Console.WriteLine("5. Sort by Employee Gross Pay");
        Console.WriteLine("\n6. Exit");
        Console.Write("\nEnter choice: ");

        return Console.ReadLine();
    }

    /// <summary>
    /// This method reads the data file and stores all of the employee information in an array of Employees
    /// </summary>
    /// <param name="employees">The array of employees</param>
    /// <param name="count">How many employees are in use</param>
    private static void Read(out Employee[] employees, out int count)
    {
        count = 0;                                    // The current number of employees
        string input;                                 // One line of data read from the file
        employees = new Employee[100];                // The Employee array

        try
        {
            // Open the file for reading purposes
            FileStream file = new FileStream("employees.txt", FileMode.Open, FileAccess.Read);
            StreamReader reader = new StreamReader(file);

            // As long as there is data in the file, keep processing 
            // Each employee record is comma separated, so explode each piece into an array,
            // create a new employee object and increment the count
            while ((input = reader.ReadLine()) != null)
            {
                string[] exploded = input.Split(',');
                employees[count] = new Employee(exploded[0], int.Parse(exploded[1]), decimal.Parse(exploded[2]), double.Parse(exploded[3]));
                count++;
            }

            reader.Close();                             // Always good form to close the file
        }

        // Just in case the file can't be found - graceful exit
        catch (IOException e)
        {
            Console.WriteLine("*** File is empty - Program Aborting ***\n");
            Environment.Exit(1);
        }
    }

    /// <summary>
    /// This method uses a Bubble Sort to rearrange the Employee array in order.  Since there are 
    /// five possible ways to sort, the outer loops remain the same and instead the condition is
    /// different for each field to sort on.
    /// </summary>
    /// <param name="emps">The array of employees</param>
    /// <param name="size">How many employees are in use</param>
    /// <param name="choice">The user selected sort choice</param>
    public static void Sort(Employee[] emps, int size, int choice)
    {
        for (int a = 0; a < size - 1; a++)
            for (int b = 0; b < size - 1; b++)
                switch (choice)
                {
                    case 1:  // Sort by employee name - ascending
                        if (emps[b].GetName().CompareTo(emps[b + 1].GetName()) > 0)
                            Swap(ref emps[b], ref emps[b + 1]);
                        break;

                    case 2:  // Sort by employee ID - ascending
                        if (emps[b].GetNumber() > emps[b + 1].GetNumber())
                            Swap(ref emps[b], ref emps[b + 1]);
                        break;

                    case 3:  // Sort by hourly rate -descending
                        if (emps[b].GetRate() < emps[b + 1].GetRate())
                            Swap(ref emps[b], ref emps[b + 1]);
                        break;

                    case 4:  // Sort by weekly hours - descending
                        if (emps[b].GetHours() < emps[b + 1].GetHours())
                            Swap(ref emps[b], ref emps[b + 1]);
                        break;

                    case 5:  // Sort by gross pay - descending
                        if (emps[b].GetGross() < emps[b + 1].GetGross())
                            Swap(ref emps[b], ref emps[b + 1]);
                        break;
                }
    }

    /// <summary>
    /// This method is a utility method for the Bubble Sort.  It just swaps two elements in the array
    /// </summary>
    /// <param name="a">First employee</param>
    /// <param name="b">Second employee</param>
    private static void Swap(ref Employee a, ref Employee b)
    {
        Employee temp;                              // A temporary holding spot for an employee

        temp = a;
        a = b;
        b = temp;
    }
}
```

# The approch Sort() method of the List < T > collection
## I used a lambda expression for sotring instead of the IComparable techique.

## Employee.cs
```c#
public class Employee
    {
        public string Name{ get; set;}                  // The employee name
        public string Number { get; set; }              // The employee number(Id)
        public decimal Rate { get; set; }               // The hourly rate
        public double Hours { get; set; }               // The weekely hours
        public double OvertimeThreadhold = 40;          // The standard working hours a week
        public double OvertimeRatio = 1.5;              // Overtime pay ratio

        /// <summary>
        /// This is Calculated as rate of pay * hours worked and after 40hours overtime is at time and a half.
        /// </summary>
        public decimal Gross                            
        {
            get
            {
                double overtime = Hours - OvertimeThreadhold;           // Calculate the overtime
                
                if (overtime > 0)
                {
                    return ((decimal)overtime * Rate * (decimal)OvertimeRatio) + (decimal)OvertimeThreadhold * Rate;
                }
                else
                {
                    return Rate * (decimal)Hours;
                }
            }
        }

        /// <summary>
        /// This method will read the employee.txt data file and stores the information in an array of Employee objects
        /// </summary>
        /// <param name="employeeData">Using StreamReader class, it will have a information of Employee one by one</param>
        /// <param name="delimiter">Split by delimeter</param>
        /// <returns>Return Employee's object</returns>
        public static Employee Parse(string employeeData, char delimiter = ',')
        {
            try
            {
                string[] inputData = employeeData.Split(delimiter);
                Employee employee = new Employee();

                employee.Name = inputData[0];
                employee.Number = inputData[1];
                employee.Rate = decimal.Parse(inputData[2]);
                employee.Hours = double.Parse(inputData[3]);

                return employee;
            }
            catch(Exception e)
            {
                Console.WriteLine(e.Message);
                return null;
            }

        }
        /// <summary>
        /// Override the ToString method of object
        /// </summary>
        /// <returns></returns>
        public override string ToString()
        {
            return $"{Name, 20}{Number, 15}{Rate, 15:c2}{Hours, 15:n2}{Gross, 15:c2}";
        }

    }
```

## Main.cs (Using Sort() method)
```c#
class Main
    {
        /// <summary>
        /// The main method reads the data file, populates the Employee array and provides a menu of sort options.
        /// </summary>
        /// <param name="args"></param>
        static void Main(string[] args)
        {
            // Open the file for reading purpose
            StreamReader sr = new StreamReader("employees.txt");

            // The List of Employee objects
            List<Employee> Employees = new List<Employee>();
            string[] options = { "Sort by Employee Name", "Sort by Employee Number", "Sort by Employee Pay Rate", "Sort by Employee Hours", "Sort by Employee Gross Pay", "Exit" };
            while (!sr.EndOfStream)
            {
                Employee employee = Employee.Parse(sr.ReadLine());
                if(employee != null)
                {
                    Employees.Add(employee);
                }
            }
            sr.Close();

            // Keep the menu loop running so the can sort several times.
            int input = DisplayMenu(options);
            while (input != 6)
            {
                Console.Clear();
                switch (input)
                {
                    //Usig a lambda expression for sorting instead of the IComparable technique.
                    case 1:
                        Employees.Sort((Employee x, Employee y) => x.Name.CompareTo(y.Name));        //Sort by Employee name(ascending)
                        DisplayList(Employees);
                        break;
                    case 2:
                        Employees.Sort((Employee x, Employee y) => x.Number.CompareTo(y.Number));    //Sort by Employee number(ascending)
                        DisplayList(Employees);
                        break;
                    case 3:
                        Employees.Sort((Employee x, Employee y) => y.Rate.CompareTo(x.Rate));        //Sort by Employee rate(descending)
                        DisplayList(Employees);
                        break;
                    case 4:
                        Employees.Sort((Employee x, Employee y) => y.Hours.CompareTo(x.Hours));       //Sort by Employee hours(descending)
                        DisplayList(Employees);
                        break;
                    case 5:
                        Employees.Sort((Employee x, Employee y) => y.Gross.CompareTo(x.Gross));      //Sort by Employee gross(descending)
                        DisplayList(Employees);
                        break;
                    default:
                        input = DisplayMenu(options);
                        break;
                }
                input = DisplayMenu(options);
            }
        }


        /// <summary>
        /// This method displays the menu options to the user and returns their selection.
        /// </summary>
        /// <param name="options">menu options</param>
        /// <returns>Console.ReadLine()</returns>
        static int DisplayMenu(string[] options)
        {
            for(int i = 0; i < options.Length; i++)
            {
                Console.WriteLine($"{i + 1}. {options[i]}");
            }
            Console.WriteLine("\r\n\r\n");
            Console.Write("Enter option: ");

            return int.Parse(Console.ReadLine());
        }

        /// <summary>
        /// This method displays the entire table, including column headers.
        /// </summary>
        /// <param name="employees"></param>
        static void DisplayList(List<Employee> employees)
        {
            string heading = $"{"Name",20}{"Number",15}{"Rate",15:c2}{"Hours",15:n2}{"Gross",15:c2}";
            Console.WriteLine(heading);
            Console.WriteLine(new String('=', heading.Length));
            foreach (Employee e in employees)
            {
                Console.WriteLine(e);
            }
            Console.WriteLine("\r\n\r\n");
        }
    }
```
