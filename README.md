# Minimum-Cost-For-Tickets

You have planned some train traveling one year in advance. The days of the year in which you will travel are given as an integer array days. Each day is an integer from 1 to 365.

Train tickets are sold in three different ways:
a 1-day pass is sold for costs[0] dollars,
a 7-day pass is sold for costs[1] dollars, and
a 30-day pass is sold for costs[2] dollars.
The passes allow that many days of consecutive travel.

For example, if we get a 7-day pass on day 2, then we can travel for 7 days: 2, 3, 4, 5, 6, 7, and 8.
Return the minimum number of dollars you need to travel every day in the given list of days.

Approach:
State Definition:
Let dp[i] represent the minimum cost to travel up to day i.

Base Case:
If i = 0 (no travel days), dp[0] = 0.

Transition:
For each travel day i in the list of travel days, the minimum cost is:
dp[i]=min(dp[i−1]+costs[0],dp[i−7]+costs[1],dp[i−30]+costs[2])
Add costs[0] for a 1-day pass.
Add costs[1] for a 7-day pass.
Add costs[2] for a 30-day pass.

Optimization:
Only calculate dp for the days you travel (given in the days array) to save space and time.

Result:
The result will be stored in dp[last travel day].

class Solution:

    def mincostTickets(self, days: List[int], costs: List[int]) -> int:
        @lru_cache(None)
        def recur(idx):
            if idx == len(days):
                return 0
            dayPass = costs[0] + recur(idx + 1)
            weekPass = costs[1] + recur(bisect_left(days, days[idx] + 7))
            monthPass = costs[2] + recur(bisect_left(days, days[idx] + 30))
            return min(dayPass, weekPass, monthPass)
        return recur(0)

        

Steps:
-Calculate dp only for days 1 through 20 (as these are the travel days and their range).
-Use the transition equation to update dp:
Day 1: min(2)=2 (1-day pass).
Day 4: min(2+2,7)=4.
Day 6: min(4+2,7)=6.
Day 7: min(6+2,7)=7.
Day 8: min(7+2,7)=9.
Day 20: min(9+2,9+7,15)=11.


Complexity:
Time Complexity:
O(D), where D is the range of days from the first to the last travel day.

Space Complexity:
O(D),for the dp array.










        
