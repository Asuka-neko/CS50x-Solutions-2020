# Mario

```c
#include <stdio.h>
#include <cs50.h>

int main(void)
{
    int height, row, column, space;
  
  	//set the range of height to 1-8
    do
    {
        height = get_int("Height: ");
    }
    while (height < 1 || height > 8); 

    // draw the pyramid
    for (row = 0; row < height; row++)
    {
      	//print the space before the #
        for (space = 0; space < height - row - 1; space++)
        {
            printf(" ");
        }
      
      	//print the #
        for (column = 0; column <= row ; column++)
        {
            printf("#");
        }
      
      	//print the two spaces inside the pyramids
        printf("  "); 
      	
      	//print another pyramid
        for (column = 0; column <= row ; column++)
        {
            printf("#");
        }
      
        printf("\n");
    }
}
```

