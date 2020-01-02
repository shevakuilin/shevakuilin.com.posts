# SDWebImage框架下的图片渐变加载效果

地址：https://www.jianshu.com/p/7a93a9f262fd, https://shevakuilin.com/sdwebimage-gradient/

创建时间：2017-10-29



众所周知，在SDWebImage框架下想要对图片在下载和下载过程中的状态进行处理，需要使用`SDWebImageOptions`，我们首先看一下SDWebImage中有哪些可选项

```objc
typedef NS_OPTIONS(NSUInteger, SDWebImageOptions) {
    /**
     * By default, when a URL fail to be downloaded, the URL is blacklisted so the library won't keep trying.
     * This flag disable this blacklisting.
     */
    SDWebImageRetryFailed = 1 << 0,

    /**
     * By default, image downloads are started during UI interactions, this flags disable this feature,
     * leading to delayed download on UIScrollView deceleration for instance.
     */
    SDWebImageLowPriority = 1 << 1,

    /**
     * This flag disables on-disk caching
     */
    SDWebImageCacheMemoryOnly = 1 << 2,

    /**
     * This flag enables progressive download, the image is displayed progressively during download as a browser would do.
     * By default, the image is only displayed once completely downloaded.
     */
    SDWebImageProgressiveDownload = 1 << 3,

    /**
     * Even if the image is cached, respect the HTTP response cache control, and refresh the image from remote location if needed.
     * The disk caching will be handled by NSURLCache instead of SDWebImage leading to slight performance degradation.
     * This option helps deal with images changing behind the same request URL, e.g. Facebook graph api profile pics.
     * If a cached image is refreshed, the completion block is called once with the cached image and again with the final image.
     *
     * Use this flag only if you can't make your URLs static with embedded cache busting parameter.
     */
    SDWebImageRefreshCached = 1 << 4,

    /**
     * In iOS 4+, continue the download of the image if the app goes to background. This is achieved by asking the system for
     * extra time in background to let the request finish. If the background task expires the operation will be cancelled.
     */
    SDWebImageContinueInBackground = 1 << 5,

    /**
     * Handles cookies stored in NSHTTPCookieStore by setting
     * NSMutableURLRequest.HTTPShouldHandleCookies = YES;
     */
    SDWebImageHandleCookies = 1 << 6,

    /**
     * Enable to allow untrusted SSL certificates.
     * Useful for testing purposes. Use with caution in production.
     */
    SDWebImageAllowInvalidSSLCertificates = 1 << 7,

    /**
     * By default, images are loaded in the order in which they were queued. This flag moves them to
     * the front of the queue.
     */
    SDWebImageHighPriority = 1 << 8,
    
    /**
     * By default, placeholder images are loaded while the image is loading. This flag will delay the loading
     * of the placeholder image until after the image has finished loading.
     */
    SDWebImageDelayPlaceholder = 1 << 9,

    /**
     * We usually don't call transformDownloadedImage delegate method on animated images,
     * as most transformation code would mangle it.
     * Use this flag to transform them anyway.
     */
    SDWebImageTransformAnimatedImage = 1 << 10,
    
    /**
     * By default, image is added to the imageView after download. But in some cases, we want to
     * have the hand before setting the image (apply a filter or add it with cross-fade animation for instance)
     * Use this flag if you want to manually set the image in the completion when success
     */
    SDWebImageAvoidAutoSetImage = 1 << 11
};
```

可以看出，最接近我们所需要的「渐变」效果的，就是这个`SDWebImageProgressiveDownload`，这确实是一个逐步显示图片的过程，但是这个类型所完成的效果，仅仅是一个从上到下逐渐显示的效果，离我们需要的「渐变」效果相差甚远。所以说的直白一些，SDWebImage这个框架目前是不支持大多数主流成熟框架均支持的图片加载「渐变」效果的。

### 这样一来，要想实现「渐变」效果，我们似乎只剩下两条路可以走：

1.替换库，如YYImage等

