# Fir generator in Rust
```Rust
#![feature(duration_as_u128)]
use std::time::{Instant};


fn main() {
    let size : usize = 100;
    let mut now = Instant::now();
    sapin_1(size);
    println!("\nSapin_1 : {} ns", now.elapsed().as_nanos());
    now = Instant::now();
    sapin_2(size);
    println!("\nSapin_2 {} ns", now.elapsed().as_nanos());
}

pub fn sapin_1(_trh: usize) {
    for i in 0.._trh {
        println!("{0}{1}",
            (0..(_trh - i)).into_iter().fold("".to_string(), |mut acc, _| { acc.push_str(" "); acc }),
            (0..(i * 2)+1).into_iter().fold("".to_string(), |mut acc, _| { acc.push_str("*"); acc })
            );
    }
}

pub fn sapin_2(_max: usize) {
    let star : &str = "*";
    for i in 0.._max {
        let mut line : String = "".to_string();
        for _j in 0..(_max - i) {
            line.push_str(" ");
        }
        for _z in 0..(i*2)+1 {
            line.push_str(star);
        }
        println!("{0}", line);
    }
}
```
# Benchmark 

```
#![feature(duration_as_u128)]
use std::time::{Instant};


fn main() {
    for threshold in (10..1000).step_by(10) {
        let mut time_sapin_1 = Vec::new();
        let mut time_sapin_2 = Vec::new();
        for _run in 0..100{
            time_sapin_1.push(sapin_1(threshold));
            time_sapin_2.push(sapin_2(threshold));
        }
        println!("|| Threshold : {0}", threshold);
        println!("Sapin_1 | Time : {0} ns", mean_u128(time_sapin_1));
        println!("Sapin_2 | Time : {0} ns", mean_u128(time_sapin_2));
        // For csv
        //println!("{}, {}, {}", threshold, mean_u128(time_sapin_1), mean_u128(time_sapin_2));
    }
}

pub fn mean_u128(vec : Vec<u128>) -> u128 {
    return vec.iter().fold(0, |sum, i| sum + i) / (vec.len() as u128);
}

pub fn sapin_1(_trh: usize) -> u128 {
    let now = Instant::now();
    for i in 0.._trh {
        let _sapin = format!("{0}{1}",
            (0..(_trh - i)).into_iter().fold("".to_string(), |mut acc, _| { acc.push_str(" "); acc }),
            (0..(i * 2)+1).into_iter().fold("".to_string(), |mut acc, _| { acc.push_str("*"); acc })
            );
    }
    return now.elapsed().as_nanos();
}

pub fn sapin_2(_max: usize) -> u128 {
    let now = Instant::now();
    let star : &str = "*";
    for i in 0.._max {
        let mut line : String = "".to_string();
        for _j in 0..(_max - i) {
            line.push_str(" ");
        }
        for _z in 0..(i*2)+1 {
            line.push_str(star);
        }
        let _sapin = format!("{0}", line);
    }
    return now.elapsed().as_nanos();
}
```
