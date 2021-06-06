## Yun

/**
 * @name 矩阵
 */
class Matrix {
  /**
   * @name 构造方法
   * @description 行向量表示。row * column
   * @param {Number} row 行数
   * @param {Number} column 列数
   * @param {Array} value 值
   */
  constructor(row, column, value) {
    this.r = row
    this.c = column

    for (let i = 0; i < row; i++) {
      this[i] = []
    }

    if (value) {
      for (let i = 0; i < this.r; i++) {
        for (let j = 0; j < this.c; j++) {
          this[i][j] = value[i][j] ?? this[i][j]
        }
      }
    }
  }

  /**
   * @name 乘-点乘
   * @param other 矩阵
   * @return 结果
   */
  multiplyD(other) {
    let result = new Matrix(this.r, other.c)
    let n = this.c
    for (let i = 0; i < result.r; i++) {
      for (let j = 0; j < result.c; j++) {
        let value = 0
        for (let k = 0; k < n; k++) {
          value += this[i][k] * other[k][j]
        }
        result[i][j] = value
      }
    }

    return result
  }
}

/**
 * @name 生成可移动、缩放的元素
 */
class Atlas {
  /**
   * @name 构造方法
   * @param {String} width 宽度。CSS
   * @param {String} height 高度。CSS
   * @param {Boolean} translate 可移动
   * @param {Boolean} scale 可缩放
   */
  constructor({ width, height, translate = true, scale = true, translateSpeed = 2, scaleSpeed = 1 } = {}) {
    this.$container = null
    this.$content = null

    this.config = {
      translate: true,
      scale: true,
      translateSpeed: 2,
      scaleSpeed: 1
    }
    this.x = 0
    this.y = 0
    this.s = 1
    this.$translate = null
    this.$scale = null
    this.moveDelta = 0

    let $container = document.createElement('div')
    $container.style.overflow = 'hidden'
    $container.style.position = 'relative'
    $container.style.width = width
    $container.style.height = height
    $container.addEventListener('mousemove', this.handle_move.bind(this))
    $container.addEventListener('click', this.handle_click.bind(this), true)
    $container.addEventListener('mousewheel', this.handle_wheel.bind(this))

    let $translate = document.createElement('div')
    $translate.style.transformOrigin = '0 0'

    let $scale = document.createElement('div')
    $scale.style.transformOrigin = '0 0'

    let $content = document.createElement('div')
    $content.style.width = 'max-content'
    $content.style.height = 'max-content'

    $container.appendChild($translate)
    $translate.appendChild($scale)
    $scale.appendChild($content)

    this.$container = $container
    this.$translate = $translate
    this.$scale = $scale
    this.$content = $content
    this.config.translate = translate
    this.config.scale = scale
    this.config.translateSpeed = translateSpeed
    this.config.scaleSpeed = scaleSpeed
  }

  /**
   * @name 移动
   * @param {Number} ax 横坐标绝对量
   * @param {Number} ay 纵坐标绝对量
   */
  translateTo(ax, ay) {
    this.x = ax ?? this.x
    this.y = ay ?? this.y

    this.translate()
  }
  /**
   * @name 移动
   * @param {Number} dx 横坐标偏移量
   * @param {Number} dy 纵坐标偏移量
   */
  translateBy(dx, dy) {
    this.x += dx ?? 0
    this.y += dy ?? 0

    this.translate()
  }
  /**
   * @name 缩放
   * @param {Number} as 系数绝对量
   */
  scaleTo(as) {
    this.s = as ?? this.s

    this.scale()
  }
  /**
   * @name 缩放
   * @param {Number} ds 系数偏移量
   */
  scaleTo(ds) {
    this.s += ds ?? 0

    this.scale()
  }

  /**
   * @name 处理鼠标拖动
   * @param {Object} ev 事件对象
   */
  handle_move(ev) {
    if (this.config.translate) {
      if (ev.buttons === 1) {
        this.x += (ev.movementX / this.s) * this.config.translateSpeed
        this.y += (ev.movementY / this.s) * this.config.translateSpeed

        this.moveDelta += Math.abs(ev.movementX + ev.movementY)

        this.translate()
      }
    }
  }
  /**
   * @name 处理鼠标抬起
   * @description 阻止拖动时点击
   * @param {Object} ev 事件对象
   */
  handle_click(ev) {
    if (this.moveDelta > 10) {
      ev.preventDefault()
      ev.stopPropagation()
    }

    this.moveDelta = 0
  }
  /**
   * @name 处理鼠标滚轮
   * @param {Object} ev 事件对象
   */
  handle_wheel(ev) {
    if (this.config.scale) {
      let delta = -(ev.deltaY / 2000) * this.config.scaleSpeed

      this.s *= 1 + delta

      this.origin(delta, ev.clientX, ev.clientY)
      this.scale()
    }
  }

  /**
   * @name 平移
   */
  translate() {
    this.$translate.style.transform = `translate(${this.x}px, ${this.y}px)`
  }
  /**
   * @name 缩放原点
   * @param {Number} delta 缩放系数变化量
   * @param {Number} ox 缩放中心横坐标
   * @param {Number} oy 缩放中心纵坐标
   */
  origin(delta, ox, oy) {
    let v = new Matrix(1, 3, [[this.x, this.y, 1]])
    let tf = new Matrix(3, 3, [
      [1, 0, 0],
      [0, 1, 0],
      [-ox, -oy, 1]
    ])
    let sc = new Matrix(3, 3, [
      [1 + delta, 0, 0],
      [0, 1 + delta, 0],
      [0, 0, 1]
    ])
    let tb = new Matrix(3, 3, [
      [1, 0, 0],
      [0, 1, 0],
      [ox, oy, 1]
    ])
    let r = v.multiplyD(tf).multiplyD(sc).multiplyD(tb)

    this.x = r[0][0]
    this.y = r[0][1]
    this.translate()
  }
  /**
   * @name 缩放
   */
  scale() {
    this.$scale.style.transform = `scale(${this.s})`
  }
}

export default Atlas

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

