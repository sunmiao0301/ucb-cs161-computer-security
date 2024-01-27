![image-20240117170521822](https://raw.githubusercontent.com/sunmiao0301/Public-Pic-Bed/main/imgfromPicGO/202401171705002.png)



![image-20240117170740195](https://raw.githubusercontent.com/sunmiao0301/Public-Pic-Bed/main/imgfromPicGO/202401171707266.png)

and many examples about memory safety bugs

##### Defensive programming

means that each module takes responsibility for checking the validity of all inputs sent to it. Even if you “know” that your callers will never send you a NULL pointer, you check for NULL anyway, just in case, because sometimes what you “know” isn’t actually true, and even if it is true today, it might not be true tomorrow as the code evolves.

Debugging is twice as hard as writing the code in the first place. Therefore, if you write the code as cleverly as possible, you are, by definition, not smart enough to debug it.

**Static Analysis**

Generally speaking, the earlier a bug is found, the cheaper it can be to fix, which makes static analysis tools attractive.

**One challenge with static analysis tools is that they make errors. This is fundamental: detecting security bugs can be shown to be undecidable (like the Halting Problem)**, so it follows that any static analysis tool will either miss some bugs (false negatives), or falsely warn about code that is correct (false positives), or both.

由于停机问题的存在，静态检查并不是完美的。
停机问题证明了程序的运行结果是不可预测的，有可能进入死循环，有可能运行结束停机。

##### Testing

and

##### Fuzz testing

- Random inputs. Construct a random input file, and run the program on that input. The file is constructed by choosing a totally random sequence of bytes, with no structure. **完全无规则的测试用例，很有可能第一步就因为文件格式不可用直接被拒绝，导致很多bugs没测**

- Mutated inputs. Start with a valid input file, randomly modify a few bits in the file, and run the program on the mutated input.**在合法的输入中小改，然后测试。** 

- Structure-driven input generation. Taking into account the intended format of the input, devise a program to independently “fuzz” each field of the input file. For instance, if we know that one part of the input is a string, generate random strings (of random lengths, with random characters, some of them with % signs to try to trigger format string bugs, some with funny Unicode characters, etc.). If another part of the input is a length, try random integers, try a very small number, try a very large number, try a negative number (or an integer whose binary representation has its high bit set).**根据程序的数据结构，针对性地设计测试用例。**

通常，第二种和第三种方式比第一种更好。