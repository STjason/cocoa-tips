COCOA-TIPS
==========

UIView
------

### Get subview by tag
```
UIView *subview = [[UIView alloc] init];
subview.tag = 1;
[self addSubview:subview];
[self viewWithTag:1]; // return subview
```

UIColor
-------

### Tile image to background
it's a bit weird this method belongs to `UIColor` not `UIImage`
```
UIImage *backgroundPattern = [UIImage imageNamed:@"backgroundPattern.png"];
view.backgroundColor = [UIColor colorWithPatternImage:backgroundPattern];
```


UIButton
--------

### Padding a UIButton's title [via](http://blog.daveworld.net/2011/03/how-to-add-padding-to-uibuttons-title.html)
```
button1.titleEdgeInsets = UIEdgeInsetsMake(0, 5, 0, 5);
```


UIImage
-------

### Generate animated images like GIF
```
UIImage *section1 = [UIImage imageNamed:@"image1.png"];
UIImage *section2 = [UIImage imageNamed:@"image2.png"];
UIImage *animatedImage = [UIImage animatedImageWithImages:@{section1, section2} duration:0.2];

// u can use `[UIImage animatedImageNamed:(NSString *)name duration:(NSTimeInterval)duration]` as well
```

### Generate `UIImage` by url [via](http://stackoverflow.com/a/2782491/94962)
```
UIImage *image = [UIImage imageWithData:[NSData dataWithContentsOfURL:[NSURL URLWithString:MyURL]]];
```

### Get first image from animated image
Suppose u use `SDWebImage` to download a bunch of GIFs, and put them to a scrollView, the app will become very slow, so it's better to just show first image as a preview.

```
// first u should check `animatedImage.images.count`
UIImage *firstImage = animatedImage.images[0];
```

UITableView
-----------

### Disable selection highlighting
```
cell.selectionStyle = UITableViewCellSelectionStyleNone;
```


UILabel
-------

* `sizeToFit` will shrink label's additional area, set `numberOfLines` to 0 means unlimited number of lines(multiple line). [via](http://stackoverflow.com/q/1054558/94962)


NSArray
-------

### Filter array
```
NSArray *arr = @[@"hello", @"world", @"foo", @"bar"];
NSIndexSet *indexSet = [arr indexesOfObjectsPassingTest:^BOOL(id obj, NSUInteger idx, BOOL *stop){
	if ([obj rangeOfString:@"o"].location != NSNotFound) {
		return YES;
	}
	return NO;
}];
NSArray *filteredArr = [arr objectsAtIndexes:indexSet];
NSLog(@"filteredArr:%@", filteredArr);
```

### Sort array [via](http://stackoverflow.com/a/805589/94962)
```
NSArray *sortedArray;
sortedArray = [drinkDetails sortedArrayUsingComparator:^NSComparisonResult(id a, id b) {
    NSDate *first = [(Person*)a birthDate];
    NSDate *second = [(Person*)b birthDate];
    return [first compare:second];
}];
```


NSString
--------

`UIKit` add some methods to `NSString` to make it easier to process certain tasks.

### Calculate string's size
```
NSString *title = @"Foobar";
CGSize titleSize = [title sizeWithFont:[UIFont systemFontOfSize:14]];
```

### Draw string
to reduce UIView's hierarchy, sometimes its convenience to draw all the content in `drawRect` method
```
NSString *title = @"Foobar";
CGPoint startPoint = CGPointMake(10, 10);
UIFont *font = [UIFont systemFontOfSize:14];

// draw the string in a single line
[title drawAtPoint:startPoint withFont:font];

// draw the string in a given area
CGRect rect = CGRectMake(0, 0, 150, 30);
[title drawInRect:rect withFont:font];
```

### Check if contains certain substring
```
NSString *string = @"Hello World!";
NSLog(@"contains 'll'?: %i", [string rangeOfString:@"ll"].location != NSNotFound);
```


NSData
------

### Encode a string to NSData and Decode back [via](http://stackoverflow.com/q/901357/94962)
```
NSString *originStr = @"teststring";
NSData *data = [originStr dataUsingEncoding:NSUTF8StringEncoding];
NSString *str = [NSString stringWithUTF8String:[data bytes]];
```


Grand Central Dispatch (GCD)
----------------------------

### Perform a block after duration
```
// Xcode has builtin snippets for this `dispatch_after` 
double delayInSeconds = 2.0;
dispatch_time_t popTime = dispatch_time(DISPATCH_TIME_NOW, (int64_t)(delayInSeconds * NSEC_PER_SEC));
dispatch_after(popTime, dispatch_get_main_queue(), ^(void){
	// do some stuff
});
```


Miscellaneous
-------------

### Invoke a method of the receiver even it is private.
```
// this method has no return value!
[self performSelector:@selector(privateMethod) withObject:someObject];
```

### Use @property in category with `objc_setAssociatedObject` and  `objc_getAssociatedObject`
```
#import <objc/runtime.h>

static char  ObjectTagKey;

@implementation UIView (ObjectTagAdditions)
@dynamic objectTag;

- (id)objectTag {
    return objc_getAssociatedObject(self, ObjectTagKey);
}

- (void)setObjectTag:(id)newObjectTag {
    objc_setAssociatedObject(self, ObjectTagKey, newObjectTag, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}
```

### Register a custom app opening URL scheme

```
edit info.plist, add "URL types" section, add "URL Schemes" sub section.
```

### Get current app version

```
NSDictionary *infoDictionary = [[NSBundle mainBundle] infoDictionary];
NSString *version = [infoDictionary objectForKey:@"CFBundleShortVersionString"];
```

### Create repeating and non-repeating timers

```
NSTimer *timer;

// set `repeats` to NO if u don't want it to be repeated 
timer = [NSTimer scheduledTimerWithTimeInterval:3.0 target:self 
   selector:@selector(updateActivityIndicator:) userInfo:nil repeats:YES];

// stop the timer
[timer invalidate];
```

* If an object is designed to be able to write to disk, it must implement `NSCoding` protocol.

* `NSAssert` is used more then `NSException`

* Variable inside block are retained, so if the block is retained by `self`, u should use `weakSelf` inside this block.

* u can use `keychain` to save secure data like username/password, thus even the app is uninstalled, the data is still there.

* use `forwardingTargetForSelector:` to redirect unknown message to new target.

* set `name` to `nil` and `object` to `nil` to spy all notifications.

* `NSCache` is like `NSMutableDictionary` but with builtin memory management.

* use `stringByEvaluatingJavascriptFromString:` to communicate with javascript.

* use `+ (void)load` if u want to perform some init work on a category.

