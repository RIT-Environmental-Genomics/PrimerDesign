# Primer Design via Command Line

**You made it!** For the rest of this lab, you are going to reproduce the work you did in the Benchling GUI using the command line version of the Primer3 algorithm (the same algorithm used in Benchling and most other primer design tools).

To start, you need to access the Bioinformatics cluster, **Oedipus**.

## Working in the cluster
> **NOTE:** You should have received an email with your username and password info for accessing Oedipus. I *do not* have access to this information.

### Accessing the cluster via terminal
To access the cluster remotely, you will use <code>ssh</code>. Remember to change "XXX" to your assigned username.
```
ssh XXX@oedipus.bioinformatics.rit.edu
```
You should be prompted to enter your password.

### Navigating to your home directory
Now that you are connected to the cluster, navigate to your home directory using <code>cd</code>. Is there anything in it?

Create a new directory for this lab exercise called "Lab2" using <code>mkdir</code>. We will use this directory to store all your input and output files.
```
mkdir Lab2
```
### Moving files from your computer to the cluster
Outside the terminal, download the tar file (known as a tarball) in this Git repository called "Lab2_cl.tar" to your computer. We are going to use the files within this tar for parts of the lab.

Once you have the tar downloaded to your computer, you need to move it to your directory on the cluster so you can use it within Primer3.

> I know I just had you login to the cluster, *but* you need your terminal to be logged into your local computer to upload to the cluster from your computer. To exit the cluster, just use <code>exit</code>.

You can use the <code>scp</code> command to move files between machines. Make sure to change out the actual file paths from my example code below and to place them in the Lab2 directory you just created.

To move an individual file, use: 
```
scp /path/to/your/file.txt user@cluster_address:/path/in/cluster
```
To move an entire directory, you need to use the -r (recursive) flag after <code>scp</code>: 
```
scp -r directory/on/local/computer/ user@cluster_address:/path/in/cluster
```
The terminal will then ask you for your cluster password, which is the same one you use for ssh. After entering your password, the file(s) will be moved! You can ssh back into the cluster and check that the files are there.

> **NOTE:** If you have a Windows computer, you might notice that files paths on your machine use backslashes instead of forward slashes, which are used by Mac/Linux computers.

### Accessing software and packages
By default, modules are not loaded onto our individual work space. If we want to use a module or package, we will need to “load” it. You might wonder **why** modules/packages are not autoloaded and the biggest reasons are incompatbility and versioning. 

Compartmentalizing modules/packages allows you to select which versions you want to use when. This is important for ensuring your scripts will work in perpetuity, even when developers update packages.

To see available modules on the HPC, use <code>module avail</code> command. 
```
module avail
```
What modules are available?

## Running Primer3
### Running Primer3 on an example file
Load Primer3
```
module load primer3
```
We are going to run Primer3 on an example file that I included in the tarball you downloaded called "example.txt".

Find this file in the tarball. Remember, that you need to extract the files from it first using the <code>tar</code> command.

What is in "example.txt"? If you need help interpreting the lines, see the primer3 documentation here: https://primer3.org/manual.html

> **Important:** Do you notice that the file ends with an equal sign? This is important to the formatting of Primer3. The last line of the file *always* needs to be an equals sign!

Okay, now run the primer design tool using the <code>primer3_core</code> command.
```
primer3_core example.txt
```
This should automatically print the output to your terminal. Seriously, that's it!

To save the output, redirect it to a new file using “>”. 

Note that I am specifying for Primer3 to save our file to the Lab2 directory. If you already set your directory to Lab2, you wouldn't need to include that in the path.
```
primer3_core Lab2/example.txt > Lab2/primer_output.txt
```
Take some time to view the output. You will see that each line will contain information about the attributes of the generated primer pairs. The best primer pair (according to Primer3) and its attributes will be listed with a zero (e.g., PRIMER_LEFT_0, PRIMER_RIGHT_0).

### Running Primer3 with your own sequences

**Why use command line?** It can be much more efficient when you need to design primers for a large number of regions (e.g., universal primers, microarrays). Technically, microarray primers are called "hybridization probes" (single primers).

**For the final part of this lab, use the command line to:**
1) Design PCR primers for the same sequence you used in Benchling with the same parameters
   - Make sure to compare your results from the two methods
2) Design a hybridization probe to your sequence
   - See *pick_hyb_probe_only* in the Primer3 manual
3) Design PCR primers for a batch of sequences listed in "batch_seq.txt"

Use example.txt as a template. If you get stuck, ask for help or check out the Primer3 manual linked above.

Save your entire command line process as a Markdown file or html and attach/link it to your lab notebook.

> **NOTE:** This is an extremely simplified process. Primer3_core is designed to pick primers from one sequence at a time. If you wanted to design universal primers (e.g., a single primer pair common to multiple sequences) then you would need to include additional steps before primer3 (e.g., aligning sequences, generating a consensus sequence) and after primer3 (e.g., automating consolidation of primer candidate, checking specificity against a database, like BLAST). Luckily, there are some great GUI tools, like Primer-BLAST on NCBI, that already do this for you, so no need to reinvent the wheel! Hopefully, you now understand the behind-the-scenes process of primer design whether in the command line or a GUI.
