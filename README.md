
# File System Reports

This Algorithm is generate reports  about the file system that display information about the files and directories that appear in a directory that you have supplied to your program as a command line argument.

Basically, it extracts data in two forms of sortings done on basis of - 

    1. Alphabetical order of the file/directories
    2. Inode Number of the file/directory

### Input Format 

Program will take two inputs in command line.

    1. Tree/Inode order
    2. Path of the Root Directory 

<br>
Example

    $ ./FSreport -tree /home/myname/rootDir
    
    $ ./FSreport -inode /home/myname/rootDir

### Simulation Outputs 

###### 1. Tree directory structure
    Line 1: owner name (group name) | inode number | permissions | size in bytes | file/directory name
    Line 2: Date of last access | Date of last modification

###### 1. Inode directory structure
    Inode number | size (in bytes) | number of 512-byte blocks allocated to the file |  file size/512 | file/directory name


### Functionality 

***struct stat** is mainly used to retrieve data of the files/directory which is an inbuilt Unix system call that returns file attributes.*


#### 1. Tree directory structure Algorithm

At each level, Tree is sorted as alphabetical order of the directories apperaring and also the sub directories and files are appearing in alphabetical sequence with all the needed information.
If no files or subdirectories then that level would not appear.
    
*We are using **Merge Sort** to sort in alphabetical order using "strcmp"*

We are stimulating the file reports in the path given to us to all further levels in tree form

Main function used for traversing all files -  

    func getTREE:
        struct dirent *dp
        DIR *dir = opendir(path)

        while ((dp = readdir(dir)) != NULL):
            .......
            getTREE(newpath,level+1)
           

This algo is storing only name and path of subdirectory and files inside and printing its information in while loop in main function after (merge) sorting.


###### Structure used

Store paths and name of files & directories.

    struct file_info:
        char *name;
        char *path;
<br>

List of directories and files at current level.

    struct directory:           
        char *name;
        int num_files;
        int num_subdirs;
        file_info *files_in_dir;
        file_info *sub_dirs;

<br>
        
Level in the recursion tree.

    struct dir_lev:             
        int num_dirs;
        directory *directories;

<br>

#### 2. Inode directory structure Algorithm

At each level, file/dir data is extracted and printed in increasing order of the inodes of sub-directories as well as files inside.
If no files or subdirectories then that level would not appear.

*Levels' Directory names are not sorted in Alphabetical order...*

*We are using **Merge Sort** to sort in inode increasing order.*


###### Structure used

Main structure for storing the File/Dir Information.

    struct inode_file_info:
        char *name;
        int inode_num;
        int size;      /* Bytes */
        int blocks;
        int division;


Algorithm Structure is similar to the Tree directory Structure only.

###### Freeing up all the Structures Dynamically allocated at the end.

<br>

#### MAIN DRAWBACK - 
Due to recursive calls,this algorithm is quite memory deficient and also slow if the input files/directories has large data .
Hence Limit is set to - 

    -> 100 directory levels
        -> 200 directories at each level
            -> 200 files 
            -> 200 subdirectories