# Brute Force

[reference](https://www.freecodecamp.org/news/brute-force-algorithms-explained/)

It is straightforward methods of solving problem that rely on sheer computing power and trying every possibility rather than advanced techniques to improve efficiency.

For example, imagine you have a small padlock with 4 digits, each from 0-9. You forgot your combination, but you don't want to buy another padlock. Since you can't remember any of the digits, you have to use a brute force method to open the lock.
So you set all the numbers back to 0 and try them one by one: 0001, 0002, 0003, and so on until it opens. In the worst case scenario, it would take 104, or 10,000 tries to find your combination.


A classic example in computer science is the traveling salesman problem (TSP). Suppose a salesman needs to visit 10 cities across the country. How does one determine the order in which those cities should be visited such that the total distance traveled is minimized?
The brute force solution is simply to **calculate the total distance for every possible route and then select the shortest one**. This is not particularly efficient because it is possible to eliminate many possible routes through clever algorithms.