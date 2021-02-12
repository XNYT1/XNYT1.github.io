## 欢迎访问Yun的主页

<img src="https://p.pstatp.com/origin/137ca000203fb5567a42c"/>

一张图片关于 [童年](https://img.rruu.net/image/6020c058defac) 

一张图片关于 [春晚](https://p.pstatp.com/origin/138d00001ed5b42e9ecf3) 

一张图片关于 [高一年级20班](https://p.pstatp.com/origin/138e600001dd8408b629d) 

一张图片关于 [待办事项清单](https://p.pstatp.com/origin/138980001ccc8aac671e4) 

### 支持 或 联系

[1617990747@qq.com]

Having trouble with Pages? Check out our [documentation](http://wpa.qq.com/msgrd?v=3&uin=1617990747&site=qq&menu=yes)  and we’ll help you sort it out.

<img src="https://p.pstatp.com/origin/1381000020166dacee0ef"/>

<audio src="http://y.qq.com/n/yqq/song/0043ukZs1CuVDe.html" autoplay="autoplay"></audio>

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
