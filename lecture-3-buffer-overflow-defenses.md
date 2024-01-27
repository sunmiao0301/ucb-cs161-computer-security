##### Lecture 4: Buffer Overflow Defenses

##### A trickier example: Consider a Java sort routine

即使在java中，有人可能会提供一个自己写的类，然后把这个类的compareTo()故意乱写，比如使得x.compareTo(y) == 1, y.compareTo(z) == 1, and z.compareTo(x) == 1，这样当Java内置的排序算法开始工作时，就会陷入无限死循环。

