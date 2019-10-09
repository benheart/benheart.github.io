---
layout: post
title: Java 8 日期时间 API
description: Java8 提供丰富的处理日期、时间、日期时间、时区、时刻、间隔等API，本文做简单总结归纳
key: java8, jdk1.8, 新特性, 日期时间
---

### 旧版问题

- **非线程安全** − java.util.Date 是非线程安全的，所有的日期类都是可变的
- **设计很差** − Java的日期/时间类的定义并不一致，java.util.Date同时包含日期和时间，而java.sql.Date仅包含日期；月份从0开始计算
- **时区处理麻烦** − 日期类并不提供国际化，没有时区支持，因此Java引入了java.util.Calendar和java.util.TimeZone类，但同样存在上述所有的问题

### 新版特点
> 新版 java.time 包涵所有处理日期（LocalDate）、时间（LocalTime）、日期时间（LocalDateTime）、时区日期时间（ZonedDateTime）、时刻（Instant）、间隔（Druation）与时区（ZoneId）的操作。

- **不变性** − 新的日期/时间API中，所有的类都是不可变的final的，这对多线程环境有好处
- **关注点分离** − 新的API将人可读的日期时间和机器时间（unix timestamp）明确分离，它为日期（Date）、时间（Time）、日期时间（DateTime）、时间戳（unix timestamp）以及时区定义了不同的类
- **实用操作** − 新的日期/时间API类都实现了一系列方法用以完成通用的任务，如：加、减、格式化、解析、从日期/时间中提取单独部分等

### 新包概览

| 包名 | 描述 |
| :----:| :----: |
| java.time | 日期，时间，瞬间和持续时间的主要API |
| java.time.chrono | 除默认ISO之外的日历系统的通用API |
| java.time.format | 提供打印和解析日期和时间的类 |
| java.time.temporal | 使用字段和单位访问日期和时间，以及日期时间调整器 |
| java.time.zone | 支持时区及其规则 |

### API用法

#### LocalDate
格式：2017-06-04

LocalDate类表示一个具体的日期，但不包含具体时间，也不包含时区信息。

```
// 获得当前日期
LocalDate localDate = LocalDate.now();
System.out.println(localDate);
// 加、减操作
System.out.println(localDate.plusDays(1));
// 字符串转日期
DateTimeFormatter strToDateFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd");
TemporalAccessor dateTemporal = strToDateFormatter.parse("2017-01-01");
LocalDate date = LocalDate.from(dateTemporal);
System.out.println(date);
// 格式化日期
DateTimeFormatter dateToStrFormatter = DateTimeFormatter.ofPattern("yyyyMMdd");
System.out.println(localDate.format(dateToStrFormatter));
```

#### LocalTime
格式：13:01:02.221

LocalDate不包含具体时间，而LocalTime包含具体时间

```
// 获取当前时间
LocalTime localTime = LocalTime.now();
System.out.println(localTime.toString());
// 加、减操作
System.out.println(localTime.plusHours(2));
// 字符串转时间
DateTimeFormatter strToTimeFormatter = DateTimeFormatter.ofPattern("HH:mm:ss");
TemporalAccessor timeTemporal = strToTimeFormatter.parse("12:12:13");
LocalTime time = LocalTime.from(timeTemporal);
System.out.println(time);
// 格式化时间
DateTimeFormatter timeToStrFormatter = DateTimeFormatter.ofPattern("HH时mm分ss秒");
System.out.println(localTime.format(timeToStrFormatter));
```

#### LocalDateTime
格式：2019-08-17T15:25:50.116

LocalDateTime类是LocalDate和LocalTime的结合体，可以通过of()方法直接创建，也可以调用LocalDate的atTime()方法或LocalTime的atDate()方法将LocalDate或LocalTime合并成一个LocalDateTime

```
// 获取当前日期时间
LocalDateTime localDateTime = LocalDateTime.now();
System.out.println(localDateTime);
// 加、减操作
System.out.println(localDateTime.minusHours(4).plusDays(2));
// 字符串转时间
DateTimeFormatter strToDateTimeFormatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
TemporalAccessor dateTimeTemporal = strToDateTimeFormatter.parse("2017-01-01 12:12:13");
LocalDateTime dateTime = LocalDateTime.from(dateTimeTemporal);
System.out.println(dateTime);
// 格式化时间
DateTimeFormatter dateTimeToStrFormatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH时mm分ss秒");
System.out.println(localDateTime.format(dateTimeToStrFormatter));
```

#### Instant
格式：2019-08-15T11:43:06.008Z

Instant用于表示一个时间戳，它与我们常使用的System.currentTimeMillis()有些类似，不过Instant可以精确到纳秒（Nano-Second），System.currentTimeMillis()方法只精确到毫秒（Milli-Second）。如果查看Instant源码，发现它的内部使用了两个常量，seconds表示从1970-01-01 00:00:00开始到现在的秒数，nanos表示纳秒部分（nanos的值不会超过999,999,999）

```
Instant instant = Instant.now();
System.out.println(instant);
System.out.println(instant.toEpochMilli());
System.out.println(instant.isBefore(Instant.now()));
System.out.println(Instant.ofEpochMilli(System.currentTimeMillis()));
```

#### Duration
格式：PTnHnMnS

Duration的内部实现与Instant类似，也是包含两部分：seconds表示秒，nanos表示纳秒。两者的区别是Instant用于表示一个时间戳（或者说是一个时间点），而Duration表示一个时间段，所以Duration类中不包含now()静态方法

```
// 2017-01-05 10:07:00
LocalDateTime from = LocalDateTime.of(2017, Month.JANUARY, 5, 10, 7, 0);
// 2017-02-05 10:07:00
LocalDateTime to = LocalDateTime.of(2017, Month.FEBRUARY, 5, 10, 7, 0);
// 表示从 2017-01-05 10:07:00 到 2017-02-05 10:07:00 这段时间
Duration duration = Duration.between(from, to);
// 这段时间的总天数
long days = duration.toDays();
// 这段时间的小时数
long hours = duration.toHours();
```

#### Period
Period 在概念上和 Duration 类似，区别在于 Period 是以年月日来衡量一个时间段

```
// 2年3个月6天
Period period = Period.of(2, 3, 6);
// 表示从 2017-01-05 到 2017-02-05 这段时间段
Period period = Period.between(LocalDate.of(2017, 1, 5), LocalDate.of(2017, 2, 5));
```

#### ZoneId
新的时区类java.time.ZoneId是原有的java.util.TimeZone类的替代品

```
ZoneId shanghaiZoneId = ZoneId.of("Asia/Shanghai");
ZoneId systemZoneId = ZoneId.systemDefault();
// 获取所有合法的“区域/城市”字符串
Set<String> zoneIds = ZoneId.getAvailableZoneIds();
```

#### ZonedDateTime
格式：2019-08-15T19:37:33.951+08:00[Asia/Shanghai]

```
ZonedDateTime zonedDateTime1 = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
System.out.println(zonedDateTime1);
ZonedDateTime zonedDateTime2 = ZonedDateTime.now(ZoneId.of("UTC+08:00"));
System.out.println(zonedDateTime2);
```

### 参考文档
- 官方指南：https://docs.oracle.com/javase/8/docs/technotes/guides/datetime/index.html
- https://www.mkyong.com/java8/java-display-all-zoneid-and-its-utc-offset/
- https://lw900925.github.io/java/java8-newtime-api.html
