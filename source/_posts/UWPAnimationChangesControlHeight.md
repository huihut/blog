---

title: UWP 动画改变控件大小（高度）
subtitle: UWP 动画改变控件大小（高度）
date: 2018-07-18 23:09:46
author: huihut
tags:
	- Dotnet
	- UWP
categories: 
	- CS
	
---

有这样一个需求：

* 鼠标移动到（悬停在）控件上（PointerEntered），控件大小（高度）发生变化，以显示更多内容；
* 鼠标移出控件（PointerExited），控件大小恢复原状。

本文通过 UWP 动画，用两种方法实现这个效果，用于改变周贡献榜和粉丝榜的 Grid 的高度。

<!-- more -->

## 方法一：XAML 实现动画

### XAML：

```html
<UserControl.Resources>
    <!--周贡、粉丝榜下拉恢复动画-->
    <Storyboard x:Name="SeeMoreAnimation" Storyboard.TargetName="WeekFansGrid">
        <DoubleAnimation Duration="0:0:0.2" EnableDependentAnimation="True" Storyboard.TargetProperty="Height" From="140" To="400"/>
    </Storyboard>
    <Storyboard x:Name="RestoreAnimation" Storyboard.TargetName="WeekFansGrid">
        <DoubleAnimation  Duration="0:0:0.2" EnableDependentAnimation="True" Storyboard.TargetProperty="Height" From="400" To="140"/>
    </Storyboard>
</UserControl.Resources>

<Grid x:Name="WeekFansGrid" Background="White" VerticalAlignment="Top" Height="140"
              PointerEntered="Grid_PointerEntered" PointerExited="Grid_PointerExited">
    <!--Grid 里面的一些内容-->
</Grid>
```

### C#：

```cs
// 鼠标悬停周贡、粉丝榜的 Grid
private void Grid_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    try
    {
        SeeMoreAnimation.Begin();
    }
    catch (Exception e1)
    {
        System.Diagnostics.Debug.WriteLine("Grid_PointerEntered " + e1.Message.ToString());
    }
}

// 鼠标离开周贡、粉丝榜的 Grid
private void Grid_PointerExited(object sender, PointerRoutedEventArgs e)
{
    try
    {
        RestoreAnimation.Begin();
    }
    catch (Exception e1)
    {
        System.Diagnostics.Debug.WriteLine("Grid_PointerExited " + e1.Message.ToString());
    }
}
```

## 方法二：后台实现动画

### XAML：

```html
<Grid x:Name="WeekFansGrid" Background="White" VerticalAlignment="Top" Height="140"
              PointerEntered="Grid_PointerEntered" PointerExited="Grid_PointerExited">
    <!--Grid 里面的一些内容-->     
</Grid>
```

### C#：

```cs
// 鼠标悬停周贡、粉丝榜的 Grid
private void Grid_PointerEntered(object sender, PointerRoutedEventArgs e)
{
    try
    {
        Grid grid = sender as Grid;
        if (grid != null)
        {
            DoubleAnimation SeeMoreAnimation = new DoubleAnimation();
            if (SeeMoreAnimation != null)
            {
                // 高度从 140 变化到 400
                SeeMoreAnimation.From = 140;
                SeeMoreAnimation.To = 400;
                // 用时 200 毫秒
                SeeMoreAnimation.Duration = new Duration(TimeSpan.FromMilliseconds(200));
                SeeMoreAnimation.EnableDependentAnimation = true;

                // 目标 Grid 的 Height
                Storyboard.SetTarget(SeeMoreAnimation, grid);
                Storyboard.SetTargetProperty(SeeMoreAnimation, "Height");

                Storyboard storyboard = new Storyboard();
                if (storyboard != null)
                {
                    storyboard.Children.Add(SeeMoreAnimation);
                    // 执行动画
                    storyboard.Begin();
                }
            }
        }
    }
    catch (Exception e1)
    {
        System.Diagnostics.Debug.WriteLine("Grid_PointerEntered " + e1.Message.ToString());
    }
}

// 鼠标离开周贡、粉丝榜的 Grid
private void Grid_PointerExited(object sender, PointerRoutedEventArgs e)
{
    try
    {
        Grid grid = sender as Grid;
        if (grid != null)
        {
            DoubleAnimation SeeMoreAnimation = new DoubleAnimation();
            if (SeeMoreAnimation != null)
            {
                // 高度从 400 变化到 140
                SeeMoreAnimation.From = 400;
                SeeMoreAnimation.To = 140;
                // 用时 200 毫秒
                SeeMoreAnimation.Duration = new Duration(TimeSpan.FromMilliseconds(200));
                SeeMoreAnimation.EnableDependentAnimation = true;

                // 目标 Grid 的 Height
                Storyboard.SetTarget(SeeMoreAnimation, grid);
                Storyboard.SetTargetProperty(SeeMoreAnimation, "Height");

                Storyboard storyboard = new Storyboard();
                if (storyboard != null)
                {
                    storyboard.Children.Add(SeeMoreAnimation);
                    // 执行动画
                    storyboard.Begin();
                }
            }
        }
    }
    catch (Exception e1)
    {
        System.Diagnostics.Debug.WriteLine("Grid_PointerExited " + e1.Message.ToString());
    }
}
```

## 实现效果

![GridSeeMoreAnimation](http://huihut-img.oss-cn-shenzhen.aliyuncs.com/GridSeeMoreAnimation1.gif)