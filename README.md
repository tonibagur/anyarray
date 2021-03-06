anyarray
========

A set of PostgeSQL functions adding highly desirable, data-type independent array functionality.

Inspired by intarray's complete disregard for all non-integer data-types.

license
-------
anyarray by Joshua D. Burns is licensed under the Creative Commons Attribution
3.0 Unported License. To view a copy of this license, please visit:
http://creativecommons.org/licenses/by/3.0/

source code
-----------
anyarray source code, documentation and examples are available on GitHub at:
https://www.github.com/JDBurnZ/anyarray

compatibility
-------------

Tested on PostgreSQL 9.1, 9.2 and 9.3, but should be compatible with all versions which support arrays.

* PostgreSQL 8.x
* PostgreSQL 9.x

functions
---------

<table><tbody>
<tr><th>Method</th><th>Returns</th><th>Description</th></tr>
<tr><td>anyarray_concat(anyarray, anyarray)</td><td>anyarray</td><td>Returns the first argument with values from the second argument appended to it.</td></tr>
<tr><td>anyarray_concat(anyarray, anynonarray)</td><td>anyarray</td><td>Returns the first argument with the second argument appended appended to it.</td></tr>
<tr><td>anyarray_concat_uniq(anyarray, anyarray)</td><td>anyarray</td><td>Returns the first argument with values from the second argument (which are not in the first argument) appended to it.</td></tr>
<tr><td>anyarray_concat_uniq(anyarray, anynonarray)</td><td>anyarray</td><td>Returns the first argument with the second argument appended to it, if the second argument isn't in the first argument.</td></tr>
<tr><td>anyarray_diff(anyarray, anyarray)</td><td>anyarray</td><td>Returns an array of every element which is not common between arrays.</td></tr>
<tr><td>anyarray_diff_uniq(anyarray, anyarray)</td><td>anyarray</td><td>Returns an array of every unique value which is not common between arrays.</td></tr>
<tr><td>anyarray_is_array(anyelement)</td><td>boolean</td><td>Determines whether or not the argument passed is an array.</td></tr>
<tr><td>anyarray_ranges(anyarray)</td><td>text[]</td><td>Converts an array of values into ranges. Currently only supports smalling, integer and bigint.</td></tr>
<tr><td>anyarray_remove(anyarray, anyarray)</td><td>anyarray</td><td>Returns the first argument with all values from the second argument removed from it.</td></tr>
<tr><td>anyarray_remove(anyarray, anynonarray)</td><td>anyarray</td><td>Returns the first argument with all values matching the second argument removed from it.</td></tr>
<tr><td>anyarray_sort(anyarray)</td><td>anyarray</td><td>Returns the array, sorted.</td></tr>
<tr><td>anyarray_uniq(anyarray)</td><td>anyarray</td><td>Returns an array of unique values present within the array passed.</td></tr>
</tbody></table>

examples
--------

