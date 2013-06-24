---
layout: post
title:  "Computer science in JavaScript: Quicksort"
date:   2012-11-27 23:00:49
author: Nicholas C. Zakas
categories: program
---

## Computer science in JavaScript: Quicksort
### by Nicholas C. Zakas
### at 2012-11-27 23:00:49
### original <http://feedproxy.google.com/~r/nczonline/~3/eUC6tGjFkzA/>

<p>Most discussions about sorting algorithms tend to end up discussing quicksort because of its speed. Formal computer science programs also tend to cover quicksort<sup>[1]</sup> last because of its excellent average complexity of O(n log n) and relative performance improvement over other, less efficient sorting algorithms such as bubble sort and insertion sort for large data sets. Unlike other sorting algorithms, there are many different implementations of quicksort that lead to different performance characteristics and whether or not the sort is stable (with equivalent items remaining in the same order in which they naturally occurred).</p>
<p>Quicksort is a divide and conquer algorithm in the style of merge sort. The basic idea is to find a “pivot” item in the array to compare all other items against, then shift items such that all of the items before the pivot are less than the pivot value and all the items after the pivot are greater than the pivot value. After that, recursively perform the same operation on the items before and after the pivot. There are many different algorithms to achieve a quicksort and this post explores just one of them.</p>
<p>There are two basic operations in the algorithm, swapping items in place and partitioning a section of the array. The basic steps to partition an array are:</p>
<ol>
<li>Find a “pivot” item in the array. This item is the basis for comparison for a single round.</li>
<li>Start a pointer (the left pointer) at the first item in the array.</li>
<li>Start a pointer (the right pointer) at the last item in the array.</li>
<li>While the value at the left pointer in the array is less than the pivot value, move the left pointer to the right (add 1). Continue until the value at the left pointer is greater than or equal to the pivot value.</li>
<li>While the value at the right pointer in the array is greater than the pivot value, move the right pointer to the left (subtract 1). Continue until the value at the right pointer is less than or equal to the pivot value.</li>
<li>If the left pointer is less than or equal to the right pointer, then swap the values at these locations in the array.</li>
<li>Move the left pointer to the right by one and the right pointer to the left by one.</li>
<li>If the left pointer and right pointer don’t meet, go to step 1.</li>
</ol>
<p>As with many algorithms, it’s easier to understand partitioning by looking at an example. Suppose you have the following array:</p>
<pre><code>var items = [4, 2, 6, 5, 3, 9];</code></pre>
<p>There are many approaches to calculating the pivot value. Some algorithms select the first item as a pivot. That’s not the best selection because it gives worst-case performance on already sorted arrays. It’s better to select a pivot in the middle of the array, so consider 5 to be the pivot value (length of array divided by 2). Next, start the left pointer at position 0 in the right pointer at position 5 (last item in the array). Since 4 is less than 5, move the left pointer to position 1. Since 2 is less than 5, move the left pointer to position 2. Now 6 is not less than 5, so the left pointer stops moving and the right pointer value is compared to the pivot. Since 9 is greater than 5, the right pointer is moved to position 4. The value 3 is not greater than 5, so the right pointer stops. Since the left pointer is at position 2 and the right pointer is at position 4, the two haven’t met and the values 6 and 3 should be swapped.</p>
<p>Next, the left pointer is increased by one in the right pointer is decreased by one. This results in both pointers at the pivot value (5). That signals that the operation is complete. Now all items in the array to the left of the pivot are less than the pivot and all items to the right of the pivot are greater than the pivot. Keep in mind that this doesn’t mean the array is sorted right now, only that there are two sections of the array: the section where all values are less than the pivot and the section were all values are greater than the pivot. See the figure below.</p>
<p><a href="http://www.nczonline.net/blog/wp-content/uploads/2012/11/quicksort_partition1.png"><img src="http://www.nczonline.net/blog/wp-content/uploads/2012/11/quicksort_partition1.png" alt="Quicksort step-by-step" title="" width="600" height="944"></a></p>
<p>The implementation of a partition function relies on there being a <code>swap()</code> function, so here’s the code for that:</p>
<pre><code>function swap(items, firstIndex, secondIndex){
    var temp = items[firstIndex];
    items[firstIndex] = items[secondIndex];
    items[secondIndex] = temp;
}</code></pre>
<p>The partition function itself is pretty straightforward and follows the algorithm almost exactly:</p>
<pre><code>function partition(items, left, right) {

    var pivot   = items[Math.floor((right + left) / 2)],
        i       = left,
        j       = right;


    while (i &lt;= j) {

        while (items[i] &lt; pivot) {
            i++;
        }

        while (items[j] &gt; pivot) {
            j--;
        }

        if (i &lt;= j) {
            swap(items, i, j);
            i++;
            j--;
        }
    }

    return i;
}</code></pre>
<p>This function accepts three arguments: <code>items</code>, which is the array of values to sort, <code>left</code>, which is the index to start the left pointer at, and <code>right</code>, which is the index to start the right pointer at. The pivot value is determined by adding together the <code>left</code> and <code>right</code> values and then dividing by 2. Since this value could potentially be a floating-point number, it’s necessary to perform some rounding. In this case, I chose to use the floor function, but you could just as well use the ceiling function or round function with some slightly different logic. The <code>i</code> variable is the left pointer and the <code>j</code> variable is the right pointer.</p>
<p>The entire algorithm is just a loop of loops. The outer loop determines when all of the items in the array range have been processed. The two inner loops control movement of the left and right pointers. When both of the inner loops complete, then the pointers are compared to determine if the swap is necessary. After the swap, both pointers are shifted so that the outer loop continues in the right spot. The function returns the value of the left pointer because this is used to determine where to start partitioning the next time. Keep in mind that the partitioning is happening in place, without creating any additional arrays.</p>
<p>The quicksort algorithm basically works by partitioning the entire array, and then recursively partitioning the left and right parts of the array until the entire array is sorted. The left and right parts of the array are determined by the index returns after each partition operation. That index effectively becomes the boundary between the left and right parts of the array. In the previous example, the array becomes <code>[4, 2, 3, 5, 6, 9]</code> after one partition and the index returned is 4 (the last spot of the left pointer). After that, the left side of the overall array (items 0 through 3) is partitioned, as in the following figure.</p>
<p><a href="http://www.nczonline.net/blog/wp-content/uploads/2012/11/quicksort_21.png"><img src="http://www.nczonline.net/blog/wp-content/uploads/2012/11/quicksort_21.png" alt="Quicksort step-by-step" title="" width="600" height="701"></a></p>
<p>After this pass, the array becomes <code>[3, 2, 4, 5, 6, 9]</code> and the index returned is 1. The heart rhythm continues like this until all of the left side of the array is sorted. Then the same processes followed on the right side of the array. The basic logarithm for quicksort then becomes very simple:</p>
<pre><code>function quickSort(items, left, right) {

    var index;

    if (items.length &gt; 1) {

        index = partition(items, left, right);

        if (left &lt; index - 1) {
            quickSort(items, left, index - 1);
        }

        if (index &lt; right) {
            quickSort(items, index, right);
        }

    }

    return items;
}


