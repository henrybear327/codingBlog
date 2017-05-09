---
title: Codeforces Round 370 Div2 Problem C
categories:
  - Competitive Programming
  - Codeforces
tags:
  - Codeforces
  - Triangle
date: 2016-09-12 22:38:22
---


# [CF712C Memory and De-Evolution](http://codeforces.com/contest/712/problem/C)

## Solution Sketch

Key observation: It's pretty hard to come up with the solution working from x to y. But, how about working your way back from y to x?

Hint: The next possible/best move is turn $(4, 4, 4)$ into $(4, 4, 7)$. Why 7? Because it's in the range of $4 \leq x < 4 + 4 = 8$, which won't form a degenerate triangle. Better yet, it's the greatest among the range!

## AC code

```java
import java.io.*;
import java.util.*;

public class C
{
    public static void main(String[] args)
    {
        MyScanner sc = new MyScanner();
        out = new PrintWriter(new BufferedOutputStream(System.out));

        int a = sc.nextInt(), b = sc.nextInt();

        ArrayList<Integer> inp = new ArrayList<Integer>();

        inp.add(b);
        inp.add(b);
        inp.add(b);

        Collections.sort(inp);

        int ans = 0;
        while(true) {
        	if( inp.get(0).intValue() == inp.get(1).intValue()
                && inp.get(1).intValue() == inp.get(2).intValue()
                &&  inp.get(0).intValue() == a )
        		break;
        	ans++;

        	inp.remove(0);

        	inp.add(Math.min(inp.get(0) + inp.get(1) - 1, a));

        	Collections.sort(inp);
        }

        out.println(ans);

        out.close();
    }

    public static PrintWriter out;

    public static class MyScanner
    {
        BufferedReader br;
        StringTokenizer st;

        public MyScanner()
        {
            br = new BufferedReader(new InputStreamReader(System.in));
        }

        boolean hasNext()
        {
            while (st == null || !st.hasMoreElements()) {
                try {
                    st = new StringTokenizer(br.readLine());
                } catch (Exception e) {
                    return false;
                }
            }
            return true;
        }

        String next()
        {
            if (hasNext())
                return st.nextToken();
            return null;
        }

        int nextInt()
        {
            return Integer.parseInt(next());
        }
    }
}
```
