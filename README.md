# Unix-IO.Operations
Assignment for Systems Class - Part 4 Spell Check Removed from requirements

Finish listFiles() 
Finish printFile() 
Finish enterFile() 
Finish spellCheckFile() <- requirement removed

Assignment:

Copy-and-paste the following:

/*-------------------------------------------------------------------------*
 *---									---*
 *---		createListAndSpellCheck.c				---*
 *---									---*
 *---	    This file defines a C program that lets a user create,	---*
 *---	print, list and spell-check text files.				---*
 *---									---*
 *---	----	----	----	----	----	----	----	----	---*
 *---									---*
 *---	Version 1.0		2016 February 23	Joseph Phillips	---*
 *---									---*
 *-------------------------------------------------------------------------*/

#include	<stdlib.h>
#include	<stdio.h>
#include	<string.h>	// strchr(), strstr()
#include	<dirent.h>	// opendir(), readdir(), closedir()
#include	<sys/types.h>	// open(), read(), write(), close(), stat()
#include	<sys/stat.h>	// open(), read(), write(), close(), stat()
#include	<fcntl.h>	// open(), read(), write(), close()
#include	<unistd.h>	// stat()
#include	<ctype.h>	// toupper()


#define		BUFFER_LEN		256
#define		SPELLER_PROGNAME	"/usr/bin/aspell"
#define		SPELLER_PROG_OPTION	"list"
#define		ENDING_TEXT		"xxxyyyzzz"


//  PURPOSE:  To ask the user the yes/no question given in 'questionText',
//	get a legal (y)es or (n)o reply, and to return '1' if the user entered
//	yes or '0' otherwise.
int		yesNo		(const char*	questionText
				)
{
  //  I.  Application validity check:

  //  II.  Repeatedly ask until get valid response:
  char	text[BUFFER_LEN];

  do
  {
    printf("%s (Y/N)? ",questionText);
    fgets(text,BUFFER_LEN,stdin);
    text[0]	= toupper(text[0]);
  }
  while  ( (text[0] != 'Y')  &&  (text[0] != 'N') );

  //  III.  Finished:
  return(text[0] == 'Y');
}


//  PURPOSE:  To let the user enter a filepath into a static buffer of length
//	'BUFFER_LEN', to remove the ending '\n', and to return the static
//	address.
char*		getFilePath	()
{
  //  I.  Application validity check:

  //  II.  Ask for file path:
  static
  char	filePath[BUFFER_LEN];

  //  II.A.  Ask for and get file path:
  printf("File path? ");
  fgets(filePath,BUFFER_LEN,stdin);

  //  II.B.  Remove ending '\n':
  char*	cPtr	= strchr(filePath,'\n');

  if  (cPtr != NULL)
    *cPtr = '\0';

  //  III.  Finished:
  return(filePath);
}


//  PURPOSE:  To open the current directory ("."), and the names of all files
//	and directories in it.  Prints "Could not open current directory\n"
//	to 'stderr' and immediately returns if current directory cannot be
//	opened.  No return value.
void		listFiles	()
{
  //  I.  Application validity check:

  //  II.  List items in directory ".":

  //  YOUR CODE HERE

  //  III.  Finished:
}


//  PURPOSE:  To print the contents of the file whose path is given by
//	'filePath'.  If 'filePath' cannot be opened then prints
//	"Could not read <filePath>\n" to 'stderr' and immediately returns.
//	No return value.
void		printFile	(char*	filePath
				)
{
  //  I.  Application validity check:
  if  (filePath == NULL)
  {
    fprintf(stderr,"BUG: NULL ptr to printFile()\n");
    exit(EXIT_FAILURE);
  }

  //  II.  Print file:

  //  YOUR CODE HERE

  //  III.  Finished:
}


//  PURPOSE:  To let the user enter text to write (or over-write) the text
//	file 'filePath'.  The user signifies they are finished entering lines
//	by entering a blank line.
//	(1) If 'filePath' does *not* exist then the file may be created.
//	(2) If 'filePath' does exist *and* is a "regular file" then
//	    over-writing the file may only procede if the user verifies
//	    they are willing to over-wrirte that file (use 'yesNo()').
//	(3) If 'filePath' does exist *but* it is *not* a "regular file"
//	    (e.g. it is a directory) then
//	    "Attempt to overwrite a non-file!\n" is print to 'stderr' and
//	    it immediately returns.
//	Prints "Could not write to <filePath>.\n" to 'stderr' if could not
//	write to file.  No return value.
//
//	HINTS:
//	For (1): A blank line will have '\n' as its first char.
//	For (2) and (3): Use 'stat()' and 'S_ISREG()'.
void		enterFile	(char*	filePath
				)
{
  //  I.  Application validity check:
  if  (filePath == NULL)
  {
    fprintf(stderr,"BUG: NULL ptr to enterFile()\n");
    exit(EXIT_FAILURE);
  }

  //  II.  Enter file:

  //  YOUR CODE HERE

  //  III.  Finished:
}