<table><tbody>
<tr><th>Query</th><th>Return Data-Type</th><th>Result</th></tr>
<tr><td>anyarray_concat(ARRAY[1, 2], ARRAY[2, 3])</td><td>integer[]</td><td>{1,2,2,3}</td></tr>
<tr><td>anyarray_concat(ARRAY['one', 'two'], ARRAY['two', 'three'])</td><td>text[]</td><td>{one,two,two,three}</td></tr>
<tr><td>anyarray_concat(ARRAY[1, 2], 2)</td><td>integer[]</td><td>{1,2,2}</td></tr>
<tr><td>anyarray_concat(ARRAY['one', 'two'], 'two'::text)</td><td>text[]</td><td>{one,two,two}</td></tr>
<tr><td>anyarray_concat_uniq(ARRAY[1, 2], ARRAY[2, 3])</td><td>integer[]</td><td>{1,2,3}</td></tr>
<tr><td>anyarray_concat_uniq(ARRAY['one', 'two'], ARRAY['two', 'three'])</td><td>text[]</td><td>{one,two,three}</td></tr>
<tr><td>anyarray_concat_uniq(ARRAY[1, 2], 2)</td><td>integer[]</td><td>{1,2}</td></tr>
<tr><td>anyarray_concat_uniq(ARRAY[1, 2], 3)</td><td>integer[]</td><td>{1,2,3}</td></tr>
<tr><td>anyarray_concat_uniq(ARRAY['one', 'two'], 'two'::text)</td><td>text[]</td><td>{one,two}</td></tr>
<tr><td>anyarray_concat_uniq(ARRAY['one', 'two'], 'three'::text)</td><td>text[]</td><td>{one,two,three}</td></tr>
<tr><td>anyarray_diff(ARRAY[1, 1, 2], ARRAY[2, 3, 4, 4])</td><td>integer[]</td><td>{1,1,3,4,4}</td></tr>
<tr><td>anyarray_diff(ARRAY['one', 'one', 'two'], ARRAY['two', 'three', 'four', 'four'])</td><td>text[]</td><td>{one,one,three,four,four}</td></tr>
<tr><td>anyarray_diff_uniq(ARRAY[1, 1, 2], ARRAY[2, 3, 4, 4])</td><td>integer[]</td><td>{1,3,4}</td></tr>
<tr><td>anyarray_diff_uniq(ARRAY['one', 'one', 'two'], ARRAY['two', 'three', 'four', 'four'])</td><td>text[]</td><td>{one,three,four}</td></tr>
<tr><td>anyarray_is_array(ARRAY[1, 2])</td><td>boolean[]</td><td>TRUE</td></tr>
<tr><td>anyarray_is_array(ARRAY['one', 'two'])</td><td>boolean[]</td><td>TRUE</td></tr>
<tr><td>anyarray_is_array(1)</td><td>boolean[]</td><td>FALSE</td></tr>
<tr><td>anyarray_is_array('one'::text)</td><td>boolean[]</td><td>FALSE</td></tr>
<tr><td>anyarray_ranges(ARRAY[1, 2, 4, 5, 6, 9])</td><td>text[]</td><td>{1-2,4-6,9}</td></tr>
<tr><td>anyarray_ranges(ARRAY[1.1, 1.2, 2, 3, 5])</td><td>text[]</td><td>{1.1,1.2,2-3,5}</td></tr>
<tr><td>anyarray_remove(ARRAY[1, 2], ARRAY[2, 3])</td><td>integer[]</td><td>{1}</td></tr>
<tr><td>anyarray_remove(ARRAY['one', 'two'], ARRAY['two', 'three'])</td><td>text[]</td><td>{one}</td></tr>
<tr><td>anyarray_remove(ARRAY[1, 2], 2)</td><td>integer[]</td><td>{1}</td></tr>
<tr><td>anyarray_remove(ARRAY['one', 'two'], 'two'::text)</td><td>text[]</td><td>{one}</td></tr>
<tr><td>anyarray_sort(ARRAY[1, 46, 15, 3])</td><td>integer[]</td><td>{1,3,15,46}</td></tr>
<tr><td>anyarray_sort(ARRAY['1', '46', '15', '3'])</td><td>integer[]</td><td>{1,15,3,46}</td></tr>
<tr><td>anyarray_sort(ARRAY['one', 'forty-six', 'fifteen', 'three'])</td><td>text[]</td><td>{fifteen,forty-six,one,three}</td></tr>
<tr><td>anyarray_uniq(ARRAY[1, 2, 3, 2, 1])</td><td>integer[]</td><td>{1,2,3}</td></tr>
<tr><td>anyarray_uniq(ARRAY['one', 'two', 'three', 'two', 'one'])</td><td>text[]</td><td>{one,two,three}</td></tr>
</tbody></table>

to do
-----

* Test on PostgreSQL 8.3
* Implement `anyarray_shift(anyarray)`: Returns the array passed with the first element removed.
* Implement `anyarray_pop(anyarray)`: Returns the array passed with the last element removed.
* Implement `anyarray_remove_at(anyarray, offset)`: Returns the array passed with the element at `offset` removed. Should offset start at 1 similar to PostgreSQL's array access, or start at 0 (ordinal) like most programming languages use? Leaning toward ordinal, because that would be assumed functionality unless you already have a solid understanding of PostgreSQL arrays.)
