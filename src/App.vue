<template>
  <input type="text" v-model="exp" placeholder="输入表达式查看未来运行时间 0 6 * * *" />

  <pre v-if="result">{{result}}</pre>

  <div class>
    <h5>Linux crontab定时执行任务命令</h5>
    <p>cron是一个linux下的定时执行工具，可以在无需人工干预的情况下运行作业。</p>
    <pre><code>cron服务操作命令
service crond start    # 启动服务
service crond stop     # 关闭服务
service crond restart  # 重启服务
service crond reload   # 重新载入配置
service crond status   # 查看服务状态

crontab -l   # 查看crontab
crontab -e   # 编辑crontab
crontab -r    #删除crontab</code></pre>
    <pre>			<code>
crontab表达式说明：
*    *    *    *    *    [user]   [command]
-    -    -    -    -
|    |    |    |    |
|    |    |    |    |
|    |    |    |    +----- 星期 (0 - 7) (Sunday=0 or 7)
|    |    |    +---------- 月 (1 - 12)
|    |    +--------------- 日 (1 - 31)
|    +-------------------- 时 (0 - 23)
+------------------------- 分 (0 - 59)
</code></pre>
    <h5>crontab命令实例</h5>
    <pre><code>0 0 L * *        # 每月的最后1天执行
* * * * *        # 每1分钟执行一次
*/10 * * * *     # 每隔10分钟执行一次
0 */1 * * *      # 每隔1小时执行一次
30 21 * * *      # 每晚的21:30执行
3,15 * * * *     # 每小时的第3和第15分钟执行
3,15 8-11 * * *  # 在上午8点到11点的第3和第15分钟执行</code></pre>
  </div>
</template>

<script>

function dateFormat(fmt, date) {
  let ret;
  const opt = {
    "Y+": date.getFullYear().toString(),
    "m+": (date.getMonth() + 1).toString(),
    "d+": date.getDate().toString(),
    "H+": date.getHours().toString(),
    "M+": date.getMinutes().toString(),
    "S+": date.getSeconds().toString()
  };
  for (let k in opt) {
    ret = new RegExp("(" + k + ")").exec(fmt);
    if (ret) {
      fmt = fmt.replace(ret[1], (ret[1].length == 1) ? (opt[k]) : (opt[k].padStart(ret[1].length, "0")))
    }
  }
  return fmt;
}

import parser from 'cron-parser'
import {ref, watch} from 'vue'

export default {
  setup() {
    const exp = ref('')
    const result = ref('')

    const updateResult = (newValue) => {
      if (!newValue) {
        result.value = ""
        return
      }

      try {
        const interval = parser.parseExpression(newValue)
        const items = []
        for (let i = 0; i < 10; i++) {
          items.push(dateFormat("YYYY-mm-dd HH:MM:SS", interval.next()))
        }
        result.value = items.join('\n')
      } catch (err) {
        result.value = `Error: ${err.message}`
      }
    }

    watch(exp, updateResult)

    return {exp, result}
  }
}
</script>

<style>
#app {
  margin-top: 60px;
}
</style>
