![image](https://beren-obsidian-images.imgix.net/623f8181c81323e831a831bd69b76d95.png)
##### ***Printing & inputs***
**A function that takes an integer as an argument and prints it:**
- `void` means this function doesn't return a value.
- `printNumber` is the name of the function.
- `int num` is the parameter; it takes an integer.
- `printf` is a standard I/O function used to print the number.
```
void printNumber(int num) {
    printf("%d\n", num);
}
```

**A function that takes two integers and returns the sum:**
- `int` indicates that this function returns an integer.
- `addNumbers` is the function name.
- `int num1, int num2` are parameters; the function takes two integers.
- `return` statement returns the sum of `num1` and `num2`.
```
int addNumbers(int num1, int num2) {
    return num1 + num2;
}
```

**Main function**
- Every C program must have a `main` function. This is where your program starts execution.
- Inside `main`, you can call the functions you've defined. For example:
```
int main() {
    printNumber(5);
    int sum = addNumbers(3, 4);
    printf("Sum is: %d\n", sum);
    return 0;
}
```
- `return 0` indicates that the program executed successfully.

**Converting Celsius to Fahrenheit**
```
#include <stdio.h>

int main(){
    //Initialize variables with integer types
    int fahr, celcius;
    int lower, upper, step;

    //Declare values for variables. Start at "lower" for the loop. And end the loop with "upper". Increase farenheight by 20 each time.
    lower = 0;
    upper = 300;
    step = 20;
    fahr = lower;

    //Farenheight will start with the value of 0. Then the value of celcius will be caclulated by a formula which converts farenheight to celcius
    while(fahr <= upper){
        celcius = 5 * (fahr-32) / 9;
        //Use %d to reference a parameter in order
        printf("%d\t%d\n", fahr, celcius);
        //Increase by 20, looping through 16 iterations
        fahr = fahr + step;
    }
    return 0;
}
```