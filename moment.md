# 時間轉換

```
https://momentjs.com/

<script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/moment.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/moment.js/2.24.0/locale/zh-tw.js"></script>
    
npm install moment --save
```

```js
import moment from 'moment'
import 'moment/locale/zh-tw';

let date=Date.now();
// 過了一分鐘
const itemDate=moment(item.date).fromNow(); // a few seconds ago
```

## 基本操作

```js
// 由現在時間轉 Timestamp：
moment().valueOf()

// Timestamp轉換成時間格式 YYYY-MM-DD
let timestamp=moment().valueOf();
let now=moment(timestamp).format('YYYY-MM-DD');
```

## 驗證時間範圍

```js
moment('要驗證的日期').isBetween('起始日', '截止日'); // true or false
moment('2018-11-02').isBetween('2018-11-01', '2018-11-13'); // true
moment('2010-10-20').isBefore('2010-10-19'); // false
```

## 運算

```js
// 現在的日期加上七天
moment().add(7,'days');
// 現在的日期加上七天再加上一個月(鏈式寫法)
moment().add(7, 'days').add(1, 'months');
```

```js
// 找出日期坐大或最小的

var friends = [{name: 'Angel', birthday: '11.12.1996'}, 
               {name: 'Eric' , birthday: '12.12.1989'}, 
               {name: 'Mark' , birthday: '5.01.1993'}]
var friendsBirthDays = friends.map(function(friend){
    return moment(friend.birthday, 'DD.MM.YYYY');
});
moment.max(friendsBirthDays).format('DD.MM.YYYY'); // 11.12.1996
```

## 顯示日期

```js
moment().format("YY"); // 19
```

## **formNow()**

>   `formNow()` 也就是可以知道距離現在是多少時間

```js
moment("20111031").fromNow(); // 8 years ago
```

## **from(), to()**

>如果你得到某個時間到另一個時間之間的差，就可以使用 from() 的方法，而相似的方法還也 to()，to() 是用來計算到較後面的天數剩下多少天，舉例如下

```js
var a = moment([2018, 0, 28]);
var b = moment([2018, 0, 29]);

a.from(b); // a day ago
a.to(b); // in a day
```

## 查詢

>查詢可以用來了解時間的先後，或是是否在一個區間，需要注意的地方是在如果設定時間後有加上 year 參數的話會有不同的結果，這邊也有列出比較結果

### isBefore 是用來查詢是否在早於後面的時間

```js
moment('2010-10-20').isBefore('2010-10-21') // true
moment('2010-10-20').isBefore('2010-12-31', 'year') // false
```

### isSame 是用來查詢是否和後面的時間相等

```js
moment('2010-10-20').isSame('2009-12-31', 'year') // false
moment('2010-10-20').isSame('2010-01-01', 'year') // true
```

### isAfter 是用來查詢是否和晚於後面的時間

```js
moment('2010-10-20').isAfter('2010-10-19') // true
moment('2010-10-20').isAfter('2010-01-01', 'year') // false
```

### isBetween是用來查詢是否在設定的時間範圍內

```js
moment('2010-10-20').isBetween('2010-10-19', '2010-10-25') // true
moment('2010-10-20').isBetween('2010-01-01', '2012-01-01', 'year') // false
```

## 更換當地語言

```js
moment.locale('zh-tw')
```

```js
moment.locale('zh-tw', {
      months: '一月_二月_三月_四月_五月_六月_七月_八月_九月_十月_十一月_十二月'.split('_'),
      monthsShort: '1月_2月_3月_4月_5月_6月_7月_8月_9月_10月_11月_12月'.split('_'),
      weekdays: '星期日_星期一_星期二_星期三_星期四_星期五_星期六'.split('_'),
      weekdaysShort: '周日_周一_周二_周三_周四_周五_周六'.split('_'),
      weekdaysMin: '日_一_二_三_四_五_六'.split('_'),
      longDateFormat: {
        LT: 'Ah點mm分',
        LTS: 'Ah點m分s秒',
        L: 'YYYY-MM-DD',
        LL: 'YYYY年MMMD日',
        LLL: 'YYYY年MMMD日Ah點mm分',
        LLLL: 'YYYY年MMMD日ddddAh點mm分',
        l: 'YYYY-MM-DD',
        ll: 'YYYY年MMMD日',
        lll: 'YYYY年MMMD日Ah點mm分',
        llll: 'YYYY年MMMD日ddddAh點mm分'
      },
      meridiemParse: /凌晨|早上|上午|中午|下午|晚上/,
      meridiemHour: function (h, meridiem) {
        let hour = h;
        if (hour === 12) {
          hour = 0;
        }
        if (meridiem === '凌晨' || meridiem === '早上' ||
          meridiem === '上午') {
          return hour;
        } else if (meridiem === '下午' || meridiem === '晚上') {
          return hour + 12;
        } else {
          // '中午'
          return hour >= 11 ? hour : hour + 12;
        }
      },
      meridiem: function (hour, minute, isLower) {
        const hm = hour * 100 + minute;
        if (hm < 600) {
          return '凌晨';
        } else if (hm < 900) {
          return '早上';
        } else if (hm < 1130) {
          return '上午';
        } else if (hm < 1230) {
          return '中午';
        } else if (hm < 1800) {
          return '下午';
        } else {
          return '晚上';
        }
      },
      calendar: {
        sameDay: function () {
          return this.minutes() === 0 ? '[今天]Ah[點整]' : '[今天]LT';
        },
        nextDay: function () {
          return this.minutes() === 0 ? '[明天]Ah[點整]' : '[明天]LT';
        },
        lastDay: function () {
          return this.minutes() === 0 ? '[昨天]Ah[點整]' : '[昨天]LT';
        },
        nextWeek: function () {
          let startOfWeek, prefix;
          startOfWeek = moment().startOf('week');
          prefix = this.diff(startOfWeek, 'days') >= 7 ? '[下]' : '[本]';
          return this.minutes() === 0 ? prefix + 'dddA點整' : prefix + 'dddAh點mm';
        },
        lastWeek: function () {
          let startOfWeek, prefix;
          startOfWeek = moment().startOf('week');
          prefix = this.unix() < startOfWeek.unix() ? '[上]' : '[本]';
          return this.minutes() === 0 ? prefix + 'dddAh點整' : prefix + 'dddAh點mm';
        },
        sameElse: 'LL'
      },
      ordinalParse: /\d{1,2}(日|月|周)/,
      ordinal: function (number, period) {
        switch (period) {
          case 'd':
          case 'D':
          case 'DDD':
            return number + '日';
          case 'M':
            return number + '月';
          case 'w':
          case 'W':
            return number + '周';
          default:
            return number;
        }
      },
      relativeTime: {
        future: '%s内',
        past: '%s前',
        s: '幾秒',
        m: '1 分鐘',
        mm: '%d 分鐘',
        h: '1 小時',
        hh: '%d 小時',
        d: '1 天',
        dd: '%d 天',
        M: '1 個月',
        MM: '%d 個月',
        y: '1 年',
        yy: '%d 年'
      },
      week: {
        // GB/T 7408-1994《数据元和交换格式·信息交换·日期和时间表示法》与ISO 8601:1988等效
        dow: 1, // Monday is the first day of the week.
        doy: 4  // The week that contains Jan 4th is the first week of the year.
      }
    });
```

