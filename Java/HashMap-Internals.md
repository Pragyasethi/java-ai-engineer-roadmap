#HashMap Internals

HashMap stores data as key-value pairs and uses hashing to provide fast insertion and retrieval.

Think of a dictionary.

The word is the key.

The meaning is the value.

You search using the word instead of reading every page.

Internal Working

1. Java calls hashCode() on the key.
2. A hash code is generated.
3. Java calculates the bucket.
4. The key-value pair is stored.
5. During lookup, Java again calculates the bucket.
6. If multiple keys share a bucket, Java uses equals() to find the correct one.

Important Terms

Key

Used to identify data.

Value

Information associated with the key.

Bucket

Storage location inside HashMap.

Collision

Two different keys are stored in the same bucket.

hashCode()

Helps Java find the correct bucket.

equals()

Identifies the correct key inside a bucket.

Important Points

* HashMap stores key-value pairs.
* Average lookup is O(1).
* Collisions are possible.
* hashCode() and equals() work together.

Real-world Examples

* API cache
* User sessions
* Configuration lookup

Memory Trick

Library Catalog → hashCode()

Bookshelf → Bucket

Book Title → Key

Book → Value


Imagine we have a cupboard with 16 drawers, numbered 0 to 15.

I want to store your details in one of these drawers.

Your name is:

“Pragya”

Java first takes your name and calculates a hash code.

Let’s say (for understanding) it gets:

135678932

Now Java calculates which drawer (bucket) to use.

For simplicity, think of it like:

135678932 % 16 = 4

So Java stores your data in Drawer 4.

Later, when we search:

map.get("Pragya");

Java doesn’t open all 16 drawers.

Instead, it:

1. Calculates the hash code for "Pragya" again.
2. Determines that the correct drawer is Drawer 4.
3. Goes directly to Drawer 4.
4. Checks whether the stored key equals "Pragya".
5. Returns the associated value.

Now imagine another person’s name also maps to Drawer 4.

Both entries are stored in the same drawer. This situation is called a collision.

Java then uses equals() to identify which entry actually belongs to "Pragya".

That’s why both hashCode() and equals() are required:

* hashCode() helps Java find the correct drawer (bucket).
* equals() helps Java identify the correct key inside that drawer.

60-Second Revision

* Key-value storage
* Uses hashing
* hashCode() finds bucket
* equals() finds key
* Collision = Same bucket
* Average lookup O(1)

--------------------------------------------------------------------

#Why Do We Need Linked Lists / Red-Black Trees If equals() Exists?

Common Doubt

Question: If equals() can identify whether two objects are the same, why does HashMap need a Linked List or a Red-Black Tree?

Short Answer

equals() does not prevent collisions.

Its job is only to determine whether two objects in the same bucket are logically equal.

The Linked List or Red-Black Tree exists to store multiple objects that end up in the same bucket because of a hash collision.

⸻

Responsibilities of Each Component

Component	Responsibility
hashCode()	Generates the hash value and helps determine the bucket.
Bucket (Linked List / Red-Black Tree)	Stores all entries that map to the same bucket.
equals()	Checks whether two objects in that bucket are logically equal.

Think of them as a team rather than alternatives.

⸻

Example

Suppose all these students produce the same hash code:

* Pragya
* Rahul
* Amit
* Neha
* Riya
* Karan

All of them are placed in Bucket 5.

Bucket 5
Pragya
Rahul
Amit
Neha
Riya
Karan

Now we execute:

map.get(new Student("Karan"));

Java performs the following steps:

1. Calls hashCode().
2. Determines Bucket 5.
3. Starts comparing entries inside Bucket 5 using equals().

equals(Karan, Pragya) -> false
equals(Karan, Rahul)  -> false
equals(Karan, Amit)   -> false
equals(Karan, Neha)   -> false
equals(Karan, Riya)   -> false
equals(Karan, Karan)  -> true

Notice that equals() had to be called multiple times before finding the correct object.

⸻

Why Was a Linked List Used?

Before Java 8, collisions were stored as a Linked List.

Bucket 5
Pragya -> Rahul -> Amit -> Neha -> Riya -> Karan

To find the last element, Java may have to traverse every node.

Worst-case complexity:

O(n)

where n is the number of entries in that bucket.

⸻

Problem with Linked Lists

Imagine a poor hashCode() implementation:

@Override
public int hashCode() {
    return 1;
}

Now every object is stored in the same bucket.

If there are 10,000 objects:

Bucket 1
Node1 -> Node2 -> Node3 -> ... -> Node10000

Searching for the last node may require nearly 10,000 comparisons, making lookup slow.

⸻

Java 8 Improvement – Red-Black Tree

To improve worst-case performance, Java 8 introduced treeification.

If:

* Bucket size ≥ 8 (TREEIFY_THRESHOLD)
* Table capacity ≥ 64

Java converts the Linked List into a Red-Black Tree.

             Root
            /    \
         Node     Node
        /   \     /  \
      ...   ... ... ...

Now Java can locate the desired node much faster.

Worst-case lookup complexity becomes:

O(log n)

instead of

O(n)

⸻

Does Java Still Use equals()?

Yes.

The Red-Black Tree does not replace equals().

It only helps Java find the most likely node faster.

Once Java reaches a candidate node, it still uses equals() to verify whether it is the correct key.

So the overall flow remains:

hashCode()
      ↓
Find Bucket
      ↓
Linked List / Red-Black Tree
      ↓
equals()
      ↓
Object Found

⸻

Memory Trick

Think of an apartment building.

* hashCode() → Apartment number (bucket)
* Linked List / Red-Black Tree → People living in that apartment
* equals() → Asking each person, “Are you the one I’m looking for?”

If only one person lives there, the answer is immediate.

If many people live there, Java needs an efficient way to locate the correct person. Before Java 8, it walked through the residents one by one (Linked List). From Java 8 onwards, it can organize them into a Red-Black Tree, making the search much faster.

⸻

Interview Question

Q: If equals() already determines whether two keys are equal, why did Java 8 introduce Red-Black Trees in HashMap?

Answer:

equals() only checks logical equality after Java reaches the correct bucket. When many entries collide into the same bucket, searching through a Linked List becomes O(n). Java 8 converts heavily populated buckets into a Red-Black Tree, reducing worst-case lookup time to O(log n) while still using equals() for the final equality check.