//  PURPOSE:  This function opens 'filePath' as a file descriptor in
//	read-write mode.  It does this to put 'ENDING_TEXT' (a purposeful
//	 misspelling) at the end of the file.  This function also opens a pipe
//	for the child process to communicate back to the parent.  If opening
//	either 'filePath' or the pipe fails then it prints
//	"Error opening <filePath> or creating pipes\n" to 'stderr' and returns.
//	  This function then 'fork()'s a child process.  The child process
//	has its input re-directed from the 'filePath' file descriptor, and its
//	output re-directed to the output side of the pipe.  Then, the child
//	process executes program 'SPELLER_PROGNAME' with command line argument
//	'SPELLER_PROG_OPTION'.
//	  If the child process cannot execute the program, then it prints
//	"Could not find <SPELLER_PROGNAME>\n" to 'stderr' and does:
//		exit(EXIT_FAILURE);
//
//	  Meanwhile, the parent process continually reads from the input end
//	of the pipe if it finds 'ENDING_TEXT' it knows to print up to that
//	occurrence, and then stop the loop.
//	  The parent after the reading loop, the parent process should
//	'wait()' for the child process to end, remove 'ENDING_TEXT' from
//	the 'filePath' file descriptor, and close that file descriptor and the
//	pipe.
//
//	HINTS:
//	(*) To look for one string inside of another, use 'strstr()'
void		spellCheckFile	(char*	filePath
				)
{
  //  I.  Application validity check:
  if  (filePath == NULL)
  {
    fprintf(stderr,"BUG: NULL ptr to spellCheckFile()\n");
    exit(EXIT_FAILURE);
  }

  //  II.  Spell-check file:
  //  II.A.  Open file and pipe to get output of spell-checking process:
  int	fromSpeller[2];
  int	inFileFd;

  //  YOUR CODE HERE

  //  II.B.  Append 'ENDING_TEXT' (a purposefully mispelling) to end of file
  //	     so parent process can detect end-of-output of child process:
  long  length		 = lseek(inFileFd,0,SEEK_END);
  int	status		 = write(inFileFd,ENDING_TEXT,sizeof(ENDING_TEXT)-1);

  //  II.C.  Create spell-checking process and have it run spell-checker:

  pid_t	spellerPid;

  //  YOUR CODE HERE

  if  (spellerPid == 0)
  {
    lseek(inFileFd,0,SEEK_SET);
    close(0);
    dup(inFileFd);
    close(1);
    dup(fromSpeller[1]);
    //  YOUR CODE HERE
  }

  //  II.D.  Read response from spell-checking child process:

  //  YOUR CODE HERE

  //  II.E.  Wait for child process to officially end:

  //  YOUR CODE HERE

  //  II.F.  Remove 'ENDING_TEXT' from end of file:
  ftruncate(inFileFd,length);

  //  II.G.  Close file and pipe:

  //  YOUR CODE HERE

  //  III.  Finished:
}


//  PURPOSE:  To let the user decide what they want to do, and to call the
//	appropriate function to do it.  Ignores command line arguments.
//	Returns 'EXIT_SUCCESS' to OS.
int		main		()
{
  //  I.  Application validity check:

  //  II.  Do commands of user until they decide to quit:
  int	option	=	!0;

  while  (option != 0)
  {

    //  II.A.  Obtain a legal command from user:
    do
    {
      char	optionText[BUFFER_LEN];

      printf
	("\n"
	 "(1) List files\n"
	 "(2) Print a file\n"
	 "(3) Enter a file\n"
	 "(4) Spell check a file\n"
	 "(0) Quit\n"
	 "Your choice? "
	);
      fgets(optionText,BUFFER_LEN,stdin);
      option	= atoi(optionText);
    }
    while  ( (option < 0)  ||  (option > 4) );

    //  II.B.  Take action decided by user:
    switch  (option)
    {
    case 1 :
      listFiles();
      break;
    case 2 :
      printFile(getFilePath());
      break;
    case 3 :
      enterFile(getFilePath());
      break;
    case 4 :
      spellCheckFile(getFilePath());
      break;
    }
  }

  //  III.  Finished:
}
      
Finish listFiles() according to the comment above the function

Finish printFile() according to the comment above the function

Finish enterFile() according to the comment above the function

Finish spellCheckFile() according to the comment above the function
