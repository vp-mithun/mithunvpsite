---
title: 'Building Responsive UI using Async Await in C#'
tags:
  - AsyncAwait
  - 'C#'
url: 175.html
id: 175
categories:
  - .NET
date: 2015-09-13 02:58:02
---

Why build responsive UI? Answer seems obvious that end-user should experience that application doesn't hang often (for developers POV, a time taking background operation makes it look like hanging). 
So lets learn building responsive UI using Async Await keywords

> Visual Studio 2012 introduced a simplified approach, async programming, that leverage asynchronous support in the .NET Framework 4.5 and the Windows Runtime. The compiler does the difficult work that the developer used to do, and your application retains a logical structure that resembles synchronous code. This is extract from [MSDN](https://msdn.microsoft.com/en-us/library/hh191443.aspx)

##### Source Code was in written Visual Studio 2015 Community Edition, WPF, C#, .NET 4.5 on Windows 7 OS.

Building Responsive UI using Async Await in C#
----------------------------------------------

A simple WPF application which reads 12MB text file, then loops through and returns unique words with count of its occurrences in text file. Instead of using Thread Sleep or HttpClient to demonstrate async await, I have word count kind of example.

### Create time taking library "WordCountLib"

*   Create blank solution naming "AsyncAwaitDemoApp".
*   Create class library naming "WordCountLib" and create C# class file "WordCountClass".
*   Copy below code containing methods "FindWordCounts" and "FindWordCountsAsync".
{% codeblock lang:cs %}
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text.RegularExpressions;
using System.Threading.Tasks;

namespace WordCountLib
{
    public class WordCountClass
    {
        /// 
        /// Reads through the file, generates unique words and its number of occurrences
        /// 
        /// 
        public List FindWordCounts()
        {
            //Ensure that LongFile.txt exists
            var words = Regex.Matches(File.ReadAllText(@"D:\\LongFile.txt"), @"\\w+").Cast()
            .Select((m, pos) => new { Word = m.Value, Pos = pos })
            .GroupBy(s => s.Word, StringComparer.CurrentCultureIgnoreCase)
            .Select(g => new Words { WordName = g.Key, NoOfOccurance = g.Select(z => z.Pos).Count() })
            .ToList();

            return words;
        }

        /// 
        /// Reads through the file, generates unique words and its number of occurrences using Async and Await
        /// 
        /// 
        public async Task FindWordCountsAsync()
        {
            //Ensure that LongFile.txt exists
            var words = Regex.Matches(File.ReadAllText(@"D:\\LongFile.txt"), @"\\w+").Cast()
            .Select((m, pos) => new { Word = m.Value, Pos = pos })
            .GroupBy(s => s.Word, StringComparer.CurrentCultureIgnoreCase)
            .Select(g => new Words { WordName = g.Key, NoOfOccurance = g.Select(z => z.Pos).Count() });

            //This is more like Task-Based Asynchronous Pattern
            return await Task.Run(() => words.ToList());           
        }
    }
}
{% endcodeblock %}
*   Create C# class "Words". This is POCO class to hold words name and its occurrences. Copy below
{% codeblock lang:cs %}
namespace WordCountLib
{
    public class Words
    {
        public string WordName { get; set; }
        public int NoOfOccurance { get; set; }
    }
}
{% endcodeblock %}
*   LongFile.txt contains text copied from http://norvig.com/big.txt ; I have copied all text twice so that file is 12MB and processing should take time. Ensure that you have file at D:\\LongFile.txt or else FileNotFound exception. _Its present in GitHub source now._

### Building UI using WPF

*   Create WPF application "WordCount.UI", add "WordCountLib" assembly reference so that we can call WordCountClass methods.
*   Open MainWindow.xaml and copy below code. This is startup window for our WPF. Draw using toolbar not so elegant but that's  not needed.

Due to some issues in pasting XAML code, I have moved the entire demo application to GitHub, download it from [Async Await Demo App](https://github.com/mithunvp/AsyncAwaitDemoApp)

*   Open MainWindow.xaml.cs, its code behind file for our start up window. Copy the below code.
    *   Method "btndwn_Click" is old school type of button click event handler for "Search Words", it instantiate "WordCountClass", calls "FindWordsCounts", shows listbox and binds a list of words to listbox
    *   Method "btndwnasyn_Click" is old school type of button click event handler for "Search Words Async Way", it instantiate "WordCountClass", calls "FindWordsCountsAsync", shows listbox and binds a list of words to listbox. Note that it has async keyword in its method signature and await keyword while "FindWordsCountsAsync"
    *   "Log" is very simple method to log information to screen.
*   Build and run it, check out image after it loads screen.

> If we carefully see, await keyword is placed in-line while calling "FindWordsCountsAsync". It clearly indicates this method is time-consuming and will block UI thread.
{% codeblock lang:cs %}
using System;
using System.Windows;
using WordCountLib;

namespace WordCount.UI
{
    /// 
    /// Interaction logic for MainWindow.xaml
    /// 
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }

        /// 
        /// Button click Synchronous processing
        /// 
        
        private  void btndwn_Click(object sender, RoutedEventArgs e)
        {
            Log("START");
            
            if (listBox.Visibility == Visibility.Visible)
            {
                listBox.Visibility = Visibility.Collapsed;
            }

            WordCountClass wrdSimple = new WordCountClass();            
            var listCount = wrdSimple.FindWordCounts();            
            listBox.Visibility = Visibility.Visible;
            listBoxasync.Visibility = Visibility.Collapsed;
            listBox.ItemsSource = listCount;            
            Log("Done ");
        }
        
        private async void btndwnasyn_Click(object sender, RoutedEventArgs e)
        {
            Log("START Async");
            if (listBoxasync.Visibility == Visibility.Visible)
            {
                listBoxasync.Visibility = Visibility.Collapsed;
            }
            WordCountClass wrdSimple = new WordCountClass();
            var listCount = await wrdSimple.FindWordCountsAsync();
            listBoxasync.Visibility = Visibility.Visible;
            listBox.Visibility = Visibility.Collapsed;
            listBoxasync.ItemsSource = listCount;
            
            Log("Done Async");
        }

        private void Log(string text)
        {
            string line = string.Format("{0:HH:mm:ss.fff}: {1}\\r\\n", DateTime.Now, text);
            logtxtBlock.AppendText(line);
        }
    }
}
{% endcodeblock %}
{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977745/asyncWindow_f9tyhx.png 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "WPF Main Window for Async Await Demo" %}

### Testing the responsiveness of the WPF UI

* Click on button "Search Words", try to move window, resize it. You can't do anything as it reads the file, finds all words count and binds to list box. This is called non responsive UI or application hanging. 

Check out GIF image below. Notice that after it loads list box, screen window moves bit because after button click I tried to use window.
{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977742/asyncNONresponsive_ku6yu3.gif 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Non Responsive UI on button click" %}

*   Now run application again, click "Search Words Async Way". Just move around window, resize it, see Log information being written. This called **RESPONSIVE UI using Async Await in C#**
*   Just play around it by clicking buttons back and forth.

{% cloudinary https://res.cloudinary.com/dqnzwoh8g/image/upload/v1532977737/NewResponsiveUI_d0ja7v.gif 320px=c_scale,q_auto:good,w_320;640px=c_scale,q_auto:good,w_640 "Responsive UI using Async Await" %}

> *   Async methods are intended to be non-blocking operations. An await expression in an async method doesn’t block the current thread while the awaited task is running.
> *   The async and await keywords don't cause extra threads to be created. Async methods don't need multi-threading because an async method doesn't run on its own thread.