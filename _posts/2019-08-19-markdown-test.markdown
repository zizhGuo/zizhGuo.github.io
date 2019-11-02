---
layout: post
title:  "Markdown Syntax Test"
date:   2019-08-18 19:34:06
author: Zizhun Guo
category: Articles
categories: jekyll update
visible: 0
---

# First Level Header

>This is a block quote.

> ## 美国一位作家
> 美国一个五年就觉得五年前的自己是个混蛋。
> 

Some of these words are emphisized.


- A list item.
   
    With multiple paragraphs

- Another item in the list.
    
<ul>
    <li>
        <p>A list item.</p>
        <p>With multiple paragraphs.</p>
    </li>
    <li>
        <p>Another item in the list.</p>
    </li>
</ul>

1. Ni hao.
2. Bu Yong Xie.

This is [google](https://google.com "Click it for further search")

<!-- Blank line space is important to denote. -->
我喜欢上[知乎][1]更于[baidu][2].

[1]:https://zhihu.com/ "知乎"
[2]:https://baidu.com/ "Baidu"

![杜甫草堂](https://pbs.twimg.com/profile_images/1053055123193122816/IUwo6l_Q_400x400.jpg)

![杜甫草堂]({{site.url}}/assets/img-bg/caotang1.jpg)

I strongly recommend against using any `<blink>` tags.

I wish SmartyPants used named entities like `&mdash;`
instead of decimal-encoded entites like `&#8212;`.

```cpp
void render(const std::vector<Sphere>& spheres)
{
    unsigned width = 640, height = 480;
    Vec3f* image = new Vec3f[width * height], * pixel = image;
    float invWidth = 1 / float(width), invHeight = 1 / float(height);
    float fov = 30, aspectratio = width / float(height);
    float angle = tan(M_PI * 0.5 * fov / 180.);
    // Trace rays
    for (unsigned y = 0; y < height; ++y) {
        for (unsigned x = 0; x < width; ++x, ++pixel) {
            float xx = (2 * ((x + 0.5) * invWidth) - 1) * angle * aspectratio;
            float yy = (1 - 2 * ((y + 0.5) * invHeight)) * angle;
            Vec3f raydir(xx, yy, -1);
            raydir.normalize();
            *pixel = trace(Vec3f(0), raydir, spheres, 0);
        }
    }
    // Save result to a PPM image (keep these flags if you compile under Windows)
    std::ofstream ofs("./untitled.ppm", std::ios::out | std::ios::binary);
    ofs << "P6\n" << width << " " << height << "\n255\n";
    for (unsigned i = 0; i < width * height; ++i) {
        ofs << (unsigned char)(std::min(float(1), image[i].x) * 255) <<
            (unsigned char)(std::min(float(1), image[i].y) * 255) <<
            (unsigned char)(std::min(float(1), image[i].z) * 255);
    }
    ofs.close();
    delete[] image;
}
```

<blockquote>
    <p>For example.</p>
</blockquote>


