---
layout: post
title: "Animating Child Views"
date:   2015-01-31 16:03:08
comments: true
categories: 
---
Animating individual Views is something that has become easier with each Android release. There are times, however, when you may want to animate multiple child Views at once, such as when loading a new Activity or Fragment. This could be difficult to achieve, particularly if you want to time each animation to occur after the previous one. 

Luckily, there's already a tool just that: Layout Animations.

<!-- more -->

We'll create a simple layout to demonstrate:

{% highlight xml %}
<LinearLayout
    xlmns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >

    <TextView
        android:id="@+id/heading"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:text="@string/heading" />

    <TextView
        android:id="@+id/first_paragraph"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="14sp"
        android:text="@string/first_paragraph" />

    <TextView
        android:id="@+id/second_paragraph"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="14sp"
        android:text="@string/second_paragraph" />

</LinearLayout>
{% endhighlight %}

A simple enough layout. Just assign it to a Fragment or Activity, stick in the needed Strings in your strings.xml file and test it out. Nothing too special. Let's try adding some animations. Put this in the anim direction in your resources folder:

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<set
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:interpolator="@android:anim/linear_interpolator"
    android:fillAfter="true" >

    <translate
        android:fromXDelta="-100%p"
        android:toXDelta="0%p"
        android:duration="400" />

</set>
{% endhighlight %}

This will create an animation that translates (moves) a View from off the screen (-100%) until its left side is at the left side of the screen (0%), giving the impression that it's sliding in from the left. Now, you could apply this animation to each child View in the LinearLayout yourself but then they would all animate at the same time, which is not the effect we are looking for. So, we need to add another file to the anim folder.

{% highlight xml %}
<layoutAnimation 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:delay="50%"
    android:animation="@anim/slide_in_left" />
{% endhighlight %}

If you named your animation something other than slide_in_left.xml then name it appropriately here in the animation field. You can tweak the delay field all you want. When the previous child View's animation reaches this point (in our case, halfway completed) it will initiate the next child's animation.

Finally, let's add this to our LinearLayout:

{% highlight xml %}
<LinearLayout
    xlmns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:layoutAnimation="@anim/linearlayout_enter" >
{% endhighlight %}

Again, if you've named your LayoutAnimation something else use that name here.

Just run this code on your device or emulator and all the child Views should slide in from the left with the delay you specified. This can be applied to other Views too, such as ListViews or RecycleViews and the animations can be more complex as well, but the basic principles remain the same.
