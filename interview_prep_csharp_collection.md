# C# Interview Prep: Core Programs

## 1. Array

**Problem Statement:** Given marks of students in one test, find the topper, class average, failed students, and top 3 scores. This is a common interview-style use case because arrays are fixed-size, index-based, and fast for direct access.

```csharp
using System;
using System.Linq;

class ArrayDemo
{
	static void Main()
	{
		int[] marks = { 72, 88, 45, 91, 67, 91, 39 };

		Console.WriteLine("Marks: " + string.Join(", ", marks));
		Console.WriteLine("Total Students: " + marks.Length);
		Console.WriteLine("First Student Marks: " + marks[0]);

		// Update by index when revaluation changes a score.
		marks[2] = 50;
		Console.WriteLine("After Revaluation: " + string.Join(", ", marks));

		int topper = marks.Max();
		double average = marks.Average();
		int failedCount = marks.Count(mark => mark < 40);

		Array.Sort(marks);
		Array.Reverse(marks);

		Console.WriteLine("Topper Marks: " + topper);
		Console.WriteLine("Average Marks: " + average);
		Console.WriteLine("Failed Students: " + failedCount);
		Console.WriteLine("Top 3 Scores: " + string.Join(", ", marks.Take(3)));
	}
}
```

## 2. List

**Problem Statement:** Maintain an interview shortlist where candidates are added dynamically, one candidate is inserted for urgent referral, rejected candidates are removed, and final candidates are sorted and filtered.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class ListDemo
{
	static void Main()
	{
		List<string> candidates = new List<string> { "Amit", "Sneha", "Rahul" };

		candidates.Add("Priya");
		candidates.Insert(1, "Vikas"); // referral candidate gets priority slot

		Console.WriteLine("Shortlist: " + string.Join(", ", candidates));
		Console.WriteLine("Total Candidates: " + candidates.Count);
		Console.WriteLine("Rahul Applied: " + candidates.Contains("Rahul"));

		candidates.Remove("Sneha"); // rejected in screening
		candidates.RemoveAt(0);      // first candidate dropped out

		Console.WriteLine("After Updates: " + string.Join(", ", candidates));

		candidates.Sort();
		Console.WriteLine("Sorted Shortlist: " + string.Join(", ", candidates));

		List<string> pCandidates = candidates.Where(name => name.StartsWith("P")).ToList();
		Console.WriteLine("Candidates Starting With P: " + string.Join(", ", pCandidates));
	}
}
```

## 3. Dictionary

**Problem Statement:** Track product stock using product code as a unique key. Interviewers often ask this because dictionaries provide fast lookup, safe insert/update, delete, and key-value iteration.

```csharp
using System;
using System.Collections.Generic;

class DictionaryDemo
{
	static void Main()
	{
		Dictionary<string, int> stock = new Dictionary<string, int>();

		stock.Add("LAPTOP", 10);
		stock.Add("MOUSE", 25);
		stock["KEYBOARD"] = 15;
		Console.WriteLine("Total Products: " + stock.Count);

		string productCode = "MOUSE";
		if (stock.TryGetValue(productCode, out int quantity))
		{
			Console.WriteLine(productCode + " Stock: " + quantity);
		}

		if (stock.ContainsKey("LAPTOP"))
		{
			stock["LAPTOP"] -= 2; // order placed
		}

		stock["MONITOR"] = 8;   // insert using indexer
		stock.Remove("KEYBOARD");

		foreach (KeyValuePair<string, int> item in stock)
		{
			Console.WriteLine($"Product: {item.Key}, Available: {item.Value}");
		}
	}
}
```

## 4. Set / HashSet

**Problem Statement:** Compare two interview panels' selected candidate IDs. HashSet is ideal when uniqueness matters and interviewers ask about union, intersection, difference, and fast duplicate checks.

```csharp
using System;
using System.Collections.Generic;

class HashSetDemo
{
	static void Main()
	{
		HashSet<int> panelASelected = new HashSet<int> { 101, 102, 103, 104, 104 };
		HashSet<int> panelBSelected = new HashSet<int> { 103, 104, 105, 106 };

		panelASelected.Add(107);
		panelASelected.Remove(101);

		Console.WriteLine("Panel A Selected: " + string.Join(", ", panelASelected));
		Console.WriteLine("Panel A Count: " + panelASelected.Count);
		Console.WriteLine("Candidate 103 Selected By Panel A: " + panelASelected.Contains(103));

		HashSet<int> allSelected = new HashSet<int>(panelASelected);
		allSelected.UnionWith(panelBSelected);
		Console.WriteLine("All Unique Selected Candidates: " + string.Join(", ", allSelected));

		HashSet<int> selectedByBoth = new HashSet<int>(panelASelected);
		selectedByBoth.IntersectWith(panelBSelected);
		Console.WriteLine("Selected By Both Panels: " + string.Join(", ", selectedByBoth));

		HashSet<int> onlyPanelA = new HashSet<int>(panelASelected);
		onlyPanelA.ExceptWith(panelBSelected);
		Console.WriteLine("Only Panel A Selected: " + string.Join(", ", onlyPanelA));
	}
}
```

## 5. StringBuilder

**Problem Statement:** In an interview-style log sanitizer, build a user activity report efficiently and modify it using append, insert, replace, remove, and length operations.

```csharp
using System;
using System.Text;

