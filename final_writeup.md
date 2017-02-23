### Reflection

###1. Pipeline

My pipeline followed the normal 5 steps laid out for the project. First
an image is taken in and converted to grayscale. Next a gaussian blur is applied
to the image. Next canny edges are detected from the blurred image.

With the canny edges found, a region of interest is defined in front of the vehicle to
avoid excess noise of other edges outside this region. This region was
hardcoded in this case. Hough lines are drawn using this output. These
hough lines end up looking like dashed lines where dashed lane lines are
present. Due to this, the draw_lines() function is edited to change how
the hough lines are represented.

I achieved the solid hough lines by combining all points used for the hough
lines into a scatter where the image height and width are the x and y.
To ensure the lines are correctly identified, the slope of each line is used.
The slope allows us to identify right and left lines. Once split, x and y
points can be stored in a global variable.

If the global variable were not used, we would only see one frame's worth
of lines at a time. This is problematic as some frames do not detect all lanes
equally which causes the lines to jump around a bit in the video. By storing
3 frames worth of lines in the global variable before dropping the oldest,
we can include all of these points in the final scatters. We are then able
to fit a polynomial line to the points so that we can predict the end points
of the line to draw it on each frame. This line fit to all of the points
causes a smooth line across frames while not being too heavily influenced
by old lines.

###2. Advantages and Disadvantages of this Approach

This approach has one big advantage. Having the points fit by a polynomial
line allows for later editing that would allow the line to fit curves.

There are a few disadvantages in my approach that would be good to address.
The first issue I realized in my failed attempt at the bonus video was the
hardcoded region of interest. Since I set it using pixels, it does not adapt
well to images with different sizing. I'll address my thoughts on fixing in
the third section.

Another possible disadvantage was the way in which I used global variables.
These variable can become confusing, such as forgetting to reset the variables
if using more than once.

One final issue with my current approach would be outliers. I do some filtering
to try to prevent odd lines from influencing the final lines, but it could be improved.


###3. Improvements

The first improvement I would make is to make the region of interest flexible.
It seems this could be accomplished by setting each region corner as the
quotient of the size of the image rather than the specific location of the
pixel.

The next improvement would be to implement better filtering of the lines.
I have not determined how I would achieve this one, but would like to give
it some more thought.
