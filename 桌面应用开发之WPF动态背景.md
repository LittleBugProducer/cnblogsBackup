&emsp;&emsp;因为项目需要，在WPF开发的桌面应用中，登陆页面需使用动态背景。由于没有前端开发人员，所以由半吊子的后端开发人员根据效果图写前端xaml。去掉页面上边框，抽离动态背景设置代码：


	<Window x:Class="DynamicBg.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:DynamicBg"
        mc:Ignorable="d"
        Title="MainWindow" Height="580" Width="581"
        AllowsTransparency="True" WindowStyle="None"  ResizeMode="CanResize"
        WindowState="Normal"
        >
    <Window.Resources>
        <ResourceDictionary>
            <WindowChrome x:Key="WindowChromeKey">
                <WindowChrome.ResizeBorderThickness>
                    <Thickness>5</Thickness>
                </WindowChrome.ResizeBorderThickness>
            </WindowChrome>            
        </ResourceDictionary>
    </Window.Resources>
    <Grid>
        <Grid.Background>
            <VisualBrush>
                <VisualBrush.Visual>
                    <MediaElement>
                        <MediaElement.Triggers>
                            <EventTrigger RoutedEvent="MediaElement.Loaded">
                                <EventTrigger.Actions>
                                    <BeginStoryboard>
                                        <Storyboard>
                                            <MediaTimeline Source="C:\Users\lC\Desktop\ad3837d12f2eb938d70576e6d0628535e4dd6f9d.gif" RepeatBehavior="Forever"/>
                                        </Storyboard>
                                    </BeginStoryboard>
                                </EventTrigger.Actions>
                            </EventTrigger>
                        </MediaElement.Triggers>
                    </MediaElement>
                </VisualBrush.Visual>
            </VisualBrush>
        </Grid.Background>
        <Button Width="50" Height="50" Margin="50" Content="按钮"/>
    </Grid>
	</Window>

运行即可。

*TIP:动态图须为本地资源。*

[源码传送](https://github.com/LittleBugProducer/other/tree/master/DynamicBg)