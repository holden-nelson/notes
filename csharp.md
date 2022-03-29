# Learning C# in 5 Days

## Collections and Data Structures

https://docs.microsoft.com/en-us/dotnet/standard/collections/

### Arrays

```c#
int[] myIntArray = new int[5] { 1, 2, 3, 4, 5 };
int[] myEmptyArray = new int[] {};
```

Note that this is not a generic.

https://docs.microsoft.com/en-us/dotnet/api/system.array?view=net-6.0

### Lists

https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-6.0

```c#
// declaring
List<int> nums = new List<int>();

// adding
nums.Add(1);
nums.Add(3);
nums.Add(4);
nums.Add(5);

// traversing
nums.ForEach(n => Console.Write(n));

// check containment
nums.Contains(3); // True

// insert at index
nums.Insert(1, 2); // 1,2,3,4,5

// remove
nums.Remove(1); // only removes first | 2,3,4,5  
nums.RemoveAt(1); // 2,4,5
```

#### Doubly Linked

https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.linkedlist-1?view=net-6.0

```c#
// construct
string[] words = { "the", "fox", "jumps", "over", "the", "dog" };
LinkedList<string> sentence = new LinkedList<string>(words);

// traverse
foreach (string word in words)
	Console.Write(word + " ");

// containment
sentence.Contains("jumps")); // True

// Add to beginning
sentence.AddFirst("today");
sentence.AddLast("tomorrow");

// remove first occurence
sentence.Remove("brown");

// remove First, remove last
sentence.RemoveFirst();
sentence.RemoveLast();

// get first, get last
LinkedListNode<string> fw = sentence.First;
LinkedListNode<string> lw = sentence.Last;

// add after
sentence.AddAfter('the', "old");
sentence.AddBefore("quick", "brown");

// find a node
current = sentence.Find("fox");
```



### Dictionary

https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2?view=net-6.0

```c#
// declaring
Dictionary<string,string> openWith = new Dictionary<string, string>();

// adding
openWith.Add("txt", "notepad.exe");
openWith.Add("bmp", "paint.exe");
openWith.Add("dib", "paint.exe");
openWith.Add("rtf", "wordpad.exe");

// The Add method throws an exception if the new key is
// already in the dictionary.
try {openWith.Add("txt", "winword.exe");}
catch (ArgumentException) {Console.WriteLine("An element with Key = \"txt\" already exists.");}

// access - retrieving, printing, changing
openWith["rtf"] = "winword.exe";

// The indexer throws an exception if the requested key is
// not in the dictionary.
try {Console.WriteLine("For key = \"tif\", value = {0}.",
        openWith["tif"]);}
catch (KeyNotFoundException) {Console.WriteLine("Key = \"tif\" is not found.");}

// Containment
if (!openWith.ContainsKey("ht")) {
    openWith.Add("ht", "hypertrm.exe");
}

// When you use foreach to enumerate dictionary elements,
// the elements are retrieved as KeyValuePair objects.
foreach( KeyValuePair<string, string> kvp in openWith )
{
    Console.WriteLine("Key = {0}, Value = {1}",
        kvp.Key, kvp.Value);
}

// Use the Remove method to remove a key/value pair.
openWith.Remove("doc");
```

### Queue

https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.queue-1?view=net-6.0

```c#
// declare
Queue<string> numbers = new Queue<string>();

// enqueue - "get in line"
numbers.Enqueue("one");
numbers.Enqueue("two");
numbers.Enqueue("three");
numbers.Enqueue("four");
numbers.Enqueue("five");

// A queue can be enumerated without disturbing its contents.
foreach( string number in numbers )
	Console.WriteLine(number);

// dequeue
numbers.Dequeue();

// peek
numbers.Peek()
```

### Stack

https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.stack-1?view=net-6.0

```c#
// declare
Stack<string> numbers = new Stack<string>();

// push - "put on top"
numbers.Push("one");
numbers.Push("two");
numbers.Push("three");
numbers.Push("four");
numbers.Push("five");

// A stack can be enumerated without disturbing its contents.
foreach( string number in numbers )
    Console.WriteLine(number);

// pop
numbers.Pop();

// peek
numbers.Peek();
```





