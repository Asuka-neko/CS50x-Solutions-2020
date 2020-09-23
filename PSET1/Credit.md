# Credit

```c
#include <stdio.h>
#include <cs50.h>
#include <string.h>

int main(void)
{
    long cardNumber;

    // get card number
    do
    {
        cardNumber = get_long("Number: ");
    }
    while (cardNumber <= 0);

    long CC = cardNumber; //keep the cardnumber unaffected
    int sum = 0;
    int count = 0;
    long divisor = 10;

    //1st step
    CC = cardNumber;
    while (CC > 0)
    {
        int lastDigit = CC % 10;
        sum = sum + lastDigit;
        CC = CC / 100;
    }

    //2nd step
    CC = cardNumber / 10;
    while (CC > 0)
    {
        int secondToLastDigit = CC % 10;
        int timesTwo = secondToLastDigit * 2;
        sum = sum + (timesTwo % 10) + (timesTwo / 10);
        CC = CC / 100;
    }

    // length of the card number
    CC = cardNumber;
    while (CC != 0)
    {
        CC = CC / 10;
        count++;
    }

    // divisor
    for (int i = 0; i < count - 2; i++)
    {
        divisor = divisor * 10;
    }

    int firstDigit = cardNumber / divisor;
    int firstTwoDigit = cardNumber / (divisor / 10);

    // check
    if (sum % 10 == 0)
    {
        if (firstDigit == 4 && (count == 13 || count == 16))
        {
            printf("VISA\n");
        }
        else if ((firstTwoDigit == 34 || firstTwoDigit == 37) && count == 15)
        {
            printf("AMEX\n");
        }
        else if ((50 < firstTwoDigit && firstTwoDigit < 56) && count == 16)
        {
            printf("MASTERCARD\n");
        }
        else
        {
            printf("INVALID\n");
        }
    }
    else
    {
        printf("INVALID\n");
    }
}
```

