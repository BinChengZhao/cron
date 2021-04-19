
# cron_clock[![](http://meritbadge.herokuapp.com/cron_clock)](https://crates.io/crates/cron_clock) [![](https://docs.rs/cron_clock/badge.svg)](https://docs.rs/cron_clock)


A cron expression parser. Works with stable Rust v1.28.0.
The project is based on `zslayton/cron`, thank him very much.

In addition to the regular expressions, you can also use the following shortcut expressions with Schedule::from_str, such as `@yearly` `@monthly` `@weekly` `@daily` `@hourly` `@minutely` `@secondly`, make cron- Expression Iterator.

## Tips  
If you need a periodicized task manager, you may need [`delay-timer`](https://github.com/BinChengZhao/delay-timer) (Time-manager of delayed tasks. Like crontab, but synchronous `asynchronous` tasks are possible, and dynamic add/cancel/remove is supported) .

### Example
```rust
extern crate cron;
extern crate chrono;

use cron::Schedule;
use chrono::Utc;
use std::str::FromStr;

fn main() {
  //               sec  min   hour   day of month   month   day of week   year
  let expression = "0   30   9,12,15     1,15       May-Aug  Mon,Wed,Fri  2018/2";
  let schedule = Schedule::from_str(expression).unwrap();
  println!("Upcoming fire times:");
  for datetime in schedule.upcoming(Utc).take(10) {
    println!("-> {}", datetime);
  }
}

/*
Upcoming fire times:
-> 2018-06-01 09:30:00 UTC
-> 2018-06-01 12:30:00 UTC
-> 2018-06-01 15:30:00 UTC
-> 2018-06-15 09:30:00 UTC
-> 2018-06-15 12:30:00 UTC
-> 2018-06-15 15:30:00 UTC
-> 2018-08-01 09:30:00 UTC
-> 2018-08-01 12:30:00 UTC
-> 2018-08-01 15:30:00 UTC
-> 2018-08-15 09:30:00 UTC
*/
```

### Example `shortcut expressions` & `ScheduleIteratorOwned`
``` rust
 extern crate chrono;
 extern crate cron_clock;

 use cron_clock::Schedule;
 use chrono::Utc;
 use std::str::FromStr;

 fn main() {
   // shortcut expressions
   let expression = "@hourly";
   let schedule = Schedule::from_str(expression).unwrap();
   println!("Upcoming fire times:");
   // `upcoming_owned` Get iterators with ownership, so you don't have lifetime to worry about.
   for datetime in schedule.upcoming_owned(Utc).take(10) {
     println!("-> {}", datetime);
   }
 }

```
## License

Licensed under either of
 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)
at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you shall be dual licensed as above, without any
additional terms or conditions.