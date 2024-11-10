## Working with Command Line

![[Screen Shot 2024-01-19 at 16.12.45.png]]

`python decisionStump.py politicians_train.tsv politicians_test.tsv 0 pol_0_train.labels pol_0_test.labels pol_0_metrics.txt`

## Command Line Arguments

- to parameterize a program
### Python

![[Screen Shot 2024-01-19 at 16.18.13.png]]

Ex. `python example.py input.txt output.txt`
```
# example.py  
import sys  
  
# Get all of the arguments from command line and make sure  
# the number of arguments is correct.  
args = sys.argv  
assert(len(args) == 3)  
  
# Parse every argument  
input_file = args[1]  
output_file = args[2]  
  
# Open input file and read the content  
with open(input_file, 'r') as f_in:  
    content = f_in.readlines()  
  
# Open output file and write the content  
with open(output_file, 'w') as f_out:  
    for line in content:  
        f_out.write(line)
```