class StringBuilderDemo
{
	static void Main()
	{
		string userName = "Aarav";
		string email = "aarav@example.com";
		int failedLoginCount = 3;

		StringBuilder report = new StringBuilder();

		// Append and AppendLine: build output without creating many string objects.
		report.AppendLine("User Activity Report");
		report.AppendLine("--------------------");
		report.Append("User: ").AppendLine(userName);
		report.Append("Email: ").AppendLine(email);
		report.Append("Failed Login Attempts: ").AppendLine(failedLoginCount.ToString());

		// Insert: add a priority tag at the beginning.
		report.Insert(0, failedLoginCount >= 3 ? "[HIGH RISK]\n" : "[NORMAL]\n");

		// Replace: mask sensitive data before displaying/logging.
		report.Replace(email, "a***@example.com");

		// Remove: remove internal separator line before sending to customer support.
		string separator = "--------------------\n";
		int separatorIndex = report.ToString().IndexOf(separator);
		if (separatorIndex >= 0)
		{
			report.Remove(separatorIndex, separator.Length);
		}

		Console.WriteLine(report.ToString());
		Console.WriteLine("Report Length: " + report.Length);
	}
}
```

## 6. String

**Problem Statement:** Process a text value using common string operations such as trim, case conversion, search, substring, split, join, and replace.

```csharp
using System;

class StringDemo
{
	static void Main()
	{
		string text = "  CSharp Interview Preparation  ";

		Console.WriteLine("Original: '" + text + "'");
		Console.WriteLine("String Length: " + text.Length);
		Console.WriteLine("Trim: '" + text.Trim() + "'");
		Console.WriteLine("Upper: " + text.ToUpper());
		Console.WriteLine("Lower: " + text.ToLower());
		Console.WriteLine("Contains Interview: " + text.Contains("Interview"));
		Console.WriteLine("StartsWith CSharp: " + text.Trim().StartsWith("CSharp"));
		Console.WriteLine("Substring: " + text.Trim().Substring(0, 6));

		string[] words = text.Trim().Split(' ');
		Console.WriteLine("Words: " + string.Join(" | ", words));

		string replaced = text.Replace("CSharp", "C#");
		Console.WriteLine("Replaced: " + replaced.Trim());
	}
}
```

## 7. Input and Output

**Problem Statement:** Read user input from the console, validate numeric values using `TryParse`, and display formatted output safely.

```csharp
using System;

