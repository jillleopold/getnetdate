# 获取网络时间

- (NSDate *)getInternetDate{

    NSString *urlString = @"http://m.baidu.com";

    urlString = [urlString stringByAddingPercentEscapesUsingEncoding: NSUTF8StringEncoding];

    // 实例化NSMutableURLRequest，并进行参数配置

    NSMutableURLRequest *request = [[NSMutableURLRequest alloc] init];

    [request setURL:[NSURL URLWithString: urlString]];

    [request setCachePolicy:NSURLRequestReloadIgnoringCacheData];

    [request setTimeoutInterval: 2];

    [request setHTTPShouldHandleCookies:FALSE];

    [request setHTTPMethod:@"GET"];

    NSError *error = nil;

    NSHTTPURLResponse *response;

    [NSURLConnection sendSynchronousRequest:request

                          returningResponse:&response error:&error];

    // 处理返回的数据

    if (error) {

        return [NSDate date];

    }

    NSString *date = [[response allHeaderFields] objectForKey:@"Date"];

    date = [date substringFromIndex:5]；//index到这个字符串的结尾

    date = [date substringToIndex:[date length]-4];//从索引0到给定的索引index

    NSDateFormatter *dMatter = [[NSDateFormatter alloc] init];

    dMatter.locale = [[NSLocale alloc] initWithLocaleIdentifier:@"en_US"];

    [dMatter setDateFormat:@"dd MMM yyyy HH:mm:ss"];

    NSDate *netDate = [[dMatter dateFromString:date] dateByAddingTimeInterval:60*60*8];//时间差8小时

    NSTimeZone *zone = [NSTimeZone systemTimeZone];

    NSInteger interval = [zone secondsFromGMTForDate: netDate];

    netDate = [netDate  dateByAddingTimeInterval: interval];

    

    return netDate;

}
