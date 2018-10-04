[TOC]


>版权声明：本文为【欧阳鹏】原创文章，欢迎转载，转载请注明出处! 【http://blog.csdn.net/ouyang_peng/article/details/50813419】


>作者：欧阳鹏 欢迎转载，与人分享是进步的源泉！ 
>转载请保留原文地址：
>http://blog.csdn.net/ouyang_peng/article/details/50813419
     
![这里写图片描述](http://img.blog.csdn.net/20150708201910089)

#前言
>最近有个项目有关于播放音乐时候，关于歌词有以下几个功能：
>1、实现歌词同步滚动的功能，即歌曲播放到哪句歌词，就高亮地显示出正在播放的这个歌词；
>2、实现上下拖动歌词时候，可以拖动播放器的进度。即可以不停地上下拖动歌词，当手指离开屏幕时候 即从当前拖动到的歌词位置播放。
>3、实现歌词的字体大小可以进行缩放的功能。即双指在屏幕进行缩放操作时，歌词的字体大小也进行相应的缩放操作。

下面我将这几个功能做成一个demo来展示给大家。首先来看看这个demo的具体实现效果，如下面几幅图所示。

图1、同步滚动歌词
![同步滚动歌词](http://img.blog.csdn.net/20160306133820019)

图2、上下拖动歌词1
![上下拖动歌词1]
(http://img.blog.csdn.net/20160306134002504)

图3、上下拖动歌词2
![上下拖动歌词2](http://img.blog.csdn.net/20160306134120242)

图4、缩放歌词
![缩放歌词](http://img.blog.csdn.net/20160306134225630)


图5、歌词显示（较大字体）
![歌词显示（较大字体）](http://img.blog.csdn.net/20160306134337260)

图6、歌词显示（较小字体）
![歌词显示（较小字体）](http://img.blog.csdn.net/20160306134405058)

图7、歌词滚动时候，高亮地画出正滚动到的歌词内容以及歌词的开始时间，并该句歌词下面画出一条直线
![歌词滚动截图](http://img.blog.csdn.net/20160306134437402)

------


#一、LRC歌词文件简介
## 1、什么是LRC歌词文件

>lrc是英文lyric（歌词）的缩写，被用做歌词文件的扩展名。以lrc为扩展名的歌词文件可以在各类数码播放器中同步显示。
## 2、LRC歌词文件的格式

 >先来看一份标准的LRC歌词文件，下面展示的是**王力宏的《依然爱你》**的lrc歌词的内容

	[ti:依然爱你]
	[ar:王力宏]
	[al:火力全开 新歌+精选]
	[by:欧阳鹏]
	[00:01.17]一闪一闪亮晶晶 留下岁月的痕迹 
	[00:07.29]我的世界的重心 依然还是你
	[00:13.37]一年一年又一年 飞逝尽在一转眼
	[00:20.29]唯一永远不改变 是不停的改变
	[00:27.14]我不像从前的自己 你也有点不像你
	[00:33.36]但在我眼中你的笑 依然的美丽
	[00:39.53]这次只能往前走 一个方向顺时钟
	[00:46.12]不知道还要多久 所以要让你懂
	[00:51.82]我依然爱你 就是唯一的退路
	[00:57.36]我依然珍惜 时时刻刻的幸福
	[01:04.65]你每个呼吸 每个动作 每个表情
	[01:11.43]到最后一定会依然爱你
	[01:18.08]依然爱你 依然爱你
	[01:25.58]我不像从前的自己 你也有点不像你
	[01:31.52]但在我眼中你的笑 依然的美丽
	[01:37.61]这次只能往前走 一个方向顺时钟
	[01:44.42]不知道还要多久 所以要让你懂
	[01:50.18]我依然爱你 就是唯一的退路
	[01:55.65]我依然珍惜 时时刻刻的幸福
	[02:02.84]你每个呼吸 每个动作 每个表情
	[02:09.77]到最后一定会依然爱你
	[02:15.61]
	[02:17.61]lrc制作：http://blog.csdn.net/ouyang_peng 欧阳鹏
	[02:25.61]
	[02:31.06]依然爱你 依然爱你
	[02:36.63]
	[02:42.32]我依然爱你 或许是命中注定
	[02:47.70]多年之后 任何人都无法代替
	[02:54.57]那些时光 是我这一辈子 最美好
	[03:01.84]那些回忆 依然无法忘记 
	[03:07.88]我依然爱你 就是唯一的退路
	[03:13.95]我依然珍惜 时时刻刻的幸福
	[03:21.32]你每个呼吸 每个动作 每个表情
	[03:28.20]到最后一定会依然爱你
	[03:34.76]你每个呼吸 每个动作 每个表情
	[03:42.04]到永远一定会依然爱你
	[03:53.28]
	[04:01.28]  
  
###LRC歌词文件的标签类型

>lrc歌词文本中含有两类标签：一是**标识标签** ，二是**时间标签**。


####**1、标识标签**

**标识标签**，其格式为“**[标识名:值]**”，主要包含以下预定义的标签：

>- [ar:歌手名]
- [ti:歌曲名]
- [al:专辑名]
- [by:编辑者(指lrc歌词的制作人)]
- [offset:时间补偿值] （其单位是毫秒，正值表示整体提前，负值相反。这是用于总体调整显示快慢的，但多数的MP3可能不会支持这种标签）。

####**2、时间标签**
>**时间标签，形式为“[mm:ss]”或“[mm:ss.ff]”(分钟数:秒数.毫秒数)，数字须为非负整数，** 
>
>比如"[12:34.50]"是有效的，而"[0x0C:-34.50]"无效。
>
>**时间标签需位于某行歌词中的句首部分，一行歌词可以包含多个时间标签**
>
>(比如歌词中的迭句部分)。当歌曲播放到达某一时间点时，MP3就会寻找对应的时间标签并显示标签后面的歌词文本，这样就完成了“歌词同步”的功能。

例如下面的这首 **草蜢的《失恋战线联盟》**，就是一行歌词包含了多个时间标签。




	[ti:失恋战线联盟]
	[ar:草蜢]
	[al:]
	[00:00.00]草蜢-失恋战线联盟
	[00:08.78]编辑:小婧
	[01:43.33][00:16.27]她总是只留下电话号码
	[01:46.97][00:19.81]从不肯让我送她回家
	[01:50.61][00:23.43]听说你也曾经爱上过她
	[01:54.15][00:27.07]曾经也同样无法自拔
	[01:57.78][00:30.72]你说你学不会假装潇洒
	[02:01.41][00:34.36]却叫我别太早放弃她
	[02:05.05][00:37.99]把过去传说成一段神话
	[02:08.70][00:41.59]然后笑你是一样的傻
	[02:12.01][00:45.11]我们那么在乎她
	[02:14.15][00:47.01]却被她全部抹杀
	[02:15.96][00:48.87]越谈她越相信永远得不到回答
	[02:19.57][00:52.49]到底她怎么想
	[02:21.35][00:54.28]应该继续在这么
	[02:23.37][00:56.36]还是说穿跑了吧
	[02:26.89][00:59.80]找一个承认失恋的方法
	[02:30.48][01:03.41]让心情好好地放个假
	[02:34.14][01:07.00]当你我不小心又想起她
	[02:45.69][02:42.20][02:37.69][01:10.60]就在记忆里画一个叉
	[02:48.69]
	[01:33.58]编辑:小婧

------

>  [01:43.33][00:16.27]她总是只留下电话号码
>上面这行歌词表示：
>
>在 **[00:16.27]** 这个时间点播放 **“她总是只留下电话号码”** 这句歌词，
>
>在 **[01:43.33]** 这个时间点再一个播放 **“她总是只留下电话号码”** 这句歌词。

其实可以把上面这行歌词拆分为下面两句歌词：

    [00:16.27]她总是只留下电话号码
    [01:43.33]她总是只留下电话号码

------

#二、解析LRC歌词

##1、读取出歌词文件

```
		/**
	     * 从assets目录下读取歌词文件内容
	     * @param fileName
	     * @return
	     */
	    public String getFromAssets(String fileName){
	        try {
	            InputStreamReader inputReader = new InputStreamReader( getResources().getAssets().open(fileName) );
	            BufferedReader bufReader = new BufferedReader(inputReader);
	            String line="";
	            String result="";
	            while((line = bufReader.readLine()) != null){
	                if(line.trim().equals(""))
	                    continue;
	                result += line + "\r\n";
	            }
	            return result;
	        } catch (Exception e) {
	            e.printStackTrace();
	        }
	        return "";
	    }

```

>例如：从assets目录下读取test.lrc歌词文件内容，则可以调用上面的**getFromAssets(String fileName)**方法得到歌词的文本内容，如下所示：
```
    String lrc = getFromAssets("test.lrc");
```

##2、解析得到的歌词内容

###1、表示每行歌词内容的实体类LrcRow
首先封装一个表示每行歌词内容的实体类LrcRow，该类由三个属性，分别为：
strTime、time、content。

>例如一行歌词内容为：[02:34.14]当你我不小心又想起她 , 解析该行歌词后的实体类LrcRow的属性如下所示：

+ strTime表示该行歌词要开始播放的时间，格式如下：[02:34.14]
+ time表示将strTime转换为long型之后的数值
  >例如将strTime为[02:34.14]格式转换154014（154014=02 * 60 * 1000 + 34 * 1000+14）

+ content表示该行歌词的内容，如：当你我不小心又想起她


代码如下：
```
	package com.oyp.lrc.view.impl;
	
	import android.util.Log;
	
	import java.util.ArrayList;
	import java.util.List;
	
	/**
	 * 歌词行
	 * 包括该行歌词的时间，歌词内容
	 */
	public class LrcRow implements Comparable<LrcRow>{
	    public final static String TAG = "LrcRow";
	
	    /** 该行歌词要开始播放的时间，格式如下：[02:34.14] */
	    public String strTime;
	
	    /** 该行歌词要开始播放的时间，由[02:34.14]格式转换为long型，
	     * 即将2分34秒14毫秒都转为毫秒后 得到的long型值：time=02*60*1000+34*1000+14
	     */
	    public long time;
	
	    /** 该行歌词的内容 */
	    public String content;
	
	    
	    public LrcRow(){}
	    
	    public LrcRow(String strTime,long time,String content){
	        this.strTime = strTime;
	        this.time = time;
	        this.content = content;
	//      Log.d(TAG,"strTime:" + strTime + " time:" + time + " content:" + content);
	    }
	
	    @Override
	    public String toString() {
	        return "[" + strTime + " ]"  + content;
	    }
	
	    /**
	     * 读取歌词的每一行内容，转换为LrcRow，加入到集合中
	     */
	    public static List<LrcRow> createRows(String standardLrcLine){
	        /**
	            一行歌词只有一个时间的  例如：徐佳莹   《我好想你》
	            [01:15.33]我好想你 好想你
	
	            一行歌词有多个时间的  例如：草蜢 《失恋战线联盟》
	            [02:34.14][01:07.00]当你我不小心又想起她
	            [02:45.69][02:42.20][02:37.69][01:10.60]就在记忆里画一个叉
	         **/
	        try{
	            if(standardLrcLine.indexOf("[") != 0 || standardLrcLine.indexOf("]") != 9 ){
	                return null;
	            }
	            //[02:34.14][01:07.00]当你我不小心又想起她
	            //找到最后一个 ‘]’ 的位置
	            int lastIndexOfRightBracket = standardLrcLine.lastIndexOf("]");
	            //歌词内容就是 ‘]’ 的位置之后的文本   eg:   当你我不小心又想起她
	            String content = standardLrcLine.substring(lastIndexOfRightBracket + 1, standardLrcLine.length());
	            //歌词时间就是 ‘]’ 的位置之前的文本   eg:   [02:34.14][01:07.00]
	
	            /**
	                将时间格式转换一下  [mm:ss.SS][mm:ss.SS] 转换为  -mm:ss.SS--mm:ss.SS-
	                即：[02:34.14][01:07.00]  转换为      -02:34.14--01:07.00-
	             */
	            String times = standardLrcLine.substring(0,lastIndexOfRightBracket + 1).replace("[", "-").replace("]", "-");
	            //通过 ‘-’ 来拆分字符串
	            String arrTimes[] = times.split("-");
	            List<LrcRow> listTimes = new ArrayList<LrcRow>();
	            for(String temp : arrTimes){
	                if(temp.trim().length() == 0){
	                    continue;
	                }
	                /** [02:34.14][01:07.00]当你我不小心又想起她
	                 *
	                    上面的歌词的就可以拆分为下面两句歌词了
	                    [02:34.14]当你我不小心又想起她
	                    [01:07.00]当你我不小心又想起她
	                 */
	                LrcRow lrcRow = new LrcRow(temp, timeConvert(temp), content);
	                listTimes.add(lrcRow);
	            }
	            return listTimes;
	        }catch(Exception e){
	            Log.e(TAG,"createRows exception:" + e.getMessage());
	            return null;
	        }
	    }
	
	    /**
	     * 将解析得到的表示时间的字符转化为Long型
	     */
	    private static long timeConvert(String timeString){
	        //因为给如的字符串的时间格式为XX:XX.XX,返回的long要求是以毫秒为单位
	        //将字符串 XX:XX.XX 转换为 XX:XX:XX
	        timeString = timeString.replace('.', ':');
	        //将字符串 XX:XX:XX 拆分
	        String[] times = timeString.split(":");
	        // mm:ss:SS
	        return Integer.valueOf(times[0]) * 60 * 1000 +//分
	                Integer.valueOf(times[1]) * 1000 +//秒
	                Integer.valueOf(times[2]) ;//毫秒
	    }
	
	    /**
	     * 排序的时候，根据歌词的时间来排序
	     */
	    public int compareTo(LrcRow another) {
	        return (int)(this.time - another.time);
	    }
	}
```
该LrcRow的List<LrcRow> createRows(String standardLrcLine)方法 ，将循环地一行一行的去读取歌词的内容。然后对每一行的歌词进行解析，每解析出一个时间标签[XX:XX.XX]则new出一个LrcRow对象，然后加入到歌词行List集合中去。

该LrcRow类实现Comparable接口，用来进行解析之后的排序操作，排序按时间从小到大排序。

###2、解析歌词的构造器
####ILrcBuilder接口
定义一个ILrcBuilder接口，接口有一个List<LrcRow> getLrcRows(String rawLrc)方法，该方法用来解析歌词，得到LrcRow的集合
```
	package com.oyp.lrc.view;
	
	import com.oyp.lrc.view.impl.LrcRow;
	
	import java.util.List;
	
	/**
	 * 解析歌词，得到LrcRow的集合
	 */
	public interface ILrcBuilder {
	    List<LrcRow> getLrcRows(String rawLrc);
	}
```
####DefaultLrcBuilder歌词解析构造器
>DefaultLrcBuilder实现ILrcBuilder接口，List<LrcRow> getLrcRows(String rawLrc)方法会循环地读取歌词的每一行，然后调用LrcRow类的List<LrcRow> createRows(String standardLrcLine)方法，得到解析每一行歌词之后的LrcRow集合，再将每一行得到LrcRow集合中得到的LrcRow实体加入一个总 的到LrcRow集合rows中去，然后将rows集合根据歌词行的时间排序，得到排序后的LrcRow集合，该集合就是最终的解析歌词后的内容了。

代码如下：
```
	package com.oyp.lrc.view.impl;
	
	import android.util.Log;
	import com.oyp.lrc.view.ILrcBuilder;
	import java.io.BufferedReader;
	import java.io.IOException;
	import java.io.StringReader;
	import java.util.ArrayList;
	import java.util.Collections;
	import java.util.List;
	
	/**
	 * 解析歌词，得到LrcRow的集合
	 */
	public class DefaultLrcBuilder implements ILrcBuilder {
	    static final String TAG = "DefaultLrcBuilder";
	
	    public List<LrcRow> getLrcRows(String rawLrc) {
	        Log.d(TAG,"getLrcRows by rawString");
	        if(rawLrc == null || rawLrc.length() == 0){
	            Log.e(TAG,"getLrcRows rawLrc null or empty");
	            return null;
	        }
	        StringReader reader = new StringReader(rawLrc);
	        BufferedReader br = new BufferedReader(reader);
	        String line = null;
	        List<LrcRow> rows = new ArrayList<LrcRow>();
	        try{
	            //循环地读取歌词的每一行
	            do{
	                line = br.readLine();
	                /**
	                 一行歌词只有一个时间的  例如：徐佳莹   《我好想你》
	                 [01:15.33]我好想你 好想你
	
	                 一行歌词有多个时间的  例如：草蜢 《失恋战线联盟》
	                 [02:34.14][01:07.00]当你我不小心又想起她
	                 [02:45.69][02:42.20][02:37.69][01:10.60]就在记忆里画一个叉
	                 **/
	                Log.d(TAG,"lrc raw line: " + line);
	                if(line != null && line.length() > 0){
	                    //解析每一行歌词 得到每行歌词的集合，因为有些歌词重复有多个时间，就可以解析出多个歌词行来
	                    List<LrcRow> lrcRows = LrcRow.createRows(line);
	                    if(lrcRows != null && lrcRows.size() > 0){
	                        for(LrcRow row : lrcRows){
	                            rows.add(row);
	                        }
	                    }
	                }
	            }while(line != null);
	
	            if( rows.size() > 0 ){
	                // 根据歌词行的时间排序
	                Collections.sort(rows);
	                if(rows!=null&&rows.size()>0){
	                    for(LrcRow lrcRow:rows){
	                        Log.d(TAG, "lrcRow:" + lrcRow.toString());
	                    }
	                }
	            }
	        }catch(Exception e){
	            Log.e(TAG,"parse exceptioned:" + e.getMessage());
	            return null;
	        }finally{
	            try {
	                br.close();
	            } catch (IOException e) {
	                e.printStackTrace();
	            }
	            reader.close();
	        }
	        return rows;
	    }
	}

```
例如：通过下面代码来调用ILrcBuilder解析歌词，
```
		//从assets目录下读取歌词文件内容
        String lrc = getFromAssets("test.lrc");
        //解析歌词构造器
        ILrcBuilder builder = new DefaultLrcBuilder();
        //解析歌词返回LrcRow集合
        List<LrcRow> rows = builder.getLrcRows(lrc);
```
####lrc歌词原始内容

草蜢的《失恋战线联盟》，lrc原始内容如下：

	[ti:失恋战线联盟]
	[ar:草蜢]
	[al:]
	[00:00.00]草蜢-失恋战线联盟
	[00:08.78]编辑:小婧
	[01:43.33][00:16.27]她总是只留下电话号码
	[01:46.97][00:19.81]从不肯让我送她回家
	[01:50.61][00:23.43]听说你也曾经爱上过她
	[01:54.15][00:27.07]曾经也同样无法自拔
	[01:57.78][00:30.72]你说你学不会假装潇洒
	[02:01.41][00:34.36]却叫我别太早放弃她
	[02:05.05][00:37.99]把过去传说成一段神话
	[02:08.70][00:41.59]然后笑你是一样的傻
	[02:12.01][00:45.11]我们那么在乎她
	[02:14.15][00:47.01]却被她全部抹杀
	[02:15.96][00:48.87]越谈她越相信永远得不到回答
	[02:19.57][00:52.49]到底她怎么想
	[02:21.35][00:54.28]应该继续在这么
	[02:23.37][00:56.36]还是说穿跑了吧
	[02:26.89][00:59.80]找一个承认失恋的方法
	[02:30.48][01:03.41]让心情好好地放个假
	[02:34.14][01:07.00]当你我不小心又想起她
	[02:45.69][02:42.20][02:37.69][01:10.60]就在记忆里画一个叉
	[02:48.69]
	[01:33.58]编辑:小婧


读取该歌词内容，过程中的打印日志为：

	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [ti:失恋战线联盟]
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [ar:草蜢]
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [al:]
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [00:00.00]草蜢-失恋战线联盟
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [00:08.78]编辑:小婧
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [01:43.33][00:16.27]她总是只留下电话号码
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [01:46.97][00:19.81]从不肯让我送她回家
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [01:50.61][00:23.43]听说你也曾经爱上过她
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [01:54.15][00:27.07]曾经也同样无法自拔
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [01:57.78][00:30.72]你说你学不会假装潇洒
	03-06 00:41:15.352 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:01.41][00:34.36]却叫我别太早放弃她
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:05.05][00:37.99]把过去传说成一段神话
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:08.70][00:41.59]然后笑你是一样的傻
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:12.01][00:45.11]我们那么在乎她
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:14.15][00:47.01]却被她全部抹杀
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:15.96][00:48.87]越谈她越相信永远得不到回答
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:19.57][00:52.49]到底她怎么想
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:21.35][00:54.28]应该继续在这么
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:23.37][00:56.36]还是说穿跑了吧
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:26.89][00:59.80]找一个承认失恋的方法
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:30.48][01:03.41]让心情好好地放个假
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:34.14][01:07.00]当你我不小心又想起她
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:45.69][02:42.20][02:37.69][01:10.60]就在记忆里画一个叉
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [02:48.69]
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: [01:33.58]编辑:小婧
	03-06 00:41:15.362 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrc raw line: null

