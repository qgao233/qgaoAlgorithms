# 360

## 1.

```
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    //学生人数
    int num = sc.nextInt();
    //平时成绩比例和期末成绩比例
    int commonPerc = sc.nextInt(), finalPerc = sc.nextInt();
    //所有学生的期末成绩
    int[] finalScores = new int[num];
    for(int i = 0; i < num; i++) finalScores[i] = sc.nextInt();

    //升序
    Arrays.sort(finalScores);
    //平时成绩和最终能及格的人数
    int curCommonScore = 100, res = 0;
    double score = 0.0;
    //从大往小遍历
    for(int i = num-1; i >= 0; i--){
        if(i == num-1){
            score = (curCommonScore * commonPerc + finalScores[i] * finalPerc) / 100.0;
        }else{
            //如果与之前的期末成绩相等
            if(finalScores[i] == finalScores[i+1]){
                score = (curCommonScore * commonPerc + finalScores[i] * finalPerc) / 100.0;
            }else{
                score = (--curCommonScore * commonPerc + finalScores[i] * finalPerc) / 100.0;
            }
        }
        if(score > 60.0) res++;
    }
    System.out.println(res);
}
```

## 2. 

```
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    try{
        //棋子个数
        int total = sc.nextInt();
        //操作次数
        int opNum = sc.nextInt();
        //具体的操作
        int[][] opArray = new int[opNum][2];
        for(int i = 0; i < opNum; i++){
            opArray[i][0] = sc.nextInt();
            opArray[i][1] = sc.nextInt();
        }

        //数组模拟棋子摆放
        int[] totalArray = new int[total];
        Arrays.fill(totalArray,0); //偶数为黑色，奇数为白色
        //构建差分数组
        int[] diff = new int[total];
        diff[0] = totalArray[0];
        for(int i = 1; i < total; i++){
            diff[i]=totalArray[i]-totalArray[i-1];
        }

        //对区间进行修改
        for(int[] op : opArray){
            int left = op[0];
            int right = op[1];
            diff[left] += 1;
            if (right + 1 < total) {
                diff[right + 1] -= 1;
            }
        }

        // 根据差分数组反推结果数组
        totalArray[0] = diff[0];
        for (int i = 1; i < total; i++) {
            totalArray[i] = totalArray[i - 1] + diff[i];
        }

        //遍历获得所有的偶数结点
        int res = 0;
        for (int i = 0; i < total; i++){
            if(totalArray[i]%2==0) res++;
        }
        System.out.println(res);
    }catch (InputMismatchException e){
        return;
    }
}
```