# Decoding_Wealth_Fund_Data_for-SQLite3

The data excel file can be downloaded from their site.
The format is as shown. 

```

rpi:~/stocksdir $ cat EQ_2022_Country.csv | head -n 5
Region,Country,Name,Industry,Market Value(NOK),Market Value(USD),Voting,Ownership,Incorporation Country
Oceania,Australia,29Metals Ltd,Basic Materials,"4,24,85,096","43,12,770",0.69,0.69,Australia
Oceania,Australia,4DMedical Ltd,Health Care,"2,44,87,945","24,85,833",2.96,2.96,Australia
Oceania,Australia,5E Advanced Materials Inc,Basic Materials,"1,963",199,,0,United States
Oceania,Australia,A2B Australia Ltd,Financials,"4,964",504,,0,Australia

```

In order to import to SQLite3 a table has to be created with these fields

```
Region,Country,Name,Industry,Market Value(NOK),Market Value(USD),Voting,Ownership,Incorporation Country

```



Problem : The line cannot be inserted as it has comma demiters for columns as well as ** Market Value ** in NOK and USD.
The commas in the 'Market value' has to be removed.

Solution : C program to remove the commas in the marketvalue
```
#include <stdio.h>
#include <stdlib.h>

int main()
{
    int num;

    FILE *file = fopen("EQ_2022_Country.csv", "r");
    //FILE *file = fopen("EQ_2022_1.csv", "r");
    char *code;
    size_t n = 0;
    int c;

    if (file == NULL)
        return -1; //could not open file

   // code = malloc(1000);

    while ((c = fgetc(file)) != EOF)
    {

	if ( c == '\"' ) {
 	   //printf ( "%c", c );
           while ((c = fgetc(file)) != EOF){
	       if ( c == '\"' ) {
		break;
		}
               if ( c != ',' ) {
 	       printf ( "%c", c );
               }
 	   }
        } else {
	    printf ( "%c",c );
        }
    }

    // don't forget to terminate with the null character
    //code[n] = '\0';        

    //return code;
   return 0;
}
```