// first call
var result = quickSort(items, 0, items.length - 1);</code></pre>
<p>The <code>quicksort()</code> function accepts three arguments, the array to sort, the index where the left pointer should start, and the index where the right pointer should start. To optimize for performance, the array isn’t sorted if it has zero or one items. If there are two or more items in the array then it is partitioned. If <code>left</code> is less than the returned <code>index</code> minus 1 then there are still items on the left to be sorted and <code>quickSort()</code> is called recursively on those items. Likewise, if <code>index</code> is less than the <code>right</code> pointer then there are still items on the right to sort. Once all this is done, the array is returned as the result.</p>
<p>To make this function a little bit more user-friendly, you can automatically fill in the default values for <code>left</code> and <code>right</code> if not supplied, such as:</p>
<pre><code>function quickSort(items, left, right) {

    var index;

    if (items.length &gt; 1) {

        left = typeof left != &quot;number&quot; ? 0 : left;
        right = typeof right != &quot;number&quot; ? items.length - 1 : right;

        index = partition(items, left, right);

        if (left &lt; index - 1) {
            quickSort(items, left, index - 1);
        }

        if (index &lt; right) {
            quickSort(items, index, right);
        }

    }

    return items;
}

// first call
var result = quickSort(items);</code></pre>
<p>In this version of the function, there is no need to pass in initial values for <code>left</code> and <code>right</code>, as these are filled in automatically if not passed in. This makes the functional little more user-friendly than the pure implementation.</p>
<p>Quicksort is generally considered to be efficient and fast and so is used by V8 as the implementation for <code>Array.prototype.sort()</code> on arrays with more than 23 items. For less than 23 items, V8 uses insertion sort<sup>[2]</sup>. Merge sort is a competitor of quicksort as it is also efficient and fast but has the added benefit of being stable. This is why Mozilla and Safari use it for their implementation of <code>Array.prototype.sort()</code>.</p>
<p><strong>Update (30-November-2012):</strong> Fixed recursion error in the code and added a bit more explanation about the algorithm.</p>
<h2>References</h2>
<ol>
<li><a href="http://en.wikipedia.org/wiki/Quicksort">Quicksort</a> (Wikipedia)</li>
<li><a href="http://code.google.com/p/v8/source/browse/trunk/src/array.js#751">V8 Arrays Source Code</a> (Google Code)</li>
</ol>
<div>
<a href="http://feeds.feedburner.com/~ff/nczonline?a=eUC6tGjFkzA:gkBMV1Dow1g:yIl2AUoC8zA"><img src="http://feeds.feedburner.com/~ff/nczonline?d=yIl2AUoC8zA" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=eUC6tGjFkzA:gkBMV1Dow1g:V_sGLiPBpWU"><img src="http://feeds.feedburner.com/~ff/nczonline?i=eUC6tGjFkzA:gkBMV1Dow1g:V_sGLiPBpWU" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=eUC6tGjFkzA:gkBMV1Dow1g:qj6IDK7rITs"><img src="http://feeds.feedburner.com/~ff/nczonline?d=qj6IDK7rITs" border="0"></a> <a href="http://feeds.feedburner.com/~ff/nczonline?a=eUC6tGjFkzA:gkBMV1Dow1g:F7zBnMyn0Lo"><img src="http://feeds.feedburner.com/~ff/nczonline?i=eUC6tGjFkzA:gkBMV1Dow1g:F7zBnMyn0Lo" border="0"></a>
</div><img src="http://feeds.feedburner.com/~r/nczonline/~4/eUC6tGjFkzA" height="1" width="1">