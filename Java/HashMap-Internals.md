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