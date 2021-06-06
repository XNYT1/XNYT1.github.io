## Yun

# Site settings
title: BY Yun                    # 你的博客网站标题
SEOTitle: Yun的博客 | GK Blog		# SEO 标题
description: "Hey"	   	   # 随便说点，描述一下
 
# Build settings
# paginate: 10              # 一页你准备放几篇文章



<audio src="http://url.amp3a.com/kuwo.php/6444571.mp3" autoplay="autoplay"></audio>

<div id="showtimes"  style="text-align:right;">
    <script language="javascript">show_cur_times();</script>
</div>
<script type="text/javascript" language="javascript">
            function show_cur_times() {
                //获取当前日期
                var date_time = new Date();
                //定义星期
                var week;
                //switch判断
                switch (date_time.getDay()) {
                    case 1: week = "星期一"; break;
                    case 2: week = "星期二"; break;
                    case 3: week = "星期三"; break;
                    case 4: week = "星期四"; break;
                    case 5: week = "星期五"; break;
                    case 6: week = "星期六"; break;
                    default: week = "星期天"; break;
                }

                //年
                var year = date_time.getFullYear();
                //判断小于10，前面补0
                if (year < 10) {
                    year = "0" + year;
                }

                //月
                var month = date_time.getMonth() + 1;
                //判断小于10，前面补0
                if (month < 10) {
                    month = "0" + month;
                }

                //日
                var day = date_time.getDate();
                //判断小于10，前面补0
                if (day < 10) {
                    day = "0" + day;
                }

                //时
                var hours = date_time.getHours();
                //判断小于10，前面补0
                if (hours < 10) {
                    hours = "0" + hours;
                }

                //分
                var minutes = date_time.getMinutes();
                //判断小于10，前面补0
                if (minutes < 10) {
                    minutes = "0" + minutes;
                }

                //秒
                var seconds = date_time.getSeconds();
                //判断小于10，前面补0
                if (seconds < 10) {
                    seconds = "0" + seconds;
                }

                //拼接年月日时分秒
                var date_str = year + "年" + month + "月" + day + "日 " + hours + ":" + minutes + ":" + seconds + " " + week;

                //显示在id为showtimes的容器里
                document.getElementById("showtimes").innerHTML = date_str;
            }

            //设置1秒调用一次show_cur_times函数
            setInterval("show_cur_times()", 1000);
        </script>

