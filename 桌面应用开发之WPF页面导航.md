##先看效果图
<img src="https://raw.githubusercontent.com/LittleBugProducer/other/master/0.gif" width="500" hegiht="313" align=center />
##Get Start
&emsp;&emsp;为了项目解耦，使用mvvmlight框架。MVVM设计模式请自行了解。
###1 新建项目
&emsp;&emsp;新建一个MvvmLight(WPF)项目，删除其中无关文件夹：Design+Model。
###2 添加类
&emsp;&emsp;添加NavigationService类，继承ViewModelBase和INavigationService。

	using GalaSoft.MvvmLight;
	using GalaSoft.MvvmLight.Views;
	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Threading.Tasks;
	using System.Windows;
	using System.Windows.Controls;
	using System.Windows.Media;
	
	namespace PageNavigation
	{
	    public class NavigationService : ViewModelBase, INavigationService
	    {
	        private readonly Dictionary<string, Uri> _pagesByKey;
	
	        private string _currentPageKey;
	
	        public string CurrentPageKey
	        {
	            get
	            {
	                return _currentPageKey;
	            }
	
	            private set
	            {
	                Set(() => CurrentPageKey, ref _currentPageKey, value);
	            }
	        }
	
	        public NavigationService()
	        {
	            _pagesByKey = new Dictionary<string, Uri>();
	        }
	
	        public void GoBack()
	        {
	            throw new NotImplementedException();
	        }
	
	        public void NavigateTo(string pageKey)
	        {
	            NavigateTo(pageKey, "Next");
	        }
	
	        public void NavigateTo(string pageKey, object parameter)
	        {
	            var frame = GetDescendantFromName(Application.Current.MainWindow, "MainFrame") as Frame;
	
	            if (frame != null)
	            {
	
	                frame.Source = _pagesByKey[pageKey];
	            }
	
	            CurrentPageKey = pageKey;
	        }
	
	        public void Configure(string key, Uri pageType)
	        {
	            lock (_pagesByKey)
	            {
	                if (_pagesByKey.ContainsKey(key))
	                {
	                    _pagesByKey[key] = pageType;
	                }
	                else
	                {
	                    _pagesByKey.Add(key, pageType);
	                }
	            }
	        }
	
	        public Uri getUri(string key)
	        {
	            if (_pagesByKey.ContainsKey(key))
	            {
	                return _pagesByKey[key];
	            }
	
	            return null;
	        }
	
	        private static FrameworkElement GetDescendantFromName(DependencyObject parent, string name)
	        {
	            if (parent == null)
	            {
	                for (int i = 0; i < 10; i++)
	                {
	
	                }
	            }
	            var count = VisualTreeHelper.GetChildrenCount(parent);
	
	            if (count < 1)
	            {
	                return null;
	            }
	
	            for (var i = 0; i < count; i++)
	            {
	                var frameworkElement = VisualTreeHelper.GetChild(parent, i) as FrameworkElement;
	                if (frameworkElement != null)
	                {
	                    if (frameworkElement.Name == name)
	                    {
	                        return frameworkElement;
	                    }
	
	                    frameworkElement = GetDescendantFromName(frameworkElement, name);
	                    if (frameworkElement != null)
	                    {
	                        return frameworkElement;
	                    }
	                }
	            }
	            return null;
	        }
	    }
	}
新建views文件夹，在其中添加用户控件
FirstPage控件

	<UserControl x:Class="PageNavigation.views.FirstPage"
	             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
	             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
	             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
	             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
	             xmlns:local="clr-namespace:PageNavigation.views"
	             mc:Ignorable="d" 
	             d:DesignHeight="300" d:DesignWidth="400">
	    <Grid Background="Green">
	        <Label Content="First" FontSize="40"/>
	    </Grid>
	</UserControl>
