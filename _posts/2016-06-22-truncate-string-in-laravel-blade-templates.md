Laravel is a modern PHP MVC framework. Its default template engine is Blade. Blade template engine is interesting because it compiles the views into PHP and cache them until they are changed. Blade also allows developers to embed PHP expressions in a view.

Laravel contains multiple helper functions that can speed up the development of web application. In the below post I will introduce the ```str_limit``` helper function. The ```str_limit``` function accepts 3 arguments, where the last two are optional.

{% highlight php %}

str_limit($string, $length = 150, $end = '...')

{% endhighlight %}

The ```$string``` argument is a string to be truncated.

The ```$length``` argument specifies the length of truncated string.

The ```$end``` argument specifies a string that will be appended at the end of our truncated string.

Example usage is presented below:

{% highlight php %}

{{ str_limit($article->updated_at,10, '') }}

{% endhighlight %}

It will truncate the date stored in update_at property to 10 characters and will append empty string after truncated string.
So from string ```2016-06-19 10:23:46``` it will create truncated string ```2016-06-19```.
