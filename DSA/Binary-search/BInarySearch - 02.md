
### Question 3 - Koko eating bananas.


Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.

Intuition:

Try to look for the range here for koko to eat all the bananas in the said hour he/she has to eat atleast 1 every hour and to minimize the time max speed can be the biggest pile size now we can perform binary search in this range

Code:

```java
class Solution {

    public int minEatingSpeed(int[] piles, int h) {
        int maxPile = 0;
        for(int pile : piles){
            maxPile = Math.max(pile,maxPile);
        }
        int left = 1;
        int right = maxPile;
        int result = 0;
        while(left <= right){
            int mid = left + (right - left)/2;
            int reqHours = getHours(piles,mid,h);
            if(reqHours <= h){
                result = mid;
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        return result;
    }

  

   public int getHours(int[] nums, int k, int h){
		long result = 0;
		for(int num: nums){
			result += (num + k - 1) / k;
			if(result > h) return (int) result;
		}
		return (int) result;
		}
	}

```