SecondPage控件

	<UserControl x:Class="PageNavigation.views.SecondPage"
	             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
	             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
	             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
	             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
	             xmlns:local="clr-namespace:PageNavigation.views"
	             mc:Ignorable="d" 
	             d:DesignHeight="300" d:DesignWidth="400">
	    <Grid Background="Pink">
	        <Label Content="Second" FontSize="40"/>
	    </Grid>
	</UserControl>

ViewModel文件夹中添加FirstViewModel类，继承ViewModelBase

	using GalaSoft.MvvmLight;
	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Threading.Tasks;
	
	namespace PageNavigation.ViewModel
	{
	    public class FirstViewModel:ViewModelBase
	    {
	    }
	}
ViewModel文件夹中添加SecondViewModel类，继承ViewModelBase

	using GalaSoft.MvvmLight;
	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Threading.Tasks;
	
	namespace PageNavigation.ViewModel
	{
	    public class SecondViewModel:ViewModelBase
	    {
	    }
	}


###3 修改类
主界面MainWindow.xaml类

		<Window x:Class="PageNavigation.MainWindow"
		        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
		        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
		        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
		        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
		        xmlns:ignore="http://www.galasoft.ch/ignore"
		        mc:Ignorable="d ignore"
		        Height="600"
		        Width="1000"
		        Title="MVVM Light Application"
		        DataContext="{Binding Main, Source={StaticResource Locator}}">
		    
		    <Window.Resources>
		        <ResourceDictionary>
		            <ResourceDictionary.MergedDictionaries>
		                <ResourceDictionary Source="Skins/MainSkin.xaml" />
		            </ResourceDictionary.MergedDictionaries>
		        </ResourceDictionary>
		    </Window.Resources>
		
		    <Grid x:Name="LayoutRoot" Width="950" Height="550">
		        <DockPanel Width="900" Height="500">
		            <StackPanel DockPanel.Dock="Left" Width="200">
		                <Button Height="50" Content="first" Command="{Binding FirstPageCommand}"></Button>
		                <Button Height="50" Content="second" Command="{Binding SecondPageCommand}"></Button>
		            </StackPanel>
		            <Grid DockPanel.Dock="Right" Width="600" Height="450" Background="Yellow">
		                <Frame x:Name="MainFrame" JournalOwnership="UsesParentJournal" Background="Transparent" Width="500" Height="400"/>
		            </Grid>
		        </DockPanel>
		    </Grid>
		</Window>
ViewModelLocator类，在其中注册各ViewModel，初始化导航类
	
	/*
	  In App.xaml:
	  <Application.Resources>
	      <vm:ViewModelLocatorTemplate xmlns:vm="clr-namespace:PageNavigation.ViewModel"
	                                   x:Key="Locator" />
	  </Application.Resources>
	  
	  In the View:
	  DataContext="{Binding Source={StaticResource Locator}, Path=ViewModelName}"
	*/
	
	using CommonServiceLocator;
	using GalaSoft.MvvmLight;
	using GalaSoft.MvvmLight.Ioc;
	using GalaSoft.MvvmLight.Views;
	using System;
	namespace PageNavigation.ViewModel
	{
	    /// <summary>
	    /// This class contains static references to all the view models in the
	    /// application and provides an entry point for the bindings.
	    /// <para>
	    /// See http://www.mvvmlight.net
	    /// </para>
	    /// </summary>
	    public class ViewModelLocator
	    {
	        static ViewModelLocator()
	        {
	            ServiceLocator.SetLocatorProvider(() => SimpleIoc.Default);
	
	            SimpleIoc.Default.Register<MainViewModel>();
	            SimpleIoc.Default.Register<FirstViewModel>();
	            SimpleIoc.Default.Register<SecondViewModel>();
	
	            var navigationService = CreateNavigationService();
	            SimpleIoc.Default.Register<INavigationService>(() => navigationService);
	        }
	
	        private static INavigationService CreateNavigationService()
	        {
	            var nav = new NavigationService();
	
	            var navigationService = new NavigationService();
	            navigationService.Configure("First", new Uri("/PageNavigation;component/views/FirstPage.xaml", UriKind.Relative));
	            navigationService.Configure("Second", new Uri("/PageNavigation;component/views/SecondPage.xaml", UriKind.Relative));            
	            return navigationService;
	        }
	
	        /// <summary>
	        /// Gets the Main property.
	        /// </summary>
	        [System.Diagnostics.CodeAnalysis.SuppressMessage("Microsoft.Performance",
	            "CA1822:MarkMembersAsStatic",
	            Justification = "This non-static member is needed for data binding purposes.")]
	        public MainViewModel Main
	        {
	            get
	            {
	                return ServiceLocator.Current.GetInstance<MainViewModel>();
	            }
	        }
	
	        public FirstViewModel First
	        {
	            get
	            {
	                return ServiceLocator.Current.GetInstance<FirstViewModel>();
	            }
	        }
	
	        public SecondViewModel Second
	        {
	            get
	            {
	                return ServiceLocator.Current.GetInstance<SecondViewModel>();
	            }
	        }
	
	        /// <summary>
	        /// Cleans up all the resources.
	        /// </summary>
	        public static void Cleanup()
	        {
	        }
	    }
	}