- 优点：框架自带支持，无需使用者自己处理，方便快捷省事
- 缺点：显然，在开发过程中，突然替换基础库的成本是很高的，尤其是在DeadLine为期不远的时候。如果团队中有人不熟悉新库，那么还会额外衍生学习时间成本，这对快速迭代来说，是很难接受的一件事

2.对SDWebImage进行修改，手动完成对「渐变」效果的支持

- 优点：节省了更换其他库的时间成本，降低了替换库所造成的风险和回归测试的成本
- 缺点：可能需要对源代码进行修改，如果是使用CocoaPods集成的，那就很麻烦了

#### 这么对比一看，似乎每一条路的成本都不低，难道真的只剩下这两条路可以走了吗？

答案是： NO

我看到大多数文章对于SDWebImage实现渐变效果的处理方式都是修改源码，这个做法实在是太Low了👎，`任何需要修改成熟开源框架源码才能完成的事情，都是最烂的思路和做法`。SDWebImage既然能够成为适用范围最广，最主流的网络图片框架之一，其对各种情况和需求的支持力度必然不可能如此局限。即使它不直接支持，那么我们为何不换个思路和做法呢？SDWebImage提供的方法如此丰富，何必要局限于一种，下面我们就来看看如何在不修改SDWebImage源码的情况下，来达到图片加载的「渐变」效果，Let's go!

首先，我们不妨可以先思考一下什么是「渐变」效果：无非就是`随着图片的加载逐渐由模糊到清晰的显示出来`，这种效果完全可以通过一个简单的`CATransition`动画来完成，唯一的“难点”不过是这个动画是add和remove的时机而已，大多数此类解决方案都是直接把这个简单的动画效果嵌入SDWebImage源码中，这个做法不仅Low，而且对SDWebImage源码也带来了伤害，最重要的是，如果你继续用这个库，难道每次SDWebImage升级你都要再修改一遍源码吗，exm???

所以，通过修改SDWebImage源码来嵌入这种动画的开发者，几乎可以肯定，全都是对`CAAnimation`基础完全不熟悉的人。只要是对`CAAnimation`熟悉的开发者都会知道，`CAAnimationDelegate`协议中提供了一个`animationDidStop(_ anim: CAAnimation, finished flag: Bool)`方法，该方法可以根据Animation的key对所有基于`CAAnimation`的动画进行监听操作。这么一来，我们接下来要做什么，你是不是就很明白了呢。

我们可以来看一下SDWebImage提供的加载网络图片的方法中，是有当图片完成后的回调方法的，如:

```objc
/**
 * Set the imageView `image` with an `url`, placeholder and custom options.
 *
 * The download is asynchronous and cached.
 *
 * @param url            The url for the image.
 * @param placeholder    The image to be set initially, until the image request finishes.
 * @param options        The options to use when downloading the image. @see SDWebImageOptions for the possible values.
 * @param completedBlock A block called when operation has been completed. This block has no return value
 *                       and takes the requested UIImage as first parameter. In case of error the image parameter
 *                       is nil and the second parameter may contain an NSError. The third parameter is a Boolean
 *                       indicating if the image was retrieved from the local cache or from the network.
 *                       The fourth parameter is the original image url.
 */
- (void)sd_setImageWithURL:(NSURL *)url placeholderImage:(UIImage *)placeholder options:(SDWebImageOptions)options completed:(SDWebImageCompletionBlock)completedBlock;
```

我们可以在`SDWebImageCompletionBlock`这个Block块中来实现我们的`CATransition`动画

```swift
/*
*  @param options: 选择.lowPriority(oc中是SDWebImageLowPriority)，是为了禁止在UI交互过程中启动图像下载，保证列表流畅性。例如，导致UIScrollView减速的下载延迟
*
*
*/
imageView.sd_setImage(with: theUrl as URL, placeholderImage: UIImage(named: "newVote_ placeholder"), options: .lowPriority, completed: { [weak self] (image, error, cacheType, url) in
   guard let strongSelf = self else { return }
   if cacheType == .none { // 只有当缓存中没有图片，也就是首次加载时才实现CATransition动画
      let transition:CATransition = CATransition()
      transition.type = kCATransitionFade // 褪色效果，渐进效果的基础
      transition.duration = 0.85
      transition.timingFunction = CAMediaTimingFunction(name:kCAMediaTimingFunctionEaseInEaseOut) // 先慢后快再慢 
      strongSelf.layer.add(transition, forKey: "newVoteTimeline") // 在layer中加入动画，并约定好该动画的key
   }
})
```

