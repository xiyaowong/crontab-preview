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

  <div style="text-align: center;margin-top: 20px;">
    <a href="https://github.com/xiyaowong/crontab-preview" style="color: grey;" title="source code">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        xmlns:xlink="http://www.w3.org/1999/xlink"
        viewBox="0 0 496 512"
        style="width: 25px;"
      >
        <path
          d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6c-3.3.3-5.6-1.3-5.6-3.6c0-2 2.3-3.6 5.2-3.6c3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9c2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9c.3 2 2.9 3.3 5.9 2.6c2.9-.7 4.9-2.6 4.6-4.6c-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2c12.8 2.3 17.3-5.6 17.3-12.1c0-6.2-.3-40.4-.3-61.4c0 0-70 15-84.7-29.8c0 0-11.4-29.1-27.8-36.6c0 0-22.9-15.7 1.6-15.4c0 0 24.9 2 38.6 25.8c21.9 38.6 58.6 27.5 72.9 20.9c2.3-16 8.8-27.1 16-33.7c-55.9-6.2-112.3-14.3-112.3-110.5c0-27.5 7.6-41.3 23.6-58.9c-2.6-6.5-11.1-33.3 2.6-67.9c20.9-6.5 69 27 69 27c20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27c13.7 34.7 5.2 61.4 2.6 67.9c16 17.7 25.8 31.5 25.8 58.9c0 96.5-58.9 104.2-114.8 110.5c9.2 7.9 17 22.9 17 46.4c0 33.7-.3 75.4-.3 83.6c0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252C496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2c1.6 1.6 3.9 2.3 5.2 1c1.3-1 1-3.3-.7-5.2c-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9c1.6 1 3.6.7 4.3-.7c.7-1.3-.3-2.9-2.3-3.9c-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2c2.3 2.3 5.2 2.6 6.5 1c1.3-1.3.7-4.3-1.3-6.2c-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9c1.6 2.3 4.3 3.3 5.6 2.3c1.6-1.3 1.6-3.9 0-6.2c-1.4-2.3-4-3.3-5.6-2z"
          fill="currentColor"
        />
      </svg>
    </a>
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
  margin-top: 30px;
}
</style>
