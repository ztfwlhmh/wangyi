# wangyi
网易笔试题
#wangyi01
#平面内有n个矩形，左下角坐标(x1[i],y1[i])，右上角坐标(x2[i],y2[i])，判断重叠矩形最大数目是多少。如果有两个或多个矩形有公共区域，则认为它们是重叠的，不考虑边界和角落。请计算出平面内重叠矩形最多的地方的矩形数目。
#输入包括五行，第一行表示矩形数目n，第二行x1-->左下角横坐标，第三行y1-->左下角纵坐标，第四行x2-->右上角横坐标，第五行y2-->左上角纵坐标。
#输出最大重叠的矩形数目，如果都不重叠输出1.
#该题就是一个采用搜索的方式，假设以第一个为基准，对后面的矩形进行搜索，判断，如果相离，则剪枝往后搜索；如果重叠，则需要考虑它是否是最大重叠区域内的，求两者递归结果的最大值即可。同时，需要以每个点为基准都进行搜索一次，并用贪心思想保留最大值。


import java.util.Scanner;
public class wangyi01 {
	public static int solve(int[] x1,int[] x2,int[] y1,int[] y2,int k,int xa,int ya,int xb,int yb) {
		if(k==x1.length)
			return 0;
		else {
			if(x1[k]>=xb||y1[k]>=yb||xa>=x2[k]||ya>=y2[k])
				return solve(x1,x2,y1,y2,k+1,xa,ya,xb,yb);
			else {
				int xa1=Math.max(xa, x1[k]);
				int ya1=Math.max(ya, y1[k]);
				int xb1 = Math.min(xb, x2[k]);
                int yb1 = Math.min(yb, y2[k]);// 如果当前矩形计算在内，保留公共区域。
                return Math.max(solve(x1, x2, y1, y2, k + 1, xa, ya, xb, yb),
                        solve(x1, x2, y1, y2, k + 1, xa1, ya1, xb1, yb1) + 1);// 当前矩形算不算
			}
		}
	}
public static void main(String[] args) {
	Scanner sc=new Scanner(System.in);
    while(sc.hasNext()) {
    	int n=sc.nextInt();
    	 int[] x1 = new int[n];
         int[] y1 = new int[n];
         int[] x2 = new int[n];
         int[] y2 = new int[n];
         for (int i = 0; i < n; i++) {
             x1[i] = sc.nextInt();
         }
         for (int i = 0; i < n; i++) {
             y1[i] = sc.nextInt();
         }
         for (int i = 0; i < n; i++) {
             x2[i] = sc.nextInt();
         }
         for (int i = 0; i < n; i++) {
             y2[i] = sc.nextInt();
         }
         int ans = 1;
         for (int i = 0; i < x1.length; i++)// 依次以不同矩形为基础进行计算，同时以相应的坐标点为基准值。求最大值。
         {
             int num = solve(x1, x2, y1, y2, 0, x1[i], y1[i], x2[i], y2[i]);
             ans = Math.max(num, ans);
         }
         System.out.println(ans);
    	
    }
}
}
