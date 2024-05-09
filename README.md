# con-gen-csu-final-project

Here is the repo for my final project! 

Briefly, these data and corresponding code are for a recent small greenhouse project involving subjecting maize plants to low-phosphorus (P) stress. Maize seedlings were planted in soils with low bioavailable P but higher total P. Therefore, to grow without showcasing symptoms of P deficiency, maize must induce P-starvation strategies, such as accumulating an abundance of P-solubilizing bacteria in the rhizosphere. Half the maize plants were fertilized to P sufficiency, and half were left unfertilized. After the growing cycle concluded, we harvested and collected rhizosphere soil samples. We froze the soil samples and extracted DNA. Using Illumina sequencing, we amplified the V3-V4 region of the 16S rRNA gene. 

Here, you will see the use of the tourmaline workflow which utilizes qiime and Snakemake to process Illumina outputs from the recent experiment I conducted. I have 20 fastq files which comprise 10 replications of 2 fertilizer treatments (P fertilized and P unfertilized). 



**UPDATE 5/9/24**



Unfortunately, I was unable to complete the workflow for this project. I tried selecting a smaller section of my fastq files and I tried only running this workflow on 3 samples per treatment. Still, I ran into errors. The workflow ran fine using the sample data provided by tournaline, and I joined their gitter to talk to people and try to work through what is going on. 

I am consistently able to do the following: 
 - filter
      Using dada2, I was able to successfully filter and denoise my data. This was where I had my first hurdles becuase I was getting timed out. 
 - classify features
      I use the input table sequences to get a taxonomy table using dada2 error correction tools.

However, I then get stuck. I cannot get past this error:
`````
     Plugin error from deicode:

       `A` must not be empty.

     Debug info has been saved to /tmp/qiime2-q2cli-err-6eudpysa.log
`````


I looked in the log file, which showed that there was an error in this file path:
`````
  File "/projects/dixonm@colostate.edu/miniforge3/envs/qiime2-2023.5/lib/python3.8/site-packages/scipy/sparse/linalg/_eigen/_svds.py", line 70, in _iv
    raise ValueError(message)
ValueError: `A` must not be empty.
`````

I navigated to that location and line 70 referenced in the error messaged said this:
`````
def _iv(A, k, ncv, tol, which, v0, maxiter,
        return_singular, solver, random_state):

    # input validation/standardization for `solver`
    # out of order because it's needed for other parameters
    solver = str(solver).lower()
    solvers = {"arpack", "lobpcg", "propack"}
    if solver not in solvers:
        raise ValueError(f"solver must be one of {solvers}.")

    # input validation/standardization for `A`
    A = aslinearoperator(A)  # this takes care of some input validation
    if not (np.issubdtype(A.dtype, np.complexfloating)
            or np.issubdtype(A.dtype, np.floating)):
        message = "`A` must be of floating or complex floating data type."
        raise ValueError(message)
    if np.prod(A.shape) == 0:
        message = "`A` must not be empty."
        raise ValueError(message)
`````

This is now where I am stuck. :( 
The `A` that can't be empty is a linear operator that helps with input validation (?). I am unsure where to go from here. 



I really enjoyed this class. I started without any knowedge of linux/unix or alpine. I really learned a lot and appreciate all you did to help me keep up with the rest of the class. Thanks for a great semester, Eric!

