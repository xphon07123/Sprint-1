/*

** Must be able to accept single and multiple files in cmd line.
** If directory is given, must traverse through all the files.
** Create log file that logs the run.


Example:

 $ wc foo bar
   newline		wordcount		byte count (characters)
      40     		149     		947 foo	
    2294   			16638   		97724 bar
    2334   			16787   		98671 total

	
wc will print instructions for how to use wc 
wc -l <filename> will print the line count of a file
wc -c <filename> will print the character count
wc -w <filename> will print the word count
wc <filename> will print all of the above
	
	
use fileinputstream to get bytes
	
*/

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class WC{
	
	private int lineCount;
	private int wordCount;
	private int charCount;
	
	private void resetWC() {
		lineCount = 0;
		wordCount = 0;
		charCount = 0;
	}
	
	private int getLineCount() {
		return lineCount;
	}
	
	private int getWordCount() {
		return wordCount;
	}
	
	private int getCharCount() {
		return charCount;
	}
	
	private void getInstructions() {
		System.out.println("\nwc -l <filename> will print the line count of a file");
		System.out.println("wc -c <filename> will print the character count");
		System.out.println("wc -w <filename> will print the word count");
		System.out.println("wc <filename> will print all of the above");
	}
	
	private void setLineCount(String[] args, int i) {
		
		try{
			BufferedReader file = new BufferedReader(new FileReader(args[i])); // Creates a FileReader object to read from file, then uses bufferedReader to buffer characteres from file.
			
			while (file.readLine() != null) {
				lineCount++;
			}
			
			file.close();
			
		} catch (IOException e) {
			System.out.println("Failed to setLineCount");
		}
	}
	
	private void setWordCount(String[] args, int i) {
		
		try{
			
			BufferedReader file = new BufferedReader(new FileReader(args[i])); //Creates a FileReader object to read from file, then uses bufferedReader to buffer characteres from file.
			String bufferedText = file.readLine();	// Needs a String variate to use .split().
			
			while (bufferedText != null) {
				String temp[] = bufferedText.split(" ");
				wordCount += temp.length + 1;
				bufferedText = file.readLine();
			}
			
			file.close();
			
		} catch (IOException e) {
			System.out.println("Failed to setWordCount");
		}
	}
	
	private void setCharCount(String[] args, int i) {
		
		try{
			BufferedReader file = new BufferedReader(new FileReader(args[i])); // Creates a FileReader object to read from file, then uses bufferedReader to buffer characteres from file.
			
			while (file.read() != -1) {
				charCount++;
			}
			file.close();
			
		} catch (IOException e) {
			System.out.println("Failed to setCharCount");
		}
	}
	
	public static void main(String[] args) {
		WC wc = new WC();
		
		if (args.length == 0) {
			wc.getInstructions();
		}
		
		else {
			System.out.println("\nLines:\t\tCharacter Count:\tWord Count:\t\t");
			for (int i = 0; i < (args.length); i++) {
				switch (args[0]) {
					
					case "-l":
						if (i < args.length-1) {
							wc.setLineCount(args, i+1);
							System.out.println(wc.getLineCount() + "\t\t\t\t\t\t\t" + args[i+1]);
							wc.resetWC();
						}
						break;
					case "-c":
						if (i < args.length-1) {
							wc.setCharCount(args, i+1);
							System.out.println("\t\t"+wc.getCharCount() + "\t\t\t\t\t" + args[i+1]);
							wc.resetWC();
						}
						break;
					case "-w":
						if (i < args.length-1) {
							wc.setWordCount(args, i+1);
							System.out.println("\t\t\t\t\t"+wc.getWordCount() + "\t\t" + args[i+1]);
							wc.resetWC();
						}
						break;
					default:
						wc.setLineCount(args, i);
						wc.setCharCount(args, i);
						wc.setWordCount(args, i);
						System.out.println(wc.getLineCount() + "\t\t" + wc.getCharCount() + "\t\t\t" + wc.getWordCount() + "\t\t" + args[i]);
						wc.resetWC();
						
				}
			}
		}
	}
}
