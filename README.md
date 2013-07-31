COCOA-TIPs
==========

UIColor
-------

**Tile image to background**

it's a bit weird this method belongs to `UIColor` not `UIImage`
```
UIImage *backgroundPattern = [UIImage imageNamed:@"backgroundPattern.png"];
view.backgroundColor = [UIColor colorWithPatternImage:backgroundPattern];
```

UIImage
-------

**Generate animated images like GIF**

```
UIImage *section1 = [UIImage imageNamed:@"image1.png"];
UIImage *section2 = [UIImage imageNamed:@"image2.png"];
UIImage *animatedImage = [UIImage animatedImageWithImages:@{section1, section2} duration:0.2];

// u can use `[UIImage animatedImageNamed:(NSString *)name duration:(NSTimeInterval)duration]` as well
```

**Get first image from animated image**

Suppose u use `SDWebImage` to download a bunch of GIFs, and put them to a scrollView, the app will become very slow, so it's better to just show first image as a preview.

```
// first u should check if `animatedImage.images.count`
UIImage *firstImage = animatedImage.images[0];
```