我们来解释一下上面这段代码究竟做了什么:

`options`: 选择`.lowPriority`(oc中是`SDWebImageLowPriority`)，是为了禁止在UI交互过程中启动图像下载，保证列表流畅性。例如，导致UIScrollView减速的下载延迟

判断`cacheType == .none`: 只有当缓存中没有图片，也就是首次加载时才实现CATransition动画，因为SDWebImage会根据url从缓存中查找是否存在相同的图片，只有当缓存中不存在该图片时才会启动下载，同理，我们的「渐变」效果也是只有当启动下载是才会发生，否则，在列表中复用时会持续显示「渐变」效果，无论是否已经下载完成

然后，我们遵循`CAAnimationDelegate`协议，并实现`animationDidStop(_ anim: CAAnimation, finished flag: Bool)`方法：

```swift
// MARK: 监听渐变动画是否已完成
func animationDidStop(_ anim: CAAnimation, finished flag: Bool) {
   if flag {
      self.layer.removeAnimation(forKey: "newVoteTimeline") // 当newVoteTimeline动画已经完成，将其从layer中移除，避免复用中的产生反复「渐变」效果
   }
}
```

以上就完成了针对SDWebImage框架的「渐变」效果修改，几行代码即可搞定，完全不需要对SDWebImage进行内部修改，有时候换一种思路思考问题，世界会豁然开朗。

上面的代码只是为了演示，最好还是将其进行封装：

```objc
// UIImageView+IETransition.h

#import <UIKit/UIKit.h>

@interface UIImageView (IETransition)<CAAnimationDelegate>

/** 渐变动画加载 URL 图片
 *
 * @param imageURL					图片 URL
 * @param placeholderImage	占位图
 *
 */
- (void)ie_transitionFadeLoadImage:(NSURL *)imageURL placeholderImage:(UIImage * _Nullable)placeholderImage;

@end

```

```objc
//  UIImageView+IETransition.m

#import "UIImageView+IETransition.h"

static NSString *const ANIMATIONKEY_TRANSITION_FADE = @"IETransitionFade";

@implementation UIImageView (IETransition)

#pragma mark - 渐变动画加载 URL 图片

- (void)ie_transitionFadeLoadImage:(NSURL *)imageURL placeholderImage:(UIImage * _Nullable)placeholderImage {
// - Note: 如果需要纯色占位图，可以将下面注释打开替换成具体颜色
//    if (!placeholderImage) {
//        self.backgroundColor = MainDefaultImageBgColor;
//    }
    @weakify(self)
    [self sd_setImageWithURL:imageURL placeholderImage:placeholderImage options:SDWebImageLowPriority completed:^(UIImage * _Nullable image, NSError * _Nullable error, SDImageCacheType cacheType, NSURL * _Nullable imageURL) {
        @strongify(self)
        if (image && cacheType == SDImageCacheTypeNone) { // 只有当缓存中没有图片，也就是首次加载时才实现CATransition动画
            CATransition *transition = [CATransition animation];
            transition.delegate = self;
            transition.type = kCATransitionFade; // 褪色效果，渐进效果的基础
            transition.duration = 0.85f;
            transition.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut]; // 先慢后快再慢
            [self.layer addAnimation:transition forKey:ANIMATIONKEY_TRANSITION_FADE];
        }
    }];
}

#pragma mark - CAAnimationDelegate

- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag {
    if (flag) {
        [self.layer removeAnimationForKey:ANIMATIONKEY_TRANSITION_FADE];
    }
}

@end
```

