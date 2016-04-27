import java.util.Scanner;

public class LatinBand
{
	private static Scanner keyboard = new Scanner(System.in);
	
	public static void main (String args[])
	{
		double[][] bandArray;
		char userPick;
		
		bandArray = rowAndPosArray();
		bandDisplay(bandArray);
		
		System.out.print("\n(A)dd,   (R)emove,   (P)rint,    e(X)it   : ");
		
		do
		{
			userPick = Character.toUpperCase(keyboard.next().charAt(0));
			keyboard.nextLine();
			
			
			switch(userPick)
			{
				case 'A':
					bandArray = addMusician(bandArray);
					System.out.print("\n(A)dd,   (R)emove,   (P)rint,    e(X)it   : ");
					break;
				case 'R':
					bandArray = removeMusician(bandArray);
					System.out.print("\n(A)dd,   (R)emove,   (P)rint,    e(X)it   : ");
					break;
				case 'P':
					bandDisplay(bandArray);
					System.out.print("\n(A)dd,   (R)emove,   (P)rint,    e(X)it   : ");
					break;
				case 'X':
					break;
				default:
					System.out.print("Error: Invalid option, please try again   : ");	
			}
			
		}while(userPick != 'X');
	}

	private static double[][] removeMusician(double[][] band)
	{
		int posNum, validatedRow, validatedPos;
		char rowLetter;
		boolean removed;
		
		System.out.print("Please enter row letter                   : ");
		
		rowLetter = Character.toUpperCase(keyboard.next().charAt(0));
        validatedRow = validate(rowLetter, band.length);
			
		System.out.print("Please enter position number (1 to " +band[validatedRow].length + ")     : ");
		posNum = keyboard.nextInt();
		validatedPos  = validate(posNum - 1, band[validatedRow].length);
		
		removed = false;
		
		if(band[validatedRow][validatedPos] == 0.0)
		{
			System.out.print("Error: That position is vacant.");
			System.out.println();
		}
		else
		{
			for(int index = 0; index < band[validatedRow].length; index++)
			{
				band[validatedRow][index] = 0.0;
				removed = true;
			}
		}
		
		if(removed)
		{
			System.out.println("****** Musician removed.");
		}
		
		return(band);
	}
	
	private static int validate(char myChar, int length)
	{
		boolean valid = false;
		
		while (!valid)
		{
			if ((int)(myChar - 'A') < 0 || (int)(myChar - 'A') > length)
				System.out.print("ERROR: Out of range, try again            : ");
			else 
			{
				valid = true;
				return (int)(myChar - 'A');
			}
			
			myChar = Character.toUpperCase(keyboard.next().charAt(0));
		}
		
		return (int)(myChar - 'A');
	}
	
	private static int validate(int myChar, int length)
	{
		boolean valid = false;
		
		while (!valid)
		{	
			if (myChar < 0 || myChar  > length)
				System.out.print("ERROR: Out of range, try again            : ");
			else 
			{
				valid = true;
				return (myChar );
			}
			
			myChar = keyboard.nextInt();
		}
		
		return myChar;
	}
	
	private static double[][] addMusician(double[][] band)
	{
		final double ABSOLUTE_WEIGHT = 100,
					 MIN_WEIGHT = 45,
					 MAX_WEIGHT = 200;
		double musicianWeight, total = 0, avg = 0;
		int posNum, validatedRow, validatedPos;
		char rowLetter;
		boolean weightCheck, add; 
		
		System.out.print("Please enter row letter                   : ");
		rowLetter = Character.toUpperCase(keyboard.next().charAt(0));
		validatedRow = validate(rowLetter, band.length);
		
		System.out.print("Please enter position number (1 to " +band[validatedRow].length + ")     : ");
		posNum = keyboard.nextInt();
		validatedPos  = validate(posNum - 1, band[validatedRow].length);
		
		if(!(band[validatedRow][validatedPos] == 0.0))
		{
			System.out.print("Error: There is already a musican there.");
			System.out.println();
		}
		else
		{
			weightCheck = true;
			add = false;
			
			System.out.print("Please enter weight (45.0 to 200.0)       : ");
			musicianWeight = keyboard.nextDouble();
			
			for(int index2 = 0; index2 < band[validatedRow].length; index2++)
			{	
				do
				{
					if(musicianWeight < MIN_WEIGHT || musicianWeight > MAX_WEIGHT)
					{
						System.out.print("Error: Out of range, try again            : ");
						musicianWeight = keyboard.nextDouble();
					}
					else if((total + musicianWeight)/band[validatedRow].length > ABSOLUTE_WEIGHT)
					{
						System.out.print("Error: That would exceed the average weight limit.");
						weightCheck = false;
					}
					else
					{
						weightCheck = false;
						add = true;
					}
					
				}while(weightCheck);
			
				total += band[validatedRow][index2];
			}
			
			if(add)
			{
				band[validatedRow][validatedPos] = musicianWeight;
				System.out.println("****** Musician added.");
			}
		}
		
		return(band);
	}
	
	public static void bandDisplay(double[][] band)
	{	
		int rowNum, posNum;
		double total = 0, avg = 0;
		
		System.out.println();
		
		for(rowNum = 0; rowNum < band.length; rowNum++)
		{
			System.out.print((char)(rowNum + 65) + ": ");
			total = 0;	
			
			for(posNum = 0; posNum < band[rowNum].length; posNum++)
			{
				total += band[rowNum][posNum];
				avg = total / band[rowNum].length;
				System.out.print("\t" + band[rowNum][posNum]);
			}
			
			for(int posNumBl = posNum; posNumBl < 8; posNumBl++)
			{
				System.out.print("\t");
			}
			
			System.out.printf("[%.1f, %.1f]", total,avg);
			System.out.println();
		}		
	}
	
	private static double[][] rowAndPosArray()
	{
		double[][] bandSize;
		int userInput;
		boolean repeat, posCheck;
		
		System.out.println("Welcome to the band of the Hour");
		System.out.println("-------------------------------");
		System.out.print("Please enter number of rows               : ");
		
		userInput = keyboard.nextInt();
		repeat = true;
		
		while(repeat)
		{
			if((userInput <= 0) || (userInput > 10))
			{
				System.out.print("Error: Out of range, try again            : ");
				userInput = keyboard.nextInt();
			}
			else
			{
				repeat = false;
			}
		}
		
		bandSize = new double[userInput][]; 
		
		for(int rowNum = 0; rowNum < bandSize.length; rowNum++)
		   {
				System.out.print("Please enter number of positions in row " + (char)(rowNum + 65) + " : ");
				posCheck = true;
				
				while(posCheck)
				{
					userInput = keyboard.nextInt();
					
					if((userInput <= 0) || (userInput > 8))
					{
						System.out.print("Error: Out of range, try again            : ");
					}
					else
					{
						bandSize[rowNum] = new double[userInput];
						posCheck = false;
					}
				}
		   }
		
		return(bandSize);
	}
}
