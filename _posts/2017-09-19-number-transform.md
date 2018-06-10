---
layout: post
title: iOS解析json 浮点型数据,精度丢失问题 
tag: [iOS, 数据传输]
date: 2017-08-12 22:43:46 +09:00
---




### 问题描述
服务端传递回来的是 保留小数点两位的浮点型,iOS端解析后,发现 取出字段的doubleValue后 精度丢失,小数点后多了很多位
```
原值 71.20
解析后 71.199997
```

原始数据

```
{
  "bigDecimalNumber": 71.20,
  "bigDecimalString":"71.20"
}

```

![number-transfor-01](http://p3q1ykanf.bkt.clouddn.com/201806/number-transfor-01.png)

![number-transfor-02](http://p3q1ykanf.bkt.clouddn.com/201806/number-transfor-02.png)

```
#import "NSNumberTest.h"

NSString const * kBigDecimalNumberConst = @"bigDecimalNumber";
NSString const * kBigDecimalStringConst = @"bigDecimalString";


@implementation NSNumberTest

+ (NSString *)valueOfNSNumber:(NSNumber *)number {

  NSLog(@"%s",__FUNCTION__);
  
  NSLog(@"number: %@",number);
  
  double doubleValue = [number doubleValue];
  
  NSLog(@"doubleValue: %lf",doubleValue);
  
  NSLog(@"doubleValue - 2: %.2lf",doubleValue);

  float floatValue = [number floatValue];
  NSLog(@"floatValue: %lf",floatValue);
  
  NSLog(@"floatValue-2: %.2lf",floatValue);

  NSString *stringValue = [number stringValue];
  NSLog(@"stringValue: %@",stringValue);
  
  
  
  return stringValue;
}

+ (id)loadLocalJsonFile
{
  NSString *jsonPath = [[NSBundle mainBundle] pathForResource:@"test.json" ofType:nil];
  NSData *data = [NSData dataWithContentsOfFile:jsonPath];
  NSError *error = nil;
  id result = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingAllowFragments error:&error];
  
  if (error == nil) {
    BOOL isJson = [NSJSONSerialization isValidJSONObject:result];
    
    if (isJson) {
      NSLog(@"这是一个合格的json: %@",result);
      return result;
    }
    
    NSLog(@"json格式不对");
    return nil;
  }
  
  
  NSLog(@"\n%@", [error localizedDescription]);
  return nil;
}

+(NSString *)testJsonDictNumber
{
  NSLog(@"%s",__FUNCTION__);

  
  id result =  [self loadLocalJsonFile];
  
  if (result == nil) {
    NSLog(@"------------------------加载本地json格式错误,没有数据------------------------");
    return @"";
  }
  
  
  
  if ([result isKindOfClass:[NSDictionary class]]) {
    NSDictionary *dict = (NSDictionary *)result;
    
    NSNumber *number = [dict objectForKey:kBigDecimalNumberConst];
    
    NSLog(@"------------------------测试数值类型------------------------");
    if ([number isKindOfClass:[NSNumber class]]) {
      NSLog(@"传递的是数值,注意json不区分 整数还是浮点数");
      [self valueOfNSNumber:number];
    } else if ([number isKindOfClass:[NSString class]]) {
      NSLog(@"传递的是字符串:  %@",number);
    } else {
      NSLog(@"其他类型");
    }
    

    NSLog(@"------------------------测试字符串类型------------------------");
    NSString *string = [dict objectForKey:kBigDecimalStringConst];

    if ([string isKindOfClass:[NSNumber class]]) {
      NSLog(@"传递的是数值,注意json不区分 整数还是浮点数");
      NSNumber  *number = (NSNumber *)string;
      [self valueOfNSNumber:number];
    } else if ([string isKindOfClass:[NSString class]]) {
      NSLog(@"传递的是字符串:  %@",string);
      
      double doubleValue = [string doubleValue];
      NSLog(@"字符串转换成数值:  %f",doubleValue);
      NSLog(@"%%f,只能接受小数点点后六位。如果要接受64位的浮点型用%%lf");
    } else {
      NSLog(@"其他类型");
    }

    return [result description] ;
  } else if ([result isKindOfClass:[NSArray class]]) {
    NSLog(@"要测试的是字典,不是数组");
    return @"";
  }
  
  return @"";
}


@end
```

打印信息
```
 +[NSNumberTest testJsonDictNumber]
 这是一个合格的json: {
    bigDecimalNumber = "71.2";
    bigDecimalString = "71.20";
}
 ------------------------测试数值类型------------------------
传递的是数值,注意json不区分 整数还是浮点数
 +[NSNumberTest valueOfNSNumber:]
 number: 71.2
 doubleValue: 71.200000
 doubleValue - 2: 71.20
 floatValue: 71.199997
 floatValue-2: 71.20
stringValue: 71.2
------------------------测试字符串类型------------------------
传递的是字符串:  71.20
字符串转换成数值:  71.200000
%f,只能接受小数点点后六位。如果要接受64位的浮点型用%lf
```

猜测是NSNumber的问题

>结论: 浮点型数据的传输后的iOS解析
直接转换成数值,不论是float 还是double都会有精度的变化. 
那么我们在传递过程中就要规定数值保留小数点几位来完整表达数值的精确.

>而移动端一般只是进行显示,最好的处理方式就是 传递字符串.
这样不论 安卓还是iOS 都不会有处理上的不同.


### 解决方案参考

* [iOS - Json解析精度丢失处理(NSString, Double, Float)](http://www.jianshu.com/p/83d4bc28cc7c)

* [关于OC中的小数精确计算---NSDecimalNumber](http://www.cnblogs.com/denz/p/5330771.html)



