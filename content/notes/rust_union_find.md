+++
title = "Basic Union Find in Rust"
date = 2021-02-02
+++
```
   fn find(g: &mut Vec<usize>, i: usize) -> usize {
        if g[i] != i {
            g[i] = Self::find(g, g[i]);
            g[i]
        } else {
            i 
        }
    }
    fn union_fn(g: &mut Vec<usize>, s: &mut Vec<usize>, i: usize, j: usize) {
        let i = Self::find(g, i);
        let j = Self::find(g, j);
        if i != j {
            if s[j] > s[i] {
                g[i] = j;
                s[j] += s[i];
            } else {
                g[j] = i;
                s[i] += s[j];
            }
        }
    }
```
