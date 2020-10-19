<!DOCTYPE html>
<head>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
<style>
    .row {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
    }
</style>
<script>
    (() => {
        /* 并集 */
        const union = (a, b) => a.concat(b.filter(v => !a.includes(v))); /* [1,2,3,4,5] */
        /* 交集 */
        const intersection = (a, b) => a.filter(v => b.includes(v)); /* [2] */
        /* diff */
        const difference = (a, b) => a.concat(b).filter(v => !a.includes(v) || !b.includes(v)); /* [1,3,4,5] */
        /* 减集 */
        const subtract = (a, b) => a.filter(v => !b.includes(v)); /* [2] */
        /* 名单匹配 */
        const listMatch = (a, b) => a.filter(val => b.filter(vb => val.match('\\b' + vb + '\\b')).length);
        // 
        const update = () => {
            const 当前名单文本 = document.querySelector('#current').value
            const 目标名单 = document.querySelector('#target').value.split(/\r?\n|,|\|/).map(e => e.trim())
            let 当前名单 = 当前名单文本.split(/\r?\n|,|\|/).map(e => e.trim())
            当前名单 = 当前名单.concat(目标名单.map(e => (e => e && e[0])(当前名单文本.match(e))).filter(e => e))
            document.querySelector('#done').value = intersection(目标名单, 当前名单).join(",")
            document.querySelector('#a-b').value = subtract(目标名单, 当前名单).join(",")
            document.querySelector('#b-a').value = subtract(当前名单, 目标名单).join(",")
            // document.querySelector('#diff').value = difference(目标名单, 当前名单).join(",")
            document.querySelector('#match').value = listMatch(目标名单, 当前名单).join("\n")
        }
        window.update = update
    })()
</script>
</head>


# 雪星实验室 - 名单完成统计工具


<div class=row>
    <div class=col>
        <h3>全部名单/目标名单</h3>
        <textarea rows=20 cols=80 id="target" autofocus=true oninput="update()" placeholder="张三,李四,王五……"></textarea>
    </div>
    <div class=col>
        <h3>当前已完成名单</h3>
    <textarea rows=20 cols=80 id="current" oninput="update()" placeholder="李四, ..."></textarea>
    </div>
</div>
<div class=row>
    <div class=col>
        <h3>✔已完成列表</h3>
        <textarea rows=20 cols=80 id="done" readonly=true></textarea>
    </div>
    <div class=col>
        <h3>❎未完成列表</h3>
        <textarea rows=20 cols=80 id="a-b" readonly=true></textarea>
    </div>
    <div class=col>
        <h3>意外名单（不应出现的人）</h3>
        <textarea rows=20 cols=80 id="b-a" readonly=true></textarea>
    </div>
    <div class=col>
        <h3>查找名单（补全姓名学号）</h3>
        <textarea rows=20 cols=80 id="match" readonly=true></textarea>
    </div>
</div>