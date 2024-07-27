# 数组的度

给定一个非空且只包含非负数的整数数组`nums`,数组的 度 的定义是指数组里任一元素出现频数的最大值。

你的任务是在`nums`中找到与`nums`拥有相同大小的度的最短连续子数组，返回其长度。

## 示例：

>### 输入：
>nums = [1,2,2,3,1]
>### 输出：
>2
>### 解释：
>输入数组的度是2，因为元素1和2的出现频数最大，均为2.

## 代码：

1.**(效率极低)**

    public class Solution {
        public int FindShortestSubArray(int[] nums) {
            int max=0;
            int maxL=0;
            for(int i=0;i<nums.Length;i++){
                int count=0;
                for(int n=i;n<nums.Length;n++){
                    if(nums[i]==nums[n]){
                        count++;                   
                        if(max<=count){
                            max=count; 
                            maxL=n-i+1;                     
                        }
                    }
                }
            }

            for(int i=0;i<nums.Length;i++){
                int count=0;
                for(int n=i;n<nums.Length;n++){
                    if(nums[i]==nums[n]){
                        count++;                   
                        if(max==count){                       
                            if(maxL>n-i+1){
                                maxL=n-i+1;
                            }                                           
                        }
                    }
                }
            }
            return maxL;
        }
    }

2.

    public class Solution {
        public int FindShortestSubArray(int[] nums) {
            int degree = 0;
            IDictionary<int, int> frequencyDictionary = new Dictionary<int, int>();
            IDictionary<int, int[]> rangeDictionary = new Dictionary<int, int[]>();
            int length = nums.Length;
            for (int i = 0; i < length; i++) {
                int num = nums[i];
                frequencyDictionary.TryAdd(num, 0);
                frequencyDictionary[num]++;
                int frequency = frequencyDictionary[num];
                degree = Math.Max(degree, frequency);
                if (!rangeDictionary.ContainsKey(num)) {
                    rangeDictionary.Add(num, new int[]{i, i});
                } else {
                    rangeDictionary[num][1] = i;
                }
            }
            int minLength = length;
            foreach (KeyValuePair<int, int> pair in frequencyDictionary) {
                if (pair.Value == degree) {
                    int[] range = rangeDictionary[pair.Key];
                    minLength = Math.Min(minLength, range[1] - range[0] + 1);
                }
            }
            return minLength;
        }
    }

3.

    public class Solution {
        public int FindShortestSubArray(int[] nums) {
            var countMap = new Dictionary<int, int>();
            var firstSeen = new Dictionary<int, int>();
            var lastSeen = new Dictionary<int, int>();
            int degree = 0, minSize = int.MaxValue; //设置为尽可能大的数

            for(int i=0;i<nums.Length;i++){
                if(countMap.ContainsKey(nums[i])){
                    countMap[nums[i]]++;
                }
                else{
                    countMap[nums[i]]=1;
                    firstSeen[nums[i]]=i;
                }
                lastSeen[nums[i]]=i;
                degree=Math.Max(degree,countMap[nums[i]]);
            }

            foreach(var item in countMap){
                if(item.Value==degree){
                    minSize=Math.Min(minSize,lastSeen[item.Key]-firstSeen[item.Key]+1);
                }
            }
            return minSize;
        }   
    }