解析歌词后遍历List<LrcRow>集合的打印日志为：

	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:00.00 ]草蜢-失恋战线联盟
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:08.78 ]编辑:小婧
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:16.27 ]她总是只留下电话号码
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:19.81 ]从不肯让我送她回家
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:23.43 ]听说你也曾经爱上过她
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:27.07 ]曾经也同样无法自拔
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:30.72 ]你说你学不会假装潇洒
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:34.36 ]却叫我别太早放弃她
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:37.99 ]把过去传说成一段神话
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:41.59 ]然后笑你是一样的傻
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:45.11 ]我们那么在乎她
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:47.01 ]却被她全部抹杀
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:48.87 ]越谈她越相信永远得不到回答
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:52.49 ]到底她怎么想
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:54.28 ]应该继续在这么
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:56.36 ]还是说穿跑了吧
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[00:59.80 ]找一个承认失恋的方法
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:03.41 ]让心情好好地放个假
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:07.00 ]当你我不小心又想起她
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:10.60 ]就在记忆里画一个叉
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:33.58 ]编辑:小婧
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:43.33 ]她总是只留下电话号码
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:46.97 ]从不肯让我送她回家
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:50.61 ]听说你也曾经爱上过她
	03-06 00:41:15.372 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:54.15 ]曾经也同样无法自拔
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[01:57.78 ]你说你学不会假装潇洒
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:01.41 ]却叫我别太早放弃她
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:05.05 ]把过去传说成一段神话
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:08.70 ]然后笑你是一样的傻
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:12.01 ]我们那么在乎她
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:14.15 ]却被她全部抹杀
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:15.96 ]越谈她越相信永远得不到回答
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:19.57 ]到底她怎么想
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:21.35 ]应该继续在这么
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:23.37 ]还是说穿跑了吧
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:26.89 ]找一个承认失恋的方法
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:30.48 ]让心情好好地放个假
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:34.14 ]当你我不小心又想起她
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:37.69 ]就在记忆里画一个叉
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:42.20 ]就在记忆里画一个叉
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:45.69 ]就在记忆里画一个叉
	03-06 00:41:15.382 5265-5265/com.oyp.lrc D/DefaultLrcBuilder: lrcRow:[02:48.69 ]