class InputOutputDemo
{
	static void Main()
	{
		Console.Write("Enter your name: ");
		string name = Console.ReadLine();
		Console.WriteLine("Name Length: " + name.Length);

		Console.Write("Enter a character: ");
		char character = Console.ReadKey().KeyChar;
		Console.WriteLine("\nCharacter Entered: " + character);

		Console.Write("Enter your age: ");
		if (!int.TryParse(Console.ReadLine(), out int age))
		{
			Console.WriteLine("Invalid age entered.");
			return;
		}

		Console.Write("Enter salary: ");
		if (!double.TryParse(Console.ReadLine(), out double salary))
		{
			Console.WriteLine("Invalid salary entered.");
			return;
		}

		Console.WriteLine();
		Console.WriteLine("Output using concatenation: Name = " + name + ", Age = " + age);
		Console.WriteLine("Output using interpolation: Name = {0}, Age = {1}, Salary = {2}", name, age, salary);
		Console.WriteLine($"Output using string interpolation: {name} is {age} years old and earns {salary:C}");
	}
}
```

## 8. LINQ Important Queries / Clauses

**Problem Statement:** Query a collection of student records using LINQ clauses for filtering, projection, sorting, grouping, aggregation, and pagination.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Student
{
	public int Id { get; set; }
	public string Name { get; set; }
	public int Age { get; set; }
	public string City { get; set; }
	public int Marks { get; set; }
}

class LinqDemo
{
	static void Main()
	{
		List<Student> students = new List<Student>
		{
			new Student { Id = 1, Name = "Amit", Age = 22, City = "Pune", Marks = 85 },
			new Student { Id = 2, Name = "Sneha", Age = 24, City = "Mumbai", Marks = 92 },
			new Student { Id = 3, Name = "Rahul", Age = 21, City = "Pune", Marks = 76 },
			new Student { Id = 4, Name = "Priya", Age = 23, City = "Delhi", Marks = 92 },
			new Student { Id = 5, Name = "Vikas", Age = 25, City = "Mumbai", Marks = 65 }
		};

		// Where
		var puneStudents = students.Where(s => s.City == "Pune");
		Console.WriteLine("Pune Students: " + string.Join(", ", puneStudents.Select(s => s.Name)));

		// Select
		var names = students.Select(s => s.Name);
		Console.WriteLine("Names: " + string.Join(", ", names));

		// OrderBy and ThenBy
		var sortedStudents = students.OrderByDescending(s => s.Marks).ThenBy(s => s.Name);
		Console.WriteLine("Sorted By Marks Desc Then Name:");
		foreach (var student in sortedStudents)
		{
			Console.WriteLine($"{student.Name} - {student.Marks}");
		}

		// First, FirstOrDefault
		var firstMumbaiStudent = students.FirstOrDefault(s => s.City == "Mumbai");
		Console.WriteLine("First Mumbai Student: " + firstMumbaiStudent?.Name);

		// Any and All
		Console.WriteLine("Any student above 90: " + students.Any(s => s.Marks > 90));
		Console.WriteLine("All students passed: " + students.All(s => s.Marks >= 35));

		// Count, Max, Min, Sum, Average
		Console.WriteLine("Total Students: " + students.Count());
		Console.WriteLine("Max Marks: " + students.Max(s => s.Marks));
		Console.WriteLine("Min Marks: " + students.Min(s => s.Marks));
		Console.WriteLine("Sum Marks: " + students.Sum(s => s.Marks));
		Console.WriteLine("Average Marks: " + students.Average(s => s.Marks));

		// GroupBy
		var groupedByCity = students.GroupBy(s => s.City);
		Console.WriteLine("Grouped By City:");
		foreach (var group in groupedByCity)
		{
			Console.WriteLine(group.Key + ": " + string.Join(", ", group.Select(s => s.Name)));
		}

		// Skip and Take
		var topTwo = students.OrderByDescending(s => s.Marks).Take(2);
		Console.WriteLine("Top 2 Students: " + string.Join(", ", topTwo.Select(s => s.Name)));

		var skipTwo = students.OrderByDescending(s => s.Marks).Skip(2);
		Console.WriteLine("After Skipping Top 2: " + string.Join(", ", skipTwo.Select(s => s.Name)));

		// Distinct
		var cities = students.Select(s => s.City).Distinct();
		Console.WriteLine("Distinct Cities: " + string.Join(", ", cities));

		// Query syntax
		var querySyntaxResult = from s in students
								where s.Marks >= 80
								orderby s.Name
								select new { s.Name, s.Marks };

		Console.WriteLine("Query Syntax Result:");
		foreach (var item in querySyntaxResult)
		{
			Console.WriteLine($"{item.Name} - {item.Marks}");
		}
	}
}
```

## 9. Char and Individual Character Manipulation

**Problem Statement:** Analyze a password-like string character by character. Count letters, vowels, digits, spaces, and special characters; change character case; replace one character; reverse the text; and find the first non-repeating character. This demonstrates common interview questions based on `char` and string traversal.

