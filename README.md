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

### Get first image from animated image

Suppose u use `SDWebImage` to download a bunch of GIFs, and put them to a scrollView, the app will become very slow, so it's better to just show first image as a preview.

```
// first u should check `animatedImage.images.count`
UIImage *firstImage = animatedImage.images[0];
```

NSString

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
