#   Virtual column functions

    Virtual columns are special functions in HQL.
    Right now, there are two virtual columns: INPUT__FILE__NAME and BLOCK__OFFSET__INSIDE__FILE.

#   INPUT__FILE__NAME
    The INPUT__FILE__NAME function shows the input file's name for a mapper task.

    SELECT INPUT__FILE__NAME,BLOCK__OFFSET__INSIDE__FILE as OFFSIDE
    FROM employee;

#   BLOCK__OFFSET__INSIDE__FILE
    The BLOCK__OFFSET__INSIDE__FILE function shows the current global file position or the current block's file offset 
    if the file is compressed. 


