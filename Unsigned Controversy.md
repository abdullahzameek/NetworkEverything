Many developers (and some large development houses, such as Google) believe that developers should generally avoid unsigned integers.
This is largely because unexpected behavior can result when you mix signed and unsigned integers.

Consider the following snippet:

''''C++
void doSomething(unsigned int x)
{
    // Run some code x times
}
 
int main()
{
    doSomething(-1);
}
'''


What happens in this case? -1 gets converted to some large number (probably 4294967295), and your program goes ballistic.
But even worse, there’s no good way to guard against this condition from happening. C++ will freely convert between signed 
and unsigned numbers, but it won’t do any range checking to make sure you don’t overflow your type.
Many modern programming languages (such as Java and C#) either don’t include unsigned types, or limit their use. 
Bjarne Stroustrup, the designer of C++, said, “Using an unsigned instead of an int to gain one more bit to represent positive integers is 
almost never a good idea”.
This doesn’t mean you have to avoid unsigned types altogether -- but if you do use them, use them only where 
they really make sense, and take care not to mix signed and unsigned numbers.
