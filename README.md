# AnimatedBottomBar
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![API](https://img.shields.io/badge/API-16%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=16) 
[![Download](https://api.bintray.com/packages/droppers/AnimatedBottomBar/nl.joery.animatedbottombar/images/download.svg) ](https://bintray.com/droppers/AnimatedBottomBar/nl.joery.animatedbottombar/_latestVersion)
![androidweekly.net](https://androidweekly.net/issues/issue-404/badge)

A customizable and easy to use bottom bar view with sleek animations.

## Examples
<img src="./media/example/example-3.gif" width="550" />
<br>
<img src="./media/example/example-1.gif" width="550" />
<br>
<img src="./media/example/example-2.gif" width="550" />
<br>
<img src="./media/example/example-4.gif" width="550" />

### Playground app
<img src="./media/static/playground-demo.png"  width="500" />

Download the playground app: <a href="./media/demo.apk">demo.apk</a>. In this app you can try out all features and even generate XML layouts.

## Contents
- [Getting started](#getting-started)
- [Managing tabs](#managing-tabs)
- [Usage with ViewPager](#usage-with-viewpager)
- [Configuration](#configuration)

## Getting started
<img src="./media/getting-started-demo.gif" width="400" />

Add the following dependency to your <b>build.gradle</b>:
```gradle
implementation 'nl.joery.animatedbottombar:library:1.0.8'
```

Define `AnimatedBottomBar` in your XML layout with custom attributes.
```xml
<nl.joery.animatedbottombar.AnimatedBottomBar
    android:id="@+id/bottom_bar"
    android:background="#FFF"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:abb_selectedTabType="text"
    app:abb_indicatorAppearance="round"
    app:abb_indicatorMargin="16dp"
    app:abb_indicatorHeight="4dp"
    app:abb_tabs="@menu/tabs"
    app:abb_selectedIndex="1" />
```

Create a file named `tabs.xml` in the `res/menu/` resources folder:
```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:id="@+id/tab_home"
        android:icon="@drawable/home"
        android:title="@string/home" />
    <item
        android:id="@+id/tab_alarm"
        android:icon="@drawable/alarm"
        android:title="@string/alarm" />
    <item
        android:id="@+id/tab_bread"
        android:icon="@drawable/bread"
        android:title="@string/bread" />
    <item
        android:id="@+id/tab_cart"
        android:icon="@drawable/cart"
        android:title="@string/cart" />
</menu>
```

Get notified when the selected tab changes by setting callbacks:
```kotlin
bottom_bar.onTabSelected = {
    Log.d("bottom_bar", "Selected tab: " + it.title)
}
bottom_bar.onTabReselected = {
    Log.d("bottom_bar", "Reselected tab: " + it.title)
}
```

Or set a listener if you need more detailed information:
```kotlin
bottom_bar.setOnTabSelectListener(object : AnimatedBottomBar.OnTabSelectListener {
    override fun onTabSelected(
        lastIndex: Int,
        lastTab: AnimatedBottomBar.Tab?,
        newIndex: Int,
        newTab: AnimatedBottomBar.Tab
    ) {
        Log.d("bottom_bar", "Selected index: $newIndex, title: ${newTab.title}")
    }

    // An optional method that will be fired whenever an already selected tab has been selected again.
    override fun onTabReselected(index: Int, tab: AnimatedBottomBar.Tab) {
        Log.d("bottom_bar", "Reselected index: $index, title: ${tab.title}")
    }
})
```

## Managing tabs
Short overview on how to manage tabs using code.

### Creating new tabs
```kotlin
// Creating a tab by passing values
val bottomBarTab1 = AnimatedBottomBar.createTab(drawable, "Tab 1")

// Creating a tab by passing resources
val bottomBarTab2 = AnimatedBottomBar.createTab(R.drawable.ic_home, R.string.tab_2, R.id.tab_home)
```
### Adding new tabs
```kotlin 
// Adding a tab at the end
AnimatedBottomBar.addTab(bottomBarTab1)

// Add a tab at a specific position
AnimatedBottomBar.addTabAt(1, bottomBarTab2)
```

### Removing tabs
```kotlin
// Removing a tab by object reference
val tabToRemove = AnimatedBottomBar.tabs[1]
AnimatedBottomBar.removeTab(tabToRemove)

// Removing a tab at a specific position
AnimatedBottomBar.removeTabAt(tabPosition)

// Removing a tab by the given ID resource
AnimatedBottomBar.removeTabById(R.id.tab_home)
```

### Selecting tabs
```kotlin
// Selecting a tab by object reference
val tabToSelect = AnimatedBottomBar.tabs[1]
AnimatedBottomBar.selectTab(tabToSelect)

// Selecting a tab at a specific position
AnimatedBottomBar.selectTabAt(1)

// Selecting a tab by the given ID resource
AnimatedBottomBar.selectTabById(R.id.tab_home)
```

### Enabling / disabling tabs
```kotlin
// Disabling a tab by object reference
val tabToDisable = AnimatedBottomBar.tabs[1]
AnimatedBottomBar.setTabEnabled(tabToDisable, false) // Use true for re-enabling the tab

// Disabling a tab at a specific position
AnimatedBottomBar.setTabEnabledAt(1, false)

// Disabling a tab by the given ID resource
AnimatedBottomBar.setTabEnabledById(R.id.tab_home, false)
```

### Intercepting tabs
Could be useful for example restricting access to a premium area. You can use a callback or a more detailed listener:
```kotlin
bottom_bar.onTabIntercepted = {
    if (newTab.id == R.id.tab_pro_feature && !hasProVersion) {
        // e.g. show a dialog
        false
    }
    true
}
```

Detailed listener:
```kotlin
bottom_bar.setOnTabInterceptListener(object : AnimatedBottomBar.OnTabInterceptListener {
    override fun onTabIntercepted(
        lastIndex: Int,
        lastTab: AnimatedBottomBar.Tab?,
        newIndex: Int,
        newTab: AnimatedBottomBar.Tab
    ): Boolean {
        if (newTab.id == R.id.tab_pro_feature && !hasProVersion) {
            // e.g. show a dialog
            return false
        }
        return true
    }
})
```

## Usage with ViewPager
It is easy to use the BottomBar with a ViewPager or ViewPager2, you can simply use the `setupWithViewPager()` method. Please note that the number of tabs and ViewPager pages need to be identical in order for it to function properly.

<b>Usage</b>
```kotlin
// For ViewPager use:
bottom_bar.setupWithViewPager(yourViewPager)

// For ViewPager2 use:
bottom_bar.setupWithViewPager2(yourViewPager2)
```

## Configuration
An overview of all configuration options. All options can also be accessed and set programmatically, by their equivalent name.

### Tabs
<table>
    <tr>
        <th>Attribute</th>
        <th>Description</th>
        <th>Default</th>
    </tr>
    <tr>
        <td><b>abb_tabs</b></td>
        <td>Tabs can be defined in a menu file (<a href="https://developer.android.com/guide/topics/resources/menu-resource">Menu resource</a>), in the <i>res/menu/</i> resource folder.<br><br>The <b>icon</b> and <b>title</b> attribute are required. By default all tabs are enabled, set <b>android:enabled</b> to <i>false</i> to disable a tab.
<pre lang="xml">
&#x3C;menu xmlns:android=&#x22;http://schemas.android.com/apk/res/android&#x22;&#x3E;
    &#x3C;item
        android:id=&#x22;@+id/tab_example&#x22;
        android:icon=&#x22;@drawable/ic_example&#x22;
        android:title=&#x22;@string/tab_example&#x22;
        android:enabled=&#x22;true|false&#x22; /&#x3E;
    ...etc
&#x3C;/menu&#x3E;
</pre></td>
        <td></td>
    </tr>
    <tr>
        <td><b>abb_selectedIndex</b></td>
        <td>Define the default selected tab index.</td>
        <td></td>
    </tr>
    <tr>
        <td><b>abb_selectedTabId</b></td>
        <td>Define the default selected tab by its ID, for example <i>@id/tab_id</i></td>
        <td></td>
    </tr>
</table>

### Tab appearance
<table>
    <tr>
        <th>Attribute</th>
        <th>Description</th>
        <th>Default</th>
    </tr>
    <tr>
        <td><b>abb_selectedTabType</b></td>
        <td>
            Determines whether the icon or text should be shown when a tab has been selected.<br><br>
            <b>icon</b><br>
            <img src="./media/static/type-icon.png" width="240" /><br>
            <b>text</b><br>
            <img src="./media/static/type-text.png" width="240" /><br>
        </td>
        <td>icon</td>
    </tr>
    <tr>
        <td><b>abb_tabColor</b></td>
        <td>The color of the icon or text when the tab is not selected.</td>
        <td>@color/textColorPrimary</td>
    </tr>
    <tr>
        <td><b>abb_tabColorSelected</b></td>
        <td>The color of the icon or text when the tab is selected.</td>
        <td>@color/colorPrimary</td>
    </tr>
    <tr>
        <td><b>abb_tabColorDisabled</b></td>
        <td>The color of the icon or text whenever the tab has been disabled.</td>
        <td>@color/textColorSecondary</td>
    </tr>
    <tr>
        <td><b>abb_textAppearance</b></td>
        <td>Customize the look and feel of text in tabs, down below is an example of a custom text appearance.<br><br>
            Define a new style in <i>res/values/styles.xml</i>:
<pre lang="xml">
&lt;style name=&quot;CustomText&quot;&gt;
    &lt;item name=&quot;android:textAllCaps&quot;&gt;true&lt;/item&gt;
    &lt;item name=&quot;android:fontFamily&quot;&gt;serif&lt;/item&gt;
    &lt;item name=&quot;android:textSize&quot;&gt;16sp&lt;/item&gt;
    &lt;item name=&quot;android:textStyle&quot;&gt;italic|bold&lt;/item&gt;
&lt;/style&gt;
</pre>
<img src="./media/static/text-appearance.png" width="240" />
        </td>
        <td></td>
    </tr>
    <tr>
        <td><b>abb_textStyle</b></td>
        <td>Style (normal, bold, italic, bold|italic) for the text.<br><br>
            Use <i>bottom_bar.typeface</i> to programmatically set text style.</td>
        <td>normal</td>
    </tr>
    <tr>
        <td><b>abb_textSize</b></td>
        <td>Size of the text. Recommended dimension type for text is "sp" for scaled-pixels (example: 15sp).</td>
        <td>14sp</td>
    </tr>
    <tr>
        <td><b>abb_iconSize</b></td>
        <td>Increase or decrease the size of the icon.</td>
        <td>24dp</td>
    </tr>
    <tr>
        <td><b>abb_rippleEnabled</b></td>
        <td>Enables the 'ripple' effect when selecting a tab.<br><br>
            <img src="./media/static/ripple.png" width="240" /></td>
        <td>false</td>
    </tr>
    <tr>
        <td><b>abb_rippleColor</b></td>
        <td>Change the color of the aforementioned ripple effect.</td>
        <td>Default theme color</td>
    </tr>
</table>

### Animations
<table>
    <tr>
        <th>Attribute</th>
        <th>Description</th>
        <th>Default</th>
    </tr>
    <tr>
        <td><b>abb_animationDuration</b></td>
        <td>
            The duration of all animations, including the indicator animation.
        </td>
        <td>400</td>
    </tr>
    <tr>
        <td><b>abb_tabAnimation</b></td>
        <td>
            The enter and exit animation style of the tabs which are not selected.<br><br>
            <b>none</b><br>
            <img src="./media/anim-none.gif" width="240" /><br>
            <b>slide</b><br>
            <img src="./media/anim-slide.gif" width="240" /><br>
            <b>fade</b><br>
            <img src="./media/anim-active-fade.gif" width="240" /><br>
        </td>
        <td>fade</td>
    </tr>
    <tr>
        <td><b>abb_tabAnimationSelected</b></td>
        <td>
            The enter and exit animation style of the selected tab.<br><br>
            <b>none</b><br>
            <img src="./media/anim-active-none.gif" width="240" /><br>
            <b>slide</b><br>
            <img src="./media/anim-active-slide.gif" width="240" /><br>
            <b>fade</b><br>
            <img src="./media/anim-active-fade.gif" width="240" /><br>
        </td>
        <td>slide</td>
    </tr>
    <tr>
        <td><b>abb_animationInterpolator</b></td>
        <td>
            The interpolator used for all animations.<br><br>See "<a href="https://thoughtbot.com/blog/android-interpolators-a-visual-guide">Android Interpolators: A Visual Guide</a>" for more information on available interpolators.<br><br>
            Example value: <i>@android:anim/overshoot_interpolator</i><br>
            <img src="./media/interpolator-overshoot.gif" width="240" />
        </td>
        <td>FastOutSlowInInterpolator</td>
    </tr>
</table>

### Indicator
<table>
    <tr>
        <th>Attribute</th>
        <th>Description</th>
        <th>Default</th>
    </tr>
    <tr>
        <td><b>abb_indicatorColor</b></td>
        <td>The color of the indicator.</td>
        <td>@android/colorPrimary</td>
    </tr>
    <tr>
        <td><b>abb_indicatorHeight</b></td>
        <td>The height of the indicator.</td>
        <td>3dp</td>
    </tr>
    <tr>
        <td><b>abb_indicatorMargin</b></td>
        <td>The horizontal margin of the indicator. This determines the width of the indicator.</td>
        <td>0dp</td>
    </tr>
    <tr>
        <td><b>abb_indicatorAppearance</b></td>
        <td>
            Configure the shape of the indicator either to be square or round.<br><br>
            <b>invisible</b><br>
            <img src="./media/static/shape-invisible.png" width="140" /><br>
            <b>square</b><br>
            <img src="./media/static/shape-square.png" width="140" /><br>
            <b>round</b><br>
            <img src="./media/static/shape-round.png" width="140" /><br>
        </td>
        <td>square</td>
    </tr>
    <tr>
        <td><b>abb_indicatorLocation</b></td>
        <td>
            Configure the location of the selected tab indicator, top, bottom or invisible.<br><br>
            <b>top</b><br>
            <img src="./media/static/type-icon.png" width="240" /><br>
            <b>bottom</b><br>
            <img src="./media/static/type-icon-bottom.png" width="240" /><br>
        </td>
        <td>top</td>
    </tr>
    <tr>
        <td><b>abb_indicatorAnimation</b></td>
        <td>
            The animation type of the indicator when changing the selected tab.<br><br>
            <b>none</b><br>
            <img src="./media/anim-indicator-none.gif" width="240" /><br><br>
            <b>slide</b><br>
            <img src="./media/anim-slide.gif" width="240" /><br>
            <b>fade</b><br>
            <img src="./media/anim-indicator-fade.gif" width="240" />
        </td>
        <td>slide</td>
    </tr>
</table>

## Credits
- https://dribbble.com/shots/6130593-Readable-Tab-Bar<br>Inspiration for creating this library.

## License
```
MIT License

Copyright (c) 2020 Joery Droppers (https://github.com/Droppers)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