####lrc歌词解析后的内容

即，草蜢的《失恋战线联盟》的lrc歌词解析完后的内容如下：

	[00:00.00 ]草蜢-失恋战线联盟
	[00:08.78 ]编辑:小婧
	[00:16.27 ]她总是只留下电话号码
	[00:19.81 ]从不肯让我送她回家
	[00:23.43 ]听说你也曾经爱上过她
	[00:27.07 ]曾经也同样无法自拔
	[00:30.72 ]你说你学不会假装潇洒
	[00:34.36 ]却叫我别太早放弃她
	[00:37.99 ]把过去传说成一段神话
	[00:41.59 ]然后笑你是一样的傻
	[00:45.11 ]我们那么在乎她
	[00:47.01 ]却被她全部抹杀
	[00:48.87 ]越谈她越相信永远得不到回答
	[00:52.49 ]到底她怎么想
	[00:54.28 ]应该继续在这么
	[00:56.36 ]还是说穿跑了吧
	[00:59.80 ]找一个承认失恋的方法
	[01:03.41 ]让心情好好地放个假
	[01:07.00 ]当你我不小心又想起她
	[01:10.60 ]就在记忆里画一个叉
	[01:33.58 ]编辑:小婧
	[01:43.33 ]她总是只留下电话号码
	[01:46.97 ]从不肯让我送她回家
	[01:50.61 ]听说你也曾经爱上过她
	[01:54.15 ]曾经也同样无法自拔
	[01:57.78 ]你说你学不会假装潇洒
	[02:01.41 ]却叫我别太早放弃她
	[02:05.05 ]把过去传说成一段神话
	[02:08.70 ]然后笑你是一样的傻
	[02:12.01 ]我们那么在乎她
	[02:14.15 ]却被她全部抹杀
	[02:15.96 ]越谈她越相信永远得不到回答
	[02:19.57 ]到底她怎么想
	[02:21.35 ]应该继续在这么
	[02:23.37 ]还是说穿跑了吧
	[02:26.89 ]找一个承认失恋的方法
	[02:30.48 ]让心情好好地放个假
	[02:34.14 ]当你我不小心又想起她
	[02:37.69 ]就在记忆里画一个叉
	[02:42.20 ]就在记忆里画一个叉
	[02:45.69 ]就在记忆里画一个叉
	[02:48.69 ]