MainViewModel类，其中定义了按钮的点击绑定命令，与view之间用消息通信，即Messenger.Default......

	using CommonServiceLocator;
	using GalaSoft.MvvmLight;
	using GalaSoft.MvvmLight.Command;
	using GalaSoft.MvvmLight.Messaging;
	using GalaSoft.MvvmLight.Views;
	using System;
	using System.Windows.Input;
	
	namespace PageNavigation.ViewModel
	{
	    /// <summary>
	    /// This class contains properties that the main View can data bind to.
	    /// <para>
	    /// See http://www.mvvmlight.net
	    /// </para>
	    /// </summary>
	    public class MainViewModel : ViewModelBase
	    {
	
	        public ICommand FirstPageCommand { get; set; }
	
	        public ICommand SecondPageCommand { get; set; }
	
	        public MainViewModel()
	        {
	            FirstPageCommand = new RelayCommand(First);
	            SecondPageCommand = new RelayCommand(Second);
	        }
	
	        private void First()
	        {
	            var navigationService = ServiceLocator.Current.GetInstance<INavigationService>();
	            NavigationService service = (NavigationService)navigationService;
	            Uri sendUri = service.getUri("First");
	            Messenger.Default.Send<Uri>(sendUri, "FrameSourceName");
	        }
	
	        private void Second()
	        {
	            var navigationService = ServiceLocator.Current.GetInstance<INavigationService>();
	            NavigationService service = (NavigationService)navigationService;
	            Uri sendUri = service.getUri("Second");
	            Messenger.Default.Send<Uri>(sendUri, "FrameSourceName");
	        }
	        
	    }
	}
在MainWindow.xaml.cs定义消息接收以及消息处理

	using System.Windows;
	using PageNavigation.ViewModel;
	using GalaSoft.MvvmLight.Messaging;
	using System;
	
	namespace PageNavigation
	{
	    /// <summary>
	    /// Interaction logic for MainWindow.xaml
	    /// </summary>
	    public partial class MainWindow : Window
	    {
	        /// <summary>
	        /// Initializes a new instance of the MainWindow class.
	        /// </summary>
	        public MainWindow()
	        {
	            InitializeComponent();
	            Messenger.Default.Register<Uri>(this, "FrameSourceName", ChangeFrameSource);
	            Closing += (s, e) => ViewModelLocator.Cleanup();
	        }
	        private void ChangeFrameSource(Uri uri)
	        {
	            this.MainFrame.Source = uri;
	        }
	    }
	}
###end
[源码传送](https://github.com/LittleBugProducer/other/tree/master/PageNavigation)

contact me by email--&emsp;&emsp;duwinter@163.com