```csharp
using System;
using System.Collections.Generic;

class CharDemo
{
	static void Main()
	{
		string text = "Hello@123 World";
		int vowels = 0;
		int consonants = 0;
		int digits = 0;
		int spaces = 0;
		int specialCharacters = 0;
		Dictionary<char, int> frequency = new Dictionary<char, int>();
		Console.WriteLine("Total Characters: " + text.Length);

		// Access and analyze each individual character by index.
		for (int index = 0; index < text.Length; index++)
		{
			char current = text[index];
			Console.WriteLine($"Index {index}: '{current}'");

			if (char.IsLetter(current))
			{
				char lowerCharacter = char.ToLower(current);
				if ("aeiou".Contains(lowerCharacter))
					vowels++;
				else
					consonants++;
			}
			else if (char.IsDigit(current))
			{
				digits++;
			}
			else if (char.IsWhiteSpace(current))
			{
				spaces++;
			}
			else
			{
				specialCharacters++;
			}

			char frequencyKey = char.ToLower(current);
			if (!char.IsWhiteSpace(frequencyKey))
			{
				frequency[frequencyKey] = frequency.ContainsKey(frequencyKey)
					? frequency[frequencyKey] + 1
					: 1;
			}
		}

		Console.WriteLine($"Vowels: {vowels}");
		Console.WriteLine($"Consonants: {consonants}");
		Console.WriteLine($"Digits: {digits}");
		Console.WriteLine($"Spaces: {spaces}");
		Console.WriteLine($"Special Characters: {specialCharacters}");

		// Strings are immutable, so convert to char[] before changing an index.
		char[] characters = text.ToCharArray();
		characters[0] = char.ToLower(characters[0]);
		characters[5] = '#';
		string modifiedText = new string(characters);
		Console.WriteLine("Modified Text: " + modifiedText);

		Array.Reverse(characters);
		Console.WriteLine("Reversed Modified Text: " + new string(characters));

		char? firstNonRepeating = null;
		foreach (char current in text)
		{
			char key = char.ToLower(current);
			if (!char.IsWhiteSpace(key) && frequency[key] == 1)
			{
				firstNonRepeating = current;
				break;
			}
		}

		Console.WriteLine(firstNonRepeating.HasValue
			? "First Non-Repeating Character: " + firstNonRepeating.Value
			: "No Non-Repeating Character Found");
	}
}
```

**Important Interview Points:**

- A `char` stores one UTF-16 code unit and uses single quotes, such as `'A'`.
- A `string` uses double quotes, such as `"ABC"`, and is immutable.
- Read a character with `text[index]`. To modify it:

```csharp
char[] characters = text.ToCharArray();
characters[0] = 'Y';
text = new string(characters);
```

- Useful methods include `char.IsLetter`, `char.IsDigit`, `char.IsWhiteSpace`, `char.IsUpper`, `char.ToUpper`, and `char.ToLower`.

## Quick-Reference Summary Table

| # | Concept | Core technique |
|---|---|---|
| 1 | Program entry point | `static void Main()` |
| 2 | Variables and types | `int`, `double`, `bool`, `char`, `string`, `var` |
| 3 | Read input | `Console.ReadLine()`; parse safely with `TryParse()` |
| 4 | Print output | `Console.Write()` and `Console.WriteLine()` |
| 5 | Format values | String interpolation: `$"Name: {name}"`; precision: `{amount:F2}` |
| 6 | Conditions | `if` / `else if` / `else`, `switch` expression or statement |
| 7 | Loops | `for`, `foreach`, `while`, `do` / `while` |
| 8 | Array | Fixed-size data: `int[] values = { 1, 2, 3 }` |
| 9 | List | Dynamic ordered data: `List<T>`; `.Add()`, `.AddRange()`, `.Insert()`, `.Remove()`, `.RemoveAt()`, `.Contains()`, `.Count`, `.Sort()` |
| 10 | Dictionary | Key-value data: `Dictionary<TKey, TValue>`; `.Add()`, `dictionary[key] = value`, `.Remove()`, `.TryGetValue()`, `.ContainsKey()`, `.ContainsValue()`, `.Keys`, `.Values`, `.Count` |
| 11 | HashSet | Unique unordered data: `HashSet<T>`; `.Add()`, `.Remove()`, `.Contains()`, `.Count`, `.UnionWith()`, `.IntersectWith()`, `.ExceptWith()`; no `.Sort()` |
| 12 | String | Immutable text; `.Trim()`, `.Split()`, `.Replace()`, `.Contains()` |
| 13 | Character | `char`; use `char.IsLetter()`, `char.IsDigit()`, `char.ToUpper()` |
| 14 | Efficient text building | `StringBuilder`; `.Append()`, `.AppendLine()`, `.Insert()`, `.Replace()`, `.Remove()`, `.Clear()`, `.Length`, `.ToString()` |
| 15 | Methods | Return type, parameters, `return`; optional parameters and method overloads |
| 16 | Error handling | `try` / `catch` / `finally`; throw with `throw new Exception()` |
| 17 | Null safety | Nullable reference types, `?.`, `??`, and `??=` |
| 18 | LINQ filtering and projection | `.Where()`, `.Select()`, `.FirstOrDefault()` |
| 19 | LINQ sorting and grouping | `.OrderBy()`, `.ThenBy()`, `.GroupBy()` |
| 20 | LINQ aggregation and paging | `.Count()`, `.Sum()`, `.Average()`, `.Skip()`, `.Take()` |
| 21 | OOP | `class`, constructor, properties, inheritance, interfaces, polymorphism |
| 22 | Async work | `async` / `await` with `Task` for non-blocking I/O |
