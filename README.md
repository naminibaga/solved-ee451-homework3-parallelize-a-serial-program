Download Link: https://assignmentchef.com/product/solved-ee451-homework3-parallelize-a-serial-program
<br>
<h1>1             Estimating <em>π </em></h1>

In this problem, you will use OpenMP directives to parallelize a serial program. The serial program is given in ‘p1 serial.c’, which uses the algorithm we discussed to estimate the value of <em>π</em>. You need use OpenMP to parallelize the loop between Line 28 and Line 32. To compile the program, type: gcc -lrt -fopenmp -o run p1 serial.c

<ol>

 <li>Version a: Use 4 threads and the <strong>DO/for </strong>directive to parallelize the loop.</li>

 <li>Version b: Use 2 threads and the <strong>SECTIONS </strong>directive to parallelize the loop.</li>

 <li>Report the execution time of serial version, Version a and Version b, respectively.</li>

</ol>

<strong>Hint: </strong>you may also need the <strong>REDUCTION </strong>data attribute.

<h1>2             Sorting</h1>

In this problem, you need implement the quick sort algorithm to sort an array in ascending order. Quick sort is a divide and conquer algorithm. The algorithm first divides a large array into two smaller sub-arrays: the low elements and the high elements; then recursively sorts the sub-arrays. The steps are:

<ul>

 <li>Pick an element, called a pivot, from the array.</li>

 <li>Reorder the array so that all elements with values less than the pivot come before the pivot, while all elements with values greater than the pivot come after it (equal values can go either way). After this partitioning, the pivot is in its final position. This is called the partition operation.</li>

 <li>Recursively apply the above steps to the sub-array of elements with smaller values and separately to the sub-array of elements with greater values.</li>

</ul>

In the given program ‘p2.c’, the array <em>m </em>(<em>size </em>of <em>m </em>= 16<em>M</em>) which you need sort is generated.

<ol>

 <li>Serial Quicksort function: implement a Quicksort function to sort the array. The input of the function includes the pointer of the array, the index of the starting element and ending element. For example, <strong>Quicksort(</strong><em>m</em><strong>, 20, 100) </strong>will sort the array <em>m </em>from <em>m</em>[20] to <em>m</em>[100]. Report the execution time of the serial version by calling Quicksort(<em>m</em>, 0, <em>size </em>− 1).</li>

 <li>OpenMP Quicksort function: randomly pick up an element of <em>m</em>, <em>m</em>[<em>rand</em>()%<em>size</em>], as the pivot, reorder the array so that all elements with values less than the pivot come before the pivot, while all elements with values greater than the pivot come after it (equal values can go either way). After this partitioning, the pivot is in its final position, say <em>f</em>. Run 2 threads in parallel, one calling Quicksort(<em>m,</em>0<em>,f </em>− 1) and the other calling Quicksort(<em>m,f,size </em>− 1). Report its execution time. <strong>Note that the Quicksort function is the same as the one used in serial version.</strong></li>

</ol>

<strong>Note: </strong>you can get more details about quick sort algorithm from textbook, Section 9.4.

<h1>3             Parallel K-Means</h1>

In PHW 2, you implemented the <em>K</em>-Means algorithm using Pthreads. In this problem, you will use mutex and condition variable to realize synchronization instead of iteratively joining the threads. Let <em>p </em>denote the number of threads that you need. In this version, you only create and join <em>p </em>threads once (not iteratively create and join the threads). This version of <em>K</em>-Means algorithm has the following steps:

<ol>

 <li>Initialize a mean value for each cluster.</li>

 <li>Partition and distribute the data elements among the threads. Each thread is responsible for a subset of data elements.</li>

 <li>Each thread assigns its data elements to the corresponding cluster. Each thread also keeps track of the number of data element assigned to each cluster in current iteration and the corresponding sum value.</li>

 <li>Let us use <em>r </em>to denote the number of threads which have completed the work for the current iteration. At the beginning of each iteration, <em>r </em>= 0. When a thread finishes its work for the current iteration, it checks the value of <em>r</em>. (Note that <em>r </em>is a shared variable, a mutex is needed whenever any thread tries to read/write it.)

  <ul>

   <li>If <em>r &lt; p </em>− 1, <em>r </em>← <em>r </em>+ 1, the thread goes to sleep.</li>

   <li>If <em>r </em>= <em>p</em>−1, <em>r </em>← 0, the thread recomputes the mean value of each cluster based on the local intermediate data of all the threads, then reinitializes the intermediate data of each thread. If the algorithm does not converge after the current iteration, this thread sends a broadcast single to wake up all the threads. Go to Step 3 to start a new iteration.</li>

  </ul></li>

 <li>Join the threads. Replace the value of each data with the mean value of the cluster which the data belongs to.</li>

</ol>

In this problem, the input data and initial mean values for the clusters are the same as in PHW2. To simplify the implementation, you do not need check the convergence; run <strong>50 </strong>iterations and output the matrix into the file named ‘output.raw’. Pass the number of threads <em>p </em>as a command line parameter similar to PHW2. In your report, you need report the execution time for the <strong>50 </strong>iterations (excluding the read/write time) for <em>p </em>= 1<em>,</em>2<em>,</em>4<em>,</em>8. Compare the execution time with the version which you implemented in PHW 2, discuss your observation in your report.