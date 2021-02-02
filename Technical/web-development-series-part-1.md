# Web Development Series (Part 1)

Scrolling this web page vertically, you see the next `section` (or `div` if you prefer) completely overlap the current.

````html
<!DOCTYPE HTML>
<html>

<head>
    <title>Full Page Sections</title>
    <link rel="stylesheet" href="main.css" />
</head>

<body>
    <section class="cover-thing first">Section-1</section>
    <section class="cover-thing second">Section-2</section>
    <section class="cover-thing third">Section-3</section>
    <section class="cover-thing fourth">Section-4</section>
</body>
<footer>
    <span>Photo by <a
            href="https://unsplash.com/@pankajpatel?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Pankaj
            Patel</a> on <a
            href="https://unsplash.com/s/photos/programming?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText">Unsplash</a></span>
</footer>

</html>
````

Use the `main.css` provided below to set a background image and set `background-attachment` to `fixed` for the image remaining on screen depite scrolling effect. You might want to obtain the image mentioned in *Footer* of HTML markup to place it in inside an `images/` folder at the same level as these files

````css
html, body {
    height: 100%;
}

body {
    margin: 0;
}

footer {
    background-color: white;
    color: black;
}

.cover-thing {
    width: 100%;
    height: 100%;
    float:left;
}

.first {
    background-color: #333;
    color: white;
    background-image: url('images/pankaj-patel-size2.jpg');
    background-attachment: fixed;
}

.second {
    background-color: #eee;
}

.third {
    background-color: #f06;
}

.fourth {
    background-color: violet;
}
````
