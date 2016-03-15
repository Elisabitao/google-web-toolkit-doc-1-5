# Why do I see poor performance when using a one pixel tall background image? #

It is common to use a one pixel image for a repeating background image.  Be advised, however, that there can be a performance penalty for covering a large area with a small image (in particular, we noticed the problem on Internet Explorer).  This is particularly noticable when using GWT animations.

One way to work around the problem is to increase the size of your image so that the rendering image doesn't have to repeat the image as often.  So, instead of using a 20 x 1 sized image, you might try a 20 x 100 image.  The tradeoff here is that the browser will have to download a larger image over the network.