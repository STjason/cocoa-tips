COCOA-TIPS
==========

UIColor
-------

### Tile image to background

it's a bit weird this method belongs to `UIColor` not `UIImage`
```
UIImage *backgroundPattern = [UIImage imageNamed:@"backgroundPattern.png"];
view.backgroundColor = [UIColor colorWithPatternImage:backgroundPattern];
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


UIButton
--------

### Padding a UIButton's title [via](http://blog.daveworld.net/2011/03/how-to-add-padding-to-uibuttons-title.html)

```
button1.titleEdgeInsets = UIEdgeInsetsMake(0, 5, 0, 5);
```


NSString
--------

`UIKit` add some methods to `NSString` to make it easier to process certain tasks.

### Calculate string's size

```
NSString *title = @"Foobar";
CGSize titleSize = [title sizeWithFont:[UIFont systemFontOfSize:14]];
```

### draw string

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

