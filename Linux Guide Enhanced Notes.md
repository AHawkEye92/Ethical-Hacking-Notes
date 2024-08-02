## Basic Linux Notes/Guide

### Common Commands

- **pwd**
    
    - **Purpose:** Show the current directory.
    - **Example Output:**
        
        bash
        
        Copy code
        
        `/root`
        
- **ls**
    
    - **Purpose:** List all directories and files in the current directory.
    - **Example Output:**
        
        perl
        
        Copy code
        
        `Desktop    Downloads  Pictures  Templates  embedded-browser-no-sandbox.json Documents  Music      Public    Videos`
        

### Navigation

- **Change Directory: `cd`**
    - **cd downloads**
        
        - **Purpose:** Move to the Downloads directory.
        - **Commands:**
            
            bash
            
            Copy code
            
            `pwd /root/Downloads ls`
            
        - **Description:** This sequence shows how to navigate to the Downloads directory, display the current path, and list its contents.
    - **cd ..**
        
        - **Purpose:** Move back to the parent directory.
        - **Commands:**
            
            bash
            
            Copy code
            
            `pwd /root ls`
            
        - **Description:** This sequence shows how to move back to the parent directory, display the current path, and list its contents.

### Viewing Command Usage

- **man ls**
    
    - **Purpose:** Show the manual (help) text for the `ls` command.
    - **Usage:** Type `man ls` and press Enter. To quit the manual, press `q`.
- **ls -l**
    
    - **Purpose:** Show detailed information about files and directories.
    - **Example Output:**
        
        sql
        
        Copy code
        
        `total 0 drwxr-xr-x  2 user group 4096 date time Desktop drwxr-xr-x  2 user group 4096 date time Documents`
        
- **clear**
    
    - **Purpose:** Clear the terminal screen.
- **ls --help**
    
    - **Purpose:** Show information on how to use the `ls` command, including examples and arguments.

### Useful Shortcuts and Tips

- **Tab Completion**
    - **Purpose:** Auto-complete commands or file names. For example, typing `Doc` and pressing Tab might complete it to `Documents` if the directory exists.

### Managing Software

- **apt-get update**
    
    - **Purpose:** Update the list of available packages and their versions.
    - **Usage:** `sudo apt-get update`
- **apt-get install [package_name]**
    
    - **Purpose:** Install a new software package.
    - **Example:** `sudo apt-get install terminator`
        - **Description:** Installs the Terminator terminal emulator, which allows multiple terminal sessions in one window.