下面是解析歌词前后的对比图
![这里写图片描述](http://img.blog.csdn.net/20160306151059728)

至此，歌词解析完毕！

------

#三、显示LRC歌词内容

##1、定义一个ILrcViewListener接口
ILrcViewListener接口，该接口定义了一个onLrcSeeked方法用来监听用户上下拖动歌词的动作定义了一个方法

+ onLrcSeeked(int newPosition, LrcRow row)
	
>当歌词被用户上下拖动的时候回调该方法

```
	package com.oyp.lrc.view;
	
	import com.oyp.lrc.view.impl.LrcRow;
	
	/**
	 * 歌词拖动时候的监听类
	 */
	public interface ILrcViewListener {
	    /**
	     * 当歌词被用户上下拖动的时候回调该方法
	     */
	    void onLrcSeeked(int newPosition, LrcRow row);
	}
```
##2、定义一个ILrcView接口

ILrcView接口接口，定义了三个方法

+ setLrc(List<LrcRow> lrcRows)
	
>调用该方法设置要展示的歌词行集合

+ seekLrcToTime(long time)
>音乐播放的时候调用该方法滚动歌词，高亮正在播放的那句歌词

+ setListener(ILrcViewListener l)
>调用该方法设设置歌词拖动时候的监听类，用以回调ILrcViewListener的onLrcSeeked(int newPosition, LrcRow row)方法
```
	package com.oyp.lrc.view;
	
	import com.oyp.lrc.view.impl.LrcRow;
	
	import java.util.List;
	
	/**
	 * 展示歌词的接口
	 */
	public interface ILrcView {
	
	    /**
	     * 设置要展示的歌词行集合
	     */
	    void setLrc(List<LrcRow> lrcRows);
	
	    /**
	     * 音乐播放的时候调用该方法滚动歌词，高亮正在播放的那句歌词
	     */
	    void seekLrcToTime(long time);
	    /**
	     * 设置歌词拖动时候的监听类
	     */
	    void setListener(ILrcViewListener l);
	}
```

##3、自定义一个LrcView

自定义一个LrcView，该LrcView继承android.view.View对象，实现了ILrcView接口。该自定义LrcView可以实现了同步显示歌词，拖动歌词，缩放歌词等功能。

###同步显示歌词功能
首先来说说显示歌词的实现思路，要显示歌词即把歌词的内容绘制出来，可以分以下三步来绘制歌词：

>第1步：高亮地画出正在播放的那句歌词
>
>第2步：画出正在播放的那句歌词的上面可以展示出来的歌词
>
>第3步：画出正在播放的那句歌词的下面的可以展示出来的歌词

重写onDraw(Canvas canvas)方法，在方法中按照上面的思路来绘制者三部分的歌词。代码如下：

```
 @Override
    protected void onDraw(Canvas canvas) {
        final int height = getHeight(); // height of this view
        final int width = getWidth(); // width of this view
        //当没有歌词的时候
        if (mLrcRows == null || mLrcRows.size() == 0) {
            if (mLoadingLrcTip != null) {
                // draw tip when no lrc.
                mPaint.setColor(mHignlightRowColor);
                mPaint.setTextSize(mLrcFontSize);
                mPaint.setTextAlign(Align.CENTER);
                canvas.drawText(mLoadingLrcTip, width / 2, height / 2 - mLrcFontSize, mPaint);
            }
            return;
        }

        int rowY = 0; // vertical point of each row.
        final int rowX = width / 2;
        int rowNum = 0;
        /**
         * 分以下三步来绘制歌词：
         *
         * 	第1步：高亮地画出正在播放的那句歌词
         *	第2步：画出正在播放的那句歌词的上面可以展示出来的歌词
         *	第3步：画出正在播放的那句歌词的下面的可以展示出来的歌词
         */
        // 1、 高亮地画出正在要高亮的的那句歌词
        String highlightText = mLrcRows.get(mHignlightRow).content;
        int highlightRowY = height / 2 - mLrcFontSize;
        mPaint.setColor(mHignlightRowColor);
        mPaint.setTextSize(mLrcFontSize);
        mPaint.setTextAlign(Align.CENTER);
        canvas.drawText(highlightText, rowX, highlightRowY, mPaint);

        // 上下拖动歌词的时候 画出拖动要高亮的那句歌词的时间 和 高亮的那句歌词下面的一条直线
        if (mDisplayMode == DISPLAY_MODE_SEEK) {
            // 画出高亮的那句歌词下面的一条直线
            mPaint.setColor(mSeekLineColor);
            //该直线的x坐标从0到屏幕宽度  y坐标为高亮歌词和下一行歌词中间
            canvas.drawLine(mSeekLinePaddingX, highlightRowY + mPaddingY, width - mSeekLinePaddingX, highlightRowY + mPaddingY, mPaint);

            // 画出高亮的那句歌词的时间
            mPaint.setColor(mSeekLineTextColor);
            mPaint.setTextSize(mSeekLineTextSize);
            mPaint.setTextAlign(Align.LEFT);
            canvas.drawText(mLrcRows.get(mHignlightRow).strTime, 0, highlightRowY, mPaint);
        }

        // 2、画出正在播放的那句歌词的上面可以展示出来的歌词
        mPaint.setColor(mNormalRowColor);
        mPaint.setTextSize(mLrcFontSize);
        mPaint.setTextAlign(Align.CENTER);
        rowNum = mHignlightRow - 1;
        rowY = highlightRowY - mPaddingY - mLrcFontSize;
        //只画出正在播放的那句歌词的上一句歌词
//        if (rowY > -mLrcFontSize && rowNum >= 0) {
//            String text = mLrcRows.get(rowNum).content;
//            canvas.drawText(text, rowX, rowY, mPaint);
//        }

        //画出正在播放的那句歌词的上面所有的歌词
		while( rowY > -mLrcFontSize && rowNum >= 0){
			String text = mLrcRows.get(rowNum).content;
			canvas.drawText(text, rowX, rowY, mPaint);
			rowY -=  (mPaddingY + mLrcFontSize);
			rowNum --;
		}

        // 3、画出正在播放的那句歌词的下面的可以展示出来的歌词
        rowNum = mHignlightRow + 1;
        rowY = highlightRowY + mPaddingY + mLrcFontSize;

        //只画出正在播放的那句歌词的下一句歌词
//        if (rowY < height && rowNum < mLrcRows.size()) {
//            String text2 = mLrcRows.get(rowNum).content;
//            canvas.drawText(text2, rowX, rowY, mPaint);
//        }

		//画出正在播放的那句歌词的所有下面的可以展示出来的歌词
		while( rowY < height && rowNum < mLrcRows.size()){
			String text = mLrcRows.get(rowNum).content;
			canvas.drawText(text, rowX, rowY, mPaint);
			rowY += (mPaddingY + mLrcFontSize);
			rowNum ++;
		}

    }
```

为了实现同步显示功能的功能，则需要不停地将自定义的LrcView进行重绘。首先当MediaPlayer开始播放的时候，同步的启动一个TimerTask来进行歌词的滚动操作。如代码所示：

```
mPlayer.setOnPreparedListener(new OnPreparedListener() {
                //准备完毕
				public void onPrepared(MediaPlayer mp) {
					mp.start();
			        if(mTimer == null){
			        	mTimer = new Timer();
			        	mTask = new LrcTask();
			        	mTimer.scheduleAtFixedRate(mTask, 0, mPalyTimerDuration);
			        }
				}
			});
```

上面代码的意思是，当MediaPlayer开始播放的时候，启动一个定时器Timer，然后通过这个定时器每隔mPalyTimerDuration时间来执行一次LrcTask任务。LrcTask的代码如下：

```
 /**
     * 展示歌曲的定时任务
     */
    class LrcTask extends TimerTask{
        @Override
        public void run() {
            //获取歌曲播放的位置
            final long timePassed = mPlayer.getCurrentPosition();
            MainActivity.this.runOnUiThread(new Runnable() {
                public void run() {
                    //滚动歌词
                    mLrcView.seekLrcToTime(timePassed);
                }
            });

        }
    };
```
上面的代码是：首先获取MediaPlayer的播放进度值，然后调用了LrcView的seekLrcToTime(long time)方法进行歌词同步滚动，LrcView的seekLrcToTime(long time)方法的实现代码如下：

```
/**
     * 播放的时候调用该方法滚动歌词，高亮正在播放的那句歌词
     * @param time
     */
    public void seekLrcToTime(long time) {
        if (mLrcRows == null || mLrcRows.size() == 0) {
            return;
        }
        if (mDisplayMode != DISPLAY_MODE_NORMAL) {
            return;
        }
        Log.d(TAG, "seekLrcToTime:" + time);

        for (int i = 0; i < mLrcRows.size(); i++) {
            LrcRow current = mLrcRows.get(i);
            LrcRow next = i + 1 == mLrcRows.size() ? null : mLrcRows.get(i + 1);
            /**
             *  正在播放的时间大于current行的歌词的时间而小于next行歌词的时间， 设置要高亮的行为current行
             *  正在播放的时间大于current行的歌词，而current行为最后一句歌词时，设置要高亮的行为current行
             */
            if ((time >= current.time && next != null && time < next.time)
                    || (time > current.time && next == null)){
                seekLrc(i, false);
                return;
            }
        }
    }
```

上面代码意思是，首先通过传入进来的MediaPlayer的播放进度值，来判断需要高亮地歌词行LrcRow是哪一行，然后调用seekLrc(int position, boolean cb)方法来进行歌词重绘操作。seekLrc(int position, boolean cb)方法的实现如下所示：

```
/**
     * 设置要高亮的歌词为第几行歌词
     *
     * @param position 要高亮的歌词行数
     * @param cb       是否是手指拖动后要高亮的歌词
     */
    public void seekLrc(int position, boolean cb) {
        if (mLrcRows == null || position < 0 || position > mLrcRows.size()) {
            return;
        }
        LrcRow lrcRow = mLrcRows.get(position);
        mHignlightRow = position;
        invalidate();
        //如果是手指拖动歌词后
        if (mLrcViewListener != null && cb) {
            //回调onLrcSeeked方法，将音乐播放器播放的位置移动到高亮歌词的位置
            mLrcViewListener.onLrcSeeked(position, lrcRow);
        }
    }
```
上面方法是将要高亮的歌词行设置为目前正在播放的歌词行，然后重绘LrcView。


###拖动歌词的功能

要实现拖动歌词的功能，可以分为以下几步来实现
1、给LrcView注册一个ILrcViewListener监听接口。

下面是LrcView注册ILrcViewListener监听的具体实现。
```
 //设置自定义的LrcView上下拖动歌词时监听
        mLrcView.setListener(new ILrcViewListener() {
            //当歌词被用户上下拖动的时候回调该方法,从高亮的那一句歌词开始播放
            public void onLrcSeeked(int newPosition, LrcRow row) {
                if (mPlayer != null) {
                    Log.d(TAG, "onLrcSeeked:" + row.time);
                    mPlayer.seekTo((int) row.time);
                }
            }
        });
```


2、当歌词进行拖动的时候，回调ILrcViewListener接口的onLrcSeeked(int newPosition, LrcRow row)方法。

如下面代码所示：回调了onLrcSeeked(int newPosition, LrcRow row)方法。
```
/**
     * 设置要高亮的歌词为第几行歌词
     *
     * @param position 要高亮的歌词行数
     * @param cb       是否是手指拖动后要高亮的歌词
     */
    public void seekLrc(int position, boolean cb) {
        if (mLrcRows == null || position < 0 || position > mLrcRows.size()) {
            return;
        }
        LrcRow lrcRow = mLrcRows.get(position);
        mHignlightRow = position;
        invalidate();
        //如果是手指拖动歌词后
        if (mLrcViewListener != null && cb) {
            //回调onLrcSeeked方法，将音乐播放器播放的位置移动到高亮歌词的位置
            mLrcViewListener.onLrcSeeked(position, lrcRow);
        }
    }
```

3、判断手指在屏幕上的操作，来进行歌词滚动的操作。
重写onTouchEvent(MotionEvent event)方法，来判断手指的操作是拖动歌词还是缩放歌词。
```
 @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (mLrcRows == null || mLrcRows.size() == 0) {
            return super.onTouchEvent(event);
        }
        switch (event.getAction()) {
            //手指按下
            case MotionEvent.ACTION_DOWN:
                Log.d(TAG, "down,mLastMotionY:" + mLastMotionY);
                mLastMotionY = event.getY();
                mIsFirstMove = true;
                invalidate();
                break;
            //手指移动
            case MotionEvent.ACTION_MOVE:
                if (event.getPointerCount() == 2) {
                    Log.d(TAG, "two move");
                    doScale(event);
                    return true;
                }
                Log.d(TAG, "one move");
                // single pointer mode ,seek
                //如果是双指同时按下，进行歌词大小缩放，抬起其中一个手指，另外一个手指不离开屏幕地移动的话，不做任何处理
                if (mDisplayMode == DISPLAY_MODE_SCALE) {
                    //if scaling but pointer become not two ,do nothing.
                    return true;
                }
                //如果一个手指按下，在屏幕上移动的话，拖动歌词上下
                doSeek(event);
                break;
            case MotionEvent.ACTION_CANCEL:
                //手指抬起
            case MotionEvent.ACTION_UP:
                if (mDisplayMode == DISPLAY_MODE_SEEK) {
                    //高亮手指抬起时的歌词并播放从该句歌词开始播放
                    seekLrc(mHignlightRow, true);
                }
                mDisplayMode = DISPLAY_MODE_NORMAL;
                invalidate();
                break;
        }
        return true;
    }
```
如上代码所示，当一个手指移动的时候，则调用doSeek(MotionEvent event)方法来进行拖动歌词的操作，doSeek(MotionEvent event)方法的具体实现代码如下：

```
/**
     * 处理单指在屏幕移动时，歌词上下滚动
     */
    private void doSeek(MotionEvent event) {
        float y = event.getY();//手指当前位置的y坐标
        float offsetY = y - mLastMotionY; //第一次按下的y坐标和目前移动手指位置的y坐标之差
        //如果移动距离小于10，不做任何处理
        if (Math.abs(offsetY) < mMinSeekFiredOffset) {
            return;
        }
        //将模式设置为拖动歌词模式
        mDisplayMode = DISPLAY_MODE_SEEK;
        int rowOffset = Math.abs((int) offsetY / mLrcFontSize); //歌词要滚动的行数

        Log.d(TAG, "move to new hightlightrow : " + mHignlightRow + " offsetY: " + offsetY + " rowOffset:" + rowOffset);

        if (offsetY < 0) {
            //手指向上移动，歌词向下滚动
            mHignlightRow += rowOffset;//设置要高亮的歌词为 当前高亮歌词 向下滚动rowOffset行后的歌词
        } else if (offsetY > 0) {
            //手指向下移动，歌词向上滚动
            mHignlightRow -= rowOffset;//设置要高亮的歌词为 当前高亮歌词 向上滚动rowOffset行后的歌词
        }
        //设置要高亮的歌词为0和mHignlightRow中的较大值，即如果mHignlightRow < 0，mHignlightRow=0
        mHignlightRow = Math.max(0, mHignlightRow);
        //设置要高亮的歌词为0和mHignlightRow中的较小值，即如果mHignlight > RowmLrcRows.size()-1，mHignlightRow=mLrcRows.size()-1
        mHignlightRow = Math.min(mHignlightRow, mLrcRows.size() - 1);
        //如果歌词要滚动的行数大于0，则重画LrcView
        if (rowOffset > 0) {
            mLastMotionY = y;
            invalidate();
        }
    }
```
如上面代码所示，当一个手指不停的在屏幕上移动时，将会不停地调用doSeek(MotionEvent event)方法来进行LrcView的重绘操作，从而实现了歌词拖动的效果。

当手指离开屏幕的时候，即MotionEvent 为MotionEvent.ACTION_UP的时候，会调用seekLrc(int position, boolean cb)方法，从而回调ILrcViewListener接口的onLrcSeeked方法，来拖动MediaPlayer的播放进度值，从而达到了拖动歌词后从最终高亮的歌词开始重新播放歌词的功能。如下代码所示：
```
case MotionEvent.ACTION_UP:
                if (mDisplayMode == DISPLAY_MODE_SEEK) {
                    //高亮手指抬起时的歌词并播放从该句歌词开始播放
                    seekLrc(mHignlightRow, true);
                }
                mDisplayMode = DISPLAY_MODE_NORMAL;
                invalidate();
                break;
```

###缩放歌词的功能
如onTouchEvent(MotionEvent event)方法中所示，当两个手指在屏幕上移动的时候，调用doScale(MotionEvent event)方法来做缩放歌词的功能。

```
case MotionEvent.ACTION_MOVE:
                if (event.getPointerCount() == 2) {
                    Log.d(TAG, "two move");
                    doScale(event);
                    return true;
                }
                Log.d(TAG, "one move");
                // single pointer mode ,seek
                //如果是双指同时按下，进行歌词大小缩放，抬起其中一个手指，另外一个手指不离开屏幕地移动的话，不做任何处理
                if (mDisplayMode == DISPLAY_MODE_SCALE) {
                    //if scaling but pointer become not two ,do nothing.
                    return true;
                }
                //如果一个手指按下，在屏幕上移动的话，拖动歌词上下
                doSeek(event);
                break;
```
doScale(MotionEvent event)方法的具体实现代码如下所示：

```
/**
     * 处理双指在屏幕移动时的，歌词大小缩放
     */
    private void doScale(MotionEvent event) {
        //如果歌词的模式为：拖动歌词模式
        if (mDisplayMode == DISPLAY_MODE_SEEK) {
            //如果是单指按下，在进行歌词上下滚动，然后按下另外一个手指，则把歌词模式从 拖动歌词模式 变为 缩放歌词模式
            mDisplayMode = DISPLAY_MODE_SCALE;
            Log.d(TAG, "change mode from DISPLAY_MODE_SEEK to DISPLAY_MODE_SCALE");
            return;
        }
        // two pointer mode , scale font
        if (mIsFirstMove) {
            mDisplayMode = DISPLAY_MODE_SCALE;
            invalidate();
            mIsFirstMove = false;
            //两个手指的x坐标和y坐标
            setTwoPointerLocation(event);
        }
        //获取歌词大小要缩放的比例
        int scaleSize = getScale(event);
        Log.d(TAG, "scaleSize:" + scaleSize);
        //如果缩放大小不等于0，进行缩放，重绘LrcView
        if (scaleSize != 0) {
            setNewFontSize(scaleSize);
            invalidate();
        }
        setTwoPointerLocation(event);
    }
```
如上代码所示，当两个手指第一次放在屏幕上时候，调用setTwoPointerLocation(MotionEvent event)方法来记录两个手指的x坐标和y坐标，setTwoPointerLocation(MotionEvent event)方法代码如下所示：

```
 /**
     * 设置当前两个手指的x坐标和y坐标
     */
    private void setTwoPointerLocation(MotionEvent event) {
        mPointerOneLastMotion.x = event.getX(0);
        mPointerOneLastMotion.y = event.getY(0);
        mPointerTwoLastMotion.x = event.getX(1);
        mPointerTwoLastMotion.y = event.getY(1);
    }
```
当两个手指在屏幕上移动的时候，调用getScale(MotionEvent event)方法来对比两个手指前后两次的x坐标和y坐标，从而得到要缩放的比例scaleSize。getScale(MotionEvent event)方法具体实现如下所示：

```
/**
     * 获取歌词大小要缩放的比例
     */
    private int getScale(MotionEvent event) {
        Log.d(TAG, "scaleSize getScale");
        float x0 = event.getX(0);
        float y0 = event.getY(0);
        float x1 = event.getX(1);
        float y1 = event.getY(1);

        float maxOffset = 0; // max offset between x or y axis,used to decide scale size

        boolean zoomin = false;
        //第一次双指之间的x坐标的差距
        float oldXOffset = Math.abs(mPointerOneLastMotion.x - mPointerTwoLastMotion.x);
        //第二次双指之间的x坐标的差距
        float newXoffset = Math.abs(x1 - x0);

        //第一次双指之间的y坐标的差距
        float oldYOffset = Math.abs(mPointerOneLastMotion.y - mPointerTwoLastMotion.y);
        //第二次双指之间的y坐标的差距
        float newYoffset = Math.abs(y1 - y0);

        //双指移动之后，判断双指之间移动的最大差距
        maxOffset = Math.max(Math.abs(newXoffset - oldXOffset), Math.abs(newYoffset - oldYOffset));
        //如果x坐标移动的多一些
        if (maxOffset == Math.abs(newXoffset - oldXOffset)) {
            //如果第二次双指之间的x坐标的差距大于第一次双指之间的x坐标的差距则是放大，反之则缩小
            zoomin = newXoffset > oldXOffset ? true : false;
        }
        //如果y坐标移动的多一些
        else {
            //如果第二次双指之间的y坐标的差距大于第一次双指之间的y坐标的差距则是放大，反之则缩小
            zoomin = newYoffset > oldYOffset ? true : false;
        }
        Log.d(TAG, "scaleSize maxOffset:" + maxOffset);
        if (zoomin) {
            return (int) (maxOffset / 10);//放大双指之间移动的最大差距的1/10
        } else {
            return -(int) (maxOffset / 10);//缩小双指之间移动的最大差距的1/10
        }
    }
```
当通过getScale(MotionEvent event)方法获得了缩放比scaleSize后，调用setNewFontSize(int scaleSize)来设置歌词的新的字体大小，然后重绘LrcView，从而实现了缩放歌词的功能。
setNewFontSize(int scaleSize)方法的具体实现如下所示：

```
   /**
     * 设置缩放后的字体大小
     */
    private void setNewFontSize(int scaleSize) {
        //设置歌词缩放后的的最新字体大小
        mLrcFontSize += scaleSize;
        mLrcFontSize = Math.max(mLrcFontSize, mMinLrcFontSize);
        mLrcFontSize = Math.min(mLrcFontSize, mMaxLrcFontSize);

        //设置显示高亮的那句歌词的时间最新字体大小
        mSeekLineTextSize += scaleSize;
        mSeekLineTextSize = Math.max(mSeekLineTextSize, mMinSeekLineTextSize);
        mSeekLineTextSize = Math.min(mSeekLineTextSize, mMaxSeekLineTextSize);
    }
```
至此，缩放功能已经实现。

------
###LrcView的全部代码如下所示：
```
package com.oyp.lrc.view.impl;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.Paint.Align;
import android.graphics.PointF;
import android.util.AttributeSet;
import android.util.Log;
import android.view.MotionEvent;
import android.view.View;

import com.oyp.lrc.view.ILrcView;
import com.oyp.lrc.view.ILrcViewListener;

import java.util.List;

/**
 * 自定义LrcView,可以同步显示歌词，拖动歌词，缩放歌词
 */
public class LrcView extends View implements ILrcView {

    public final static String TAG = "LrcView";

    /**
     * 正常歌词模式
     */
    public final static int DISPLAY_MODE_NORMAL = 0;
    /**
     * 拖动歌词模式
     */
    public final static int DISPLAY_MODE_SEEK = 1;
    /**
     * 缩放歌词模式
     */
    public final static int DISPLAY_MODE_SCALE = 2;
    /**
     * 歌词的当前展示模式
     */
    private int mDisplayMode = DISPLAY_MODE_NORMAL;

    /**
     * 歌词集合，包含所有行的歌词
     */
    private List<LrcRow> mLrcRows;
    /**
     * 最小移动的距离，当拖动歌词时如果小于该距离不做处理
     */
    private int mMinSeekFiredOffset = 10;

    /**
     * 当前高亮歌词的行数
     */
    private int mHignlightRow = 0;
    /**
     * 当前高亮歌词的字体颜色为黄色
     */
    private int mHignlightRowColor = Color.YELLOW;
    /**
     * 不高亮歌词的字体颜色为白色
     */
    private int mNormalRowColor = Color.WHITE;

    /**
     * 拖动歌词时，在当前高亮歌词下面的一条直线的字体颜色
     **/
    private int mSeekLineColor = Color.CYAN;
    /**
     * 拖动歌词时，展示当前高亮歌词的时间的字体颜色
     **/
    private int mSeekLineTextColor = Color.CYAN;
    /**
     * 拖动歌词时，展示当前高亮歌词的时间的字体大小默认值
     **/
    private int mSeekLineTextSize = 15;
    /**
     * 拖动歌词时，展示当前高亮歌词的时间的字体大小最小值
     **/
    private int mMinSeekLineTextSize = 13;
    /**
     * 拖动歌词时，展示当前高亮歌词的时间的字体大小最大值
     **/
    private int mMaxSeekLineTextSize = 18;

    /**
     * 歌词字体大小默认值
     **/
    private int mLrcFontSize = 23;    // font size of lrc
    /**
     * 歌词字体大小最小值
     **/
    private int mMinLrcFontSize = 15;
    /**
     * 歌词字体大小最大值
     **/
    private int mMaxLrcFontSize = 35;

    /**
     * 两行歌词之间的间距
     **/
    private int mPaddingY = 10;
    /**
     * 拖动歌词时，在当前高亮歌词下面的一条直线的起始位置
     **/
    private int mSeekLinePaddingX = 0;

    /**
     * 拖动歌词的监听类，回调LrcViewListener类的onLrcSeeked方法
     **/
    private ILrcViewListener mLrcViewListener;

    /**
     * 当没有歌词的时候展示的内容
     **/
    private String mLoadingLrcTip = "Downloading lrc...";

    private Paint mPaint;

    public LrcView(Context context, AttributeSet attr) {
        super(context, attr);
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setTextSize(mLrcFontSize);
    }

    public void setListener(ILrcViewListener l) {
        mLrcViewListener = l;
    }

    public void setLoadingTipText(String text) {
        mLoadingLrcTip = text;
    }

    @Override
    protected void onDraw(Canvas canvas) {
        final int height = getHeight(); // height of this view
        final int width = getWidth(); // width of this view
        //当没有歌词的时候
        if (mLrcRows == null || mLrcRows.size() == 0) {
            if (mLoadingLrcTip != null) {
                // draw tip when no lrc.
                mPaint.setColor(mHignlightRowColor);
                mPaint.setTextSize(mLrcFontSize);
                mPaint.setTextAlign(Align.CENTER);
                canvas.drawText(mLoadingLrcTip, width / 2, height / 2 - mLrcFontSize, mPaint);
            }
            return;
        }

        int rowY = 0; // vertical point of each row.
        final int rowX = width / 2;
        int rowNum = 0;
        /**
         * 分以下三步来绘制歌词：
         *
         * 	第1步：高亮地画出正在播放的那句歌词
         *	第2步：画出正在播放的那句歌词的上面可以展示出来的歌词
         *	第3步：画出正在播放的那句歌词的下面的可以展示出来的歌词
         */
        // 1、 高亮地画出正在要高亮的的那句歌词
        String highlightText = mLrcRows.get(mHignlightRow).content;
        int highlightRowY = height / 2 - mLrcFontSize;
        mPaint.setColor(mHignlightRowColor);
        mPaint.setTextSize(mLrcFontSize);
        mPaint.setTextAlign(Align.CENTER);
        canvas.drawText(highlightText, rowX, highlightRowY, mPaint);

        // 上下拖动歌词的时候 画出拖动要高亮的那句歌词的时间 和 高亮的那句歌词下面的一条直线
        if (mDisplayMode == DISPLAY_MODE_SEEK) {
            // 画出高亮的那句歌词下面的一条直线
            mPaint.setColor(mSeekLineColor);
            //该直线的x坐标从0到屏幕宽度  y坐标为高亮歌词和下一行歌词中间
            canvas.drawLine(mSeekLinePaddingX, highlightRowY + mPaddingY, width - mSeekLinePaddingX, highlightRowY + mPaddingY, mPaint);

            // 画出高亮的那句歌词的时间
            mPaint.setColor(mSeekLineTextColor);
            mPaint.setTextSize(mSeekLineTextSize);
            mPaint.setTextAlign(Align.LEFT);
            canvas.drawText(mLrcRows.get(mHignlightRow).strTime, 0, highlightRowY, mPaint);
        }

        // 2、画出正在播放的那句歌词的上面可以展示出来的歌词
        mPaint.setColor(mNormalRowColor);
        mPaint.setTextSize(mLrcFontSize);
        mPaint.setTextAlign(Align.CENTER);
        rowNum = mHignlightRow - 1;
        rowY = highlightRowY - mPaddingY - mLrcFontSize;
        //只画出正在播放的那句歌词的上一句歌词
//        if (rowY > -mLrcFontSize && rowNum >= 0) {
//            String text = mLrcRows.get(rowNum).content;
//            canvas.drawText(text, rowX, rowY, mPaint);
//        }

        //画出正在播放的那句歌词的上面所有的歌词
		while( rowY > -mLrcFontSize && rowNum >= 0){
			String text = mLrcRows.get(rowNum).content;
			canvas.drawText(text, rowX, rowY, mPaint);
			rowY -=  (mPaddingY + mLrcFontSize);
			rowNum --;
		}

        // 3、画出正在播放的那句歌词的下面的可以展示出来的歌词
        rowNum = mHignlightRow + 1;
        rowY = highlightRowY + mPaddingY + mLrcFontSize;

        //只画出正在播放的那句歌词的下一句歌词
//        if (rowY < height && rowNum < mLrcRows.size()) {
//            String text2 = mLrcRows.get(rowNum).content;
//            canvas.drawText(text2, rowX, rowY, mPaint);
//        }

		//画出正在播放的那句歌词的所有下面的可以展示出来的歌词
		while( rowY < height && rowNum < mLrcRows.size()){
			String text = mLrcRows.get(rowNum).content;
			canvas.drawText(text, rowX, rowY, mPaint);
			rowY += (mPaddingY + mLrcFontSize);
			rowNum ++;
		}

    }

    /**
     * 设置要高亮的歌词为第几行歌词
     *
     * @param position 要高亮的歌词行数
     * @param cb       是否是手指拖动后要高亮的歌词
     */
    public void seekLrc(int position, boolean cb) {
        if (mLrcRows == null || position < 0 || position > mLrcRows.size()) {
            return;
        }
        LrcRow lrcRow = mLrcRows.get(position);
        mHignlightRow = position;
        invalidate();
        //如果是手指拖动歌词后
        if (mLrcViewListener != null && cb) {
            //回调onLrcSeeked方法，将音乐播放器播放的位置移动到高亮歌词的位置
            mLrcViewListener.onLrcSeeked(position, lrcRow);
        }
    }

    private float mLastMotionY;
    /**
     * 第一个手指的坐标
     **/
    private PointF mPointerOneLastMotion = new PointF();
    /**
     * 第二个手指的坐标
     **/
    private PointF mPointerTwoLastMotion = new PointF();
    /**
     * 是否是第一次移动，当一个手指按下后开始移动的时候，设置为true,
     * 当第二个手指按下的时候，即两个手指同时移动的时候，设置为false
     */
    private boolean mIsFirstMove = false;

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (mLrcRows == null || mLrcRows.size() == 0) {
            return super.onTouchEvent(event);
        }
        switch (event.getAction()) {
            //手指按下
            case MotionEvent.ACTION_DOWN:
                Log.d(TAG, "down,mLastMotionY:" + mLastMotionY);
                mLastMotionY = event.getY();
                mIsFirstMove = true;
                invalidate();
                break;
            //手指移动
            case MotionEvent.ACTION_MOVE:
                if (event.getPointerCount() == 2) {
                    Log.d(TAG, "two move");
                    doScale(event);
                    return true;
                }
                Log.d(TAG, "one move");
                // single pointer mode ,seek
                //如果是双指同时按下，进行歌词大小缩放，抬起其中一个手指，另外一个手指不离开屏幕地移动的话，不做任何处理
                if (mDisplayMode == DISPLAY_MODE_SCALE) {
                    //if scaling but pointer become not two ,do nothing.
                    return true;
                }
                //如果一个手指按下，在屏幕上移动的话，拖动歌词上下
                doSeek(event);
                break;
            case MotionEvent.ACTION_CANCEL:
                //手指抬起
            case MotionEvent.ACTION_UP:
                if (mDisplayMode == DISPLAY_MODE_SEEK) {
                    //高亮手指抬起时的歌词并播放从该句歌词开始播放
                    seekLrc(mHignlightRow, true);
                }
                mDisplayMode = DISPLAY_MODE_NORMAL;
                invalidate();
                break;
        }
        return true;
    }

    /**
     * 处理双指在屏幕移动时的，歌词大小缩放
     */
    private void doScale(MotionEvent event) {
        //如果歌词的模式为：拖动歌词模式
        if (mDisplayMode == DISPLAY_MODE_SEEK) {
            //如果是单指按下，在进行歌词上下滚动，然后按下另外一个手指，则把歌词模式从 拖动歌词模式 变为 缩放歌词模式
            mDisplayMode = DISPLAY_MODE_SCALE;
            Log.d(TAG, "change mode from DISPLAY_MODE_SEEK to DISPLAY_MODE_SCALE");
            return;
        }
        // two pointer mode , scale font
        if (mIsFirstMove) {
            mDisplayMode = DISPLAY_MODE_SCALE;
            invalidate();
            mIsFirstMove = false;
            //两个手指的x坐标和y坐标
            setTwoPointerLocation(event);
        }
        //获取歌词大小要缩放的比例
        int scaleSize = getScale(event);
        Log.d(TAG, "scaleSize:" + scaleSize);
        //如果缩放大小不等于0，进行缩放，重绘LrcView
        if (scaleSize != 0) {
            setNewFontSize(scaleSize);
            invalidate();
        }
        setTwoPointerLocation(event);
    }

    /**
     * 处理单指在屏幕移动时，歌词上下滚动
     */
    private void doSeek(MotionEvent event) {
        float y = event.getY();//手指当前位置的y坐标
        float offsetY = y - mLastMotionY; //第一次按下的y坐标和目前移动手指位置的y坐标之差
        //如果移动距离小于10，不做任何处理
        if (Math.abs(offsetY) < mMinSeekFiredOffset) {
            return;
        }
        //将模式设置为拖动歌词模式
        mDisplayMode = DISPLAY_MODE_SEEK;
        int rowOffset = Math.abs((int) offsetY / mLrcFontSize); //歌词要滚动的行数

        Log.d(TAG, "move to new hightlightrow : " + mHignlightRow + " offsetY: " + offsetY + " rowOffset:" + rowOffset);

        if (offsetY < 0) {
            //手指向上移动，歌词向下滚动
            mHignlightRow += rowOffset;//设置要高亮的歌词为 当前高亮歌词 向下滚动rowOffset行后的歌词
        } else if (offsetY > 0) {
            //手指向下移动，歌词向上滚动
            mHignlightRow -= rowOffset;//设置要高亮的歌词为 当前高亮歌词 向上滚动rowOffset行后的歌词
        }
        //设置要高亮的歌词为0和mHignlightRow中的较大值，即如果mHignlightRow < 0，mHignlightRow=0
        mHignlightRow = Math.max(0, mHignlightRow);
        //设置要高亮的歌词为0和mHignlightRow中的较小值，即如果mHignlight > RowmLrcRows.size()-1，mHignlightRow=mLrcRows.size()-1
        mHignlightRow = Math.min(mHignlightRow, mLrcRows.size() - 1);
        //如果歌词要滚动的行数大于0，则重画LrcView
        if (rowOffset > 0) {
            mLastMotionY = y;
            invalidate();
        }
    }

    /**
     * 设置当前两个手指的x坐标和y坐标
     */
    private void setTwoPointerLocation(MotionEvent event) {
        mPointerOneLastMotion.x = event.getX(0);
        mPointerOneLastMotion.y = event.getY(0);
        mPointerTwoLastMotion.x = event.getX(1);
        mPointerTwoLastMotion.y = event.getY(1);
    }

    /**
     * 设置缩放后的字体大小
     */
    private void setNewFontSize(int scaleSize) {
        //设置歌词缩放后的的最新字体大小
        mLrcFontSize += scaleSize;
        mLrcFontSize = Math.max(mLrcFontSize, mMinLrcFontSize);
        mLrcFontSize = Math.min(mLrcFontSize, mMaxLrcFontSize);

        //设置歌词的最新字体大小
        mSeekLineTextSize += scaleSize;
        mSeekLineTextSize = Math.max(mSeekLineTextSize, mMinSeekLineTextSize);
        mSeekLineTextSize = Math.min(mSeekLineTextSize, mMaxSeekLineTextSize);
    }

    /**
     * 获取歌词大小要缩放的比例
     */
    private int getScale(MotionEvent event) {
        Log.d(TAG, "scaleSize getScale");
        float x0 = event.getX(0);
        float y0 = event.getY(0);
        float x1 = event.getX(1);
        float y1 = event.getY(1);

        float maxOffset = 0; // max offset between x or y axis,used to decide scale size

        boolean zoomin = false;
        //第一次双指之间的x坐标的差距
        float oldXOffset = Math.abs(mPointerOneLastMotion.x - mPointerTwoLastMotion.x);
        //第二次双指之间的x坐标的差距
        float newXoffset = Math.abs(x1 - x0);

        //第一次双指之间的y坐标的差距
        float oldYOffset = Math.abs(mPointerOneLastMotion.y - mPointerTwoLastMotion.y);
        //第二次双指之间的y坐标的差距
        float newYoffset = Math.abs(y1 - y0);

        //双指移动之后，判断双指之间移动的最大差距
        maxOffset = Math.max(Math.abs(newXoffset - oldXOffset), Math.abs(newYoffset - oldYOffset));
        //如果x坐标移动的多一些
        if (maxOffset == Math.abs(newXoffset - oldXOffset)) {
            //如果第二次双指之间的x坐标的差距大于第一次双指之间的x坐标的差距则是放大，反之则缩小
            zoomin = newXoffset > oldXOffset ? true : false;
        }
        //如果y坐标移动的多一些
        else {
            //如果第二次双指之间的y坐标的差距大于第一次双指之间的y坐标的差距则是放大，反之则缩小
            zoomin = newYoffset > oldYOffset ? true : false;
        }
        Log.d(TAG, "scaleSize maxOffset:" + maxOffset);
        if (zoomin) {
            return (int) (maxOffset / 10);//放大双指之间移动的最大差距的1/10
        } else {
            return -(int) (maxOffset / 10);//缩小双指之间移动的最大差距的1/10
        }
    }

    /**
     * 设置歌词行集合
     * @param lrcRows
     */
    public void setLrc(List<LrcRow> lrcRows) {
        mLrcRows = lrcRows;
        invalidate();
    }

    /**
     * 播放的时候调用该方法滚动歌词，高亮正在播放的那句歌词
     * @param time
     */
    public void seekLrcToTime(long time) {
        if (mLrcRows == null || mLrcRows.size() == 0) {
            return;
        }
        if (mDisplayMode != DISPLAY_MODE_NORMAL) {
            return;
        }
        Log.d(TAG, "seekLrcToTime:" + time);

        for (int i = 0; i < mLrcRows.size(); i++) {
            LrcRow current = mLrcRows.get(i);
            LrcRow next = i + 1 == mLrcRows.size() ? null : mLrcRows.get(i + 1);
            /**
             *  正在播放的时间大于current行的歌词的时间而小于next行歌词的时间， 设置要高亮的行为current行
             *  正在播放的时间大于current行的歌词，而current行为最后一句歌词时，设置要高亮的行为current行
             */
            if ((time >= current.time && next != null && time < next.time)
                    || (time > current.time && next == null)){
                seekLrc(i, false);
                return;
            }
        }
    }
}

```

以上就是自定义LrcView的全部内容，下面将该自定义LrcView放在布局文件activity_main.xml中去显示出来。

activity_main.xml的代码如下所示：
```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <com.oyp.lrc.view.impl.LrcView
        android:id="@+id/lrcView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        />
</RelativeLayout>
```
###MainActivity代码如下所示：
然后通过MainActivity来加载该布局，并在MainActivity中播放音乐，MainActivity的代码如下所示：

```
package com.oyp.lrc;

import android.app.Activity;
import android.media.MediaPlayer;
import android.media.MediaPlayer.OnCompletionListener;
import android.media.MediaPlayer.OnPreparedListener;
import android.os.Bundle;
import android.util.Log;

import com.oyp.lrc.view.ILrcBuilder;
import com.oyp.lrc.view.ILrcView;
import com.oyp.lrc.view.ILrcViewListener;
import com.oyp.lrc.view.impl.DefaultLrcBuilder;
import com.oyp.lrc.view.impl.LrcRow;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.List;
import java.util.Timer;
import java.util.TimerTask;

public class MainActivity extends Activity {

	public final static String TAG = "MainActivity";

    //自定义LrcView，用来展示歌词
	ILrcView mLrcView;
    //更新歌词的频率，每秒更新一次
    private int mPalyTimerDuration = 1000;
    //更新歌词的定时器
    private Timer mTimer;
    //更新歌词的定时任务
    private TimerTask mTask;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //获取自定义的LrcView
        setContentView(R.layout.activity_main);
        mLrcView=(ILrcView)findViewById(R.id.lrcView);

        //从assets目录下读取歌词文件内容
        String lrc = getFromAssets("test.lrc");
        //解析歌词构造器
        ILrcBuilder builder = new DefaultLrcBuilder();
        //解析歌词返回LrcRow集合
        List<LrcRow> rows = builder.getLrcRows(lrc);
        //将得到的歌词集合传给mLrcView用来展示
        mLrcView.setLrc(rows);

        //开始播放歌曲并同步展示歌词
        beginLrcPlay();

        //设置自定义的LrcView上下拖动歌词时监听
        mLrcView.setListener(new ILrcViewListener() {
            //当歌词被用户上下拖动的时候回调该方法,从高亮的那一句歌词开始播放
            public void onLrcSeeked(int newPosition, LrcRow row) {
                if (mPlayer != null) {
                    Log.d(TAG, "onLrcSeeked:" + row.time);
                    mPlayer.seekTo((int) row.time);
                }
            }
        });
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
    	if (mPlayer != null) {
    		mPlayer.stop();
    	}
    }

    /**
     * 从assets目录下读取歌词文件内容
     * @param fileName
     * @return
     */
    public String getFromAssets(String fileName){
        try {
            InputStreamReader inputReader = new InputStreamReader( getResources().getAssets().open(fileName) );
            BufferedReader bufReader = new BufferedReader(inputReader);
            String line="";
            String result="";
            while((line = bufReader.readLine()) != null){
                if(line.trim().equals(""))
                    continue;
                result += line + "\r\n";
            }
            return result;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return "";
    }

    MediaPlayer mPlayer;

    /**
     * 开始播放歌曲并同步展示歌词
     */
    public void beginLrcPlay(){
    	mPlayer = new MediaPlayer();
    	try {
    		mPlayer.setDataSource(getAssets().openFd("test.mp3").getFileDescriptor());
    		//准备播放歌曲监听
            mPlayer.setOnPreparedListener(new OnPreparedListener() {
                //准备完毕
				public void onPrepared(MediaPlayer mp) {
					mp.start();
			        if(mTimer == null){
			        	mTimer = new Timer();
			        	mTask = new LrcTask();
			        	mTimer.scheduleAtFixedRate(mTask, 0, mPalyTimerDuration);
			        }
				}
			});
            //歌曲播放完毕监听
    		mPlayer.setOnCompletionListener(new OnCompletionListener() {
				public void onCompletion(MediaPlayer mp) {
					stopLrcPlay();
				}
			});
            //准备播放歌曲
    		mPlayer.prepare();
            //开始播放歌曲
    		mPlayer.start();
		} catch (IllegalArgumentException e) {
			e.printStackTrace();
		} catch (IllegalStateException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}

    }

    /**
     * 停止展示歌曲
     */
    public void stopLrcPlay(){
        if(mTimer != null){
            mTimer.cancel();
            mTimer = null;
        }
    }

    /**
     * 展示歌曲的定时任务
     */
    class LrcTask extends TimerTask{
        @Override
        public void run() {
            //获取歌曲播放的位置
            final long timePassed = mPlayer.getCurrentPosition();
            MainActivity.this.runOnUiThread(new Runnable() {
                public void run() {
                    //滚动歌词
                    mLrcView.seekLrcToTime(timePassed);
                }
            });

        }
    };
}

```

------
下面是项目的结构图。
![这里写图片描述](http://img.blog.csdn.net/20160306145252533)

#四、项目源码地址
+ CSDN下载
项目的源代码可以从下面的地址去免费下载：
[http://download.csdn.net/detail/qq446282412/9453906](http://download.csdn.net/detail/qq446282412/9453906)
+ Github下载
[https://github.com/ouyangpeng/android-lrc-view-oyp](https://github.com/ouyangpeng/android-lrc-view-oyp)

>版权声明：本文为【欧阳鹏】原创文章，欢迎转载，转载请注明出处! 【http://blog.csdn.net/ouyang_peng/article/details/50813419】


>作者：欧阳鹏 欢迎转载，与人分享是进步的源泉！ 
>转载请保留原文地址：
>http://blog.csdn.net/ouyang_peng/article/details/50813419
     
![这里写图片描述](http://img.blog.csdn.net/20150708201910089)
