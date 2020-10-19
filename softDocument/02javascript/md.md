<h1 style="text-align: center"> 策略使用及事项 </h1>
<script>
        function onlink(document,point) {
            documentDetailsModal.showDocumentDetails(document,this);
            documentDetailsModal.getDocPoint(document,point,this)
        }
</script>

---

### 量化系统策略编写使用须知

<pre>
1.API中必输参数不能带参数名称,且必输参数顺序不能修改。
2.API中非必输参数必须按照“key=value”的方式填写,例子:in_out_market='1'。
3.API中的查询非必输参数的时间类型可选格式(1.YYYYMMDD 2.YYYY/MM/DD 3.YYYY-MM-DD 4.YYYY-MM-DD HH:mm:ss 5.YYYYMMDDHHmmss)。(具体可参照api范例)
4.init_cache(缓存数据方法)需要和数据查询API组合使用,不然无法查询到数据。(具体参照api策略编写案例及API说明)


</pre>

<a onclick="onlink(&quot;api_example.md&quot;,&quot;point0&quot;);">(以上注意事项可参照具体api策略编写案例)</a>

---
<h1 style="text-align: center"> 数据预处理模块 </h1>

---
### 贵金属初始化缓存的数据
<h3>●函数定义</h3>
<pre>
pm.init_cache(source,type,symbols)
</pre>
<h3>●函数说明</h3>
<h3>只能在init函数中使用,需要和api配合使用.source和type匹配订阅行情列表清单.请参照订阅行情列表使用.</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>source</td><td>String</td><td>市场</td><td>是</td><td>关联订阅数据市场</td></tr>
<tr><td>type </td><td>String</td><td>数据类型</td><td>是</td><td>关联订阅数据数据</td></tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
pm.init_cache('DEFAULT', 'SgPubOdmBar', ['Ag(T+D)'])
</pre>
<h3>●输出结果(以队列组装的object类型展现)</h3>
<h3>无</h3>
</pre>
<h1 style="text-align: center"> 策略及案例说明 </h1>
<script>
    function onlink(document,point) {
        documentDetailsModal.showDocumentDetails(document,this);
        documentDetailsModal.getDocPoint(document,point,this)
    }
</script>

---
<pre>
'''
api模块导入,其中有quantapi提供的api服务重命名不能修改,只能按照固定名称使用.以下是所有模块。
'''
<p class="buttons" onclick="onlink(&quot;api_irs_data.md&quot;,&quot;point6&quot;)">import quantapi.ibor as ibor</p>
<p class="buttons" onclick="onlink(&quot;api_irs_data.md&quot;,&quot;point4&quot;)">import quantapi.repo as repo </p>
<p class="buttons" onclick="onlink(&quot;api_irs_data.md&quot;,&quot;point0&quot;)">import quantapi.bond as bond </p>
<p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point0&quot;)">import quantapi.pm as pm </p>
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point5&quot;)">import quantapi.qlog as qlog </p>
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point1&quot;)">import quantapi.date as date </p>
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point18&quot;)">import quantapi.p as p </p>
'''
init方法策略运行初始化触发
'''
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point10&quot;)">def init(context): </p>
    <p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point5&quot;)">qlog.info('init初始化开始')</p>
    #定义一个名为roll_pos_job的定时器
    <p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point17&quot;)">context.run_daily("roll_pos_job", "160000")</p>
    #记录日志方法
    <p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point5&quot;)">qlog.info('init初始化完成')</p>
'''
onData 由context.subscribe订阅触发 同一个时间点订阅数据触发一次
'''
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point11&quot;)">def onData(context, data):</p>
    #贵金属数据模块
    qlog.info("外资行整合最优价历史数据查询")
    comIpmBest = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point0&quot;)">pm.get_com_ipm_best(['XAGUSD'])</p>
    if comIpmBest:
        qlog.info(comIpmBest[0])
        qlog.info(comIpmBest[0].symbol)
    qlog.info("外资行整合最优价历史数据查询结束")
    qlog.info("期交所深度行情数据查询")
    comFutPubOdm = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point1&quot;)">pm.get_com_fut_pub_odm(['au1912'])</p>
    if comFutPubOdm:
        qlog.info(comFutPubOdm[0])
        qlog.info(comFutPubOdm[0].symbol)
    qlog.info("期交所深度行情查询结束")
    qlog.info("金交所深度行情查询")
    qlog.info(data)
    sgComPubOdm = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point2&quot;)">pm.get_sg_com_pub_odm(['Au99.99'])</p>
    if sgComPubOdm:
        qlog.info(sgComPubOdm[0])
        qlog.info(sgComPubOdm[0].symbol)
    qlog.info("金交所深度行情查询结束")
    qlog.info("外资行深度行情查询")
    comIpmPubOdm = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point3&quot;)">pm.get_com_ipm_pub_odm(['XAGUSD'],'DEFAULT','10000')</p>
    if comIpmPubOdm:
        qlog.info(comIpmPubOdm[0])
        qlog.info(comIpmPubOdm[0].symbol)
    qlog.info("外资行深度行情结束")
    qlog.info("外币CMDS深度行情")
    spPubOdm = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point4&quot;)">pm.get_sp_pub_odm(['USDCNY'])</p>
    if spPubOdm:
        qlog.info(spPubOdm[0])
        qlog.info(spPubOdm[0].symbol)
    qlog.info("外币CMDS深度行情结束")
    qlog.info("期货主力合约查询")
    contract = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point5&quot;)">pm.get_trade_contract('ag1906')</p>
    if contract:
        qlog.info(contract)
        qlog.info(contract.quoteCurrency)
    qlog.info("期货主力合约查询结束")
    qlog.info("外资行每日结算价查询")
    comIpmAver = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point6&quot;)">pm.get_com_ipm_aver(['XAGUSD'])</p>
    if comIpmAver:
        qlog.info(comIpmAver[0])
        qlog.info(comIpmAver[0].symbol)
    qlog.info("外资行每日结算价结束")
    qlog.info("价差实数据查询")
    spread = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point7&quot;)">pm.query_spread_bar_list('AG1912/AG2006', frequency='1M', calType='spreadPriceNarrow',start_time='20191231', end_time='20191231')</p>
    if spread:
        qlog.info(spread[0])
        qlog.info(spread[0].symbol)
    qlog.info("价差实数据查询结束")
    qlog.info("外资行整合最优价Bar数据查询")
    comPubOdmBar = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point8&quot;)">pm.get_com_pub_odm_bar(['XAGUSD'], '1N')</p>
    if comPubOdmBar:
            qlog.info(comPubOdmBar[0])
            qlog.info(comPubOdmBar[0].symbol)
    qlog.info("外资行整合最优价Bar数据结束")
    qlog.info("期交所市场行情Bar数据查询")
    shPubOdmBar = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point9&quot;)">pm.get_sh_pub_odm_bar(['ag1910'], '1N')</p>
    if shPubOdmBar:
        qlog.info(shPubOdmBar[0])
        qlog.info(shPubOdmBar[0].symbol)
    qlog.info("期交所市场行情Bar数据查询结束")
    qlog.info("金交所市场行情Bar数据查询")
    sgPubOdmBar = <p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point10&quot;)">pm.get_sg_pub_odm_bar(['Ag(T+D)'], '1N')</p>
    if sgPubOdmBar:
        qlog.info(sgPubOdmBar[0])
        qlog.info(sgPubOdmBar[0].symbol)
    qlog.info("金交所市场行情Bar数据查询结束")
    #贵金属交易模块
    qlog.info("限价单发起")
    order_id1 = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point0&quot;)">pm.limit_order('ag1906', 'S', 2.76, 10000, channel_code='CITI',in_out_market='1', time_in_force='1', expire_time='20190808', hedge_flag='1')</p>
    qlog.info(order_id1)
    qlog.info("限价单结束")
    qlog.info("贵金属撤销全部订单")
    <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point2&quot;)">pm.cancel_all()</p>
    qlog.info("贵金属撤销全部订单完成")
    qlog.info("贵金属条件撤销")
    <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point3&quot;)">pm.cancel_order(channel_code='CITI', symbol='ag1906', side='S', time_in_force='1')</p>
    qlog.info("贵金属条件撤销订单完成")
    order_id3 = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point1&quot;)">pm.rfq_order('ag1906','S',10000,channel_code='CITI',value_date='20190928',maturity_date='20190930')</p>
    <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point4&quot;)">pm.cancel_order_id(order_id3)</p>
    qlog.info("获取交易列表")
    redata = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point5&quot;)">pm.query_orders(symbol='ag1906', start_time='19920628', end_time='20250629')</p>
    if redata:
        qlog.info(redata[0])
    qlog.info("获取交易列表完成")
    qlog.info("获取成交列表")
    redata = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point6&quot;)">pm.query_trades(symbol='ag1906', start_time='20190628', end_time='20190629')</p>
    if redata:
        qlog.info(redata[0])
    qlog.info("获取成交列表完成")
    id1 = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point0&quot;)">pm.limit_order('ag1906', 'S', 2, 10000, channel_code='CITI')</p>
    id2 = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point0&quot;)">pm.limit_order('ag1906', 'S', 5, 10000, channel_code='CITI')</p>
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point7&quot;)">pm.get_order_onroad_amt('ag1906', 'S')</p>)
    id3 = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point0&quot;)">pm.limit_order('ag1906', 'S', 5, 10000, channel_code='CITI')</p>
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point8&quot;)">pm.get_order_by_id(id3)</p>)
    qlog.info("合约AUF的头寸数量")
    <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point9&quot;)">pm.get_position_quantity('AUF', None, None, True)</p>
    id = <p class="buttons" onclick="onlink(&quot;api_pm_trade.md&quot;,&quot;point10&quot;)">pm.fx_order_bl('ag1906', 'S', 10000, 2.76, '20181012', '20181015','1904535', channel_code='CITI',in_out_market='1', time_in_force='1', expire_time='20190808', hedge_flag='1')</p>
    qlog.info(id)
    #工具模块
    qlog.info("20190228日期往后按工作日往后延5个工作日")
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point1&quot;)">date.day_offset(['CNY'], 5, 20190228, 'D', 'trading')</p>)
    qlog.info("校验20190307是否为CNY假日配置内的工作日")
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point2&quot;)">date.day_check(['CNY'], 'TRADING', 20190307)</p>)
    qlog.info("20190302到20190307的工作日是哪几天")
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point3&quot;)">date.day_filter(['CNY'], "TRADING", 20190302, 20190307)</p>)
    qlog.info("20190302到20190307的工作日有几天")
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point4&quot;)">date.day_nums(['CNY'], "trading",20190302, 20190307)</p>)
    qlog.info("获取系统时间")
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point6&quot;)">date.get_sys_time('Y%m%d')</p>)
    qlog.info("千克换算成克")
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point7&quot;)">pm.unit_trans('Kg', 10000, 'g')</p>)
    qlog.info("获取业务时间")
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point9&quot;)">date.get_bus_day('bstk_bussiness_date')</p>)
    qlog.info(<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point18&quot;)">p.param("param2")</p>)
    <p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point19&quot;)">ndarray = p.matrix("Matrix")</p>
    if ndarray:
        qlog.info(ndarray[0][0])
    
'''
onOrder 由onData中订单交易状态变化后触发驱动
'''
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point12&quot;)">def onOrder(context, data):</p>
    qlog.info('触发onOrder')
'''
onTrade 由onData中订单成交后触发驱动
'''
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point13&quot;)">def onTrade(context, data):</p>
    qlog.info('触发onTrade')
'''
onMonitor函数
'''
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point15&quot;)">def onMonitor(context, data):</p>
    qlog.info('触发onMonitor')
'''
onTime函数 定时器触发器
'''
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point16&quot;)">def onTime(context,name,timer):</p>
    qlog.info('触发onTime')
'''
onBusinessDate函数 切日函数触发
'''
<p class="buttons" onclick="onlink(&quot;api_tool.md&quot;,&quot;point20&quot;)">def onBusinessDate(context,data):</p>
    qlog.info('触发onBusinessDate')
</pre> 
<h1 style="text-align: center"> 利率互换数据模块 </h1>

### 森浦QB现券成交行情 
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_QB_QBOND_PUB_TRADE</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('QB', 'QBondPubTra', symbols)
'''获取数据函数调用方法'''
bond.get_qb_qbond_pub_tra(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源:森浦QB<br>五家经济商现卷(BOND)成交行情:@10年国开债,@5年国开债</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th></tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>上游行情唯一标识</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>经纪商类型</td><td>String</td><td>brokerType</td><td>TP_BBO2:国利,ICAP_BBO2:国际,<br>BGC_BBO2:中诚,TJXT_BBO2:信唐,<br>PATR_BBO2:平安</td></tr>
<tr><td>发行市场</td><td>String</td><td>marketType</td><td>CIB：银行间/SSE：上交所/SZE：深交所</td></tr>
<tr><td>价格类型</td><td>String</td><td>priceType</td><td>QBondPubTra</td></tr>
<tr><td>扩展字段</td><td>String</td><td>text</td><td>--</td></tr>
<tr><td>报价类型</td><td>String</td><td>side</td><td>X-taken,Y-given,Z-trade</td></tr>
<tr><td>结算日期</td><td>String</td><td>settlDate</td><td>--</td></tr>
<tr><td>交易是否有效</td><td>String</td><td>tradeStatus</td><td>N、R有效，C无效（删除或撤销）</td></tr>
<tr><td>清算速度</td><td>String</td><td>clearSpeed</td><td>1-"T+0",2-"T+1"</td></tr>
<tr><td>成交状态</td><td>String</td><td>dealStatus</td><td>--</td></tr>
<tr><td>债券代码</td><td>String</td><td>bondCode</td><td>--</td></tr>
<tr><td>债券名称</td><td>String</td><td>bondName</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>收益率</td><td>Number</td><td>yield</td><td>--</td></tr>
<tr><td>净价</td><td>Number</td><td>netPrice</td><td>--</td></tr>
<tr><td>成交量</td><td>Number</td><td>tradeAmt</td><td>--</td></tr>
<tr><td>全价</td><td>Number</td><td>fullPrice</td><td>--</td></tr>
<tr><td>原始价格</td><td>Number</td><td>originalPrice</td><td>--</td></tr>
<tr><td>到期收益率</td><td>Number</td><td>marturityRate</td><td>--</td></tr>
<tr><td>行权收益率</td><td>Number</td><td>exerciseRate</td><td>--</td></tr>
<tr><td>利差</td><td>Number</td><td>spread</td><td>--</td></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.森浦QB现券成交行情查询API执行
2.获取查询集合结果集的第一条数据
3.获取查询集合结果集的第一条数据对象的symbol属性值
'''
qlog.info("森浦QB成交行情查询API执行")
qbondPubTra = bond.get_qb_qbond_pub_tra(['090012'])
if qbondPubTra:
    qlog.info(qbondPubTra[0])
    qlog.info(qbondPubTra[0].symbol)
qlog.info("森浦QB成交行情操作完成")
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询合约为{090012}的森浦QB成交行情查询API执行"
text:{"bondCode":"090012","bondName":"09附息国债12","brokerType":"TPB","clearSpeed":"T+0","dealStatus":"1","eic":"QBondPubTra#090012=QB","fullPrice":99.973,"marketType":"SSE","originalPrice":2.7656,"priceId":"1909291945370106865","priceState":"3","priceType":"YTM","quoteId":"1405268","quoteUnit":"1","receivingDate":20190929,"receivingTime":194537671,"rowId":627954156518244352,"sendDate":20190929,"sendTime":194537731,"sendingDate":20190905,"sendingTime":55153621,"settlDate":"20190924","side":"X","source":"QB","symbol":"090012","sysUpdateDate":20190929,"sysUpdateTime":194537738,"text":"{1,2,3,4,5,6}","tickSize":4,"time":1547085600000,"tradeAmt":3000.0,"tradeStatus":"N","type":"QBondPubTra","updateDate":20190829,"updateTime":200335889,"updateType":"1","yield":2.7603}
text:"090012"
text:"森浦QB成交行情操作完成"
</pre>

### XBOND现券成交行情 
<h3>●函数定义</h3> <p style="display:none">QUANT_HDS_CMDS_XBOND_PUB_TRADE</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'XBondPubTra', symbols)
'''获取数据函数调用方法'''
bond.get_cmds_xbond_pub_tra(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:CFETS-CMDSRMD<br>XBOND现券成交行情:@10年国开债,@5年国开债</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th></tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td><tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>上游行情唯一标识</td><tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>来源行情唯一标识</td><tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td><tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td><tr>
<tr><td>市场标识</td><td>String</td><td>marketIndicator</td><td>4-现券买卖</td><tr>
<tr><td>交易方式</td><td>String</td><td>tradeType</td><td>3-匿名点击</td><tr>
<tr><td>清算速度</td><td>String</td><td>clearSpeed</td><td>1-T+0,2-T+1</td><tr>
<tr><td>最新成交方向</td><td>Number</td><td>latestSide</td><td>1买 2卖</td><tr>
<tr><td>最新成交净价</td><td>Number</td><td>latestNetPrice</td><td>--</td><tr>
<tr><td>最新成交到期收益率</td><td>Number</td><td>latestYield</td><td>--</td><tr>
<tr><td>最高成交净价</td><td>Number</td><td>highestNetPrice</td><td>--</td><tr>
<tr><td>最高成交到期收益率</td><td>Number</td><td>highestYield</td><td>--</td><tr>
<tr><td>最低成交净价</td><td>Number</td><td>lowestNetPrice</td><td>--</td><tr>
<tr><td>最低成交到期收益率</td><td>Number</td><td>lowestYield</td><td>--</td><tr>
<tr><td>开盘价</td><td>Number</td><td>openingRate</td><td>--</td><tr>
<tr><td>开盘到期收益率</td><td>Number</td><td>openingYield</td><td>--</td><tr>
<tr><td>前开盘净价</td><td>Number</td><td>preOpeningNetPrice</td><td>--</td><tr>
<tr><td>前开盘到期收益率</td><td>Number</td><td>preOpeningYield</td><td>--</td><tr>
<tr><td>加权平均净价</td><td>Number</td><td>avgNetPrice</td><td>--</td><tr>
<tr><td>加权平均到期收益率</td><td>Number</td><td>avgYield</td><td>--</td><tr>
<tr><td>前加权平均净价</td><td>Number</td><td>preAvgNetPrice</td><td>--</td><tr>
<tr><td>前加权平均到期收益率</td><td>Number</td><td>preAvgYield</td><td>--</td><tr>
<tr><td>前收盘净价</td><td>Number</td><td>preCloseNetPrice</td><td>--</td><tr>
<tr><td>前收盘到期收益率</td><td>Number</td><td>preCloseYield</td><td>--</td><tr>
<tr><td>净价涨跌幅</td><td>Number</td><td>netPriceBP</td><td>--</td><tr>
<tr><td>收益率涨跌</td><td>Number</td><td>yieldBP</td><td>--</td><tr>
<tr><td>成交量</td><td>Number</td><td>tradeAmt</td><td>--</td><tr>
<tr><td>行情日期</td><td>Number</td><td>updateDate</td><td>--</td><tr>
<tr><td>行情时间</td><td>Number</td><td>updateTime</td><td>--</td><tr>
<tr><td>债券代码</td><td>String</td><td>bondCode</td><td>--</td><tr>
<tr><td>债券名称</td><td>String</td><td>bondName</td><td>--</td><tr>
<tr><td>是否是上市前债券</td><td>String</td><td>preMarketBondIndicator</td><td>--</td><tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.XBOND现券成交行情
2.获取查询集合结果集的第一条数据
3.获取XBOND现券成交行情第一条的合约
'''
qlog.info("XBOND现券成交行情")
xBondPubTra = bond.get_cmds_xbond_pub_tra(['010011'])
if xBondPubTra:
    qlog.info(xBondPubTra[0])
    qlog.info(xBondPubTra[0].symbol)
qlog.info("XBOND现券成交行情完成")
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询合约为{010011}的XBOND现券成交行情"
text:{"avgNetPrice":99.9925,"avgYield":2.6003,"bondCode":"090207","bondName":"090207","clearSpeed":"1","eic":"XBondPubTra#090207=CMDS","highestNetPrice":100.16,"highestYield":2.77,"latestNetPrice":100.08,"latestSide":1,"latestYield":2.76,"lowestNetPrice":99.88,"lowestYield":2.48,"marketIndicator":"marketIndicator","netPriceBP":0.09,"openingRate":99.99,"openingYield":2.58,"preAvgNetPrice":99.9623,"preAvgYield":2.5468,"preCloseNetPrice":99.99,"preCloseYield":2.58,"preMarketBondIndicator":"preMarketBondIndicator","preOpeningNetPrice":99.95,"preOpeningYield":2.53,"priceId":"1911011417148204549","priceState":"3","quoteUnit":"1","receivingDate":20190306,"receivingTime":100235000,"rowId":639830312985034752,"sendDate":20191101,"sendTime":141714045,"sendingDate":20190306,"sendingTime":100235000,"source":"CMDS","symbol":"010011","sysUpdateDate":20191101,"sysUpdateTime":141714088,"tickSize":4,"time":1547085600000,"tradeAmt":776.0,"tradeType":"3","type":"XBondPubTra","updateDate":20190306,"updateTime":100235000,"updateType":"1","yieldBP":19.0}
text:"010011"
text:"XBOND现券成交行情完成"
</pre>

### 森浦QB现券最优行情
<h3>●函数定义</h3> <p style="display: none">QUANT_HDS_QB_QBOND_PUB_TOP_BOOK</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('QB', 'QBondPubTop', symbols)
'''获取数据函数调用方法'''
bond.get_qb_qbond_pub_top(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:森浦QB<br>五家经济商现卷(BOND)最优报价行情:@10年国开债,@5年国开债</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
    <tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th></tr>
    <tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td><tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>上游行情唯一标识</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>买入量</td><td>Number</td><td>bidAmt</td><td>--</td></tr>
<tr><td>卖出量</td><td>Number</td><td>askAmt</td><td>--</td></tr>
<tr><td>买价</td><td>Number</td><td>bid</td><td>--</td></tr>
<tr><td>卖价</td><td>Number</td><td>ask</td><td>--</td></tr>
<tr><td>买入到期收益率</td><td>Number</td><td>bidYield</td><td>--</td></tr>
<tr><td>卖出到期收益率</td><td>Number</td><td>askYield</td><td>--</td></tr>
<tr><td>买入行权收益率</td><td>Number</td><td>bidExerciseYield</td><td>--</td></tr>
<tr><td>卖出行权收益率</td><td>Number</td><td>askExerciseYield</td><td>--</td></tr>
<tr><td>经纪商类型</td><td>String</td><td>brokerType</td><td>TP_BBO2:国利,ICAP_BBO2:国际,<br>BGC_BBO2:中诚,TJXT_BBO2:信唐,<br>PATR_BBO2:平安</td></tr>
<tr><td>发行市场</td><td>String</td><td>marketType</td><td>CIB：银行间/SSE：上交所/SZE：深交所</td></tr>
<tr><td>买价编号BIDID</td><td>String</td><td>bidId</td><td>--</td></tr>
<tr><td>卖价编号ASKID</td><td>String</td><td>askId</td><td>--</td></tr>
<tr><td>买入净价</td><td>Number</td><td>bidNetPrice</td><td>--</td></tr>
<tr><td>卖出净价</td><td>Number</td><td>askNetPrice</td><td>--</td></tr>
<tr><td>买入是否可议价</td><td>String</td><td>bidBargainFlag</td><td>1表示可议价, 2表示更可议价, 无值表示不可议价</td></tr>
<tr><td>买出是否可议价</td><td>String</td><td>askBargainFlag</td><td>1表示可议价, 2表示更可议价, 无值表示不可议价</td></tr>
<tr><td>买入收益率类型</td><td>String</td><td>bidExerciseFlag</td><td>0表示行权收益率, 1或空：到期收益率</td></tr>
<tr><td>卖出收益率类型</td><td>String</td><td>askExerciseFlag</td><td>0表示行权收益率, 1或空：到期收益率</td></tr>
<tr><td>买价订单类型</td><td>String</td><td>bidRelationFlag</td><td>1表示OCO, 2表示打包, 无值表示无OCO无打包</td></tr>
<tr><td>卖价订单类型</td><td>String</td><td>askRelationFlag</td><td>1表示OCO, 2表示打包, 无值表示无OCO无打包</td></tr>
<tr><td>卖价注释</td><td>String</td><td>askComment</td><td>--</td></tr>
<tr><td>买价注释</td><td>String</td><td>bidComment</td><td>--</td></tr>
<tr><td>bidAmt分量</td><td>String</td><td>bidAmtPieces</td><td>--</td></tr>
<tr><td>askAmt分量</td><td>String</td><td>askAmtPieces</td><td>--</td></tr>
<tr><td>买入价格类型</td><td>String</td><td>bidPriceType</td><td>--</td></tr>
<tr><td>卖出价格类型</td><td>String</td><td>askPriceType</td><td>--</td></tr>
<tr><td>买入全价</td><td>Number</td><td>bidFullPrice</td><td>--</td></tr>
<tr><td>卖出全价</td><td>Number</td><td>askFullPrice</td><td>--</td></tr>
<tr><td>买入利差</td><td>Number</td><td>bidSpread</td><td>--</td></tr>
<tr><td>卖出利差</td><td>Number</td><td>askSpread</td><td>--</td></tr>
<tr><td>债券代码</td><td>String</td><td>bondCode</td><td>--</td></tr>
<tr><td>债券名称</td><td>String</td><td>bondName</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.森浦QB现券最优行情
2.获取查询集合结果集的第一条数据
3.获取森浦QB现券最优行情第一条symbol属性值
'''
qlog.info("森浦QB现券最优行情")
qBondPubTop = bond.get_qb_qbond_pub_top(['090012'])
if qBondPubTop:
    qlog.info(qBondPubTop[0])
    qlog.info(qBondPubTop[0].symbol)
qlog.info("森浦QB现券最优行情完成")
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询合约为{090012}的森浦QB现券最优行情"
text:{"ask":100.12,"askAmt":10.0,"askAmtPieces":"1,4,5","askBargainFlag":"0","askComment":"不可议价","askExerciseFlag":"1","askId":"1405283","askNetPrice":99.919,"askPriceType":"2","askRelationFlag":"1","bid":99.987,"bidAmt":10.0,"bidAmtPieces":"1,4,5","bidBargainFlag":"0","bidComment":"不可议价","bidExerciseFlag":"1","bidId":"1405283","bidNetPrice":99.777,"bidPriceType":"2","bidRelationFlag":"1","bondCode":"090012","bondName":"09附息国债12","brokerType":"TP_BBO2","eic":"QBondPubTop#090012=QB","marketType":"SSE","priceId":"1909291951590108216","priceState":"3","quoteUnit":"1","receivingDate":20190929,"receivingTime":195159436,"rowId":627955759660924928,"sendDate":20190929,"sendTime":195159918,"sendingDate":20190906,"sendingTime":104410648,"source":"QB","symbol":"090012","sysUpdateDate":20190929,"sysUpdateTime":195159957,"tickSize":4,"time":1547085600000,"type":"QBondPubTop","updateDate":20190930,"updateTime":212624442,"updateType":"1"}
text:"090012"
text:"森浦QB现券最优行情完成"
</pre>

### 中金所国债期货行情
<h3>●函数定义</h3> <p style="display: none">QUANT_HDS_CFFEX_FUTURES_PUB_ODM_DEPT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CFFEX', 'GbFutPubOdm', symbols)
'''获取数据函数调用方法'''
bond.get_fut_pub_odm(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:中金所接口<br>DataFeed API:@10年国债期货,@5年国债期货,@2年国债期货</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>交易日</td><td>String</td><td>tradingDay</td><td>--</td></tr>
<tr><td>结算组代码</td><td>String</td><td>settlementGroupId</td><td>--</td></tr>
<tr><td>结算编号</td><td>String</td><td>settlementId</td><td>--</td></tr>
<tr><td>最新价</td><td>Number</td><td>lastPrice</td><td>--</td></tr>
<tr><td>昨结算</td><td>Number</td><td>preSettlementPrice</td><td>--</td></tr>
<tr><td>昨收盘</td><td>Number</td><td>preClosePrice</td><td>--</td></tr>
<tr><td>昨持仓量</td><td>Number</td><td>preOpenInterest</td><td>--</td></tr>
<tr><td>今开盘</td><td>Number</td><td>openPrice</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>highestPrice</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>lowestPrice</td><td>--</td></tr>
<tr><td>数量</td><td>Number</td><td>amt</td><td>--</td></tr>
<tr><td>成交金额</td><td>Number</td><td>tradeAmt</td><td>--</td></tr>
<tr><td>持仓量</td><td>Number</td><td>openInterest</td><td>--</td></tr>
<tr><td>今收盘</td><td>Number</td><td>closePrice</td><td>--</td></tr>
<tr><td>今结算</td><td>Number</td><td>settlementPrice</td><td>--</td></tr>
<tr><td>涨停板价</td><td>Number</td><td>upperLimitPrice</td><td>--</td></tr>
<tr><td>跌停板价</td><td>Number</td><td>lowerLimitPrice</td><td>--</td></tr>
<tr><td>昨虚实度</td><td>Number</td><td>preDelta</td><td>--</td></tr>
<tr><td>今虚实度</td><td>Number</td><td>currDelta</td><td>--</td></tr>
<tr><td>最后修改毫秒</td><td>Number</td><td>updateMillisec</td><td>--</td></tr>
<tr><td>结构类</td><td>Object</td><td>cbondPubOdmRateDOS</td><td>方向:side,数量:amt,层级:level,价格:price,可成交量:tradeAmt;<br>list[side,amt,level,price,tradeAmt]</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.中金所国债期货行情
2.获取查询集合结果集的第一条数据
3.获取数据对象的symbol属性值
4.获取数据对象的cbondPubOdmRateDOS属性值
5.获取数据对象的cbondPubOdmRateDOS第一个价格集合
6.获取数据对象的cbondPubOdmRateDOS第一个价格集合金额
'''
qlog.info("中金所国债期货行情")
futPubOdm = bond.get_fut_pub_odm(['TS1909'])
if futPubOdm:
    qlog.info(futPubOdm[0])
    qlog.info(futPubOdm[0].symbol)
    qlog.info(futPubOdm[0].cbondPubOdmRateDOS)
    qlog.info(futPubOdm[0].cbondPubOdmRateDOS[0])
    qlog.info(futPubOdm[0].cbondPubOdmRateDOS[0].amt)
qlog.info("中金所国债期货行情完成")
</pre>

<h3>●输出结果</h3>
<pre>
text:"查询合约为{TS1909}的中金所国债期货行情"
text:{"amt":1000.0,"cbondPubOdmRateDOS":[{"amt":7.0,"level":1,"price":99.88,"side":1},{"amt":5.0,"level":2,"price":99.878,"side":1},{"amt":8.0,"level":3,"price":99.876,"side":1},{"amt":10.0,"level":4,"price":99.875,"side":1},{"amt":11.0,"level":5,"price":99.872,"side":1},{"amt":5.0,"level":1,"price":100.02,"side":2},{"amt":7.0,"level":2,"price":100.025,"side":2},{"amt":8.0,"level":3,"price":100.028,"side":2},{"amt":5.0,"level":4,"price":100.03,"side":2},{"amt":10.0,"level":5,"price":100.032,"side":2}],"closePrice":100.01,"currDelta":0.0,"eic":"GbFutPubOdm#TS1909=CFFEX","highestPrice":100.022,"lastPrice":100.01,"lowerLimitPrice":99.445,"lowestPrice":99.845,"modifyTime":100300,"openInterest":2407.0,"openPrice":99.95,"preClosePrice":99.95,"preDelta":0.0,"preOpenInterest":2405.0,"preSettlementPrice":99.95,"priceId":"1910311628158200784","priceState":"3","quoteUnit":"Y","receivingDate":20190306,"receivingTime":100300000,"rowId":639500896891305984,"sendDate":20191031,"sendTime":162815100,"sendingDate":20190306,"sendingTime":100300000,"settlementGroupId":"140256","settlementId":"140652","settlementPrice":100.01,"source":"CFFEX","symbol":"TS1909","sysUpdateDate":20191031,"sysUpdateTime":162815171,"tickSize":3,"time":1547085600000,"tradeAmt":75198.9,"tradingDay":"20190306","type":"GbFutPubOdm","updateMillisec":100300000,"updateTime":100300,"updateType":"1","upperLimitPrice":100.45}
text:"TS1909"
text:[{"amt":7.0,"level":1,"price":99.88,"side":1},{"amt":5.0,"level":2,"price":99.878,"side":1},{"amt":8.0,"level":3,"price":99.876,"side":1},{"amt":10.0,"level":4,"price":99.875,"side":1},{"amt":11.0,"level":5,"price":99.872,"side":1},{"amt":5.0,"level":1,"price":100.02,"side":2},{"amt":7.0,"level":2,"price":100.025,"side":2},{"amt":8.0,"level":3,"price":100.028,"side":2},{"amt":5.0,"level":4,"price":100.03,"side":2},{"amt":10.0,"level":5,"price":100.032,"side":2}]
text:{"amt":7.0,"level":1,"price":99.88,"side":1}
text:7.0
text:"中金所国债期货行情完成"
</pre>    

### CFETS回购定盘利率
<h3>●函数定义</h3> <p style="display: none">QUANT_HDS_CMDS_FREPO_BENCHMARK_QUOTA</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'FRepoBM', symbols)
'''获取数据函数调用方法'''
repo.get_repo_bm(symbols)
</pre>
<h3>●函数说明<h3>
<h3>数据来源:CFETS-CMDSRMB<br>基准数据及收盘曲线数据接入:@FR007回购定盘利率<h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>期限</td><td>String</td><td>tenor</td><td>1,2-7</td></tr>
<tr><td>利率</td><td>Number</td><td>fixedBackRate</td><td>--</td></tr>
<tr><td>更新状态</td><td>String</td><td>updateType</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取Qrepo基准数第一条symbol属性值
'''
qlog.info("CFETS回购定盘利率")
repoBm = repo.get_repo_bm(['CNY_Repo_FR001'])
if repoBm:
    qlog.info(repoBm[0])
    qlog.info(repoBm[0].symbol)
qlog.info("CFETS回购定盘利率完成")
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询合约为{CNY_Repo_FR001}的CFETS回购定盘利率"
text:{"eic":"FRepoBM#CNY_Repo_FR001=CMDS","fixedRepo":2.49,"priceState":"3","quoteUnit":"1","receivingDate":20190306,"receivingTime":100220000,"rowId":639830312481718272,"sendDate":20191101,"sendTime":141713965,"sendingDate":20190306,"sendingTime":100220000,"source":"CMDS","symbol":"CNY_Repo_FR001","sysUpdateDate":20191101,"sysUpdateTime":141713968,"tenor":"2-7","tickSize":4,"time":1547085600000,"type":"FRepoBM","updateDate":20190306,"updateTime":100220000,"updateType":"1"}
text:"CNY_Repo_FR001"
text:"CFETS回购定盘利率完成"
</pre>

### CFETS质押式回购成交行情 
<h3>●函数定义</h3> <p style="display: none">QUANT_HDS_CMDS_REPO_PUB_TRADE</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'RepoPubTra', symbols)
'''获取数据函数调用方法'''
repo.get_repo_pub_tra(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源:CFETS-CMDSRMB<br>质押式回购成交数据:@R007综合行情</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>2-异常,3-正常</td></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>市场标识</td><td>String</td><td>marketIndicator</td><td>4-现券买卖</td></tr>
<tr><td>交易方式</td><td>String</td><td>tradeType</td><td>3-匿名点击</td></tr>
<tr><td>最新利率</td><td>Number</td><td>latestRate</td><td>--</td></tr>
<tr><td>开盘利率</td><td>Number</td><td>openingRate</td><td>--</td></tr>
<tr><td>最高利率</td><td>Number</td><td>highestRate</td><td>--</td></tr>
<tr><td>最低利率</td><td>Number</td><td>lowestRate</td><td>--</td></tr>
<tr><td>成交量</td><td>Number</td><td>tradeAmt</td><td>--</td></tr>
<tr><td>成交订单笔数</td><td>Number</td><td>tradeOrderAmt</td><td>--</td></tr>
<tr><td>收盘利率</td><td>Number</td><td>closingRate</td><td>--</td></tr>
<tr><td>收盘利率</td><td>Number</td><td>PreClosingRate</td><td>--</td></tr>
<tr><td>加权平均利率</td><td>Number</td><td>avgRate</td><td>--</td></tr>
<tr><td>前加权平均利率</td><td>Number</td><td>preAvgRate</td><td>--</td></tr>
<tr><td>平均回购期限</td><td>String</td><td>avgTenor</td><td>--</td></tr>
<tr><td>加权平均利率（利率债）</td><td>Number</td><td>avgIRSRate</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.CFETS质押式回购成交行情
2.获取查询集合结果集的第一条数据
3.获取CFETS质押式回购成交行情第一条symbol属性值
'''
qlog.info("CFETS质押式回购成交行情")
repoPubTra = repo.get_repo_pub_tra(['R007'])
if repoPubTra:
    qlog.info(repoPubTra[0])
    qlog.info(repoPubTra[0].symbol)
qlog.info("CFETS质押式回购成交行情完成")
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询合约为{R007}的CFETS质押式回购成交行情"
text:{"avgIRSRate":0.0,"avgRate":0.0,"avgTenor":"AvgTerm","closingRate":0.0,"eic":"RepoPubTra#R007=CMDS","highestRate":0.256,"latestRate":0.3212,"lowestRate":0.3212,"marketIndicator":"9","openingRate":0.0,"preAvgRate":0.0,"preClosingRate":0.0,"priceId":"696334061517410304","priceState":"3","quoteUnit":"1","receivingDate":20190306,"receivingTime":100225000,"rowId":696334061580320768,"sendDate":20200405,"sendTime":122257527,"sendingDate":20190306,"sendingTime":100225000,"source":"CMDS","symbol":"R007","sysUpdateDate":20200405,"sysUpdateTime":122257542,"tickSize":4,"time":1547085600000,"tradeAmt":1000.0,"tradeOrderAmt":1000,"tradeType":"3","type":"RepoPubTra","updateDate":20190306,"updateTime":100225000,"updateType":"1"}
text:"R007"
text:"CFETS质押式回购成交行情完成"
</pre>

### CFETS-Shibor基准利率 
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_CMDS_BIBOR_BENCHMARK_QUOTA</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'BIborBM', symbols)
'''获取数据函数调用方法'''
ibor.get_ibor_bm(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源:CFETS-CMDSRMB<br>基准数据及收盘曲线数据接入:@Shibor3M基准利率</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>shibor值（%）</td><td>Number</td><td>shibor</td><td>--</td></tr>
<tr><td>涨跌</td><td>Number</td><td>shiborBP</td><td>--</td></tr>
<tr><td>期限</td><td>String</td><td>tenor</td><td>O/N,3M</td></tr>
<tr><td>更新状态</td><td>String</td><td>updateType</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.Shibor基准利率
2.获取查询集合结果集的第一条数据
3.获取shibor基准数据第一条symbol属性值
'''
qlog.info("Shibor基准利率查询")
iborBm = ibor.get_ibor_bm(['CNY_Shibor_O/N'])
if iborBm:
    qlog.info(iborBm[0])
    qlog.info(iborBm[0].symbol)
qlog.info("Shibor基准利率查询完成")
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询合约为{CNY_Shibor_O/N}的Shibor基准利率查询"
text:{"eic":"BIborBM#CNY_Shibor_O/N=CMDS","priceState":"3","quoteUnit":"1","receivingDate":20190306,"receivingTime":100210000,"rowId":639830312192311296,"sendDate":20191101,"sendTime":141713894,"sendingDate":20190306,"sendingTime":100210000,"shibor":2.478,"shiborBP":0.121,"source":"CMDS","symbol":"CNY_Shibor_O/N","sysUpdateDate":20191101,"sysUpdateTime":141713899,"tenor":"O/N","tickSize":4,"time":1547085600000,"type":"BIborBM","updateDate":20190306,"updateTime":100210000,"updateType":"1"}
text:"CNY_Shibor_O/N"
text:"Shibor基准利率查询完成"
</pre>

### CFETS-Shibor报价行报价行情
<h3>●函数定义</h3> <p style="display: none">QUANT_HDS_CMDS_IBOR_QUOTE_LINE</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'IborQuote', symbols)
'''获取数据函数调用方法'''
ibor.get_ibor_quote(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:CFETS-CMDSRMB<br>Shibor报价行报价行情:@Shibor机构报价</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>0-异常,1-正常</td></tr>
<tr><td>shibor</td><td>Number</td><td>shibor</td><td>--</td></tr>
<tr><td>报价行报价</td><td>String</td><td>quoteLineId</td><td>--</td></tr>
<tr><td>报价行简称</td><td>String</td><td>quoteLine</td><td>--</td></tr>
<tr><td>更新状态</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>期限</td><td>String</td><td>tenor</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取shibor报价行情第一条symbol属性值
'''
qlog.info("Shibor报价行报价行情")
iborQuote = ibor.get_ibor_quote(['CNY_Shibor_1M'])
if iborQuote:
    qlog.info(iborQuote[0])
    qlog.info(iborQuote[0].symbol)
qlog.info("Shibor报价行报价行情完成")
</pre>   
<h3>●输出结果</h3>
<pre>
text:"查询合约为{CNY_Shibor_1M}的Shibor报价行报价行情"
text:{"eic":"IborQuote#CNY_Shibor_1M=CMDS","priceState":"3","quoteLine":"quoteLine","quoteLineId":"quoteLineID","quoteUnit":"1","receivingDate":20190306,"receivingTime":100215000,"rowId":639830313085698048,"sendDate":20191101,"sendTime":141714109,"sendingDate":20190306,"sendingTime":100215000,"shibor":3.265,"source":"CMDS","symbol":"CNY_Shibor_1M","sysUpdateDate":20191101,"sysUpdateTime":141714112,"tenor":"1M","tickSize":4,"time":1547085600000,"type":"IborQuote","updateDate":20190306,"updateTime":100215000,"updateType":"1"}
text:"CNY_Shibor_1M"
text:"Shibor报价行报价行情完成"
</pre>

### XSWAP公有成交行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_CMDS_IRS_PUB_TRADE</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'IrsPubTra', symbols)
'''获取数据函数调用方法'''
irs.get_irs_pub_tra(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:CFETS-CMDSRMB<br>XSWAP利率互换成交行情:@Shibor3M的所有基础合约<br>Spread合约
,@S3M/R07的所有Basis合约<br>@FR007的所有基础合约及Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最新利率</td><td>Number</td><td>latestRate</td><td>--</td></tr>
<tr><td>最新成交量</td><td>Number</td><td>latestAmt</td><td>--</td></tr>
<tr><td>开盘利率</td><td>Number</td><td>openingRate</td><td>--</td></tr>
<tr><td>最高利率</td><td>Number</td><td>highestRate</td><td>--</td></tr>
<tr><td>最低利率</td><td>Number</td><td>lowestRate</td><td>--</td></tr>
<tr><td>盘中参考价</td><td>Number</td><td>referPrice</td><td>--</td></tr>
<tr><td>当日累计成交量</td><td>Number</td><td>cumulTradeAmt</td><td>--</td></tr>
<tr><td>市场标识</td><td>String</td><td>marketIndicator</td><td>4-现券买卖</td></tr>
<tr><td>交易方式</td><td>String</td><td>tradeType</td><td>3-匿名点击</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.XSWAP公有成交行情
2.获取查询集合结果集的第一条数据
3.获取CMDS公有成交行第一条symbol属性值
'''
qlog.info("XSWAP公有成交行情查询")
irsPubTra = irs.get_irs_pub_tra(['FR007_1Y'])
if irsPubTra:
    qlog.info(irsPubTra[0])
    qlog.info(irsPubTra[0].symbol)
qlog.info("XSWAP公有成交行情完成")
</pre>     
<h3>●输出结果</h3>
<pre>  
text:"查询合约为{FR007_1Y}的XSWAP公有成交行情查询"
text:{"cumulTradeAmt":1.0E8,"eic":"IrsPubTra#FR007_1Y=CMDS","highestRate":2.7275,"latestAmt":5.0E7,"latestRate":2.725,"lowestRate":2.7225,"marketIndicator":"2","openingRate":2.725,"priceId":"686526717132541952","priceState":"3","quoteUnit":"A","receivingDate":20190201,"receivingTime":103000000,"referPrice":2.725,"rowId":686526717296115712,"sendDate":20200309,"sendTime":105204407,"sendingDate":20190201,"sendingTime":103000000,"smallType":"U","source":"CMDS","symbol":"FR007_1Y","sysUpdateDate":20200309,"sysUpdateTime":105204453,"tickSize":4,"time":1547085600000,"tradeType":"3","type":"IrsPubTra","updateDate":20190201,"updateTime":100600000,"updateType":"1"}
text:"FR007_1Y"
text:"XSWAP公有成交行情完成"
</pre>

### XSWAP公有最优价行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_CMDS_IRS_PUB_TOP_BOOK</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'IrsPubTop', symbols)
'''获取数据函数调用方法'''
irs.get_irs_pub_top(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于：CFETS-CMDSRMB<br>XSWAP利率互换最优价行情:@Shibor3M的所有基础合约<br>Spread合约
,@S3M/R07的所有Basis合约<br>@FR007的所有基础合约,Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>系统行情唯一标识</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>买价</td><td>Number</td><td>bid</td><td>--</td></tr>
<tr><td>卖价</td><td>Number</td><td>ask</td><td>--</td></tr>
<tr><td>买价总量</td><td>Number</td><td>bidAmt</td><td>--</td></tr>
<tr><td>卖价总量</td><td>Number</td><td>askAmt</td><td>--</td></tr>
<tr><td>行情更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>行情更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>市场标识</td><td>String</td><td>marketIndicator</td><td>4-现券买卖</td></tr>
<tr><td>交易方式</td><td>String</td><td>tradeType</td><td>3-匿名点击</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.XSWAP公有最优价行情
2.获取查询集合结果集的第一条数据
3.获取CMDS公有最优价第一条symbol属性值
'''
qlog.info("XSWAP公有最优价行情")
irsPubTop = irs.get_irs_pub_top(['FR007_1Y'])
if irsPubTop:
    qlog.info(irsPubTop[0])
    qlog.info(irsPubTop[0].symbol)
qlog.info("XSWAP公有最优价行情结束")
</pre>  
<h3>●输出结果</h3>
<pre>
text:"查询合约为{FR007_1Y}的XSWAP公有最优价行情"
text:{"ask":2.7275,"askAmt":2.0E8,"bid":2.725,"bidAmt":1.0E8,"eic":"IrsPubTop#FR007_1Y=CMDS","marketIndicator":"2","priceId":"663699232644927489","priceState":"3","quoteUnit":"A","receivingDate":20190110,"receivingTime":100000000,"rowId":663699232670089216,"sendDate":20200106,"sendTime":110347998,"sendingDate":20190110,"sendingTime":100000000,"smallType":"U","source":"CMDS","symbol":"FR007_1Y","sysUpdateDate":20200106,"sysUpdateTime":110348004,"tickSize":4,"time":1547085600000,"type":"IrsPubTop","updateDate":20190110,"updateTime":100000000}
text:"FR007_1Y"
text:"XSWAP公有最优价行情结束"
</pre>

### XSWAP私有最优价行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_XSWAP_IRS_PVT_TOP_BOOK</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('XSWAP', 'IrsPvtTop', symbols)
'''获取数据函数调用方法'''
irs.get_irs_pvt_top(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:CFETS-XSWAP<br>XSWAP利率互换私有最优价行情:@Shibor3M的所有基础合约<br>Spread合约
,@S3M/R07的所有Basis合约<br>@FR007的所有基础合约,Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>系统行情唯一标识</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>买价</td><td>Number</td><td>bid</td><td>--</td></tr>
<tr><td>卖价</td><td>Number</td><td>ask</td><td>--</td></tr>
<tr><td>买价总量</td><td>Number</td><td>bidAmt</td><td>--</td></tr>
<tr><td>卖价总量</td><td>Number</td><td>askAmt</td><td>--</td></tr>
<tr><td>买价可成交量</td><td>Number</td><td>bidTradeAmt</td><td>--</td></tr>
<tr><td>卖价可成交量</td><td>Number</td><td>askTradeAmt</td><td>--</td></tr>
<tr><td>买价订单总数</td><td>Number</td><td>bidOrderAmt</td><td>默认值：0(暂为0)</td></tr>
<tr><td>卖价订单总数</td><td>Number</td><td>askOrderAmt</td><td>默认值：0(暂为0)</td></tr>
<tr><td>买价源更新牌价日期</td><td>Number</td><td>bidUpdateDate</td><td>--</td></tr>
<tr><td>买价源更新牌价时间</td><td>Number</td><td>bidUpdateTime</td><td>--</td></tr>
<tr><td>卖价源更新牌价日期</td><td>Number</td><td>askUpdateDate</td><td>--</td></tr>
<tr><td>卖价源更新牌价时间</td><td>Number</td><td>askUpdateTime</td><td>--</td></tr>
<tr><td>中心操作标识</td><td>String</td><td>operReportId</td><td>--</td></tr>
<tr><td>订阅类型</td><td>String</td><td>mdBookType</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.XSWAP私有最优价查询
2.获取查询集合结果集的第一条数据
3.获取XSWAP私有最优价第一条symbol属性值
'''
qlog.info("XSWAP私有最优价查询")
irsPvtTop = irs.get_irs_pvt_top(['FR007_1Y'])
if irsPvtTop:
    qlog.info(irsPvtTop[0])
    qlog.info(irsPvtTop[0].symbol)
qlog.info("XSWAP私有最优价查询结束")
</pre>  
<h3>●输出结果</h3>
<pre>
text:"查询合约为{FR007_1Y}的XSWAP私有最优价查询"
text:{"ask":2.7275,"askAmt":0.0,"askOrderAmt":0,"askTradeAmt":1.0E8,"askUpdateDate":20190110,"askUpdateTime":100000000,"bid":2.725,"bidAmt":0.0,"bidOrderAmt":0,"bidTradeAmt":1.0E8,"bidUpdateDate":20190110,"bidUpdateTime":100000000,"eic":"IrsPvtTop#FR007_1Y=XSWAP","mdBookType":"2","priceId":"1910241051158224431","priceState":"3","quoteUnit":"1","receivingDate":20190110,"receivingTime":100000000,"rowId":636879375718940672,"sendDate":20191024,"sendTime":105115795,"sendingDate":20190110,"sendingTime":100000000,"smallType":"U","source":"XSWAP","symbol":"FR007_1Y","sysUpdateDate":20191024,"sysUpdateTime":105115818,"tickSize":4,"time":1547085600000,"type":"IrsPvtTop","updateType":"1"}
text:"FR007_1Y"
text:"XSWAP私有最优价查询结束"
</pre>

### 森浦QB利率互换市场综合报价行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_QB_IRS_PUB_QUOTE_DATA</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('QB', 'IrsPubQuote', symbols)
'''获取数据函数调用方法'''
irs.get_irs_pub_quote(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:森浦QB:TP<br>IRS报价数据:@Shibor3M的所有基础合约,Spread合约
<br>@S3M/R07的所有Basis合约,@FR007的所有基础合约,Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>系统行情唯一标识</td></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>买入量</td><td>Number</td><td>bidAmt</td><td>--</td></tr>
<tr><td>卖出量</td><td>Number</td><td>askAmt</td><td>--</td></tr>
<tr><td>买价</td><td>Number</td><td>bid</td><td>--</td></tr>
<tr><td>卖价</td><td>Number</td><td>ask</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>经纪商类型</td><td>String</td><td>brokerType</td><td>--</td></tr>
<tr><td>利率类型</td><td>String</td><td>irType</td><td>--</td></tr>
<tr><td>交易期限</td><td>String</td><td>tradeTenor</td><td>--</td></tr>
<tr><td>交易方式</td><td>String</td><td>tradeMode</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.森浦QB利率互换市场综合报价行情
1.森浦QB利率互换市场综合报价行情
2.获取查询集合结果集的第一条数据
3.获取市场综合报价第一条symbol属性值
'''
qlog.info("森浦QB利率互换市场综合报价行情")
irsPubQuote = irs.get_irs_pub_quote(['FR007_1Y'])
if irsPubQuote:
    qlog.info(irsPubQuote[0])
    qlog.info(irsPubQuote[0].symbol)
qlog.info("森浦QB利率互换市场综合报价行情结束")
</pre> 
<h3>●输出结果</h3>
<pre>
"森浦QB利率互换市场综合报价行情"
{"ask":2.6377,"askAmt":2.0E7,"bid":2.6306,"bidAmt":1.0E7,"bigType":"null","brokerType":"TPB_IRS","eic":"IrsPubQuote#Shibor_3M=QB","irType":"Shibor","priceId":"1910081543300127710","priceState":"3","quoteUnit":"1","receivingDate":20191008,"receivingTime":154330474,"rowId":631154715559723008,"sendDate":20191008,"sendTime":154330471,"sendingDate":20190915,"sendingTime":170553330,"smallType":"Outright","source":"QB","symbol":"Shibor_3M","sysUpdateDate":20191008,"sysUpdateTime":154330477,"tickSize":4,"time":1568538353330,"tradeMode":"Outright","tradeTenor":"3M","type":"IrsPubQuote","updateDate":20190915,"updateTime":231252273,"updateType":"1"}
"Shibor_3M"
"森浦QB利率互换市场综合报价行情结束"
</pre>

### XSWAP公有深度行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_CMDS_IRS_PUB_ODM_DEPT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('CMDS', 'IrsPubOdm', symbols)
'''获取数据函数调用方法'''
irs.get_irs_pub_odm(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:CFETS-CMDSRMB<br>XSWAP公有深度行情:@Shibor3M的所有基础合约<br>Spread合约
,@S3M/R07的所有Basis合约<br>@FR007的所有基础合约,Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>系统行情唯一标识</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>价源更新牌价日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>价源更新牌价时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>市场标识</td><td>String</td><td>marketIndicator</td><td>4-现券买卖</td></tr>
<tr><td>交易方式</td><td>String</td><td>tradeType</td><td>3-匿名点击</td></tr>
<tr><td>结构类</td><td>Object</td><td>pubOdmRateDOS</td><td>方向:side (1买 2卖),数量:amt,层级:level,价格:price,可成交量:tradeAmt;<br>list[side,amt,level,price,tradeAmt]</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.XSWAP公有深度行情
2.获取查询集合结果集的第一条数据
3.获取CMDS公有深度行情第一条symbol属性值
'''
qlog.info("XSWAP公有深度行情查询")
irsPubOdm = irs.get_irs_pub_odm(['FR007_1Y'])
if irsPubOdm:
    qlog.info(irsPubOdm[0])
    qlog.info(irsPubOdm[0].symbol)
qlog.info("XSWAP公有深度行情结束")
</pre>     
<h3>●输出结果</h3>
<pre>
text:"查询合约为{FR007_1Y}的XSWAP公有深度行情查询"
text:{"eic":"IrsPubOdm#FR007_1Y=CMDS","marketIndicator":"2","priceId":"663743951995408385","priceState":"3","pubOdmRateDOS":[{"amt":2.0E8,"level":1,"price":2.7275,"side":2},{"amt":1.0E8,"level":1,"price":2.725,"side":1},{"amt":2.0E8,"level":2,"price":2.73,"side":2},{"amt":1.0E8,"level":2,"price":2.7225,"side":1},{"amt":2.0E8,"level":3,"price":2.7325,"side":2},{"amt":1.0E8,"level":3,"price":2.72,"side":1},{"amt":2.0E8,"level":4,"price":2.735,"side":2},{"amt":1.0E8,"level":4,"price":2.7175,"side":1},{"amt":2.0E8,"level":5,"price":2.7375,"side":2},{"amt":1.0E8,"level":5,"price":2.715,"side":1}],"quoteUnit":"A","receivingDate":20190110,"receivingTime":100000000,"rowId":663743952033153024,"sendDate":20200106,"sendTime":140129922,"sendingDate":20190110,"sendingTime":100000000,"smallType":"U","source":"CMDS","symbol":"FR007_1Y","sysUpdateDate":20200106,"sysUpdateTime":140129931,"tickSize":4,"time":1547085600000,"type":"IrsPubOdm","updateDate":20190110,"updateTime":100000000,"updateType":"1"}
text:"FR007_1Y"
text:"XSWAP公有深度行情结束"
</pre>

### XSWAP私有深度行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_XSWAP_IRS_PVT_ODM_DEPT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('XSWAP', 'IrsPvtOdm', symbols)
'''获取数据函数调用方法'''
irs.get_irs_pvt_odm(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:CFETS-XSWAP<br>XSWAP私有深度行情:@Shibor3M的所有基础合约<br>Spread合约
,@S3M/R07的所有Basis合约<br>@FR007的所有基础合约,Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>1-价格更新,2-价格撤销</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>1-正常,2-价格存疑,3-价格超时</td></tr>
<tr><td>中心操作标识</td><td>String</td><td>operReportId</td><td>--</td></tr>
<tr><td>订阅类型</td><td>String</td><td>mdBookType</td><td>--</td></tr>
<tr><td>结构类</td><td>Object</td><td>pvtOdmRateDOS</td><td>方向:side (1买 2卖),数量:amt,层级:level,价格:price,可成交量:tradeAmt;<br>list[side,amt,level,price,tradeAmt]</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取XSWAP私有深度行情第一条symbol属性值
'''
qlog.info("XSWAP私有深度行情查询")
irsPvtOdm = irs.get_irs_pvt_odm(['FR007_1Y'])
if irsPvtOdm:
    qlog.info(irsPvtOdm[0])
    qlog.info(irsPvtOdm[0].symbol)
qlog.info("XSWAP私有深度行情查询结束")
</pre>    
<h3>●输出结果</h3>
<pre>    
text:"查询合约为{FR007_1Y}的XSWAP私有深度行情查询"
text:{"eic":"IrsPvtOdm#FR007_1Y=XSWAP","mdBookType":"2","priceId":"663699169659064321","priceState":"3","pvtOdmRateDOS":[{"amt":2.0E8,"level":1,"price":2.7275,"side":2,"tradeAmt":2.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":1.0E8,"level":1,"price":2.725,"side":1,"tradeAmt":1.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":2.0E8,"level":2,"price":2.73,"side":2,"tradeAmt":2.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":1.0E8,"level":2,"price":2.7225,"side":1,"tradeAmt":1.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":2.0E8,"level":3,"price":2.7325,"side":2,"tradeAmt":2.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":1.0E8,"level":3,"price":2.72,"side":1,"tradeAmt":1.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":2.0E8,"level":4,"price":2.735,"side":2,"tradeAmt":2.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":1.0E8,"level":4,"price":2.7175,"side":1,"tradeAmt":1.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":2.0E8,"level":5,"price":2.7375,"side":2,"tradeAmt":2.0E8,"updateDate":20190110,"updateTime":100000000},{"amt":1.0E8,"level":5,"price":2.715,"side":1,"tradeAmt":1.0E8,"updateDate":20190110,"updateTime":100000000}],"quoteUnit":"A","receivingDate":20190110,"receivingTime":100000000,"rowId":663699169759723520,"sendDate":20200106,"sendTime":110332981,"sendingDate":20190110,"sendingTime":100000000,"smallType":"U","source":"XSWAP","symbol":"FR007_1Y","sysUpdateDate":20200106,"sysUpdateTime":110333005,"tickSize":4,"time":1547085600000,"type":"IrsPvtOdm","updateType":"1"}
text:"FR007_1Y"
text:"XSWAP私有深度行情查询结束"
</pre>

### XSWAP公有深度行情bar数据
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_IRS_PUB_ODM_BAR</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('DEFAULT', 'IrsPubOdmBar', symbols)
'''获取数据函数调用方法'''
irs.query_irs_pub_odm_bar(symbols,frequency)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>XSWAP公有深度行情bar数据:@Shibor3M的所有基础合约<br>Spread合约
,@S3M/R07的所有Basis合约<br>@FR007的所有基础合约,Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>frequency</td><td>String</td><td>频率</td><td>是</td><td>1N:1分钟,5N:5分钟,15N:15分钟,<br>30N:30分钟,1H:60分钟,<br>1D:每日,1W:每周,1M:每月</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>否</td><td>YYYYMMDD</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>否</td><td>YYYYMMDD</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>业务更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>业务更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>high</td><td>--</td></tr>
<tr><td>开盘价</td><td>Number</td><td>open</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>low</td><td>--</td></tr>
<tr><td>收盘价</td><td>Number</td><td>close</td><td>--</td></tr>
<tr><td>频率</td><td>String</td><td>frequency</td><td>--</td></tr>
<tr><td>产品类型</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Number</td><td>timespan</td><td>--</td></tr>
<tr><td>计算价格类型</td><td>String</td><td>calType</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取DEFAULT市场ODM价格第一条symbol属性值
'''
qlog.info("DEFAULT市场ODM价格")
irsPubOdmBar = irs.query_irs_pub_odm_bar(['FR007_1Y'],'1W',start_time='20190925',end_time='20191001')
if irsPubOdmBar:
    qlog.info(irsPubOdmBar[0])
    qlog.info(irsPubOdmBar[0].symbol)
qlog.info("DEFAULT市场ODM价格结束")
</pre>    
<h3>●输出结果</h3>
<pre>   
text:"查询合约为{FR007_1Y}的XSWAP公有深度行情bar数据"
text:{"calType":"deepPrice","close":2.72625,"frequency":"1W","high":2.72625,"low":2.72625,"open":2.72625,"product":"FR007_1Y","rowId":661622160116154368,"smallType":"U","source":"DEFAULT","symbol":"FR007_1Y","sysUpdateDate":20191231,"sysUpdateTime":173015317,"time":1547085600000,"timespan":1546099200000,"type":"IrsPubOdmBar","updateDate":20181230,"updateTime":0}
text:"FR007_1Y"
text:"DEFAULT市场ODM价格结束"
</pre>

### 利率曲线
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_IRS_PUB_CURVE</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('DEFAULT', 'IrsCurve', symbols)
'''获取数据函数调用方法'''
irs.get_curve(products)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>利率曲线:@Shibor3M,FR007利率曲线计算<br>不会因为数据原因报错,没有数据就会返回空的列表对象,里面的长度为0</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>业务更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>业务更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>产品类型</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Long</td><td>timespan</td><td>--</td></tr>
<tr><td>版本</td><td>String</td><td>version</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>status</td><td>--</td></tr>
<tr><td>暂无</td><td>Map</td><td>originalData</td><td>--</td></tr>
<tr><td>暂无</td><td>Map</td><td>map</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取利率曲线第一条symbol属性值
'''
qlog.info("利率曲线查询")
curve = irs.get_curve(['FR007'])
if curve:
     qlog.info(curve[0])
     qlog.info(curve[0].rates) 
qlog.info("利率曲线查询结束")
</pre>
<h3>●输出结果</h3>
<pre>
text:"利率曲线查询"
text:[{'product': 'FR007', 'rates': "'7Y': 3.3,'9M': 2.645,'5Y': 3.03,'4Y': 2.942,'3Y': 2.813,'6M': 2.427,'2Y': 2.725,'1Y': 2.686,'3M': 2.352,'10Y': 3.48", 'time': 1539829517360, 'version': 'CurveVersion-FR007-296666'}]
text:"利率曲线查询结束"
</pre>

### 贴现因子
<h3>●函数定义</h3> <p style="display: none">QUANT_HDS_DEFAULT_IRS_PUB_DISCOUNT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('DEFAULT', 'IrsDF', symbols)
'''获取数据函数调用方法'''
irs.get_discount(product,tenor,lashed,curveVersion=None)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>贴现因子:@Shibor3M,FR007产品贴现因子计算<br>不会因为数据原因报错,没有数据就会返回空的列表对象,里面的长度为0</h3>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>product</td><td>[String]</td><td>产品</td><td>是</td><td>--</td></tr>
<tr><td>tenor</td><td>String</td><td>冲击期限(无期限为tenor='null')</td><td>是</td><td>--</td></tr>
<tr><td>lashed</td><td>String</td><td>是否向上冲击</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>版本号</td><td>String</td><td>version</td><td>--</td></tr>
<tr><td>产品</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>业务更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>业务更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Number</td><td>timespan</td><td>--</td></tr>
<tr><td>贴现因子列表</td><td>list</td><td>
</td><td>列表属性[updateDate(业务更新日期),updateTime(业务更新时间),<br>version(版本),product(产品类型),<br>lashed(是否冲击关键期限),lashUp(冲击大小),<br>lashTerm(冲击关键期限),map(贴现因子),<br>preDiscount(隔夜贴现因子),timespan(业务时间戳)</td></tr>
<tr><td>备注</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取贴现因子第一条symbol属性值
'''
qlog.info("查询3M期限向下冲击贴现因子")
irsDF = irs.get_discount(['FR007'], '1Y', 'false')
if irsDF:
    qlog.info(irsDF[0])
    qlog.info(irsDF[0].product)
qlog.info("查询无期限不冲击贴现因子")
irsDF = irs.get_discount(['FR007'], 'null', 'false')
if irsDF:
    qlog.info(irsDF[0])
    qlog.info(irsDF[0].product)
qlog.info("贴现因子查询结束")
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询3M期限向下冲击贴现因子"
text:[{"allKye":"CurveVersion-Shibor3M-29651Shibor3M1575951497096","bigType":"IR","irDiscountDOS":[{"lashSize":1,"lashTerm":"1Y","lashUp":false,"lashed":true,"map":{"9M":0.978452805386073,"5Y":0.844847102503427,"4Y":0.878005010957447,"3Y":0.910084630347045,"6M":0.985899413020125,"2Y":0.941259377559037,"1Y":0.971169576102182,"3M":0.992969233732443},"preDiscount":0.999931171404368,"product":"Shibor3M","timespan":1575951497096,"updateDate":20191210,"updateTime":121817096,"version":"CurveVersion-Shibor3M-29651"}],"markType":"Shibor3M","product":"Shibor3M","rowId":653933576819048448,"smallType":"IRS","source":"DEFAULT","symbol":"Shibor3M","sysUpdateDate":20191210,"sysUpdateTime":121834212,"time":1575951497096,"timespan":1575951497096,"type":"IrsDF","updateDate":20191210,"updateTime":121817096,"version":"CurveVersion-Shibor3M-29651"},{"allKye":"CurveVersion-FR007-29651FR0071575951496846","bigType":"IR","irDiscountDOS":[{"lashSize":1,"lashTerm":"1Y","lashUp":false,"lashed":true,"map":{"9M":0.979761109710846,"5Y":0.860966974829653,"4Y":0.889862450331656,"3Y":0.918318325216926,"6M":0.986501782979327,"2Y":0.946300128853763,"1Y":0.973220609597863,"3M":0.993325803107211},"preDiscount":0.99993083811703,"product":"FR007","timespan":1575951496846,"updateDate":20191210,"updateTime":121816846,"version":"CurveVersion-FR007-29651"}],"markType":"FR007","product":"FR007","rowId":653933576827437056,"smallType":"IRS","source":"DEFAULT","symbol":"FR007","sysUpdateDate":20191210,"sysUpdateTime":121834214,"time":1575951496846,"timespan":1575951496846,"type":"IrsDF","updateDate":20191210,"updateTime":121816846,"version":"CurveVersion-FR007-29651"}]
text:"FR007"
text:"查询无期限不冲击贴现因子"
text:[{"allKye":"CurveVersion-Shibor3M-29651Shibor3M1575951497096","bigType":"IR","irDiscountDOS":[{"lashSize":1,"lashTerm":"1Y","lashUp":false,"lashed":true,"map":{"9M":0.978452805386073,"5Y":0.844847102503427,"4Y":0.878005010957447,"3Y":0.910084630347045,"6M":0.985899413020125,"2Y":0.941259377559037,"1Y":0.971169576102182,"3M":0.992969233732443},"preDiscount":0.999931171404368,"product":"Shibor3M","timespan":1575951497096,"updateDate":20191210,"updateTime":121817096,"version":"CurveVersion-Shibor3M-29651"}],"markType":"Shibor3M","product":"Shibor3M","rowId":653933576819048448,"smallType":"IRS","source":"DEFAULT","symbol":"Shibor3M","sysUpdateDate":20191210,"sysUpdateTime":121834212,"time":1575951497096,"timespan":1575951497096,"type":"IrsDF","updateDate":20191210,"updateTime":121817096,"version":"CurveVersion-Shibor3M-29651"},{"allKye":"CurveVersion-FR007-29651FR0071575951496846","bigType":"IR","irDiscountDOS":[{"lashSize":1,"lashTerm":"1Y","lashUp":false,"lashed":true,"map":{"9M":0.979761109710846,"5Y":0.860966974829653,"4Y":0.889862450331656,"3Y":0.918318325216926,"6M":0.986501782979327,"2Y":0.946300128853763,"1Y":0.973220609597863,"3M":0.993325803107211},"preDiscount":0.99993083811703,"product":"FR007","timespan":1575951496846,"updateDate":20191210,"updateTime":121816846,"version":"CurveVersion-FR007-29651"}],"markType":"FR007","product":"FR007","rowId":653933576827437056,"smallType":"IRS","source":"DEFAULT","symbol":"FR007","sysUpdateDate":20191210,"sysUpdateTime":121834214,"time":1575951496846,"timespan":1575951496846,"type":"IrsDF","updateDate":20191210,"updateTime":121816846,"version":"CurveVersion-FR007-29651"}]
text:"FR007"
text:"贴现因子查询结束"
</pre>

### 策略维度DV01
<h3>●函数定义</h3>
<pre>
'''获取数据函数调用方法'''
irs.get_dv01byId(product)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>策略维度DV01:@Shibor3M,FR007策略维度DV01计算<br>不会因为数据原因报错,没有数据就会返回空的列表对象,里面的长度为0</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>product</td><td>[String]</td><td>产品</td><td>是</td><td>必须</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>现金流版本</td><td>String</td><td>cashVersion</td><td>--</td></tr>
<tr><td>曲线版本</td><td>String</td><td>cueveVersion</td><td>--</td></tr>
<tr><td>dv013M期限值</td><td>Number</td><td>dv013m</td><td>--</td></tr>
<tr><td>dv016M期限值</td><td>Number</td><td>dv016m</td><td>--</td></tr>
<tr><td>dv019M期限值</td><td>Number</td><td>dv019m</td><td>--</td></tr>
<tr><td>dv011Y期限值</td><td>Number</td><td>dv011y</td><td>--</td></tr>
<tr><td>dv012Y期限值</td><td>Number</td><td>dv012y</td><td>--</td></tr>
<tr><td>dv013Y期限值</td><td>Number</td><td>dv013y</td><td>--</td></tr>
<tr><td>dv014Y期限值</td><td>Number</td><td>dv014y</td><td>--</td></tr>
<tr><td>dv015Y期限值</td><td>Number</td><td>dv015y</td><td>--</td></tr>
<tr><td>dv017Y期限值</td><td>Number</td><td>dv017y</td><td>--</td></tr>
<tr><td>dv0110Y期限值</td><td>Number</td><td>dv0110y</td><td>--</td></tr>
<tr><td>产品</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>时间戳</td><td>String</td><td>timespan</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3> 
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取DV01策略维度第一条symbol属性值
'''
qlog.info("DV01策略维度查询")
dv01byId = irs.get_dv01byId(['Shibor3M'])
if dv01byId:
    qlog.info(dv01byId[0])
    qlog.info(dv01byId[0].product)
qlog.info("DV01策略维度查询结束")
</pre>
<h3>●输出结果</h3>
<pre>
text:DV01策略维度查询
text:[{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"product":"FR007","timespan":20200320143508,"today":0,"todayTradeNum":0},{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"product":"Shibor3M","timespan":20200320143508,"today":0,"todayTradeNum":0}]
text:"Shibor3M"
text:DV01策略维度查询结束
</pre>

### 产品维度DV01
<h3>●函数定义</h3>
<pre>
'''获取数据函数调用方法'''
irs.get_dv01byPro(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>产品维度DV01:@Shibor3M,FR007产品维度DV01计算<br>不会因为数据原因报错,没有数据就会返回空的列表对象,里面的长度为0</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>现金流版本</td><td>String</td><td>cashVersion</td><td>--</td></tr>
<tr><td>曲线版本</td><td>String</td><td>cueveVersion</td><td>--</td></tr>
<tr><td>dv013M期限值</td><td>Number</td><td>dv013m</td><td>--</td></tr>
<tr><td>dv016M期限值</td><td>Number</td><td>dv016m</td><td>--</td></tr>
<tr><td>dv019M期限值</td><td>Number</td><td>dv019m</td><td>--</td></tr>
<tr><td>dv011Y期限值</td><td>Number</td><td>dv011y</td><td>--</td></tr>
<tr><td>dv012Y期限值</td><td>Number</td><td>dv012y</td><td>--</td></tr>
<tr><td>dv013Y期限值</td><td>Number</td><td>dv013y</td><td>--</td></tr>
<tr><td>dv014Y期限值</td><td>Number</td><td>dv014y</td><td>--</td></tr>
<tr><td>dv015Y期限值</td><td>Number</td><td>dv015y</td><td>--</td></tr>
<tr><td>dv017Y期限值</td><td>Number</td><td>dv017y</td><td>--</td></tr>
<tr><td>dv0110Y期限值</td><td>Number</td><td>dv0110y</td><td>--</td></tr>
<tr><td>产品</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>时间戳</td><td>String</td><td>timespan</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取DV01策略维度第一条symbol属性值
'''
qlog.info("DV01策略维度查询")
dv01byPro = irs.get_dv01byPro(['Shibor3M'])
if dv01byPro:
    qlog.info(dv01byPro[0])
    qlog.info(dv01byPro[0].product)
qlog.info("DV01策略维度查询结束")
</pre> 
<h3>●输出结果</h3>
<pre>  
text:DV01策略维度查询
text:[{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"product":"FR007","timespan":20200320143508,"today":0,"todayTradeNum":0},{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"product":"Shibor3M","timespan":20200320143508,"today":0,"todayTradeNum":0}]
text:"Shibor3M"
text:DV01策略维度查询结束
</pre>

### CFETS公有成交行情Spread/Basis合约分位数
<h3>●函数定义</h3>
<pre>
'''获取数据函数调用方法'''
irs.get_fractile(symbols,frequency,start_time=None,end_time=None,fractiles=None)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>CFETS公有成交行情Spread/Basis合约分位数:
{Shibor3M_6M*9M,Shibor3M_9M*1Y,Shibor3M_1Y*2Y,Shibor3M_1Y*5Y,<br>Shibor3M_2Y*5Y
,@FR007_6M*9M,FR007_9M*1Y,FR007_1Y*2Y,FR007_1Y*5Y,FR007_2Y*5Y@S3M/R07_1Y,S3M/R07_5Y}<br>不会因为数据原因报错,没有数据就会返回空的数据集合对象</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>frequency</td><td>[String]</td><td>采样频率</td><td>是</td><td>枚举:("1N":1分钟,"5N"：5分钟)默认"1M"</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>是</td><td>YYYYMMDD</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>是</td><td>YYYYMMDD</td></tr>
<tr><td>fractiles</td><td>String</td><td>分位数几分位</td><td>否</td><td>默认查询全部,支持10%-90%中的9个分位</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>合约</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>频率</td><td>String</td><td>frequency</td><td>--</td></tr>
<tr><td>分位数单位</td><td>String</td><td>fractile</td><td>--</td></tr>
<tr><td>price</td><td>Number</td><td>price</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取DV01策略维度第一条symbol属性值
'''
qlog.info("CFETS公有成交行情Spread/Basis合约分位数")
fractile = irs.get_fractile(['shibor3m_1Y*5Y'], ['1N'], start_time=20181001, end_time=20181101, fractiles=['10%', '20%'])
qlog.info(fractile)
if fractile[0] :
    qlog.info(fractile[0]['fractile'])
    qlog.info(fractile[0]['price'])
qlog.info("CFETS公有成交行情Spread/Basis合约分位数结束")
</pre> 
<h3>●输出结果</h3>
<pre>
text:"CFETS公有成交行情Spread/Basis合约分位数"
text:[{"symbol":"shibor3m_1Y*5Y","fractile":"10%","price":0.0,"frequency":"1M"}]
text:"10%"
text:0.0
text:"CFETS公有成交行情Spread/Basis合约分位数结束"
</pre>

### 产品维度损益
<h3>●函数定义</h3>
<pre>
'''获取数据函数调用方法'''
irs.get_pnlbyPro(products)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>产品维度损益:@Shibor3M,FR007产品维度损益<br>不会因为数据原因报错,没有数据就会返回空的列表对象,里面的长度为0</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>products</td><td>[String]</td><td>产品</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><tr>
<tr><td>已实现损益</td><td>Number</td><td>realizedPl</td></tr>
<tr><td>未实现损益</td><td>Number</td><td>unRealizedPl</td></tr>
<tr><td>头寸日期</td><td>Number</td><td>positionDate</td></tr>
<tr><td>货币</td><td>Number</td><td>currency</td></tr>
<tr><td>估值日期</td><td>String</td><td>pricingDate</td></tr>
<tr><td>估值时间</td><td>String</td><td>pricingTime</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.产品维度损益
2.获取查询集合结果集的第一条数据
3.获取DV01策略维度第一条symbol属性值
'''
qlog.info("产品维度损益")
pnlbyPro = irs.get_pnlbyPro(['FR007'])
if pnlbyPro:
    qlog.info(pnlbyPro[0])
    qlog.info(pnlbyPro[0].realizedPl)
qlog.info("产品维度损益结束")
</pre>  
<h3>●输出结果</h3>
<pre>
text:"产品维度损益"
text:[{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"eachQuantitySum":0.0,"eachTradedQuantitySum":0.0,"id":2409,"positionDate":20200320143508,"product":"FR007","realizedPl":0.0,"runId":3419,"unRealizedPl":0.0,"userId":"gej"},{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"eachQuantitySum":0.0,"eachTradedQuantitySum":0.0,"id":2410,"positionDate":20200320143508,"product":"Shibor3M","realizedPl":0.0,"runId":3419,"unRealizedPl":0.0,"userId":"gej"}]
text:0.0
text:"产品维度损益结束"
</pre>

### 策略维度的损益
<h3>●函数定义</h3>
<pre>
'''获取数据函数调用方法'''
irs.get_pnlbyId(products)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于计算引擎:策略维度的损益:@Shibor3M,FR007策略维度的损益<br>不会因为数据原因报错,没有数据就会返回空的列表对象,里面的长度为0</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>products</td><td>[String]</td><td>产品</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>描述</th><tr>
<tr><td>已实现损益</td><td>Number</td><td>realizedPl</td></tr>
<tr><td>未实现损益</td><td>Number</td><td>unRealizedPl</td></tr>
<tr><td>头寸日期</td><td>Number</td><td>positionDate</td></tr>
<tr><td>货币</td><td>Number</td><td>currency</td></tr>
<tr><td>估值日期</td><td>String</td><td>pricingDate</td></tr>
<tr><td>估值时间</td><td>String</td><td>pricingTime</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.策略维度的损益
2.获取查询集合结果集的第一条数据
3.获取DV01策略维度第一条symbol属性值
'''
qlog.info("策略维度的损益")
pnlbyId = irs.get_pnlbyId(['Shibor3M'])
if pnlbyId:
    qlog.info(pnlbyId[0])
    qlog.info(pnlbyId[0].realizedPl)
qlog.info("策略维度的损益结束")
</pre>  
<h3>●输出结果</h3>
<pre>
text:"策略维度的损益"
text:[{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"eachQuantitySum":0.0,"eachTradedQuantitySum":0.0,"id":2409,"positionDate":20200320143508,"product":"FR007","realizedPl":0.0,"runId":3419,"unRealizedPl":0.0,"userId":"gej"},{"dv0110y":0.0,"dv011y":0.0,"dv012y":0.0,"dv013m":0.0,"dv013y":0.0,"dv014y":0.0,"dv015y":0.0,"dv016m":0.0,"dv017y":0.0,"dv019m":0.0,"eachQuantitySum":0.0,"eachTradedQuantitySum":0.0,"id":2410,"positionDate":20200320143508,"product":"Shibor3M","realizedPl":0.0,"runId":3419,"unRealizedPl":0.0,"userId":"gej"}]
text:0.0
text:"策略维度的损益结束"
</pre>

### XSWAP公有成交行情bar数据
<h3>●函数定义</h3> 
<pre>
'''订阅数据接入(init方法中使用)'''
irs.init_cache('DEFAULT', 'IrsPubTraBar', symbols)
'''获取数据函数调用方法'''
irs.query_irs_pub_tra_bar(symbols,frequency)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎
<br>XSWAP公有成交行情bar数据计算@Shibor3M的所有基础合约,Spread合约
<br>@S3M/R07的所有Basis合约,@FR007的所有基础合约,Spread合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>frequency</td><td>String</td><td>频率</td><td>是</td><td>1N:1分钟,5N:5分钟</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>否</td><td>YYYYMMDD</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>否</td><td>YYYYMMDD</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>业务更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>业务更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>high</td><td>--</td></tr>
<tr><td>开盘价</td><td>Number</td><td>open</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>low</td><td>--</td></tr>
<tr><td>收盘价</td><td>Number</td><td>close</td><td>--</td></tr>
<tr><td>频率</td><td>String</td><td>frequency</td><td>--</td></tr>
<tr><td>产品类型</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Number</td><td>timespan</td><td>--</td></tr>
<tr><td>计算价格类型</td><td>String</td><td>calType</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>  
<pre>
'''
1.XSWAP公有成交行情bar数据
2.获取查询集合结果集的第一条数据
3.XSWAP公有成交行情bar数据成交价第一条symbol属性值
'''
qlog.info("查询合约为{Shibor3M}的XSWAP公有深度行情bar数据")
redata = irs.query_irs_pub_tra_bar(['Shibor3M'],'1W',start_time='20190925',end_time='20191001')
if irsPubTraBar:
    qlog.info(irsPubTraBar[0])
    qlog.info(irsPubTraBar[0].symbol)
qlog.info("DEFAULT市场ODM价格结束")
</pre>    
<h3>●输出结果</h3>
<pre>   
text:"查询合约为{Shibor3M}的XSWAP公有深度行情bar数据"
text:{"calType":"deepPrice","close":2.72625,"frequency":"1W","high":2.72625,"low":2.72625,"open":2.72625,"product":"FR007_1Y","rowId":661622160116154368,"smallType":"U","source":"DEFAULT","symbol":"FR007_1Y","sysUpdateDate":20191231,"sysUpdateTime":173015317,"time":1547085600000,"timespan":1546099200000,"type":"IrsPubOdmBar","updateDate":20181230,"updateTime":0}
text:"Shibor3M"
text:"DEFAULT市场ODM价格结束"
</pre>
兴业量化系统API历史行情数据    

---

### 模块一:BOND数据查询(模块名称:bond)
### 1.QB成交行情查询 QUANT_HDS_QB_QBOND_PUB_TRADE

- 函数定义
    
    
    bond.query_qb_bond_pub_tra(symbols,start_time=None,end_time=None)

- 函数定义

|输入参数名|数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|上游行情唯一标识|
|源行情编号|String|quoteId||
|经纪商类型|String|brokerType|TP_BBO2:国利,ICAP_BBO2:国际,BGC_BBO2:中诚,TJXT_BBO2:信唐,PATR_BBO2:平安|
|发行市场|String|marketType|CIB：银行间/SSE：上交所/SZE：深交所|
|价格类型|String|priceType||
|扩展字段|String|text||
|报价类型|String|side|X-taken,Y-given,Z-trade|
|结算日期|String|settlDate||
|交易是否有效|String|tradeStatus|N、R有效，C无效（删除或撤销）|
|清算速度|String|clearSpeed|1-"T+0",2-"T+1"|
|成交状态|String|dealStatus||
|债券代码|String|bondCode||
|债券名称|String|bondName||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销||
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|收益率|Number|yield||
|净价|Number|netPrice||
|成交量|Number|tradeAmt||
|全价|Number|fullPrice||
|原始价格|Number|originalPrice||
|到期收益率|Number|marturityRate||
|行权收益率|Number|exerciseRate||
|利差|Number|spread||
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||




- API执行范例

    

        qlog.info("查询API执行")
        # 订阅数据日志输出
        qlog.info_to_str(data)
        # 查询API执行
        pubTra = bond.query_qb_bond_pub_tra(['011800780.IB'], start_time='20190628', end_time='20190629')
        # 获取查询集合结果集的第一条数据
        qlog.info(pubTra[0])
        # 获取数据对象的symbol属性值
        qlog.info(pubTra[0].symbol)
        qlog.info("QB成交行情操作完成")


- 输出结果(以列表集合的对象返回)

    
    [{"bigType":"null","bondCode":"011800780","bondName":"18CP010","brokerType":"CBBJB","clearSpeed":"null","dealStatus":"1","eic":"QBondPubTra#011800780.IB=QB","fullPrice":102.8896,"marketType":"CIB","originalPrice":4.0,"priceId":"1907111041380108563","priceState":"1","priceType":"null","quoteId":"BGC0118007802451380000","quoteUnit":"1","receivingDate":103930871,"receivingTime":0,"rowId":598825330358091776,"sendDate":20190711,"sendTime":104138835,"sendingDate":20190711,"sendingTime":24138636,"settlDate":"null","side":"Y","smallType":"null","source":"QB","symbol":"011800780.IB","sysUpdateDate":20190711,"sysUpdateTime":103804394,"text":"QBondPubTra","tickSize":4,"time":1562784098636,"tradeAmt":0.0,"tradeStatus":"R","type":"QUANT_HDS_QB_QBOND_PUB_TRADE","updateDate":0,"updateTime":0,"updateType":"1","yield":4.0007}]


### 2.CMDS成交行情查询 QUANT_HDS_CMDS_XBOND_PUB_TRADE

- 函数定义

    
    bond.query_cmds_bond_pub_tra(symbols,start_time=None,end_time=None)
        
- 函数定义

|输入参数名|数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|上游行情唯一标识|
|源行情编号|String|quoteId|来源行情唯一标识|
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|市场标识|String|marketIndicator|4-现券买卖|
|交易方式|String|tradeType|3-匿名点击|
|清算速度|String|clearSpeed|1-T+0,2-T+1|
|最新成交方向|Number|latestSide|1买 2卖|
|最新成交净价|Number|latestNetPrice||
|最新成交到期收益率|Number|latestYield||
|最高成交净价|Number|highestNetPrice||
|最高成交到期收益率|Number|highestYield||
|最低成交净价|Number|lowestNetPrice||
|最低成交到期收益率|Number|lowestYield||
|开盘价|Number|openingRate||
|开盘到期收益率|Number|openingYield||
|前开盘净价|Number|preOpeningNetPrice||
|前开盘到期收益率|Number|preOpeningYield||
|加权平均净价|Number|avgNetPrice||
|加权平均到期收益率|Number|avgYield||
|前加权平均净价|Number|preAvgNetPrice||
|前加权平均到期收益率|Number|preAvgYield||
|前收盘净价|Number|preCloseNetPrice||
|前收盘到期收益率|Number|preCloseYield||
|净价涨跌幅|Number|netPriceBP||
|收益率涨跌|Number|yieldBP||
|成交量|Number|tradeAmt||
|行情日期|Number|updateDate||
|行情时间|Number|updateTime||
|债券代码|String|bondCode||
|债券名称|String|bondName||
|是否是上市前债券|String|preMarketBondIndicator||


- API执行范例



    qlog.info("查询CMDS成交行情查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = bond.query_cmds_bond_pub_tra(['100007'], start_time='20190628', end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("CMDS成交行情操作完成")


- 输出结果(以队列的集合,对象结果集返回)


    [{"avgNetPrice":91.8,"avgYield":7.7819,"bigType":"null","bondCode":"100007","bondName":"01国开14","clearSpeed":"2","eic":"XBondPubTra#100007=CMDS","highestNetPrice":99.0,"highestYield":8.5604,"latestNetPrice":99.0,"latestSide":1,"latestYield":4.668,"lowestNetPrice":90.0,"lowestYield":4.668,"marketIndicator":"null","netPriceBP":0.0,"openingRate":90.0,"openingYield":8.5604,"preAvgNetPrice":77.7819,"preAvgYield":75.7819,"preCloseNetPrice":75.7819,"preCloseYield":75.7819,"preMarketBondIndicator":"Y","preOpeningNetPrice":91.8,"preOpeningYield":8.5604,"priceId":"1907242012400100022","priceState":"3","quoteId":"null","quoteUnit":"1","receivingDate":20190724,"receivingTime":201240548,"rowId":603715533266747392,"sendDate":20190724,"sendTime":223000134,"sendingDate":20190222,"sendingTime":171016610,"smallType":"null","source":"CMDS","symbol":"100007","sysUpdateDate":20190724,"sysUpdateTime":222959623,"tickSize":4,"time":1550826616610,"tradeAmt":2.5E7,"tradeType":"null","transactTime":"20190222-17:09:46.110","type":"XBondPubTra","updateDate":20190222,"updateTime":170946110,"updateType":"1","yieldBP":26.12}]


### 3.QB最优报价查询 QUANT_HDS_QB_QBOND_PUB_TOP_BOOK

- 函数定义


    bond.query_qb_bond_pub_top(symbols,start_time=None,end_time=None)

- 函数定义

|输入参数名|数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|


- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|上游行情唯一标识|
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|买入量|Number|bidAmt||
|卖出量|Number|askAmt||
|买价|Number|bid||
|卖价|Number|ask||
|买入到期收益率|Number|bidYield||
|卖出到期收益率|Number|askYield||
|买入行权收益率|Number|bidExerciseYield||
|卖出行权收益率|Number|askExerciseYield||
|经纪商类型|String|brokerType|TP_BBO2:国利,ICAP_BBO2:国际,BGC_BBO2:中诚,TJXT_BBO2:信唐,PATR_BBO2:平安||
|发行市场|String|marketType|CIB：银行间/SSE：上交所/SZE：深交所||
|买价编号BIDID|String|bidId||
|卖价编号ASKID|String|askId||
|买入净价|Number|bidNetPrice||
|卖出净价|Number|askNetPrice||
|买入是否可议价|String|bidBargainFlag|1表示可议价, 2表示更可议价, 无值表示不可议价|
|买出是否可议价|String|askBargainFlag|1表示可议价, 2表示更可议价, 无值表示不可议价|
|买入收益率类型|String|bidExerciseFlag|0表示行权收益率, 1或空：到期收益率|
|卖出收益率类型|String|askExerciseFlag|0表示行权收益率, 1或空：到期收益率|
|买价订单类型|String|bidRelationFlag|1表示OCO, 2表示打包, 无值表示无OCO无打包|
|卖价订单类型|String|askRelationFlag|1表示OCO, 2表示打包, 无值表示无OCO无打包|
|卖价注释|String|askComment||
|买价注释|String|bidComment||
|bidAmt分量|String|bidAmtPieces||
|askAmt分量|String|askAmtPieces||
|买入价格类型|String|bidPriceType||
|卖出价格类型|String|askPriceType||
|买入全价|Number|bidFullPrice||
|卖出全价|Number|askFullPrice||
|买入利差|Number|bidSpread||
|卖出利差|Number|askSpread||
|债券代码|String|bondCode||
|债券名称|String|bondName||


- API执行范例



    qlog.info("QB最优报价查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = bond.query_qb_bond_pub_top(['011801455.IB'], start_time='20190628', end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("QB最优报价操作完成")


- 输出结果(以队列的集合,对象结果集返回)


    [{"ask":3.37,"askAmt":2000.0,"askAmtPieces":"null","askBargainFlag":"1","askComment":"2000(*)","askExerciseFlag":"1","askId":"null","askNetPrice":100.0286,"askPriceType":"3","askRelationFlag":"null","bid":0.0,"bidAmt":0.0,"bidAmtPieces":"null","bidBargainFlag":"null","bidComment":"null","bidExerciseFlag":"null","bidId":"null","bidNetPrice":0.0,"bidPriceType":"null","bidRelationFlag":"null","bigType":"null","bondCode":"011801455","bondName":"18华菱钢铁SCP003","brokerType":"TP_BBO2","eic":"QBondPubTop#011801455.IB=QB","marketType":"CIB","priceId":"1907181439410118065","priceState":"2","quoteUnit":"1","receivingDate":20190718,"receivingTime":143729515,"rowId":601422851966238720,"sendDate":20190718,"sendTime":143941401,"sendingDate":20190718,"sendingTime":63941423,"smallType":"null","source":"QB","symbol":"011801455.IB","sysUpdateDate":20190718,"sysUpdateTime":143941805,"text":"null","tickSize":4,"time":1563403181423,"type":"QBondPubTop","updateDate":20190718,"updateTime":63941423,"updateType":"1"}]


### 4.CFFEX中金所的国债期货行情查询 QUANT_HDS_CFFEX_FUTURES_PUB_ODM_DEPT

- 函数定义


    bond.query_fut_pub_odm(symbols,start_time=None,end_time=None)


- 函数定义

|输入参数名|数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId||
|源行情编号|String|quoteId||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|交易日|String|tradingDay||
|结算组代码|String|settlementGroupId||
|结算编号|String|settlementId||
|最新价|Number|lastPrice||
|昨结算|Number|preSettlementPrice||
|昨收盘|Number|preClosePrice||
|昨持仓量|Number|preOpenInterest||
|今开盘|Number|openPrice||
|最高价|Number|highestPrice||
|最低价|Number|lowestPrice||
|数量|Number|amt||
|成交金额|Number|tradeAmt||
|持仓量|Number|openInterest||
|今收盘|Number|closePrice||
|今结算|Number|settlementPrice||
|涨停板价|Number|upperLimitPrice||
|跌停板价|Number|lowerLimitPrice||
|昨虚实度|Number|preDelta||
|今虚实度|Number|currDelta||
|最后修改时间|Number|modifyTime||
|最后修改毫秒|Number|updateMillisec||
|结构类|Object|cbondPubOdmRateDOS|方向:side,数量:amt,层级:level,价格:price;list[side,amt,level,price]|


- API执行范例



    qlog.info("CFFEX中金所的国债期货行情查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = bond.query_fut_pub_odm(['TS1909'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    # 获取数据对象的symbolcbondPubOdmRateDOS属性值
    qlog.info(redata[0].cbondPubOdmRateDOS)
    # 获取数据对象的symbolcbondPubOdmRateDOS第一个价格集合
    qlog.info(redata[0].cbondPubOdmRateDOS[0])
    # 获取数据对象的symbolcbondPubOdmRateDOS第一个价格集合金额
    qlog.info(redata[0].cbondPubOdmRateDOS[0].amt)
    qlog.info("CFFEX中金所的国债期货行情操作完成")


- 输出结果(以队列的集合,对象结果集返回)


    [{"amt":1000.0,"bigType":"null","cbondPubOdmRateDOS":[{"amt":6.0,"level":1,"price":99.926,"side":1},{"amt":7.0,"level":2,"price":99.905,"side":1},{"amt":8.0,"level":3,"price":99.906,"side":1},{"amt":9.0,"level":4,"price":99.926,"side":1},{"amt":10.0,"level":5,"price":99.862,"side":1},{"amt":6.0,"level":1,"price":99.98,"side":2},{"amt":7.0,"level":2,"price":100.046,"side":2},{"amt":8.0,"level":3,"price":100.037,"side":2},{"amt":9.0,"level":4,"price":100.079,"side":2},{"amt":10.0,"level":5,"price":99.988,"side":2}],"closePrice":100.174,"currDelta":0.0,"eic":"GbFutPubOdm#TS1909=CFFEX","highestPrice":100.136,"lastPrice":100.111,"lowerLimitPrice":99.17,"lowestPrice":100.233,"modifyTime":51906652,"openInterest":2406.9977,"openPrice":99.587,"preClosePrice":99.973,"preDelta":0.0,"preOpenInterest":2405.0005,"preSettlementPrice":99.966,"priceId":"1907182118110106863","priceState":"1","quoteId":"null","quoteUnit":"1","receivingDate":20190804,"receivingTime":140950856,"rowId":601523136357203968,"sendDate":20190718,"sendTime":211811465,"sendingDate":20190718,"sendingTime":211806288,"settlementGroupId":"123456","settlementId":"654321","settlementPrice":99.919,"smallType":"null","source":"CFFEX","symbol":"TS1909","sysUpdateDate":20190718,"sysUpdateTime":211811467,"tickSize":4,"time":1563455886288,"tradeAmt":2000.0,"tradingDay":"20190708","type":"GbFutPubOdm","updateMillisec":652,"updateType":"1","upperLimitPrice":100.398}]

---
### 模块二:REPO数据查询(模块名称:repo)
### 1.repo基准数据查询 QUANT_HDS_CMDS_FREPO_BENCHMARK_QUOTA

- 函数定义


    repo.query_repo_bm(symbols,start_time=None,end_time=None)


- 函数定义

|输入参数名 | 数据类型|描述|是否必填|备注
|---|---|---|---|---|
|symbols|[String]|产品|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|期限|String|tenor|1,2-7|
|利率|Number|fixedBackRate||
|更新状态|String|updateType||


- API执行范例



    qlog.info("repo基准数据查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = repo.query_repo_bm(['CNY_Repo_FDR001'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("repo基准数据操作完成")


- 输出结果(以队列的集合,对象结果集返回)


    [{"bigType":"null","eic":"FRepoBM#CNY_Repo_FDR001=CMDS","fixedRepo":0.5427,"priceState":"1","quoteUnit":"1","receivingDate":20190718,"receivingTime":94112702,"rowId":601347755532615680,"sendDate":20190718,"sendTime":94117418,"sendingDate":20190718,"sendingTime":94112324,"smallType":"null","source":"CMDS","symbol":"CNY_Repo_FDR001","sysUpdateDate":20190718,"sysUpdateTime":94117420,"tenor":"2-7","tickSize":4,"time":1563414072324,"type":"FRepoBM","updateDate":20190718,"updateTime":94111747,"updateType":"1"}]



### 2.CMDS质押式回购总成交行情查询 QUANT_HDS_CMDS_REPO_PUB_TRADE

- 函数定义:


    repo.query_repo_pub_tra(symbols,start_time=None,end_time=None)


- 函数定义

|输入参数名|数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|产品|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|上游行情唯一标识|
|源行情编号|String|quoteId||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|2-异常,3-正常|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|市场标识|String|marketIndicator|4-现券买卖|
|交易方式|String|tradeType|3-匿名点击|
|最新利率|Number|latestRate||
|开盘利率|Number|openingRate||
|最高利率|Number|highestRate||
|最低利率|Number|lowestRate||
|成交量|Number|tradeAmt||
|成交订单笔数|Number|tradeOrderAmt||
|收盘利率|Number|closingRate||
|收盘利率|Number|PreClosingRate||
|加权平均利率|Number|avgRate||
|前加权平均利率|Number|preAvgRate||
|平均回购期限|String|avgTenor||
|加权平均利率（利率债）|Number|avgIRSRate||



- API执行范例

    

    qlog.info("CMDS质押式回购总成交行情查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = repo.query_repo_pub_tra(['F07_5Y'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("CMDS质押式回购总成交行情操作完成")


- 输出结果(以队列的集合,对象结果集返回)


    [{"avgIRSRate":0.01,"avgRate":0.0094,"avgTenor":"AvgTerm","bigType":"null","closingRate":0.0092,"eic":"RepoPubTra#F07_5Y=CMDS","highestRate":0.2471,"latestRate":0.3187,"lowestRate":0.33,"marketIndicator":"9","openingRate":0.0099,"preAvgRate":0.0021,"preClosingRate":0.0021,"priceId":"1907242221510100007","priceState":"1","quoteId":"null","quoteUnit":"1","receivingDate":20190724,"receivingTime":222151029,"rowId":603713483606851584,"sendDate":20190724,"sendTime":222151423,"sendingDate":20190724,"sendingTime":222150884,"smallType":"null","source":"CMDS","symbol":"F07_5Y","sysUpdateDate":20190724,"sysUpdateTime":222150946,"tickSize":4,"time":1563978110884,"tradeAmt":1000.0,"tradeOrderAmt":1000,"tradeType":"3","transactTime":"20190724-22:21:51.029","type":"RepoPubTra","updateDate":20190724,"updateTime":222150744,"updateType":"1"}]

---
### 模块三:IBOR数据查询(模块名称:ibor)
### 1.shibor 基准数据查询 QUANT_HDS_CMDS_BIBOR_BENCHMARK_QUOTA

- 函数定义:


    ibor.query_ibor_bm(symbols,start_time=None,end_time=None)

- 函数定义


|输入参数名| 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|产品|是|输入为None时，查询全部|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|


- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|上游行情唯一标识|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|shibor值（%）|Number|shibor||
|涨跌|Number|shiborBP||
|期限|String|tenor|O/N,3M|
|更新状态|String|updateType||


- API执行范例
    
    

    qlog.info("shibor基准数据查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = ibor.query_ibor_bm(['CNY_Shibor_1Y'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("shibor基准数据操作完成")


- 输出结果(以队列的集合,对象结果集返回)

    
    [{"bigType":"null","eic":"BIborBM#CNY_Shibor_1Y=CMDS","priceState":"2","quoteUnit":"100","receivingDate":20190730,"receivingTime":112450005,"rowId":605722468048437252,"sendDate":20190730,"sendTime":112450132,"sendingDate":20190730,"sendingTime":112449789,"shibor":2.803,"shiborBP":11.24,"smallType":"null","source":"CMDS","symbol":"CNY_Shibor_1Y","sysUpdateDate":20190730,"sysUpdateTime":112450162,"tenor":"1Y","tickSize":4,"time":1564457089789,"type":"BIborBM","updateDate":20190730,"updateTime":112449624,"updateType":"1"}]


### 2.shibor报价行情查询(CMDS) QUANT_HDS_CMDS_IBOR_QUOTE_LINE

- 函数定义:


    ibor.query_ibor_quote(symbols,start_time=None,end_time=None)


- 函数定义

|输入参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|价格状态|String|priceState|0-异常,1-正常|
|shibor|值（%）|Number|shibor||
|报价行报价|String|quoteLineId||
|报价行简称|String|quoteLine||
|更新状态|String|updateType|1-价格更新,2-价格撤销|
|期限|String|tenor||


- API执行范例

    

    qlog.info("shibor报价行情查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = ibor.query_ibor_quote(['CNY_Shibor_1M'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("shibor报价行情操作完成")


- 输出结果(以队列的集合,对象结果集返回)

    
    [{"bigType":"null","eic":"IborQuote#CNY_Shibor_1M=CMDS","priceState":"1","quoteLine":"quoteLine","quoteLineId":"quoteLineID","quoteUnit":"1","receivingDate":20190724,"receivingTime":222151034,"rowId":603713484902891520,"sendDate":20190724,"sendTime":222151731,"sendingDate":20190724,"sendingTime":222150797,"shibor":3.26,"smallType":"null","source":"CMDS","symbol":"CNY_Shibor_1M","sysUpdateDate":20190724,"sysUpdateTime":222151255,"tenor":"1M","tickSize":4,"time":1563978110797,"transactTime":"20190724-22:21:51","type":"IborQuote","updateDate":20190724,"updateTime":222150246,"updateType":"1"}]


---
### 模块四:IRS数据查询(模块名称:irs)
### 1.CMDS公有成交行情查询 QUANT_HDS_CMDS_IRS_PUB_TRADE

- 函数定义:

    
    irs.query_irs_pub_tra(symbols,start_time=None,end_time=None)

- 函数定义

|输入参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|系统行情唯一标识|
|源行情编号|String|quoteId||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|最新利率|Number|latestRate||
|最新成交量|Number|latestAmt||
|开盘利率|Number|openingRate||
|最高利率|Number|highestRate||
|最低利率|Number|lowestRate||
|盘中参考价|Number|referPrice||
|当日累计成交量|Number|cumulTradeAmt||
|市场标识|String|marketIndicator|4-现券买卖|
|交易方式|String|tradeType|3-匿名点击|


- API执行范例



    qlog.info("CMDS公有成交行情查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pub_tra(['R07_5Y'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("CMDS公有成交行情查询")


- 输出结果(以队列的集合,对象结果集返回)

    
    [{'bigType': 'IR', 'cumulTradeAmt': 30000.0, 'highestRate': 0.5122, 'latestAmt': 20000.0, 'latestRate': 0.3223, 'lowestRate': 0.2202, 'markType': 'R07_5Y', 'marketIndicator': 2, 'openingRate': 0.3178, 'priceId': 'xswapDeal', 'priceState': 1, 'quoteUnit': 1, 'receivingDate': 20190528, 'receivingTime': 201857155, 'referPrice': 0.0, 'rowId': 583026453373779972, 'sendDate': 20190528, 'sendTime': 201856338, 'sendingDate': 20190528, 'sendingTime': 201856852, 'smallType': 'BASIS', 'source': 'CMDS', 'symbol': 'R07_5Y', 'sysUpdateDate': 20190528, 'sysUpdateTime': 201858, 'tickSize': 4, 'time': 1539829517360, 'tradeType': 3, 'type': 'IrsPubTra', 'updateDate': 20190528, 'updateTime': 201855904, 'updateType': 1, 'Unnamed: 33': nan}]



### 2.CMDS公有最优价查询 QUANT_HDS_CMDS_XBOND_PUB_TOP_BOOK
- 函数定义:

    
    irs.query_irs_pub_top(symbols,start_time=None,end_time=None)

- 函数定义

|输入参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|系统行情唯一标识|
|源行情编号|String|quoteId||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|买价|Number|bid||
|卖价|Number|ask||
|买价总量|Number|bidAmt||
|卖价总量|Number|askAmt||
|行情更新日期|Number|updateDate||
|行情更新时间|Number|updateTime||
|市场标识|String|marketIndicator|4-现券买卖|
|交易方式|String|tradeType|3-匿名点击|


- API执行范例



    qlog.info("CMDS公有最优价查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pub_top(['FR007_10Y'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("CMDS公有最优价查询结束")


- 输出结果(以队列的集合,对象结果集返回)


    [{ 'ask': 0.44741629236440095, 'askAmt': 2000.0, 'bid': 0.4464545632330571, 'bidAmt': 0.0, 'bigType': 'IR', 'markType': 'F07_1Y', 'marketIndicator': 2, 'priceId': 'xswapDeal', 'priceState': 1, 'quoteUnit': 1, 'receivingDate': 20190528, 'receivingTime': 201917184, 'rowId': 583026532415438858, 'sendDate': 20190528, 'sendTime': 201916319, 'sendingDate': 20190528, 'sendingTime': 201916880, 'smallType': 'IRS', 'source': 'CMDS', 'symbol': 'F07_1Y', 'sysUpdateDate': 20190528, 'sysUpdateTime': 201917, 'tickSize': 4, 'time': 1539829517360, 'tradeType': 3, 'type': 'IrsPubTop', 'updateDate': 20190528, 'updateTime': 201916250, 'updateType': 1, 'Unnamed: 30': nan}]


### 3.XSWAP私有最优价查询 QUANT_HDS_XSWAP_IRS_PVT_TOP_BOOK

- 函数定义


    irs.query_irs_pvt_top(symbols,start_time=None,end_time=None)

- 函数定义

|输入参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|


- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|系统行情唯一标识|
|源行情编号|String|quoteId||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|买价|Number|bid||
|卖价|Number|ask||
|买价总量|Number|bidAmt||
|卖价总量|Number|askAmt||
|买价可成交量|Number|bidTradeAmt||
|卖价可成交量|Number|askTradeAmt||
|买价订单总数|Number|bidOrderAmt||
|卖价订单总数|Number|askOrderAmt||
|买价源更新牌价日期|Number|bidUpdateDate||
|买价源更新牌价时间|Number|bidUpdateTime||
|卖价源更新牌价日期|Number|askUpdateDate||
|卖价源更新牌价时间|Number|askUpdateTime||
|中心操作标识|String|operReportId||
|订阅类型|String|mdBookType||



- API执行范例

    

    qlog.info("XSWAP私有最优价查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pvt_top(['FR007_10Y'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("XSWAP私有最优价查询结束")

- 输出结果(以队列的集合,对象结果集返回)
    
    
    [{ 'ask': 8.8591, 'askAmt': 1800.0, 'askOrderAmt': 0, 'askTradeAmt': 2000.0, 'askUpdateDate': 20180528, 'askUpdateTime': 0, 'bid': 8.847000000000001, 'bidAmt': 800.0, 'bidOrderAmt': 0, 'bidTradeAmt': 1000.0, 'bidUpdateDate': 20180528, 'bidUpdateTime': 0, 'bigType': 'IR', 'markType': 'Shibor3M_6M', 'mdBookType': 1, 'operReportId': 'operReportID', 'priceId': 'operReportID', 'priceState': 1, 'quoteUnit': 1, 'receivingDate': 20190528, 'receivingTime': 212754304, 'rowId': 583046441765699584, 'sendDate': 20190528, 'sendTime': 213227444, 'sendingDate': 20190617, 'sendingTime': 155027214, 'smallType': 'IRS', 'source': 'XSWAP', 'symbol': 'Shibor3M_6M', 'sysUpdateDate': 20190528, 'sysUpdateTime': 213824, 'tickSize': 4, 'time': 1539829517360, 'type': 'IrsPvtTop', 'updateType': 1, 'Unnamed: 36': nan}]
    
### 4.市场综合报价查询(QB) QUANT_HDS_QB_IRS_PUB_QUOTE_DATA
    
- 函数定义


    irs.query_irs_pub_quote(symbols,start_time=None,end_time=None)

- 函数定义

|输入参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|


- 输出参数解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|系统行情唯一标识|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|买入量|Number|bidAmt||
|卖出量|Number|askAmt||
|买价|Number|bid||
|卖价|Number|ask||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|经纪商类型|String|brokerType||
|利率类型|String|irType||
|交易期限|String|tradeTenor||
|交易方式|String|tradeMode||


- API执行范例

    

    qlog.info("市场综合报价查询(QB)")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pub_quote(['CNY_Shibor_3M'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("市场综合报价查询(QB)结束")

- 输出结果(以队列的集合,对象结果集返回)
    
    
    [{ 'ask': 3.1409, 'askAmt': 1000.0, 'bid': 3.1345, 'bidAmt': 1000.0, 'bigType': 'IR', 'brokerType': 'IRS', 'irType': 'shibor', 'markType': 'CNY_shibor_20190411', 'priceState': 0, 'quoteUnit': 0, 'receivingDate': 20190528, 'receivingTime': 201538142, 'rowId': 583025611132370944, 'sendDate': 20190528, 'sendTime': 201537337, 'sendingDate': 20190617, 'sendingTime': 53622637, 'smallType': 'IRS', 'source': 'QB', 'symbol': 'CNY_shibor_20190411', 'sysUpdateDate': 20190528, 'sysUpdateTime': 201537, 'tickSize': 4, 'time': 1539829517360, 'tradeMode': 'Outright', 'tradeTenor': 20190411, 'type': 'IrsPubQuote', 'updateDate': 0, 'updateTime': 0, 'updateType': 0, 'Unnamed: 31': nan}]


### 5.CMDS公有深度行情查询 QUANT_HDS_CMDS_IRS_PUB_ODM_DEPT
- 函数定义


    irs.query_irs_pub_odm(symbols,start_time=None,end_time=None)

- 函数定义

|参数名称 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出参数解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId|系统行情唯一标识|
|源行情编号|String|quoteId||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|价源更新牌价日期|Number|updateDate||
|价源更新牌价时间|Number|updateTime||
|市场标识|String|marketIndicator|4-现券买卖|
|交易方式|String|tradeType|3-匿名点击|
|结构类|Object|pubOdmRateDOS|方向:side (1买 2卖),数量:amt,层级:level,价格:price;list[side,amt,level,price]|


- API执行范例

    

    qlog.info("CMDS公有深度行情查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pub_odm(['FR007_10Y'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("CMDS公有深度行情查询结束")



- 输出结果(以队列的集合,对象结果集返回)

    
    [{ 'bigType': 'IR', 'markType': 'FR007_10Y', 'marketIndicator': 2, 'priceId': 'xswapDeal', 'priceState': 1, 'pubOdmRateDOS': '[{"amt":128.0,"level":1,"price":0.1731,"side":2},{"amt":933.0,"level":1,"price":0.1561,"side":1},{"amt":697.0,"level":2,"price":0.1709,"side":2},{"amt":216.0,"level":2,"price":0.1563,"side":1}]', 'quoteUnit': 1, 'receivingDate': 20190528, 'receivingTime': 201546902, 'rowId': 5.83026e+17, 'sendDate': 20190528, 'sendTime': 201546119, 'sendingDate': 20190528, 'sendingTime': 201545993, 'smallType': 'IRS', 'source': 'CMDS', 'symbol': 'FR007_10Y', 'sysUpdateDate': 20190528, 'sysUpdateTime': 201641, 'tickSize': 4, 'time': 1539829517360, 'tradeType': 3, 'type': 'IrsPubOdm', 'updateDate': 20190528, 'updateTime': 201545265, 'updateType': 1}]


### 6.XSWAP私有深度行情查询 QUANT_HDS_XSWAP_IRS_PVT_ODM_DEPT

- 函数定义


    irs.query_irs_pvt_odm(symbols,start_time=None,end_time=None)

- 函数定义

|参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
symbols|[String]|合约|是|需要输入对应合约|
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出参数解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|行情编号|String|priceId||
|源行情编号|String|quoteId||
|价格更新类型|String|updateType|1-价格更新,2-价格撤销|
|价格状态|String|priceState|1-正常,2-价格存疑,3-价格超时|
|中心操作标识|String|operReportId||
|订阅类型|String|mdBookType||
|结构类|Object|pvtOdmRateDOS|方向:side (1买 2卖),数量:amt,层级:level,价格:price;list[side,amt,level,price]|

- API执行范例

    

    qlog.info("XSWAP私有深度行情查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pvt_odm(['FR007_1Y'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("XSWAP私有深度行情查询结束")

- 输出结果(以队列的集合,对象结果集返回)

    
    [{ 'bigType': 'IR', 'markType': 'Shibor3M_6M', 'mdBookType': 2, 'priceId': 'operReportID', 'priceState': 1, 'pvtOdmRateDOS': '[{"amt":1000.0,"level":1,"price":8.8743,"side":2,"tradeAmt":2000.0,"updateDate":20180528,"updateTime":201549375},{"amt":1000.0,"level":1,"price":8.8665,"side":1,"tradeAmt":2000.0,"updateDate":20180528,"updateTime":201549375}]', 'quoteUnit': 1, 'receivingDate': 20190528, 'receivingTime': 201539372, 'rowId': 5.83026e+17, 'sendDate': 20190528, 'sendTime': 201548556, 'sendingDate': 20190512, 'sendingTime': 34753874, 'smallType': 'IRS', 'source': 'XSWAP', 'symbol': 'Shibor3M_6M', 'sysUpdateDate': 20190528, 'sysUpdateTime': 201648, 'tickSize': 4, 'time': 1539829517360, 'type': 'IrsPvtOdm'}]



### 7.DEFAULT市场行情成交价bar数据查询 QUANT_HDS_DEFAULT_IRS_PUB_TRADE_BAR
- 函数定义


    irs.query_irs_pub_tra_bar(symbols,frequency=None,start_time=None,end_time=None)

- 函数定义

|参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|需要输入对应合约|
|frequency|String|频率|否||
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出参数解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|业务更新日期|Number|updateDate||
|业务更新时间|Number|updateTime||
|最高价|Number|high||
|开盘价|Number|open||
|最低价|Number|low||
|收盘价|Number|close||
|频率|String|frequency||
|产品类型|String|product||
|业务时间戳|Number|timespan||
|计算价格类型|String|calType||


- API执行范例
    
    

    qlog.info("DEFAULT市场行情成交价")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pub_tra_bar(['FR007_3Y'],frequency='1N',start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("DEFAULT市场行情成交价结束")

- 输出结果(以队列的集合,对象结果集返回)
    
    
    [{ 'bigType': 'IR', 'close': 0.165, 'frequency': '1N', 'high': 0.1674, 'low': 0.16205, 'markType': 'S3M/F07_1Y', 'open': 0.16645, 'product': 'S3M/F07_1Y', 'rowId': 580359921078370304, 'smallType': 'BASIS', 'source': 'DEFAULT', 'symbol': 'S3M/F07_1Y', 'sysUpdateDate': 2019052, 'sysUpdateTime': 114307, 'time': 1539829517360, 'timespan': 1558410180000, 'type': 'IRS_PUB_BAR', 'updateDate': 20190521, 'updateTime': 114300000}]



### 8.DEFAULT市场ODM价格bar数据查询  QUANT_HDS_DEFAULT_COM_PUB_ODM_BAR
- 函数定义


    irs.query_irs_pub_odm_bar(symbols,frequency=None,start_time=None,end_time=None)

- 函数定义

|参数名|数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|symbols|[String]|合约|是|输入为None时，查询全部|
|frequency|String|频率|否||
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出参数解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|业务更新日期|Number|updateDate||
|业务更新时间|Number|updateTime||
|最高价|Number|high||
|开盘价|Number|open||
|最低价|Number|low||
|收盘价|Number|close||
|频率|String|frequency||
|产品类型|String|product||
|业务时间戳|Number|timespan||
|计算价格类型|String|calType||

- API执行范例

    

    qlog.info("DEFAULT市场ODM价格")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_irs_pub_odm_bar(['FR007_10Y'],frequency='1N',start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("DEFAULT市场ODM价格结束")

- 输出结果(以队列的集合,对象结果集返回)

    
    [{ 'bigType': 'IR', 'close': 0.165, 'frequency': '1N', 'high': 0.1674, 'low': 0.16205, 'markType': 'S3M/F07_1Y', 'open': 0.16645, 'product': 'S3M/F07_1Y', 'rowId': 580359921078370304, 'smallType': 'BASIS', 'source': 'DEFAULT', 'symbol': 'S3M/F07_1Y', 'sysUpdateDate': 2019052, 'sysUpdateTime': 114307, 'time': 1539829517360, 'timespan': 1558410180000, 'type': 'IRS_PUB_BAR', 'updateDate': 20190521, 'updateTime': 114300000}]


### 9.利率曲线查询 QUANT_HDS_DEFAULT_IRS_PUB_CURVE

- 函数定义
    
    
    irs.query_curve(products,start_time=None,end_time=None)

- 函数定义

|输入参数名|数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|products|[String]|合约|是|必输
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出参数解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|业务更新日期|String|updateDate||
|业务更新时间|String|updateTime||
|产品|String|product||
|业务时间戳|String|timespan||
|版本|String|version||
|曲线|String|rates||
|结构类|map<String,Double>|map||

- API执行范例

    
    qlog.info("利率曲线查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_curve(['FR007'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata[0])
    # 获取数据对象的symbol属性值
    qlog.info(redata[0].symbol)
    qlog.info("利率曲线查询结束")

- 输出结果(以队列的集合,对象结果集返回)

    
    暂无



### 10.贴现因子查询 QUANT_HDS_DEFAULT_IRS_PUB_DISCOUNT
- 函数定义

    
    irs.query_discount(product,start_time=None,end_time=None)


- 输出参数解释

|输入参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|product|[String]|产品|是|必输
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|

- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|版本号|String|curveVersion||
|贴现因子|String|discounts||
|期限值|String|lashSize||
|期限|Number|lashTerm||
|是否向上冲击|Number|lashUp||
|是否冲击|Number|lashed||
|Rate值|Number|onRate||
|产品|Number|product||
|时间戳|String|timespan||
|结构类|Object|irDiscountDOS|业务更新日期:updateDate,业务更新时间:updateTime,版本:version,产品:product;是否冲击关键期限:lashed,是否向上冲击关键期限:lashUp,冲击大小:lashSize,冲击关键期限:lashTerm,贴现因子:map,隔夜贴现因子:preDiscount,业务时间戳:timespan  list[updateDate,updateTime,version,product,lashed,lashUp,lashSize,lashTerm,map,preDiscount,timespan]|

- API执行范例


    

    qlog.info("贴现因子查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_discount(['FR007'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata['FR007'])
    # 获取数据对象的symbol属性值
    qlog.info(redata['FR007'].symbol)
    qlog.info("贴现因子查询结束")

- 输出结果(以队列的集合,对象结果集返回)
    
    
    [{'curveVersion': 'CurveVersion-FR007-296702', 'discounts': '20190618: 1.0,20190619: 0.9941440827999455,20190620: 0.9883442014178854,20190621: 0.9825995934333491,20190622: 0.976909510001316,20190623: 0.9712732155526899,20190624: 0.9656899875026607,20190710: 0.8830319345230394,20190711:.... 0.027455085110513113,20290803: 0.02744659409762122,20290804: 0.027438107647589458', 'lashSize': 1, 'lashTerm': "'9M'", 'lashUp': True, 'lashed': True, 'onRate': 0.9998, 'product': "'FR007'", 'time': 1539829517360}]


### 11.DV01策略维度查询
- 函数定义

    
    irs.query_dv01byId(product,start_time=None,end_time=None)

- 输出参数解释

|输入参数名 | 数据类型|描述|是否必填|备注说明|
|---|---|---|---|---|
|product | [String]|产品|是|必输
|start_time|String|开始时间|否|输入规则请查询操作指南时间格式支持|
|end_time|String|结束时间|否|输入规则请查询操作指南时间格式支持|


- 输出字段解释

|字段名称解释|字段类型|字段名称|备注说明|
|---|---|---|---|
|现金流版本|String|cashVersion||
|曲线版本|String|cueveVersion||
|dv013M期限值|Number|dv013m||
|dv016M期限值|Number|dv016m||
|dv019M期限值|Number|dv019m||
|dv011Y期限值|Number|dv011y||
|dv012Y期限值|Number|dv012y||
|dv013Y期限值|Number|dv013y||
|dv014Y期限值|Number|dv014y||
|dv015Y期限值|Number|dv015y||
|dv017Y期限值|Number|dv017y||
|dv0110Y期限值|Number|dv0110y||
|产品|String|product||
|时间戳|String|timespan||

- API执行范例

    

    qlog.info("DV01策略维度查询")
    # 订阅数据日志输出
    qlog.info_to_str(data)
    # 查询API执行
    redata = irs.query_dv01byId(['Shibor3M'],start_time='20190628',end_time='20190629')
    # 获取查询集合结果集的第一条数据
    qlog.info(redata['Shibor3M'])
    # 获取数据对象的symbol属性值
    qlog.info(redata['Shibor3M']['symbol'])
    qlog.info("DV01策略维度查询结束")

- 输出结果

    
    {'Shibor3M': {'cashVersion': 'StraCashFlowVersion-107-1971', 'curveVersion': 'CurveVersion-Shibor3M-62333', 'dv0110y': 0.0, 'dv011y': 0.0, 'dv012y': 0.0, 'dv013m': -28091.599683499997, 'dv013y': 0.0, 'dv014y': 0.0, 'dv015y': 0.0, 'dv016m': 69337.166433, 'dv017y': 0.0, 'dv019m': 16721.4291655, 'product': 'Shibor3M', 'time': 1539829517360}}

<h1 style="text-align: center"> 利率互换交易模块 </h1>

---
### 限价单发起
<h3>●函数定义</h3>
<pre>irs.limit_order(symbol, side, price, quantity, in_out_market=None, channel_code=None, time_in_force=None, expire_time=None)</pre>
<h3>●函数说明</h3>
<h3>利率互換限价单发起</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约唯一代码</td><td>是</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>交易方向</td><td>是</td><td>B:买 S:卖</td></tr>
<tr><td>price</td><td>Number</td><td>报单价格</td><td>是</td><td>价格不能为零或负数</td></tr>
<tr><td>quantity</td><td>Number</td><td>报单总数量</td><td>是</td><td>必须大于最小量且加量必须是倍数,<br>basis或者spread必须是单位量的倍数</td></tr>
<tr><td>in_out_market</td><td>String</td><td>内外部市场,执行市场<br>（默认3-内/外部市场）</td><td>否</td><td>1-内部市场,3-内/外部市场</td></tr>
<tr><td>channel_code</td><td>String</td><td>交易渠道<br>（默认XSWAP）</td><td>否</td><td>--</td></tr>
<tr><td>time_in_force</td><td>String</td><td>有效时间类型<br>（默认1-GFD）</td><td>否</td><td>交易日有效，取合约内的交易时间维护:GFD:1,<br>手工未撤销时永久有效:GTC:2,<br>指定时间有效有效:GTD:3,<br>极短时间全部成交，否则全部撤销:FOK:4,<br>极短时间成交，剩余量全部撤销:FAK:5,<br>指定交易所，交易所闭市时订单撤销:GTX:6;</td></tr>
<tr><td>expire_time</td><td>String</td><td>到期时间</td><td>否</td><td>GTD模式,expire_time是必填项,<br>仅支持YYYYMMDD-HH:MM:SS</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>返回订单ID</td><td>Number</td><td>id</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('利率互换限价单发起')
order_id_1 = irs.limit_order('FR007_1Y', 'S', 2.7, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_2 = irs.limit_order('FR007_2Y', 'S', 2.75, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_3 = irs.limit_order('FR007_3Y', 'S', 2.76, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_4 = irs.limit_order('FR007_4Y', 'S', 2.77, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_5 = irs.limit_order('FR007_5Y', 'S', 2.78, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
qlog.info_f('限价单五笔订单订单号:{},{},{},{},{}',order_id_1,order_id_2,order_id_3,order_id_4,order_id_5)
qlog.info('利率互换限价单发起结束')
</pre>      
<h3>●输出结果</h3>
<pre>
text:"利率互换限价单发起"
限价单五笔订单订单号:699221675354361856,699221675945758720,699221676147085312,699221676335828992,699221676537155584
text:"利率互换限价单发起结束"
</pre>    

### 撤销全部订单
<h3>●函数定义</h3>
<pre>irs.cancel_all()</pre>     
<h3>●函数说明</h3>
<h3>利率互換订单撤销,将还存在进行中量的订单进行撤销,触发onOrder驱动事件.<br>注意不能对未挂单成功的订单,订单状态为0和已经完结的订单状态的进行撤单</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
'''
qlog.info('利率互换订单撤销')
irs.cancel_all()
qlog.info_f('查询订单列表:{}', context.orders.orders())
qlog.info('利率互换订单撤销完成')
</pre>
<h3>●输出结果</h3>
<pre>
text:"利率互换订单撤销"
text:["查询订单列表:{}",[]]
text:"利率互换订单撤销完成"
</pre>    

### 订单条件撤销
<h3>●函数定义</h3>
<pre>irs.cancel_order( symbol=None, side=None,channel_code=None, time_in_force=None)</pre>      
<h3>●函数说明</h3>
<h3>利率互換订单条件撤销,将还存在进行中量的订单进行撤销,触发onOrder驱动事件.<br>注意不能对未挂单成功的订单,订单状态为0和已经完结的订单状态的进行撤单</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约唯一代码</td><td>否</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>交易方向</td><td>否</td><td>B:买 S:卖</td></tr>
<tr><td>channel_code</td><td>String</td><td>市场</td><td>否</td><td>--</td></tr>
<tr><td>time_in_force</td><td>String</td><td>有效时间类型</td><td>否</td><td>交易日有效，取合约内的交易时间维护:GFD:1,<br>手工未撤销时永久有效:GTC:2,<br>指定时间有效有效:GTD:3,<br>极短时间全部成交，否则全部撤销:FOK:4,<br>极短时间成交，剩余量全部撤销:FAK:5,<br>指定交易所，交易所闭市时订单撤销:GTX:6;</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info('利率互换订单条件撤销')
order_id_1 = irs.limit_order('FR007_1Y', 'S', 2.7, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_2 = irs.limit_order('FR007_2Y', 'S', 2.75, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_3 = irs.limit_order('FR007_3Y', 'S', 2.76, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_4 = irs.limit_order('FR007_4Y', 'S', 2.77, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
order_id_5 = irs.limit_order('FR007_5Y', 'S', 2.78, 1000000, in_out_market='1', channel_code='CMDS', time_in_force='1',expire_time='20201002')
qlog.info_f('限价单五笔订单订单号:{},{},{},{},{}', order_id_1, order_id_2, order_id_3, order_id_4, order_id_5)
irs.cancel_order(channel_code='CMDS', symbol='FR007_1Y', side='S', time_in_force='1')
qlog.info_f('查询订单列表:{}', context.orders.orders())
qlog.info('利率互换订单条件撤销完成')
</pre>
<h3>●输出结果</h3>
<pre>
text:"利率互换订单条件撤销"
限价单五笔订单订单号:699221676801396736,699221676964974592,699221677166301184,699221677346656256,699221677531205632
text:["查询订单列表:{}",[]]
text:"利率互换订单条件撤销完成"
</pre>   

### 根据订单id撤销
<h3>●函数定义</h3>
<pre>irs.cancel_order_id(orderId)</pre>    
<h3>●函数说明</h3>
<h3>利率互換订单ID撤销,将还存在进行中量的订单进行撤销,触发onOrder驱动事件.<br>注意不能对未挂单成功的订单,订单状态为0和已经完结的订单状态的进行撤单</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>orderId</td><td>String</td><td>订单ID</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info('利率互换订单条件撤销')
order_id = irs.limit_order('R07_5Y', 'S', 2.76, 10000,in_out_market='1',channel_code='CMDS',time_in_force='1',expire_time='20191101-23:59:59')
qlog.info('利率互换订单ID'+str(order_id)+'撤销')
qlog.info_f('查询订单列表:{}', context.orders.orders())
irs.cancel_order_id(order_id)
qlog.info('利率互换订单条件撤销完成')
</pre>
<h3>●输出结果</h3>
<pre>
text:"利率互换订单条件撤销"
text:'利率互换订单ID699221676801396735撤销')
text:"查询订单列表:699221676801396736,699221676964974592,699221677166301184,699221677346656256,699221677531205632"
text:"利率互换订单条件撤销完成"
</pre>  

### 获取交易列表
<h3>●函数定义</h3>
<pre>irs.query_orders(symbol=None, start_time=None, end_time=None)</pre>     
<h3>●函数说明</h3>
<h3>获取当前策略所有交易列表(使用中请注意最好带上时间范围，不然因为查询数据较大可能导致策略运行出现资源不够的情况)<br>这是操作数据库查询,会存在延迟,插入操作在5秒内完成.</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>实际合约名称</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>否</td><td>输入规则:YYYYMMDDHHMMSS，不输入则默认查全部</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>否</td><td>输入规则:YYYYMMDDHHMMSS，不输入则默认查全部</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Number</td><td>id</td><td>--</td></tr>			
<tr><td>运行id</td><td>Number</td><td>runId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Number</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>系统中唯一合约代码号</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>金融标的、产品</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>订单类型</td><td>String</td><td>orderType</td><td>限价单:2.点击成交:7.智能订单:指定渠道RFQ询价,选择最优价渠道成交:50.<br>外资行专用,客户端显示SOR:51,外汇订单补录:99</td></tr>
<tr><td>有效时间类型</td><td>String</td><td>timeInForce</td><td>交易日有效，取合约内的交易时间维护:GFD:1,<br>手工未撤销时永久有效:GTC:2,<br>指定时间有效有效:GTD:3,<br>极短时间全部成交，否则全部撤销:FOK:4,<br>极短时间成交，剩余量全部撤销:FAK:5,<br>指定交易所，交易所闭市时订单撤销:GTX:6;</td></tr>
<tr><td>到期时间</td><td>String</td><td>expireTime</td><td>--</td></tr>
<tr><td>报单价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>报单方向</td><td>String</td><td>side</td><td>B:买 S:卖</td></tr>
<tr><td>报单开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>报单总数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>报单已成交数量</td><td>Double</td><td>tradedQuantity</td><td>--</td></tr>
<tr><td>报单状态</td><td>String</td><td>orderStatus</td><td>初始CREATED:0,已提交Submit:9,<br>运行中New:1,订单拒绝Rejected:2,<br>订单撤销被拒绝CancelRejected:3,本地单超时撤销中ExpireCanceling:4,<br>订单已超时Expired:5,订单撤销中Canceling:6,<br>交易已撤销Canceled:7,已结束Finished:8;</td></tr>
<tr><td>订单损益</td><td>Double</td><td>profit</td><td>--</td></tr>
<tr><td>创建订单生成时间</td><td>Number</td><td>createTime</td><td>--</td></tr>
<tr><td>发单时间</td><td>Number</td><td>orderTime</td><td>--</td></tr>
<tr><td>撤单时间</td><td>Number</td><td>cancelTime</td><td>--</td></tr>
<tr><td>最后修改时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>订单关联关系</td><td>String</td><td>property</td><td>--</td></tr>
<tr><td>OCO订单关联ID</td><td>String</td><td>propertyId</td><td>--</td></tr>
<tr><td>内外部市场</td><td>String</td><td>inOutMarket</td><td>1-内部市场,3-内/外部市场</td></tr>
<tr><td>订单执行模式</td><td>String</td><td>model</td><td>1-FullAmount,2-Sweepable</td></tr>
<tr><td>订单时效时间参考的交易所</td><td>String</td><td>expireTimeOfExchange</td><td>--</td></tr>
<tr><td>价格ID</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>交易拒绝方</td><td>String</td><td>rejectSide</td><td>--</td></tr>
<tr><td>交易拒绝原因</td><td>String</td><td>errorMsg</td><td>--</td></tr>
<tr><td>已成交交易的平均价格</td><td>Double</td><td>tradedAvgPrice</td><td>--</td></tr>
<tr><td>投机套保标识</td><td>String</td><td>hedgeFlag</td><td>1.普通交易.2.投机交易.3.套保交易</td></tr>
<tr><td>近端交割日</td><td>String</td><td>valueDate</td><td>--</td></tr>
<tr><td>交易意图</td><td>String</td><td>intention</td><td>--</td></tr>
<tr><td>远端交割日</td><td>String</td><td>maturityDate</td><td>--</td></tr>
<tr><td>已撤销</td><td>Number</td><td>cancelQuantity</td><td>--</td></tr>
<tr><td>未明量</td><td>Number</td><td>unkownQuantity</td><td>--</td></tr>
<tr><td>进行中的量</td><td>Number</td><td>doingQuantity</td><td>--</td></tr>
<tr><td>版本号</td><td>Number</td><td>version</td><td>--</td></tr>
<tr><td>金融市场</td><td>String</td><td>financialMarket</td><td>--</td></tr>
<tr><td>金融工具</td><td>String</td><td>financialTool</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.获取交易列表
2.获取查询集合结果集的第一条数据
'''
qlog.info('利率互换获取交易列表')
orders = irs.query_orders(symbol='FR007_5Y', start_time='20190110100000', end_time='20200110100000')
for order in orders:
    qlog.info(order)
qlog.info('利率互换获取交易列表完成')
</pre>     
<h3>●输出结果</h3>
<pre>
text:"利率互换获取交易列表"
text:{"cancelQuantity":0.0,"channelCode":"CMDS","createTime":20200421162959,"doingQuantity":1000000.0,"expireTime":"20201002","financialMarket":"I","financialTool":"U","id":702194434015510528,"inOutMarket":"1","model":"2","orderStatus":"1","orderTime":20190110100000,"orderType":"2","price":2.78,"product":"FR007","profit":0.0,"property":"1","quantity":1000000.0,"runId":4112,"side":"S","symbol":"FR007_5Y","timeInForce":"1","tradedQuantity":0.0,"unkownQuantity":0.0,"userId":"luhai","version":2}
text:{"cancelQuantity":0.0,"channelCode":"CMDS","createTime":20200421163009,"doingQuantity":1000000.0,"expireTime":"20201002","financialMarket":"I","financialTool":"U","id":702194476759662592,"inOutMarket":"1","model":"2","orderStatus":"1","orderTime":20190127100000,"orderType":"2","price":2.78,"product":"FR007","profit":0.0,"property":"1","quantity":1000000.0,"runId":4112,"side":"S","symbol":"FR007_5Y","timeInForce":"1","tradedQuantity":0.0,"unkownQuantity":0.0,"userId":"luhai","version":2}
text:{"cancelQuantity":0.0,"channelCode":"CMDS","createTime":20200421163019,"doingQuantity":1000000.0,"expireTime":"20201002","financialMarket":"I","financialTool":"U","id":702194519109550080,"inOutMarket":"1","model":"2","orderStatus":"1","orderTime":20190128100000,"orderType":"2","price":2.78,"product":"FR007","profit":0.0,"property":"1","quantity":1000000.0,"runId":4112,"side":"S","symbol":"FR007_5Y","timeInForce":"1","tradedQuantity":0.0,"unkownQuantity":0.0,"userId":"luhai","version":2}
text:"利率互换获取交易列表完成"
</pre>

### 获取成交明细列表
<h3>●函数定义</h3>
<pre>irs.query_trades(symbol=None,start_time=None, end_time=None)</pre> 
<h3>●函数说明</h3>
<h3>获取成交明细列表(使用中请注意最好带上时间范围,不然因为查询数据较大可能导致策略运行出现资源不够的情况.)<br>这是操作数据库查询,会存在延迟,插入操作在5秒内完成.</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>是</td><td>输入规则:YYYYMMDDHHMMSS</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>是</td><td>输入规则:YYYYMMDDHHMMSS</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Number</td><td>id</td><td>--</td></tr>
<tr><td>运行id</td><td>Number</td><td>runId</td><td>--</td></tr>
<tr><td>订单编号</td><td>Number</td><td>orderId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Number</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>合约</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>撮合引擎系统中的唯一编号</td><td>String</td><td>targetTradeId</td><td>--</td></tr>
<tr><td>交易所真正成交ID</td><td>String</td><td>marketTradeId</td><td>--</td></tr>
<tr><td>成交方向</td><td>String</td><td>side</td><td>--</td></tr>
<tr><td>成交开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>成交价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>成交数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>成交时间</td><td>Number</td><td>tradeTime</td><td>--</td></tr>
<tr><td>成交入库时间</td><td>Number</td><td>createTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>成交类型</td><td>String</td><td>tradeType</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('利率互换获取成交明细列表')
trades = irs.query_trades(symbol='FR007_5Y',start_time='20190110100000', end_time='20200110100000')
for trade in trades:
    qlog.info(trade)
qlog.info('利率互换获取成交明细列表完成')
</pre>
<h3>●输出结果</h3>
<pre>
text:"利率互换获取成交明细列表"
text:{"channelCode":"XSWAP","createTime":20190925201417,"delStatus":0,"id":100,"orderId":475589,"price":37.5,"quantity":2.0E7,"runId":4116,"side":"B","symbol":"FR007_5Y","tradeTime":20190426103101,"userId":"luhai"}
text:"利率互换获取成交明细列表完成"
</pre>

### 今日损益
<h3>●函数定义</h3>
<pre>irs.get_today_pnl()</pre>    
<h3>●函数说明</h3>
<h3>当前策略维度获取今日损益</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>今日损益</td><td>Number</td><td>number</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('利率互换获取今日损益')
todayPnl = irs.get_today_pnl()
qlog.info(todayPnl)
qlog.info('利率互换获取今日损益完成')
</pre>    
<h3>●输出结果</h3>
<pre>
text:"利率互换获取今日损益"
text:524330.0
text:"利率互换获取今日损益完成"
</pre>

### 最大回撤
<h3>●函数定义</h3>
<pre>irs.get_max_drawdown()</pre>    
<h3>●函数说明</h3>
<h3>获取当前策略维度最大回撤</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>备注说明</th><tr>
<tr><td>最大回撤量</td><td>Number</td><td>values</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取最大回撤")
redata = irs.get_max_drawdown()
qlog.info(redata)
qlog.info("获取最大回撤完成")
</pre>    
<h3>●输出结果</h3>
<pre>
text:"利率互换获取最大回撤"
text:12000.0
text:"利率互换获取最大回撤完成"
</pre>

### 根据订单id批量撤销
<h3>●函数定义</h3>
<pre>irs.cancel_order_ids(orderIds)</pre>    
<h3>●函数说明</h3>
<h3>利率互換订单ID批量撤销</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>orderIds</td><td>[String]</td><td>订单ID</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info('利率互换订单ID批量撤销开始')
order_id_1 = irs.limit_order('R07_5Y', 'S', 2.76, 10000,in_out_market='1',channel_code='CMDS',time_in_force='1',expire_time='20191101-23:59:59')
order_id_2 = irs.limit_order('R07_5Y', 'S', 2.76, 10000,in_out_market='1',channel_code='CMDS',time_in_force='1',expire_time='20191101-23:59:59')
irs.cancel_order_ids([order_id,order_id_2])
qlog.info('利率互换订单ID批量撤销完成')
</pre>    
<h3>●输出结果</h3>
<pre>
text:"利率互换订单ID批量撤销开始"
text:"利率互换订单ID批量撤销完成"
</pre>  

### 获取策略维度orders
<h3>●函数定义</h3>
<pre>context.orders.orders()</pre>    
<h3>●函数说明</h3>
<h3>获取当前策略中未完结订单列表</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Number</td><td>id</td><td>--</td></tr>			
<tr><td>运行id</td><td>Number</td><td>runId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Number</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>系统中唯一合约代码号</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>金融标的、产品</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>订单类型</td><td>String</td><td>orderType</td><td>限价单:2.点击成交:7.智能订单:指定渠道RFQ询价,选择最优价渠道成交:50.<br>外资行专用,客户端显示SOR:51,外汇订单补录:99</td></tr>
<tr><td>有效时间类型</td><td>String</td><td>timeInForce</td><td>交易日有效，取合约内的交易时间维护:GFD:1,<br>手工未撤销时永久有效:GTC:2,<br>指定时间有效有效:GTD:3,<br>极短时间全部成交，否则全部撤销:FOK:4,<br>极短时间成交，剩余量全部撤销:FAK:5,<br>指定交易所，交易所闭市时订单撤销:GTX:6;</td></tr>
<tr><td>到期时间</td><td>String</td><td>expireTime</td><td>--</td></tr>
<tr><td>报单价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>报单方向</td><td>String</td><td>side</td><td>B:买 S:卖</td></tr>
<tr><td>报单开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>报单总数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>报单已成交数量</td><td>Double</td><td>tradedQuantity</td><td>--</td></tr>
<tr><td>报单状态</td><td>String</td><td>orderStatus</td><td>初始CREATED:0,已提交Submit:9,<br>运行中New:1,订单拒绝Rejected:2,<br>订单撤销被拒绝CancelRejected:3,本地单超时撤销中ExpireCanceling:4,<br>订单已超时Expired:5,订单撤销中Canceling:6,<br>交易已撤销Canceled:7,已结束Finished:8;</td></tr>
<tr><td>订单损益</td><td>Double</td><td>profit</td><td>--</td></tr>
<tr><td>创建订单生成时间</td><td>Number</td><td>createTime</td><td>--</td></tr>
<tr><td>发单时间</td><td>Number</td><td>orderTime</td><td>--</td></tr>
<tr><td>撤单时间</td><td>Number</td><td>cancelTime</td><td>--</td></tr>
<tr><td>最后修改时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>订单关联关系</td><td>String</td><td>property</td><td>--</td></tr>
<tr><td>OCO订单关联ID</td><td>String</td><td>propertyId</td><td>--</td></tr>
<tr><td>内外部市场</td><td>String</td><td>inOutMarket</td><td>1-内部市场,3-内/外部市场</td></tr>
<tr><td>订单执行模式</td><td>String</td><td>model</td><td>1-FullAmount,2-Sweepable</td></tr>
<tr><td>订单时效时间参考的交易所</td><td>String</td><td>expireTimeOfExchange</td><td>--</td></tr>
<tr><td>价格ID</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>交易拒绝方</td><td>String</td><td>rejectSide</td><td>--</td></tr>
<tr><td>交易拒绝原因</td><td>String</td><td>errorMsg</td><td>--</td></tr>
<tr><td>已成交交易的平均价格</td><td>Double</td><td>tradedAvgPrice</td><td>--</td></tr>
<tr><td>投机套保标识</td><td>String</td><td>hedgeFlag</td><td>1.普通交易.2.投机交易.3.套保交易</td></tr>
<tr><td>近端交割日</td><td>String</td><td>valueDate</td><td>--</td></tr>
<tr><td>交易意图</td><td>String</td><td>intention</td><td>--</td></tr>
<tr><td>远端交割日</td><td>String</td><td>maturityDate</td><td>--</td></tr>
<tr><td>已撤销</td><td>Number</td><td>cancelQuantity</td><td>--</td></tr>
<tr><td>未明量</td><td>Number</td><td>unkownQuantity</td><td>--</td></tr>
<tr><td>进行中的量</td><td>Number</td><td>doingQuantity</td><td>--</td></tr>
<tr><td>版本号</td><td>Number</td><td>version</td><td>--</td></tr>
<tr><td>金融市场</td><td>String</td><td>financialMarket</td><td>--</td></tr>
<tr><td>金融工具</td><td>String</td><td>financialTool</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取策略中订单列表")
orders = context.orders.orders()
qlog.info("获取策略中订单列表完成")
</pre>    
<h3>●输出结果</h3>
<pre>
text:"获取策略中订单列表"
text:"[{'id': 13, 'userId': 'test001', 'strategyId': 1, 'channelCode': nan, 'symbol': 'FR007_1Y', 'orderType': nan, 'timeInforce': nan, 'expireTime': nan, 'price': 12.2, 'side': 'B', 'effect': nan, 'quantity': 1000000, 'amount': nan, 'tradedQuantity': nan, 'orderStatus': 1, 'createTime': 20190428205246, 'orderTime': 20190425103223, 'cancelTime': nan, 'updateTime': nan, 'ext': nan, 'runId': 67, 'profit': 0, 'property': nan, 'propertyId': nan, 'inOutMarket': nan, 'model': nan, 'expireTimeOfExchange': nan, 'quoteId': nan, 'rejectSide': nan, 'errorMsg': nan, 'tradedAvgprice': nan, 'time': 1539829517360},{'id': 14, 'userId': 'test001', 'strategyId': 1, 'channelCode': nan, 'symbol': 'FR007_1Y', 'orderType': nan, 'timeInforce': nan, 'expireTime': nan, 'price': 12.2, 'side': 'B', 'effect': nan, 'quantity': 1000000, 'amount': nan, 'tradedQuantity': nan, 'orderStatus': 1, 'createTime': 20190428205247, 'orderTime': 20190425103223, 'cancelTime': nan, 'updateTime': nan, 'ext': nan, 'runId': 67, 'profit': 0, 'property': nan, 'propertyId': nan, 'inOutMarket': nan, 'model': nan, 'expireTimeOfExchange': nan, 'quoteId': nan, 'rejectSide': nan, 'errorMsg': nan, 'tradedAvgprice': nan, 'time': 1539829517360},  {'id': 15, 'userId': 'test001', 'strategyId': 1, 'channelCode': nan, 'symbol': 'FR007_1Y', 'orderType': nan, 'timeInforce': nan, 'expireTime': nan, 'price': 12.2, 'side': 'B', 'effect': nan, 'quantity': 1000000, 'amount': nan, 'tradedQuantity': nan, 'orderStatus': 1, 'createTime': 20190428205247, 'orderTime': 20190425103223, 'cancelTime': nan, 'updateTime': nan, 'ext': nan, 'runId': 67, 'profit': 0, 'property': nan, 'propertyId': nan, 'inOutMarket': nan, 'model': nan, 'expireTimeOfExchange': nan, 'quoteId': nan, 'rejectSide': nan, 'errorMsg': nan, 'tradedAvgprice': nan, 'time': 1539829517360}]</pre>
text:"获取策略中订单列表完成"
</pre>
<h1 style="text-align: center"> 贵金属数据模块 </h1>

---
### 外资行整合最优价
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_COM_PUB_TOP_BOOK</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('DEFAULT', 'ComIpmBest', symbols)
'''获取数据函数调用方法'''
pm.get_com_ipm_best(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:UBS,JP摩根,花旗,汇丰<br>外资行整合最优价:@XAU/USD即期,@XAG/USD即期</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>行情日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>行情时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最优买价</td><td>Number</td><td>bid</td><td>--</td></tr>
<tr><td>最优买价</td><td>Number</td><td>ask</td><td>--</td></tr>
<tr><td>买量</td><td>Number</td><td>bidAmt</td><td>--</td></tr>
<tr><td>卖量</td><td>Number</td><td>askAmt</td><td>--</td></tr>
<tr><td>最优买价来源</td><td>String</td><td>bidSource</td><td>--</td></tr>
<tr><td>最优买价来源</td><td>String</td><td>askSource</td><td>--</td></tr>
<tr><td>最优买价产生日期</td><td>Number</td><td>bidUpdateDate</td><td>--</td></tr>
<tr><td>最优卖价产生日期</td><td>Number</td><td>bidUpdateTime</td><td>--</td></tr>
<tr><td>最优买价产生时间</td><td>Number</td><td>askUpdateDate</td><td>--</td></tr>
<tr><td>最优卖价产生时间</td><td>Number</td><td>askUpdateTime</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('外资行整合最优价历史数据查询')
comIpmBest = pm.get_com_ipm_best(['XAGUSD'])
if comIpmBest:
    qlog.info(comIpmBest[0])
    qlog.info(comIpmBest[0].symbol)
qlog.info('外资行整合最优价历史数据查询结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"查询合约为{090012}的森浦QB成交行情查询API执行"
text:{"bondCode":"090012","bondName":"09附息国债12","brokerType":"TPB","clearSpeed":"T+0","dealStatus":"1","eic":"QBondPubTra#090012=QB","fullPrice":99.973,"marketType":"SSE","originalPrice":2.7656,"priceId":"1909291945370106865","priceState":"3","priceType":"YTM","quoteId":"1405268","quoteUnit":"1","receivingDate":20190929,"receivingTime":194537671,"rowId":627954156518244352,"sendDate":20190929,"sendTime":194537731,"sendingDate":20190905,"sendingTime":55153621,"settlDate":"20190924","side":"X","source":"QB","symbol":"090012","sysUpdateDate":20190929,"sysUpdateTime":194537738,"text":"{1,2,3,4,5,6}","tickSize":4,"time":1547085600000,"tradeAmt":3000.0,"tradeStatus":"N","type":"QBondPubTra","updateDate":20190829,"updateTime":200335889,"updateType":"1","yield":2.7603}
text:"090012"
text:"森浦QB成交行情操作完成"
</pre>
   
### 期交所深度行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_SHFE_FUTURES_PUB_ODM_DEPT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('SHFE', 'ComFutPubOdm', symbols)
'''获取数据函数调用方法'''
pm.get_com_fut_pub_odm(symbol)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:期交所接口<br>MduserAPI,@所有黄金合约,@所有白金合约<br>
数据来源于:期交所接口<br>TraderAPI:@所有黄金合约,@所有白金合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>--</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>--</td></tr>
<tr><td>行情日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>行情时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>交易日</td><td>String</td><td>tradingDay</td><td>--</td></tr>
<tr><td>结算组代码</td><td>String</td><td>settlementGroupId</td><td>--</td></tr>
<tr><td>结算编号</td><td>String</td><td>settlementId</td><td>--</td></tr>
<tr><td>最新价</td><td>Number</td><td>lastPrice</td><td>--</td></tr>
<tr><td>昨结算</td><td>Number</td><td>preSettlementPrice</td><td>--</td></tr>
<tr><td>昨收盘</td><td>Number</td><td>preClosePrice</td><td>--</td></tr>
<tr><td>昨持仓量</td><td>Number</td><td>preOpenInterest</td><td>--</td></tr>
<tr><td>今开盘</td><td>Number</td><td>openPrice</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>highestPrice</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>lowestPrice</td><td>--</td></tr>
<tr><td>数量</td><td>Number</td><td>amt</td><td>--</td></tr>
<tr><td>成交金额</td><td>Number</td><td>tradeAmt</td><td>--</td></tr>
<tr><td>持仓量</td><td>Number</td><td>openInterest</td><td>--</td></tr>
<tr><td>今收盘</td><td>Number</td><td>closePrice</td><td>--</td></tr>
<tr><td>今结算</td><td>Number</td><td>settlementPrice</td><td>--</td></tr>
<tr><td>涨停板价</td><td>Number</td><td>upperLimitPrice</td><td>--</td></tr>
<tr><td>跌停板价</td><td>Number</td><td>lowerLimitPrice</td><td>--</td></tr>
<tr><td>昨虚实度</td><td>Number</td><td>preDelta</td><td>--</td></tr>
<tr><td>今虚实度</td><td>Number</td><td>currDelta</td><td>--</td></tr>
<tr><td>最后修改时间</td><td>Number</td><td>modifyTime</td><td>--</td></tr>
<tr><td>最后修改毫秒</td><td>Number</td><td>updateMillisec</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('期交所深度行情数据查询')
comFutPubOdm = pm.get_com_fut_pub_odm(['au1912'])
if comFutPubOdm:
    qlog.info(comFutPubOdm[0])
    qlog.info(comFutPubOdm[0].symbol)
qlog.info('期交所深度行情查询结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"期交所深度行情数据查询"
text:{"amt":5619.0,"closePrice":339.87,"currDelta":2129.7,"eic":"ComFutPubOdm#au1908=SHFE","highestPrice":340.69,"lastPrice":339.87,"lowerLimitPrice":333.99,"lowestPrice":333.99,"modifyTime":240000100,"openInterest":6117.0,"openPrice":337.52,"preClosePrice":337.52,"preDelta":979.2,"preOpenInterest":5828.0,"preSettlementPrice":337.52,"priceId":"1910120211560106968","priceState":"1","quoteUnit":"K","receivingDate":20190517,"receivingTime":240000100,"rowId":632400029541203968,"sendDate":20190517,"sendTime":240000100,"sendingDate":20190517,"sendingTime":240000100,"settlementGroupId":"11111111","settlementId":"22222","settlementPrice":339.87,"shComPubOdmRateDOS":[{"amt":5000.0,"level":1,"price":337.2,"side":1},{"amt":4000.0,"level":2,"price":337.15,"side":1},{"amt":8000.0,"level":3,"price":337.12,"side":1},{"amt":6000.0,"level":4,"price":337.1,"side":1},{"amt":5000.0,"level":5,"price":337.07,"side":1},{"amt":4000.0,"level":1,"price":337.28,"side":2},{"amt":3000.0,"level":2,"price":337.3,"side":2},{"amt":8000.0,"level":3,"price":337.32,"side":2},{"amt":6000.0,"level":4,"price":337.33,"side":2},{"amt":5000.0,"level":5,"price":337.35,"side":2}],"source":"SHFE","symbol":"au1912","sysUpdateDate":20190517,"sysUpdateTime":240000100,"tickSize":4,"time":1547085600000,"tradeAmt":1876689.81,"tradingDay":"20190622","transDate":0,"type":"ComFutPubOdm","updateDate":20190517,"updateMillisec":100,"updateTime":240000100,"updateType":"1","upperLimitPrice":340.69}
text:"au1912"
text:"期交所深度行情查询结束"
</pre>

### 金交所深度行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_SGE_BID_PUB_ODM_DEPT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('SGE', 'ComBidPubOdm', symbols)
'''获取数据函数调用方法'''
pm.get_sg_com_pub_odm(symbol)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:金交所接口<br>现货实盘合约竞价行情接入,报价/撤单交易:@AU99.99,@AU99.95,数据来源于金交所接口:现货延期交收合约,@AU（T+D）,@mAU（T+D）,@AG（T+D）,递延费</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>--</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>--</td></tr>
<tr><td>行情日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>行情时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>市场类型</td><td>String</td><td>marketType</td><td>--</td></tr>
<tr><td>合约名字</td><td>String</td><td>symbolName</td><td>--</td></tr>
<tr><td>最新价</td><td>Number</td><td>lastPrice</td><td>--</td></tr>
<tr><td>昨结算</td><td>Number</td><td>preSettlementPrice</td><td>--</td></tr>
<tr><td>昨收盘</td><td>Number</td><td>preClosePrice</td><td>--</td></tr>
<tr><td>开盘价</td><td>Number</td><td>openPrice</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>highestPrice</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>lowestPrice</td><td>--</td></tr>
<tr><td>成交金额</td><td>Number</td><td>tradeAmt</td><td>--</td></tr>
<tr><td>成交（双边）重量</td><td>Number</td><td>tradeWeight</td><td>--</td></tr>
<tr><td>成交数量</td><td>Number</td><td>tradeVolume</td><td>--</td></tr>
<tr><td>涨跌</td><td>Number</td><td>upDown</td><td>--</td></tr>
<tr><td>涨跌幅度</td><td>Number</td><td>upDownRange</td><td>--</td></tr>
<tr><td>持仓量</td><td>Number</td><td>openInterest</td><td>--</td></tr>
<tr><td>收盘价</td><td>Number</td><td>closePrice</td><td>--</td></tr>
<tr><td>结算价</td><td>Number</td><td>settlementPrice</td><td>--</td></tr>
<tr><td>涨停板价</td><td>Number</td><td>upperLimitPrice</td><td>--</td></tr>
<tr><td>跌停板价</td><td>Number</td><td>lowerLimitPrice</td><td>--</td></tr>
<tr><td>均价</td><td>Number</td><td>avgPrice</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('金交所深度行情查询')
sgComPubOdm = pm.get_sg_com_pub_odm(['Au99.99'])
if sgComPubOdm:
    qlog.info(sgComPubOdm[0])
    qlog.info(sgComPubOdm[0].symbol)
qlog.info('金交所深度行情查询结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"金交所深度行情查询"
text:{"avgPrice":298.55,"closePrice":298.77,"eic":"ComBidPubOdm#Au99.99=SGE","highestPrice":299.9,"lastPrice":298.72,"lowerLimitPrice":298.2,"lowestPrice":298.2,"marketType":"00","openInterest":0.0,"openPrice":299.32,"preClosePrice":298.77,"preSettlementPrice":298.67,"priceState":"3","quoteId":"0b123456","quoteUnit":"G","receivingDate":20190929,"receivingTime":19383796,"rowId":627952392637251584,"sendDate":20190929,"sendTime":193837192,"sendingDate":20190929,"sendingTime":19383796,"settlementPrice":298.76,"sgComPubOdmRateDOS":[{"amt":96.0,"level":1,"price":299.28,"side":2},{"amt":96.0,"level":1,"price":298.83,"side":1},{"amt":96.0,"level":2,"price":299.51,"side":2},{"amt":96.0,"level":2,"price":298.63,"side":1},{"amt":96.0,"level":3,"price":299.59,"side":2},{"amt":96.0,"level":3,"price":298.41,"side":1},{"amt":96.0,"level":4,"price":299.79,"side":2},{"amt":96.0,"level":4,"price":298.38,"side":1},{"amt":96.0,"level":5,"price":299.89,"side":2},{"amt":96.0,"level":5,"price":298.23,"side":1}],"source":"SGE","symbol":"Au99.99","symbolName":"Au99.99","sysUpdateDate":20190929,"sysUpdateTime":193837196,"tickSize":2,"time":1547085600000,"tradeAmt":3.772E7,"tradeVolume":130.0,"tradeWeight":1300.0,"transDate":0,"type":"ComBidPubOdm","upDown":-0.55,"upDownRange":-0.0018,"updateDate":20190924,"updateTime":150735,"updateType":"1","upperLimitPrice":299.9}
text:"Au99.99"
text:"金交所深度行情查询结束"
</pre>

### 外资行深度行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_IPM_COM_PUB_ODM_DEPT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache(market, 'ComIpmPubOdm', symbols)
'''获取数据函数调用方法'''
pm.get_com_ipm_pub_odm(symbol,market,num)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:UBS,JP摩根,花旗,汇丰<br>外资行整合深度行情:@XAU/USD即期,@XAG/USD即期</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>market</td><td>String</td><td>市场</td><td>是</td><td>必须输入对应的市场</td></tr>
<tr><td>num</td><td>String</td><td>量</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情请求编号</td><td>String</td><td>quoteReqId</td><td>--</td></tr>
<tr><td>行情日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>行情时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最优买价</td><td>Number</td><td>bidPrice</td><td>--</td></tr>
<tr><td>最优买价来源</td><td>String</td><td>bidSourcel</td><td>--</td></tr>
<tr><td>最优买价产生日期</td><td>Number</td><td>bidUpdateDate</td><td>--</td></tr>
<tr><td>最优买价产生时间</td><td>Number</td><td>bidUpdateTime</td><td>--</td></tr>
<tr><td>askPrice最优卖</td><td>Number</td><td>askPrice</td><td>--</td></tr>
<tr><td>askSource最优卖价来源</td><td>String</td><td>askSource</td><td>--</td></tr>
<tr><td>最优卖价产生日期</td><td>Number</td><td>askUpdateDate</td><td>--</td></tr>
<tr><td>最优卖价产生时间</td><td>Number</td><td>askUpdateTime</td><td>--</td></tr>
<tr><td>更新模式</td><td>String</td><td>updateMode</td><td>--</td></tr>
<tr><td>牌价类型</td><td>String</td><td>quoteType</td><td>--</td></tr>
<tr><td>基础货币</td><td>String</td><td>currency</td><td>--</td></tr>
<tr><td>产品类型</td><td>String</td><td>produceType</td><td>--</td></tr>
<tr><td>结算日期</td><td>String</td><td>valueDate</td><td>--</td></tr>
<tr><td>期限</td><td>String</td><td>tenor</td><td>--</td></tr>
<tr><td>有效截止日期</td><td>String</td><td>vaildTime</td><td>--</td></tr>
<tr><td>中间价</td><td>Number</td><td>midPrice</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('外资行深度行情查询')
comIpmPubOdm = pm.get_com_ipm_pub_odm(['XAUUSD'],'UBS','1000')
if comIpmPubOdm:
    qlog.info(comIpmPubOdm[0])
    qlog.info(comIpmPubOdm[0].symbol)
qlog.info('外资行深度行情结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"外资行深度行情查询"
text:{"askPoint":0.0,"askQuoteType":0,"askUpdateDate":0,"askUpdateTime":0,"bidPoint":0.0,"bidQuoteType":0,"bidUpdateDate":0,"bidUpdateTime":0,"currency":"XAU","eic":"ComIpmPubOdm#XAUUSD#1000=UBS","ipmSpPubOdmRateDOS":[{"amt":1000.0,"level":1,"price":1551.8,"priceState":"1","quoteId":"2b067d2000200Xlbd8d05sei01","side":2,"updateAction":"0","updateType":"1"},{"amt":1000.0,"level":1,"price":1550.5,"priceState":"1","quoteId":"2b067d2000200Xlbd8d05sei01","side":1,"updateAction":"0","updateType":"1"}],"orderQty":1000.0,"pipPlacement":6.0,"priceId":"666733801203834880","produceType":"FXFWD","quoteReqId":"2019110713460004","quoteType":"0","quoteUnit":"O","receivingDate":20200114,"receivingTime":143457627,"rowId":666733801228992512,"sendDate":20200114,"sendTime":200205522,"sendingDate":20200114,"sendingTime":210057627,"source":"UBS","spotAsk":1551.8,"spotBid":1550.5,"symbol":"XAUUSD","sysUpdateDate":20200114,"sysUpdateTime":200205528,"tickSize":6,"time":1547085600000,"type":"ComIpmPubOdm","updateDate":20200114,"updateMode":"0","updateTime":143502}
text:"XAUUSD"
text:"外资行深度行情结束"
</pre>

### CMDS外币深度行情
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_CMDS_SPOT_PUB_ODM_DEPT</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('CMDS', 'SpPubOdm', symbols)
'''获取数据函数调用方法'''
pm.get_sp_pub_odm(symbol)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:CFETS-CMDSFX<br>接入即期ODM深度行情:@USD/CNY</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>源行情编号</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>价格更新类型</td><td>String</td><td>updateType</td><td>--</td></tr>
<tr><td>价格状态</td><td>String</td><td>priceState</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●输出扩展字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>表唯一id</td><td>Number</td><td>rowId</td><td>--</td></tr>
<tr><td>业务发生时间</td><td>String</td><td>transactTime</td><td>--</td></tr>
<tr><td>上游唯一标示</td><td>String</td><td>eic</td><td>--</td></tr>
<tr><td>合约唯一代码</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>time</td><td>--</td></tr>
<tr><td>数据类型</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>市场渠道</td><td>String</td><td>source</td><td>--</td></tr>
<tr><td>合约大类</td><td>String</td><td>bigType</td><td>--</td></tr>
<tr><td>合约小类</td><td>String</td><td>smallType</td><td>--</td></tr>
<tr><td>合约标的</td><td>String</td><td>markType</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>sysUpdateDate</td><td>yyyymmdd</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>sysUpdateTime</td><td>hhmmss</td></tr>
<tr><td>报价单位</td><td>String</td><td>quoteUnit</td><td>--</td></tr>
<tr><td>小数点位数</td><td>Number</td><td>tickSize</td><td>--</td></tr>
<tr><td>交易所发送日期</td><td>Number</td><td>sendingDate</td><td>yyyymmdd</td></tr>
<tr><td>交易所发送时间</td><td>Number</td><td>sendingTime</td><td>hhmmssSSS</td></tr>
<tr><td>向MQ发送日期</td><td>Number</td><td>sendDate</td><td>yyyymmdd</td></tr>
<tr><td>向MQ发送时间</td><td>Number</td><td>sendTime</td><td>hhmmssSSS</td></tr>
<tr><td>接口接受日期</td><td>Number</td><td>receivingDate</td><td>yyyymmdd</td></tr>
<tr><td>接口接受时间</td><td>Number</td><td>receivingTime</td><td>hhmmssSSS</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('外币CMDS深度行情')
spPubOdm = pm.get_sp_pub_odm(['USDCNY'])
if spPubOdm:
    qlog.info(spPubOdm[0])
    qlog.info(spPubOdm[0].symbol)
qlog.info('外币CMDS深度行情结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"外币CMDS深度行情"
text:{"bigType":"BIGTYPE","eic":"SpPubOdm#USDCNY=CMDS","priceId":"666660120435888128","priceState":"3","pubOdmRateDOS":[{"amt":10000.0,"level":1,"price":6.9,"side":1,"updateDate":20200114,"updateTime":213000111},{"amt":10000.0,"level":1,"price":6.905,"side":2,"updateDate":20200114,"updateTime":213000111}],"quoteUnit":"0","receivingDate":20200114,"receivingTime":213000111,"rowId":666660120603656192,"sendDate":20200114,"sendTime":150918634,"sendingDate":20200114,"sendingTime":21300111,"smallType":"SMALLTYPE","source":"CMDS","symbol":"USDCNY","sysUpdateDate":20200114,"sysUpdateTime":150918698,"tickSize":4,"time":1547085600000,"type":"SpPubOdm","updateType":"1"}
text:"USDCNY"
text:"外币CMDS深度行情结束"
</pre>

### 外资行每日结算价
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_COM_IPM_AVER</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('DEFAULT', 'ComIpmAver', symbols)
'''获取数据函数调用方法'''
pm.get_com_ipm_aver(symbols)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:UBS,JP摩根,花旗,汇丰<br>各外资行每日结算价均值:@XAU/USD即期,@XAG/USD即期</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情编号</td><td>String</td><td>priceId</td><td>--</td></tr>
<tr><td>系统更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>系统更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>暂无</td><td>Number</td><td>averAgePrice</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('外资行每日结算价查询')
comIpmAver = pm.get_com_ipm_aver(['XAGUSD'])
if comIpmAver:
    qlog.info(comIpmAver[0])
    qlog.info(comIpmAver[0].symbol)
qlog.info('外资行每日结算价结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"外资行每日结算价查询"
text:{"averAgePrice":1524.0288019999998,"eic":"ComIpmAver#XAGUSD","priceId":"1911071600008202746","rowId":642030502240321536,"sendDate":20191107,"sendTime":160000031,"source":"DEFAULT","symbol":"XAGUSD","sysUpdateDate":20191107,"sysUpdateTime":160000084,"time":1547085600000,"type":"ComIpmAver","updateDate":20191107,"updateTime":160000027}
text:"XAGUSD"
text:"外资行每日结算价结束"
</pre>

### 价差bar数据
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_COM_PUB_SPR_BAR</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('DEFAULT', 'Spread', symbols)
'''获取数据函数调用方法'''
pm.query_spread_bar_list(symbol,frequency , caltype,start_time=None, end_time=None)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>计算各合约的价差数据:@FAG-FAG,@Au(T+D)-XAU,@Au99.99-Au(T+D),@Au99.99-XAU,@FAU-Au(T+D),@FAU-FAU,@Ag(T+D)-XAG,@FAG-Ag(T+D),@FAG-XAU</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>String</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>frequency</td><td>String</td><td>频率</td><td>是</td><td>1N:1分钟,5N:5分钟,15N:15分钟,<br>30N:30分钟,1H:60分钟,<br>1D:每日,1W:每周,1M:每月</td></tr>
<tr><td>calType</td><td>String</td><td>计算价格类型</td><td>是</td><td>--</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>行情日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>行情时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Number</td><td>timespan</td><td>--</td></tr>
<tr><td>基础合约</td><td>String</td><td>bContract</td><td>--</td></tr>
<tr><td>报价合约</td><td>String</td><td>qContract</td><td>--</td></tr>
<tr><td>单位</td><td>String</td><td>unit</td><td>--</td></tr>
<tr><td>做宽价差</td><td>Number</td><td>widen</td><td>--</td></tr>
<tr><td>做窄价差</td><td>Number</td><td>narrow</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
1.订阅数据日志输出
2.获取查询集合结果集的第一条数据
3.获取价差实数据第一条symbol属性值
'''
qlog.info("价差实数据查询")
spread = pm.query_spread_bar_list('AG1912/AG2006',frequency='1M',calType='spreadPriceNarrow',start_time='20191231', end_time='20191231')
if spread:
    qlog.info(spread[0])
    qlog.info(spread[0].symbol)
qlog.info("价差实数据查询结束")
</pre>
<h3>●输出结果</h3>
<pre>
text:"价差实数据查询"
text:{"calType":"spreadPriceNarrow","close":9.6332,"frequency":"1M","high":9.9739,"low":9.5007,"open":9.6568,"product":"au2002/XAUUSD","rowId":662254771343720448,"smallType":"DEFAULT","source":"DEFAULT","symbol":"AG1912/AG2006","sysUpdateDate":20200102,"sysUpdateTime":112401587,"time":1577807999999,"timespan":1546099200000,"type":"QUANT_HDS_DEFAULT_COM_PUB_SPR_BAR","updateDate":20181230,"updateTime":0}
text:"AG1912/AG2006"
text:"价差实数据查询结束"
</pre>

### 外资行整合最优价Bar数据
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_COM_PUB_ODM_BAR</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('DEFAULT', 'ComPubOdmBar', symbols)
'''获取数据函数调用方法'''
pm.query_com_pub_odm_bar(symbol,frequency,start_time=None,end_time=None)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>外资行整合最优价Bar数据:@XAU/USD即期,@XAG/USD即期</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>frequency</td><td>String</td><td>频率</td><td>是</td><td>1N:1分钟,5N:5分钟,15N:15分钟,<br>30N:30分钟,1H:60分钟,<br>1D:每日,1W:每周,1M:每月</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>业务更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>业务更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>high</td><td>--</td></tr>
<tr><td>开盘价</td><td>Number</td><td>open</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>low</td><td>--</td></tr>
<tr><td>收盘价</td><td>Number</td><td>close</td><td>--</td></tr>
<tr><td>频率</td><td>String</td><td>frequency</td><td>--</td></tr>
<tr><td>产品类型</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Number</td><td>timespan</td><td>--</td></tr>
<tr><td>计算价格类型</td><td>String</td><td>calType</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('外资行整合最优价Bar数据查询')
comPubOdmBar = pm.get_com_pub_odm_bar(['XAGUSD'],'1N')
if comPubOdmBar:
        qlog.info(comPubOdmBar[0])
        qlog.info(comPubOdmBar[0].symbol)
qlog.info('外资行整合最优价Bar数据结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"外资行整合最优价Bar数据查询"
text:{"calType":"topPrice","close":1501.49605,"frequency":"1N","high":1501.49605,"low":1501.49605,"open":1501.49605,"product":"XAGUSD","rowId":664045210807828481,"source":"DEFAULT","symbol":"XAGUSD","sysUpdateDate":20200107,"sysUpdateTime":95835620,"time":1578362280000,"timespan":1578362280000,"type":"ComPubOdmBar","updateDate":20200107,"updateTime":95800000}
text:"XAGUSD"
text:"外资行整合最优价Bar数据结束"
</pre>

### 期交所深度行情Bar数据
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_SH_PUB_BAR</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('DEFAULT', 'ShPubOdmBar', symbols)
'''获取数据函数调用方法'''
pm.query_sh_pub_odm_bar(symbol,frequency,start_time=None,end_time=None)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>期交所深度行情Bar数据</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>frequency</td><td>String</td><td>频率</td><td>是</td><td>1N:1分钟,5N:5分钟,15N:15分钟,<br>30N:30分钟,1H:60分钟,<br>1D:每日,1W:每周,1M:每月</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>业务更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>业务更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>high</td><td>--</td></tr>
<tr><td>开盘价</td><td>Number</td><td>open</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>low</td><td>--</td></tr>
<tr><td>收盘价</td><td>Number</td><td>close</td><td>--</td></tr>
<tr><td>频率</td><td>String</td><td>frequency</td><td>--</td></tr>
<tr><td>产品类型</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Number</td><td>timespan</td><td>--</td></tr>
<tr><td>计算价格类型</td><td>String</td><td>calType</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('期交所市场行情Bar数据查询')
shPubOdmBar = pm.get_sh_pub_odm_bar(['ag1910'],'1N')
if shPubOdmBar:
    qlog.info(shPubOdmBar[0])
    qlog.info(shPubOdmBar[0].symbol)
qlog.info('期交所市场行情Bar数据查询结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"期交所市场行情Bar数据查询"
text:{"calType":"shPrice","close":46190.0127,"frequency":"1N","high":46190.0127,"low":46190.0127,"open":46190.0127,"product":"ag1910","rowId":664045084169207810,"source":"DEFAULT","symbol":"ag1910","sysUpdateDate":20200107,"sysUpdateTime":95805427,"time":1578362280000,"timespan":1578362280000,"type":"ShPubOdmBar","updateDate":20200107,"updateTime":95800000}
text:"ag1910"
text:"期交所市场行情Bar数据查询结束"
</pre>

### 金交所深度行情Bar数据
<h3>●函数定义</h3><p style="display: none">QUANT_HDS_DEFAULT_SG_PUB_BAR</p>
<pre>
'''订阅数据接入(init方法中使用)'''
pm.init_cache('DEFAULT', 'SgPubOdmBar', symbols)
'''获取数据函数调用方法'''
pm.query_sg_pub_odm_bar(symbol,frequency,start_time=None,end_time=None)
</pre>
<h3>●函数说明</h3>
<h3>数据来源于:计算引擎<br>金交所深度行情Bar数据</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>frequency</td><td>String</td><td>频率</td><td>是</td><td>1N:1分钟,5N:5分钟,15N:15分钟,<br>30N:30分钟,1H:60分钟,<br>1D:每日,1W:每周,1M:每月</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>否</td><td>策略说明中的时间类型都可支持</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>业务更新日期</td><td>Number</td><td>updateDate</td><td>--</td></tr>
<tr><td>业务更新时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>最高价</td><td>Number</td><td>high</td><td>--</td></tr>
<tr><td>开盘价</td><td>Number</td><td>open</td><td>--</td></tr>
<tr><td>最低价</td><td>Number</td><td>low</td><td>--</td></tr>
<tr><td>收盘价</td><td>Number</td><td>close</td><td>--</td></tr>
<tr><td>频率</td><td>String</td><td>frequency</td><td>--</td></tr>
<tr><td>产品类型</td><td>String</td><td>product</td><td>--</td></tr>
<tr><td>业务时间戳</td><td>Number</td><td>timespan</td><td>--</td></tr>
<tr><td>计算价格类型</td><td>String</td><td>calType</td><td>--</td></tr>
<tr><td>暂无</td><td>String</td><td>allKye</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('金交所市场行情Bar数据查询')
sgPubOdmBar = pm.get_sg_pub_odm_bar(['Ag(T+D)'],'1N')
if sgPubOdmBar:
    qlog.info(sgPubOdmBar[0])
    qlog.info(sgPubOdmBar[0].symbol)
qlog.info('金交所市场行情Bar数据查询结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"金交所市场行情Bar数据查询"
text:{"calType":"sgPrice","close":296.86,"frequency":"1N","high":296.86,"low":296.86,"open":296.86,"product":"Ag(T+D)","rowId":664044977889738753,"source":"DEFAULT","symbol":"Ag(T+D)","sysUpdateDate":20200107,"sysUpdateTime":95740088,"time":1578362220000,"timespan":1578362220000,"type":"SgPubOdmBar","updateDate":20200107,"updateTime":95700000}
text:"Ag(T+D)"
text:"金交所市场行情Bar数据查询结束"
</pre>
<h1 style="text-align: center"> 贵金属交易模块 </h1>

---
### 限价单发起
<h3>●函数定义</h3>
<pre>pm.limit_order(symbol, side, price, quantity, in_out_market=None, channel_code=None, time_in_force=None, expire_time=None,hedge_flag=None)</pre>
<h3>●函数说明</h3>
<h3>贵金属限价单发起</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th></tr>
<tr><td>symbol</td><td>String</td><td>合约唯一代码</td><td>是</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>交易方向</td><td>是</td><td>B:买 S:卖</td></tr>
<tr><td>price</td><td>Number</td><td>报单价格</td><td>是</td><td>--</td></tr>
<tr><td>quantity</td><td>Number</td><td>报单总数量</td><td>是</td><td>期交所,金交所单位是手;外资行为盎司</td></tr>
<tr><td>channel_code</td><td>String</td><td>交易渠道</td><td>否</td><td>执行市场为1时交易渠道默认</td></tr>
<tr><td>in_out_market</td><td>String</td><td>内外部市场,执行市场<br>（默认2-外部市场）</td><td>否</td><td>1-内部市场，2-外部市场，3-内/外部市场</td></tr>
<tr><td>time_in_force</td><td>String</td><td>有效时间类型<br>(默认5-FAK)</td><td>否</td><td>1-GFD(手工未撤销时永久有效)<br>2-GTC(指定时间有效)<br>3-GTD(极短时间全部成交，否则全部撤销)<br>4-FOK(极短时间成交，剩余量全部撤销)<br>5-FAK(指定交易所，交易所闭市时订单撤销)<br> 6-GFX(本节交易时段有效)</td></tr>
                                                                                          
<tr><td>expire_time</td><td>String</td><td>到期时间</td><td>否</td><td>GTD模式,expire_time是必填项,<br>仅支持YYYYMMDD-HH:MM:SS</td></tr>
<tr><td>hedge_flag</td><td>String</td><td>投机套保标识<br>(默认1.普通交易)</td><td>否</td><td>1.普通交易2.投机交易3.套保交易</td></tr>  
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>返回订单ID</td><td>Number</td><td>id</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("限价单发起")
order_id1 = pm.limit_order('ag1906', 'S', 2.76, 10000, channel_code='SHFE',
               in_out_market='2', time_in_force='5', expire_time='20190808-23:59:59', hedge_flag='1')
qlog.info(order_id1)
qlog.info("限价单结束")
</pre>
<h3>●输出结果</h3>
<pre>
"限价单发起"
"890489"
"限价单结束"  
</pre>
<h3>●函数定义</h3> 
 
### RFQ单发起
<pre>pm.rfq_order(symbol,side,price,quantity=None,channel_code=None,value_date=None,maturity_date=None)</pre>
<h3>●函数说明</h3>
<h3>贵金属RFQ单发起</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约唯一代码</td><td>是</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>交易方向</td><td>是</td><td>B:买 S:卖(远端)</td></tr>
<tr><td>quantity</td><td>Number</td><td>报单总数量</td><td>是</td><td>--</td></tr>
<tr><td>value_date</td><td>String</td><td>近端交割日</td><td>是</td><td>--</td></tr>
<tr><td>maturity_date</td><td>String</td><td>远端交割日</td><td>是</td><td>--</td></tr>
<tr><td>channel_code</td><td>String</td><td>交易渠道（默认:CITI|HSBC|JP|DB）</td><td>否</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>返回订单ID</td><td>Number</td><td>propertyId</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("RFQ单发起")
order_id2 = pm.rfq_order('XAUUSD','S',10000,channel_code='CITI|HSBC|JP|DB',value_date='20190928',maturity_date='20190930')
qlog.info(order_id2)
qlog.info("RFQ单结束")
</pre>
<h3>●输出结果</h3>
<pre>
"RFQ单发起"
"890490"
"RFQ单结束"
</pre> 
    
### 撤销全部订单  修订版
<h3>●函数定义</h3>
<pre>pm.cancel_all()</pre>
<h3>●函数说明</h3>
<h3>贵金属撤销全部订单</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info("贵金属撤销全部订单")
pm.cancel_all()
qlog.info("贵金属撤销全部订单完成")
</pre>
<h3>●输出结果</h3>
<pre>
"贵金属撤销全部订单"
"贵金属撤销全部订单完成"
</pre> 

### 订单条件撤销修订版
<h3>●函数定义</h3>
<pre>pm.cancel_order(channel_code=None, symbol=None, side=None, time_in_force=None)</pre>
<h3>●函数说明</h3>
<h3>贵金属订单条件撤销</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>channel_code</td><td>String</td><td>市场</td><td>是</td><td>--</td></tr>
<tr><td>symbol</td><td>String</td><td>合约唯一代码</td><td>是</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>交易方向</td><td>是</td><td>B:买 S:卖</td></tr>
<tr><td>time_in_force</td><td>String</td><td>有效时间类型</td><td>否</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info("贵金属条件撤销")
pm.cancel_order(channel_code='CITI|HSBC|JP|DB', symbol='XAUUSD', side='S', time_in_force='5')
qlog.info("贵金属条件撤销订单完成")
</pre>
<h3>●输出结果</h3>
<pre>
"贵金属条件撤销"
"贵金属条件撤销订单完成"
</pre> 

### 根据订单id撤销 修订版
<h3>●函数定义</h3>
<pre>pm.cancel_order_id(orderId)</pre>
<h3>●函数说明</h3>
<h3>贵金属根据订单ID撤销</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>orderId</td><td>String</td><td>订单ID</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>无</h3>
<h3>●API执行范例</h3>
<pre>
order_id = pm.rfq_order('XAUUSD','S',10000,channel_code='CITI|HSBC|JP|DB',value_date='20190928',maturity_date='20190930')
pm.cancel_order_id(order_id)
</pre>

### 获取交易列表
<h3>●函数定义</h3>
<pre>pm.query_orders(symbol=None, start_time=None, end_time=None)</pre>
<h3>●函数说明</h3>
<h3>获取交易列表(使用中请注意最好带上时间范围,不然因为查询数据较大可能导致策略运行出现资源不够的情况.)</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>是</td><td>输入规则:YYYYMMDDHHMMSS</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>是</td><td>输入规则:YYYYMMDDHHMMSS</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Long</td><td>id</td><td>--</td></tr>	
<tr><td>运行id</td><td>Long</td><td>runId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Long</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>系统中唯一合约代码号</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>订单类型</td><td>String</td><td>orderType</td><td>--</td></tr>
<tr><td>有效时间类型</td><td>String</td><td>timeInForce</td><td>--</td></tr>
<tr><td>到期时间</td><td>String</td><td>expireTime</td><td>--</td></tr>
<tr><td>报单价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>报单方向</td><td>String</td><td>side</td><td>--</td></tr>
<tr><td>报单开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>报单总数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>报单成交数量</td><td>Double</td><td>tradedQuantity</td><td>--</td></tr>
<tr><td>报单状态</td><td>String</td><td>orderStatus</td><td>初始CREATED:0,已提交Submit:9,<br>运行中New:1,订单拒绝Rejected:2,<br>订单撤销被拒绝CancelRejected:3,本地单超时撤销中ExpireCanceling:4,<br>订单已超时Expired:5,订单撤销中Canceling:6,<br>交易已撤销Canceled:7,已结束Finished:8;</td></tr>
<tr><td>订单损益</td><td>Double</td><td>profit</td><td>--</td></tr>
<tr><td>创建订单生成时间</td><td>Long</td><td>createTime</td><td>--</td></tr>
<tr><td>发单时间</td><td>Long</td><td>orderTime</td><td>--</td></tr>
<tr><td>撤单时间</td><td>Long</td><td>cancelTime</td><td>--</td></tr>
<tr><td>最后修改时间</td><td>Long</td><td>updateTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>订单关联关系</td><td>String</td><td>property</td><td>--</td></tr>
<tr><td>OCO订单关联ID</td><td>String</td><td>propertyId</td><td>--</td></tr>
<tr><td>内外部市场</td><td>String</td><td>inOutMarket</td><td>1-内部市场,3-内/外部市场</td></tr>
<tr><td>订单执行模式</td><td>String</td><td>model</td><td>--</td></tr>
<tr><td>订单时效时间参考的交易所</td><td>String</td><td>expireTimeOfExchange</td><td>--</td></tr>
<tr><td>价格ID</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>交易拒绝方</td><td>String</td><td>rejectSide</td><td>--</td></tr>
<tr><td>交易拒绝原因</td><td>String</td><td>errorMsg</td><td>--</td></tr>
<tr><td>已成交交易的平均价格</td><td>Double</td><td>tradedAvgPrice</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取交易列表")
redata = pm.query_orders(symbol='ag1906', start_time='20190628', end_time='20190629')
qlog.info(redata[0])
qlog.info("获取交易列表完成")
</pre>
<h3>●输出结果</h3>
<pre>
[0: {'id': 13, 'userId': 'test001', 'strategyId': 1, 'channelCode': nan, 'symbol': 'ag1906', 'orderType': nan, 'timeInforce': nan, 'expireTime': nan, 'price': 12.2, 'side': 'B', 'effect': nan, 'quantity': 1000000, 'amount': nan, 'tradedQuantity': nan, 'orderStatus': 1, 'createTime': 20190428205246, 'orderTime': 20190425103223, 'cancelTime': nan, 'updateTime': nan, 'ext': nan, 'runId': 67, 'profit': 0, 'property': nan, 'propertyId': nan, 'inOutMarket': nan, 'model': nan, 'expireTimeOfExchange': nan, 'quoteId': nan, 'rejectSide': nan, 'errorMsg': nan, 'tradedAvgprice': nan, 'time': 1539829517360}]
</pre>

### 获取成交明细列表
<h3>●函数定义</h3>
<pre>pm.query_trades(symbol=None,start_time=None, end_time=None)</pre>
<h3>●函数说明</h3>
<h3>获取成交明细列表(使用中请注意最好带上时间范围,不然因为查询数据较大可能导致策略运行出现资源不够的情况.)</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
<tr><td>start_time</td><td>String</td><td>开始时间</td><td>是</td><td>输入规则:YYYYMMDDHHMMSS</td></tr>
<tr><td>end_time</td><td>String</td><td>结束时间</td><td>是</td><td>输入规则:YYYYMMDDHHMMSS</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Long</td><td>id</td><td>--</td></tr>
<tr><td>运行id</td><td>Long</td><td>runId</td><td>--</td></tr>
<tr><td>订单编号</td><td>Long</td><td>orderId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Long</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>合约</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>撮合引擎系统中的唯一编号</td><td>String</td><td>targetTradeId</td><td>--</td></tr>
<tr><td>交易所真正成交ID</td><td>String</td><td>marketTradeId</td><td>--</td></tr>
<tr><td>成交方向</td><td>String</td><td>side</td><td>--</td></tr>
<tr><td>成交开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>成交价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>成交数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>成交时间</td><td>Long</td><td>tradeTime</td><td>--</td></tr>
<tr><td>成交入库时间</td><td>Long</td><td>createTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>成交类型</td><td>String</td><td>tradeType</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
pm.query_trades(symbol='ag1906', start_time='20190628', end_time='20190629')
</pre>    
<h3>●输出结果</h3>
<pre>
[0: {'id': 101, 'orderId': 259, 'userId': 'test001', 'strategyId': 1, 'channelCode': nan, 'symbol': 'ag1906', 'targetTradeId': nan, 'side': 'B', 'effect': nan, 'price': 1.65, 'quantity': 150000, 'amount': nan, 'T': 20190505103307, 'tradeTime': 20190425103223, 'ext': nan, 'runId': 166, 'marketTradeId': nan, 'tradeType': nan, 'time': 1539829517360}]
</pre>

### 获取在途单量
<h3>●函数定义</h3>
<pre>pm.get_order_onroad_amt(symbol,side)</pre> 
<h3>●函数说明</h3>
<h3>获取策略维度在途单量</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约唯一代码</td><td>是</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>交易方向</td><td>是</td><td>B:买 S:卖</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>在途单金额</td><td>Number</td><td>amt</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
id1 = pm.limit_order('ag1906', 'S', 2, 10000, channel_code='SHFE')
id2 = pm.limit_order('ag1906', 'S', 5, 10000, channel_code='SHFE')
qlog.info(pm.get_order_onroad_amt('ag1906', 'S'))
</pre>
<h3>●输出结果</h3>
<pre>1000.00</pre>    

### 根据订单id查询交易
<h3>●函数定义</h3>
<pre>pm.get_order_by_id(order_id)</pre>
<h3>●函数说明</h3>
<h3>根据订单id查询交易</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>order_id</td><td>String</td><td>订单ID</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Long</td><td>id</td><td>--</td></tr>	
<tr><td>运行id</td><td>Long</td><td>runId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Long</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>系统中唯一合约代码号</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>订单类型</td><td>String</td><td>orderType</td><td>--</td></tr>
<tr><td>有效时间类型</td><td>String</td><td>timeInForce</td><td>--</td></tr>
<tr><td>到期时间</td><td>String</td><td>expireTime</td><td>--</td></tr>
<tr><td>报单价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>报单方向</td><td>String</td><td>side</td><td>--</td></tr>
<tr><td>报单开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>报单总数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>报单成交数量</td><td>Double</td><td>tradedQuantity</td><td>--</td></tr>
<tr><td>报单状态</td><td>String</td><td>orderStatus</td><td>初始CREATED:0,已提交Submit:9,<br>运行中New:1,订单拒绝Rejected:2,<br>订单撤销被拒绝CancelRejected:3,本地单超时撤销中ExpireCanceling:4,<br>订单已超时Expired:5,订单撤销中Canceling:6,<br>交易已撤销Canceled:7,已结束Finished:8;</td></tr>
<tr><td>订单损益</td><td>Double</td><td>profit</td><td>--</td></tr>
<tr><td>创建订单生成时间</td><td>Long</td><td>createTime</td><td>--</td></tr>
<tr><td>发单时间</td><td>Long</td><td>orderTime</td><td>--</td></tr>
<tr><td>撤单时间</td><td>Long</td><td>cancelTime</td><td>--</td></tr>
<tr><td>最后修改时间</td><td>Long</td><td>updateTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>订单关联关系</td><td>String</td><td>property</td><td>--</td></tr>
<tr><td>OCO订单关联ID</td><td>String</td><td>propertyId</td><td>--</td></tr>
<tr><td>内外部市场</td><td>String</td><td>inOutMarket</td><td>1-内部市场,3-内/外部市场</td></tr>
<tr><td>订单执行模式</td><td>String</td><td>model</td><td>--</td></tr>
<tr><td>订单时效时间参考的交易所</td><td>String</td><td>expireTimeOfExchange</td><td>--</td></tr>
<tr><td>价格ID</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>交易拒绝方</td><td>String</td><td>rejectSide</td><td>--</td></tr>
<tr><td>交易拒绝原因</td><td>String</td><td>errorMsg</td><td>--</td></tr>
<tr><td>已成交交易的平均价格</td><td>Double</td><td>tradedAvgPrice</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
id = pm.limit_order('ag1906', 'S', 5, 10000, channel_code='SHFE')
qlog.info_to_str(pm.get_order_by_id(id))
</pre>     
<h3>●输出结果</h3>
<pre>
[0:{'id': 13, 'userId': 'test001', 'strategyId': 1, 'channelCode': 'SHFE', 'symbol': 'ag1906', 'orderType': nan, 'timeInforce': nan, 'expireTime': nan, 'price': 12.2, 'side': 'B', 'effect': nan, 'quantity': 1000000, 'amount': nan, 'tradedQuantity': nan, 'orderStatus': 1, 'createTime': 20190428205246, 'orderTime': 20190425103223, 'cancelTime': nan, 'updateTime': nan, 'ext': nan, 'runId': 67, 'profit': 0, 'property': nan, 'propertyId': nan, 'inOutMarket': nan, 'model': nan, 'expireTimeOfExchange': nan, 'quoteId': nan, 'rejectSide': nan, 'errorMsg': nan, 'tradedAvgprice': nan, 'time': 1539829517360}]
</pre>
    
### 获取合约的头寸数量
<h3>●函数定义</h3>
<pre>pm.get_position_quantity(symbol,maturity_date,scope,merge)</pre>
<h3>●函数说明</h3>
<h3>获取合约的头寸数量</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
<tr><td>maturity_date</td><td>String</td><td>交割日(可为空)</td><td>是</td><td>--</td></tr>
<tr><td>scope</td><td>String</td><td>支持>= <=</td><td>是</td><td>--</td></tr>
<tr><td>merge</td><td>String</td><td>是否合并</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>头寸数量</td><td>Number</td><td>quantity</td><td>--</td></tr>	
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("合约XAUUSD的头寸数量")
data = pm.get_position_quantity('XAUUSD',None,None,True)
qlog_info(data)
qlog_info(data['DEFAULT'])
</pre>
<h3>●输出结果</h3>
<pre>
合约XAUUSD的头寸数量
{DEFAULT:10000}
10000
</pre>    
    
### 交易补录
<h3>●函数定义</h3>
<pre>pm.fx_order_bl(symbol, side, quantity, price, value_date,maturity_date,make_folder, in_out_market=None, channel_code=None, time_in_force=None, expire_time=None,hedge_flag=None)</pre>      
<h3>●函数说明</h3>
<h3>交易补录函数</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>买卖方向</td><td>是</td><td>--</td></tr>
<tr><td>quantity</td><td>String</td><td>交易量</td><td>是</td><td>--</td></tr>
<tr><td>price</td><td>String</td><td>交易价格</td><td>是</td><td>--</td></tr>
<tr><td>valueDate</td><td>String</td><td>近端交割日</td><td>是</td><td>--</td></tr>
<tr><td>maturityDate</td><td>String</td><td>远端交割日</td><td>是</td><td>--</td></tr>
<tr><td>make_folder</td><td>String</td><td>交易对手账户</td><td>是</td><td>--</td></tr>
<tr><td>channel_code</td><td>String</td><td>交易渠道</td><td>否</td><td>执行市场为1时交易渠道默认</td></tr>
<tr><td>in_out_market</td><td>String</td><td>内外部市场,执行市场<br>（默认2-外部市场）</td><td>否</td><td>2-外部市场</td></tr>
<tr><td>time_in_force</td><td>String</td><td>有效时间类型<br>(默认5-FAK)</td><td>否</td><td>5-FAK</td></tr>
<tr><td>expire_time</td><td>String</td><td>到期时间</td><td>否</td><td>GTD模式,expire_time是必填项,,<br>仅支持YYYYMMDD-HH:MM:SS</td></tr>
<tr><td>hedge_flag</td><td>String</td><td>投机套保标识<br>(默认1.普通交易)</td><td>否</td><td>1.普通交易2.投机交易3.套保交易</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>返回订单ID</td><td>Number</td><td>id</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
id = pm.fx_order_bl('ag1906', 'S', 10000, 2.76, '20181012', '20181015','1904535', channel_code='SHFE',
               in_out_market='2', time_in_force='5', expire_time='20190808-23:05:05', hedge_flag='1')
qlog.info(id)
</pre>    
<h3>●输出结果</h3>
<pre>9985</pre> 

### 获取今日头寸
<h3>●函数定义</h3>
<pre>pm.get_today_quantity(symbol,side,extStr)</pre>
<h3>●函数说明</h3>
<h3>获取今日头寸</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
<tr><td>side</td><td>String</td><td>买卖方向</td><td>是</td><td>--</td></tr>
<tr><td>extStr</td><td>String</td><td>意向成交</td><td>是</td><td>右交易人员自定义内容</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>今日头寸</td><td>Number</td><td>value</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取今日头寸")
qlog.info(pm.get_today_quantity('XAUUSD','B',"movepos_order"))
</pre>    
<h3>●输出结果</h3>
<pre>10000</pre>  

### 通过合约唯一代码获取今日头寸
 <h3>●函数定义</h3>
 <pre>pm.get_today_quantity_by_symbol(String symbol)</pre>
 <h3>●函数说明</h3>
 <h3>获取今日头寸</h3>
 <h3>●输入参数定义</h3>
 <table class="hovertable">
 <tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
 <tr><td>symbol</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
 </table>
 <h3>●输出字段解释</h3>
 <table class="hovertable">
 <tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
 <tr><td>今日头寸</td><td>Number</td><td>value</td><td>--</td></tr>
 </table>
 <pre>
 qlog.info("获取今日头寸")
 qlog.info(pm.get_today_quantity_by_symbol('XAUUSD'))
 </pre>    
    
### 获取主力合约和下一个主力合约
<h3>●函数定义</h3>
<pre>pm.get_fur_main_contract(product)</pre>
<h3>●函数说明</h3>
<h3>获取主力合约和下一个主力合约</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>product</td><td>String</td><td>金融产品</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>合约队列</td><td>list</td><td>value</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取主力合约和下一个主力合约")
list = pm.get_fur_main_contract('au1906')
qlog.info_f("主力合约为列表第一个:{}",list[0])
qlog.info_f("下一个主力合约为列表第二个:{}",list[1])
</pre>    
<h3>●输出结果</h3>
<pre>
获取主力合约和下一个主力合约
au1916
au2006
</pre>
    
### 获取期货的累计头寸
<h3>●函数定义</h3>
<pre>pm.get_futures_position_quantity(product)</pre>
<h3>●函数说明</h3>
<h3>获取期货的累计头寸</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>product</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>期货的累计头寸</td><td>Number</td><td>amt</td><td>单位为(手)</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取期货的累计头寸")
qlog.info(pm.get_futures_position_quantity('au1906'))
</pre>    
<h3>●输出结果</h3>
<pre>
获取主力合约和下一个主力合约
32442
</pre>

### 发送监控信息
<h3>●函数定义</h3>
<pre>pm.send_monitor_msg(text)</pre>
<h3>●函数说明</h3>
<h3>发送监控信息</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>text</td><td>String</td><td>监控信息的文本信息</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>●无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info("发送监控信息")
pm.send_monitor_msg('监控异常信息')
</pre>
<h3>●输出结果</h3>
<pre>
发送监控信息
</pre>

### 获取产品下的那一天的合约净值
<h3>●函数定义</h3>
<pre>pm.get_quantity_by_prod_date(product,date)</pre>
<h3>●函数说明</h3>
<h3>获取产品下的那一天的合约净值</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>product</td><td>String</td><td>合约</td><td>是</td><td>--</td></tr>
<tr><td>date</td><td>String</td><td>日期</td><td>是</td><td>支持日期格式:YYYYMMDD</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>合约净值结果集合</td><td>map</td><td>value</td><td></td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取产品下的那一天的合约净值")
data = pm.get_quantity_by_prod_date('au2006',20191226)
qlog.info(data['au2006'])
</pre>    
<h3>●输出结果</h3>
<pre>
获取产品下的那一天的合约净值
2465000
</pre>


### 创建止损订单
<h3>●函数定义</h3>
<pre>
'''函数调用方法'''
pm.stop_limit_order(symbol, side,  price, stopprice,price2,quantity, in_out_market=None, channel_code=None, time_in_force=None, expire_time=None,hedge_flag=None)
</pre>
<h3>●函数说明</h3>
<h3>创建止损订单</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>String</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>side</td><td>String</td><td>交易方向</td><td>是</td><td>限(B,S)买和卖</td></tr>
<tr><td>price</td><td>String</td><td>价格</td><td>是</td><td>--</td></tr>
<tr><td>stopprice</td><td>String</td><td>止损价</td><td>是</td><td>--</td></tr>
<tr><td>price2</td><td>String</td><td>再次开仓价</td><td>是</td><td>--</td></tr>
<tr><td>in_out_market</td><td>String</td><td>市场</td><td>否</td><td>默认2-外部市场,等于2时必须入参channel_code渠道</td></tr>
<tr><td>channel_code</td><td>String</td><td>交易渠道</td><td>否</td><td>--</td></tr>
<tr><td>time_in_force</td><td>String</td><td>有效时间类型</td><td>否</td><td>3:指定时间有效,等于3时必须入参expire_time;1:交易日有效;2:手工未撤销永久有效;3:指定时间有效;
		4:极短时间全部成交，否则全部撤销;5:极短时间成交，剩余量全部撤销;6:本节交易时段有效</td></tr>
<tr><td>expire_time</td><td>String</td><td>失效时间</td><td>否</td><td>YYYYMMDD HH:MM:SS格式</td></tr>
<tr><td>hedge_flag</td><td>String</td><td>投机套保标志</td><td>否</td><td>默认1;1:普通交易;2:投机交易;3:套保交易</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>订单编号</td><td>String</td><td>--</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('创建止损订单')
orderId = pm.stop_limit_order('Ag(T+D)','B',2.1,1.9,1.89,1000, in_out_market=None,channel_code=None, time_in_force=None, expire_time=None,hedge_flag=None)
qlog.info('创建止损订单结束')
</pre>

### 获取策略维度今日损益
<h3>●函数定义</h3>
<pre>
'''函数调用方法'''
pm.get_day_pnl(time=None)
</pre>
<h3>●函数说明</h3>
<h3>获取策略维度今日损益</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>time</td><td>String</td><td>站位符</td><td>是</td><td>任意值</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>今日损益</td><td>Number</td><td>--</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('获取策略维度今日损益')
orderId = pm.get_day_pnl(time=None)
qlog.info('获取策略维度今日损益结束')
</pre>

###  获取合约对象
<h3>●函数定义</h3>
<pre>
'''函数调用方法'''
pm.get_contract('Ag(T+D)',time=None)
</pre>
<h3>●函数说明</h3>
<h3>获取合约对象</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>String</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>time</td><td>String</td><td>站位符</td><td>是</td><td>任意值</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>code</td><td>String</td><td>合约编码</td><td>--</td></tr>
<tr><td>Name</td><td>String</td><td>合约英文名称</td><td>--</td></tr>
<tr><td>localName</td><td>String</td><td>本地名称</td><td>--</td></tr>
<tr><td>productBroad</td><td>String</td><td>产品大类</td><td>对应金融工具所属组</td></tr>
<tr><td>products</td><td>String</td><td>产品小类</td><td>对应金融工具分类中的金融工具</td></tr>
<tr><td>contractType</td><td>String</td><td>合约类型</td><td>B:基础合约;D:基差合约;T:期差合约;S:连续合约;M:月份合约;N:非标准合约</td></tr>
<tr><td>status</td><td>String</td><td>合约状态</td><td>N:未生效;V:已生效;I:已失效</td></tr>
<tr><td>dealType</td><td>string</td><td>交易品种</td><td>--</td></tr>
<tr><td>dealTypeGroup</td><td>String</td><td>组合交易品种</td><td>--</td></tr>
<tr><td>tenor</td><td>String</td><td>期限</td><td>--</td></tr>
<tr><td>tenorGroup</td><td>String</td><td>组合期限</td><td>--</td></tr>
<tr><td>valueDateRule</td><td>String</td><td>起息日规则</td><td>--</td></tr>
<tr><td>startDate</td><td>String</td><td>开始交易日</td><td>--</td></tr>
<tr><td>lastDate</td><td>String</td><td>最后交易日或到期日</td><td>--</td></tr>
<tr><td>quoteCurrency</td><td>String</td><td>报价货币</td><td>--</td></tr>
<tr><td>quoteUnit</td><td>String</td><td>报价单位</td><td>--</td></tr>
<tr><td>noDecimal</td><td>int</td><td>报价有效位数</td><td>--</td></tr>
<tr><td>contractMultiplier</td><td>double</td><td>合约乘数</td><td>--</td></tr>
<tr><td>market</td><td>String</td><td>交易市场</td><td>--</td></tr>
<tr><td>sites</td><td>String</td><td>交易主体</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('获取合约对象')
orderId = pm.get_contract('Ag(T+D)',time=None)
qlog.info('获取合约对象结束')
</pre>
<h3>●输出结果</h3>
<pre>
text:"获取合约对象"
text:{"code":"Au(T+D)","contractMultiplier":1.0,"contractType":"B","dealType":"AUXCNY","dealTypeGroup":"","id":0,"lastDate":"20200831","localName":"Au(T+D)","market":"SGE","name":"Au(T+D)","noDecimal":2,"productBroad":"PM","products":"3000119","quoteCurrency":"CNY","quoteUnit":"K","sites":"CITIC","startDate":"20200823","status":"V","tenor":"TODAY","tenorGroup":"SPOT","timeStamp":1598378592000,"valueDateRule":"T+0"}
text:"获取合约对象结束"
</pre>

### 判断合约是否可以交易
<h3>●函数定义</h3>
<pre>
'''函数调用方法'''
pm.check_symbol_isdeal(time=None)
</pre>
<h3>●函数说明</h3>
<h3>判断合约是否可以交易</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>time</td><td>String</td><td>站位符</td><td>是</td><td>任意值</td></tr>
</table>
<h3>●输出参数解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>是否可以交易</td><td>Boolean</td><td>--</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('判断合约是否可以交易')
orderId = pm.check_symbol_isdeal(time=None)
qlog.info('判断合约是否可以交易结束')
</pre>
<h1 style="text-align: center"> 工具类处理模块 </h1>
<script>
        function onlink(document,point) {
            documentDetailsModal.showDocumentDetails(document,this);
            documentDetailsModal.getDocPoint(document,point,this)
        }
</script>

---
### 行情数据订阅
<h3>●函数定义</h3>
<pre>
context.subscribe(market,type,symbols)
</pre>
<h3>●函数说明</h3>
<h3>行情数据订阅(需要在init方法里面执行订阅方法),根据订阅内容触发onData函数.</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>market</td><td>String</td><td>市场</td><td>是</td><td>请对应以下type清单表</td></tr>
<tr><td>type</td><td>String</td><td>数据类型</td><td>是</td><td>请对应以下type清单表</td></tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td></td></tr>
</table>
<h3>●订阅市场数据清单</h3>
<table class="hovertable">
<tr><th>市场(market)</th><th>数据类型(type)</th><th>数据说明</th><tr>
<tr><td>DEFAULT</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point0&quot;,this)">ComIpmBest</p></td><td>四路外资行整合最优价</td></tr>
<tr><td>SHFE</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point1&quot;,this)">ComFutPubOdm</p></td><td>期交所深度行情</td></tr>
<tr><td>SGE</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point2&quot;,this)">ComBidPubOdm</p></td><td>金交所深度行情</td></tr>
<tr><td>IPM</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point3&quot;,this)">ComIpmPubOdm</p></td><td>外资行深度行情</td></tr>
<tr><td>CMDS</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point4&quot;,this)">SpPubOdm</p></td><td>CMDS外币深度行情</td></tr>
<tr><td>DEFAULT</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point6&quot;,this)">ComIpmAver</p></td><td>外资行每日结算价</td></tr>
<tr><td>DEFAULT</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point7&quot;,this)">Spread</p></td><td>价差数据</td></tr>
<tr><td>DEFAULT</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point8&quot;,this)">ComPubOdmBar</p></td><td>外资行整合最优价Bar数据</td></tr>
<tr><td>DEFAULT</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point9&quot;,this)">ShPubOdmBar</p></td><td>期交所深度行情Bar数据</td></tr>
<tr><td>DEFAULT</td><td><p class="buttons" onclick="onlink(&quot;api_pm_data.md&quot;,&quot;point10&quot;,this)">SgPubOdmBar</p></td><td>金交所深度行情Bar数据</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>●无</h3>
<h3>●API执行范例</h3>
<pre>
'''策略初始化内容,订阅商品现货深度行情(bar)Ag(T+D)合约行情'''
def init(context):
    context.subscribe("DEFAULT", "SgPubOdmBar", ["Ag(T+D)"])
    pm.init_cache("DEFAULT", "SgPubOdmBar", ["Ag(T+D)"])
</pre>
<h3>●输出结果</h3>
<pre>无</pre>

### 指定日期偏移
<h3>●函数定义</h3>
<pre>day_offset(holidayCode, nums, date, period, days)</pre>
<h3>●函数说明</h3>
<h3>根据ecas模块中(静态数据管理->假日规则)配置的节假日关联.计算日期偏移量.<br></h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>holidayCode</td><td>[String]</td><td>假日Code</td><td>是</td><td>根据SDS配置Code（例:["CNY"]）</td></tr>
<tr><td>nums</td><td>Number</td><td>偏移周期数</td><td>是</td><td>支持任何整数</td></tr>
<tr><td>date</td><td>Number</td><td>参考时间</td><td>是</td><td>日期格式只支持YYYYMMDD</td></tr>
<tr><td>period</td><td>String</td><td>偏移周期单位</td><td>是</td><td>枚举:日:D,月:M,年:Y</td></tr>
<tr><td>days</td><td>String</td><td>日期类型</td><td>是</td><td>交易日:TRADING,自然日:NATURAL</td></tr>  
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>偏移后的日期</td><td>Number</td><td>date</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('20190228日期往后按工作日往后延5个工作日')
qlog.info(date.day_offset(['CNY'], 5, 20190228, 'D', 'trading'))
</pre>
<h3>●输出结果</h3>
<pre>
text:"20190228日期往后按工作日往后延5个工作日"
text:20190305
</pre>

### 指定日期校验
<h3>●函数定义</h3>
<pre>date.day_check(holidayCode,days,date)</pre>
<h3>●函数说明</h3>
<h3>根据ecas模块中(静态数据管理->假日规则)配置的节假日关联.指定日期校验.</h3>
<h3>●输入参数解释</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>holidayCode</td><td>[String]</td><td>假日Code</td><td>是</td><td>根据SDS配置Code（例:["CNY"])</td></tr>
<tr><td>days</td><td>String</td><td>日期类型</td><td>是</td><td>交易日:TRADING,自然日:NATURAL,<br>节假日:HOLIDAY</td></tr> 
<tr><td>date</td><td>Number</td><td>参考时间</td><td>是</td><td>日期格式只支持YYYYMMDD</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>是否为节假日</td><td>布尔值</td><td>result</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('校验20190307是否为CNY假日配置内的工作日')
qlog.info(date.day_check(['CNY'], 'TRADING', 20190307))
</pre>
<h3>●输出结果</h3>
<pre>
text:"校验20190307是否为CNY假日配置内的工作日"
text:true
</pre>

### 获取日期区间
<h3>●函数定义</h3>
<pre>date.day_filter(holidayCode,days,startDate,endDate)</pre>
<h3>●函数说明</h3>
<h3>根据ecas模块中(静态数据管理->假日规则)配置的节假日关联.获取日期区间。<br>
从开始日期到结束日期包头也包尾,闭合区间内的日期进行统计</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>symbols</td><td>[String]</td><td>合约</td><td>是</td><td>需要输入对应合约</td></tr>
<tr><td>holidayCode</td><td>[String]</td><td>假日Code</td><td>是</td><td>根据SDS配置Code（例:["CNY"]）</td></tr>
<tr><td>days</td><td>String</td><td>日期类型</td><td>是</td><td>交易日:TRADING,自然日:NATURAL,<br>节假日:HOLIDAY</td></tr>  
<tr><td>startDate</td><td>Number</td><td>开始日期</td><td>是</td><td>日期格式只支持YYYYMMDD,开始日期必须小于结束日期</td></tr>
<tr><td>endDate</td><td>Number</td><td>结束日期</td><td>是</td><td>日期格式只支持YYYYMMDD,结束日期必须大于开始日期</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>日期数组</td><td>[Number]</td><td>result</td><td>含起始日期和结束日期</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('20190302到20190307的工作日是哪几天')
qlog.info(date.day_filter(['CNY'], 'TRADING', 20190302, 20190307))
</pre>
<h3>●输出结果</h3>
<pre>
text:"20190302到20190307的工作日是哪几天"
text:[20190302,20190303,20190304,20190305,20190306,20190307]
</pre>

### 获取日期数量
<h3>●函数定义</h3>
<pre>date.day_nums(holidayCode,days,startDate,endDate)</pre>
<h3>●函数说明</h3>
<h3>根据ecas模块中(静态数据管理->假日规则)配置的节假日关联.获取日期数量.<br>
从开始日期到结束日期包头也包尾,闭合区间内的日期进行统计</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>holidayCode</td><td>[String]</td><td>假日Code</td><td>是</td><td>根据SDS配置Code（例:["CNY"]）</td></tr>
<tr><td>days</td><td>String</td><td>日期类型</td><td>是</td><td>交易日:TRADING,自然日:NATURAL,<br>节假日:HOLIDAY</td></tr>  
<tr><td>startDate</td><td>Number</td><td>开始日期</td><td>是</td><td>日期格式只支持YYYYMMDD,开始日期必须小于结束日期</td></tr>
<tr><td>endDate</td><td>Number</td><td>结束日期</td><td>是</td><td>日期格式只支持YYYYMMDD,结束日期必须大于开始日期</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>日期数量</td><td>Number</td><td>result</td><td>含起始日期和结束日期</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('20190302到20190307的工作日一共有几天')
qlog.info(date.day_nums(['CNY'], 'trading',20190302, 20190307))
</pre>
<h3>●输出结果</h3>
<pre>
text:"20190302到20190307的工作日一共有几天"
text:6
</pre>

### 日志记录类
<h3>●函数定义</h3>
<pre>qlog.info(msg)</pre>
<h3>●函数说明</h3>
<h3>记录策略执行日志内容提供下载</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>msg</td><td>String</td><td>日志打印内容</td><td>是</td><td>--</td></tr></table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>日志内容</td><td>String</td><td>result</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>qlog.info("日期:20170301,星期:星期三")</pre>
<h3>●输出结果</h3>
<pre>日期:20170301,星期:星期三</pre> 

### 日志记录类
<h3>●函数定义</h3>
<pre>qlog.info_f(format,arg1,arg2...)</pre>
<h3>●函数说明</h3>
<h3>记录策略执行日志内容提供下载</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>format</td><td>String</td><td>日志打印format</td><td>是</td><td>--</td></tr>
<tr><td>arg1</td><td>Object</td><td>日志打印第一个占位值</td><td>否</td><td>--</td></tr>
<tr><td>arg2</td><td>Object</td><td>日志打印第二个占位值</td><td>否</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>日志内容</td><td>String</td><td>result</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>qlog.info_f("日期:{},星期:{}","20170301","星期三")</pre>
<h3>●输出结果</h3>
<pre>日期:20170301,星期:星期三</pre>

### 获取系统时间
<h3>●函数定义</h3>
<pre>date.get_sys_time(formart)</pre>
<h3>●函数说明</h3>
<h3>获取当前系统的系统时间(公共数据管理->系统参数中维护的系统时间)</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>formart</td><td>String</td><td>日期返回格式</td><td>是</td><td>使用具体格式时请与接口人员联系</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统日期</td><td>String</td><td>data</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('获取系统时间')
qlog.info(date.get_sys_time('yyyyMMdd'))
</pre>
<h3>●输出结果</h3>
<pre>
text:"获取系统时间"
text:"20190128"
</pre>     

### 单位换算函数
<h3>●函数定义</h3>
<pre>pm.unit_trans(nowUnit,amt,targetUnit)</pre>   
<h3>●函数说明</h3>
<h3>贵金属中1.Kg,2.g,3.ounce三个单位间的转化(1金衡盎司 = 31.1035克)</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>nowUnit</td><td>String</td><td>当前单位量</td><td>是</td><td>支持量:1.Kg,2.g,3.ounce</td></tr>
<tr><td>amt</td><td>Number</td><td>转换量</td><td>是</td><td>--</td></tr>
<tr><td>targetUnit</td><td>String</td><td>转换结果单位量</td><td>是</td><td>支持量:1.Kg,2.g,3.ounce</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>转换量</td><td>Number</td><td>amt</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info('单位换算千克换算成克')
qlog.info(pm.unit_trans('Kg', 10000, 'g'))
</pre>
<h3>●输出结果</h3>
<pre>
text:"单位换算千克换算成克"
text:1.0E7
</pre>    

### 获取业务时间
<h3>●函数定义</h3>
<pre>date.get_bus_day(type)</pre>    
<h3>●函数说明</h3>
<h3>获取当前系统的系统时间(公共数据管理->系统参数中维护的业务时间)</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>type</td><td>String</td><td>日期SDS配置的type</td><td>是</td><td>--</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统日期</td><td>Number</td><td>data</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
qlog.info("获取贵金属业务时间")
qlog.info(date.get_bus_day('bstk_bussiness_date'))
</pre>   
<h3>●输出结果</h3>
<pre>
text:"获取贵金属业务时间"
text:20190128
</pre>      

### init函数 (策略中必须添加的函数)
<h3>●函数定义</h3>
<pre>init(context)</pre>    
<h3>●函数说明</h3>
<h3>策略运行初始化方法(策略运行初始化触发,该方法必须存在于策略中)，策略开始触发后主动执行.</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>策略上下文</td><td>Object</td><td>context</td><td>默认参数,由后台服务传输</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
def init(context):
    qlog.info('触发init')
</pre>   
<h3>●输出结果</h3>
<pre>
触发init
</pre>

### onData函数 (策略中必须添加的函数)
<h3>●函数定义</h3>
<pre>onData(context, data)</pre>    
<h3>●函数说明</h3>
<h3>订阅的行情类型数据触发(由context.subscribe订阅触发,同一个时间点订阅数据行情触发一次）,<br>数据data结构由订阅数据类型决定,具体参照订阅类型数据关联.</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>策略上下文</td><td>Object</td><td>context</td><td>--</td></tr>
<tr><td>data</td><td>[Object]</td><td>订阅数据集合</td><td>返回Object结构参照订阅市场数据清单</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
def onData(context, data):
    qlog.info("触发onData")
</pre>
<h3>●输出结果</h3>
<pre>
触发onData订阅行情数据:{"symbol":"Ag(T+D)","product":"Ag(T+D)","updateDate":20200828,"updateTime":152600000,"TIME":1598599560000,"source":"DEFAULT","timespan":1598599560000,"frequency":"1N","rowId":748926478875361280,"high":4160.51,"sysUpdateDate":20200828,"low":4160.51,"time":1598599560000,"close":4160.51,"open":4160.51,"sysUpdateTime":152626945}
</pre>

### onOrder函数 (策略中必须添加的函数)
<h3>●函数定义</h3>
<pre>onOrder(context, data)</pre>    
<h3>●函数说明</h3>
<h3>交易状态变化触发(策略维度订单交易状态变化后触发函数),订单状态发生变化后调用onOrder函数,<br>data为订单数据类型.</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>策略上下文</td><td>Object</td><td>context</td><td>--</td></tr>
<tr><td>data</td><td>[OrderDO]</td><td>订单对象:具体可参照获取订单明细列表函数结构</td><td>--</td></tr>
</table>
<h3>●OrderDO字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Number</td><td>id</td><td>--</td></tr>			
<tr><td>运行id</td><td>Number</td><td>runId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Number</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>系统中唯一合约代码号</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>订单类型</td><td>String</td><td>orderType</td><td>--</td></tr>
<tr><td>有效时间类型</td><td>String</td><td>timeInForce</td><td>--</td></tr>
<tr><td>到期时间</td><td>String</td><td>expireTime</td><td>--</td></tr>
<tr><td>报单价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>报单方向</td><td>String</td><td>side</td><td>--</td></tr>
<tr><td>报单开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>报单总数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>报单已成交数量</td><td>Double</td><td>tradedQuantity</td><td>--</td></tr>
<tr><td>已撤销</td><td>Double</td><td>cancelQuantity</td><td>--</td></tr>
<tr><td>未明量</td><td>Double</td><td>unkownQuantity</td><td>--</td></tr>
<tr><td>进行中</td><td>Double</td><td>doingQuantity</td><td>--</td></tr>
<tr><td>报单状态</td><td>String</td><td>orderStatus</td><td>初始CREATED:0,已提交Submit:9,<br>运行中New:1,订单拒绝Rejected:2,<br>订单撤销被拒绝CancelRejected:3,本地单超时撤销中ExpireCanceling:4,<br>订单已超时Expired:5,订单撤销中Canceling:6,<br>交易已撤销Canceled:7,已结束Finished:8;</td></tr>
<tr><td>订单损益</td><td>Double</td><td>profit</td><td>--</td></tr>
<tr><td>创建订单生成时间</td><td>Number</td><td>createTime</td><td>--</td></tr>
<tr><td>发单时间</td><td>Number</td><td>orderTime</td><td>--</td></tr>
<tr><td>撤单时间</td><td>Number</td><td>cancelTime</td><td>--</td></tr>
<tr><td>最后修改时间</td><td>Number</td><td>updateTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>订单关联关系</td><td>String</td><td>property</td><td>1-Single, 2-OCO</td></tr>
<tr><td>OCO订单关联ID</td><td>String</td><td>propertyId</td><td>--</td></tr>
<tr><td>内外部市场</td><td>String</td><td>inOutMarket</td><td>1-内部市场,2-外部市场,3-内/外部市场</td></tr>
<tr><td>订单执行模式</td><td>String</td><td>model</td><td>1-FullAmount,2-Sweepable</td></tr>
<tr><td>订单时效时间参考的交易所</td><td>String</td><td>expireTimeOfExchange</td><td>--</td></tr>
<tr><td>价格ID</td><td>String</td><td>quoteId</td><td>--</td></tr>
<tr><td>交易拒绝方</td><td>String</td><td>rejectSide</td><td>--</td></tr>
<tr><td>交易拒绝原因</td><td>String</td><td>errorMsg</td><td>--</td></tr>
<tr><td>已成交交易的平均价格</td><td>Double</td><td>tradedAvgPrice</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
def onOrder(context, data):
    qlog.info_f('触发onOrder,order信息:{}',data)
</pre>
<h3>●输出结果</h3>
<pre>
触发onOrder,order信息:[{"cancelQuantity":0.0,"channelCode":"CMDS","createTime":20200201165814,"doingQuantity":1000000.0,"expireTime":"20201002","financialMarket":"I","financialTool":"U","id":673210517296254976,"inOutMarket":"1","model":"2","orderStatus":"9","orderTime":20190128100000,"orderType":"2","price":2.7,"product":"FR007","profit":0.0,"property":"1","quantity":1000000.0,"runId":3450,"side":"S","symbol":"FR007_1Y","timeInForce":"1","tradedQuantity":0.0,"unkownQuantity":0.0,"userId":"111","version":1}]
</pre>

### onTrade函数 (策略中必须添加的函数)
<h3>●函数定义</h3>
<pre>onTrade(context, data)</pre>    
<h3>●函数说明</h3>
<h3>策略维度订单交易状态变更为成交状态后触发onTrade,在onTrade之前会先因为订单状态的变化触发onOrder,<br>data数据为成交订单数据.结构为订单数据结构.</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>策略上下文</td><td>Object</td><td>context</td><td>--</td></tr>
<tr><td>data</td><td>TradeDO</td><td>成交对象:具体可参照获取成交明细列表函数结构</td><td>--</td></tr>
</table>
<table class="hovertable">
<tr><th>TradeDO解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>系统中唯一流水号</td><td>Number</td><td>id</td><td>--</td></tr>
<tr><td>运行id</td><td>Number</td><td>runId</td><td>--</td></tr>
<tr><td>订单编号</td><td>Number</td><td>orderId</td><td>--</td></tr>
<tr><td>用户Id</td><td>String</td><td>userId</td><td>--</td></tr>
<tr><td>策略ID</td><td>Number</td><td>strategyId</td><td>--</td></tr>
<tr><td>交易渠道</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>合约</td><td>String</td><td>symbol</td><td>--</td></tr>
<tr><td>撮合引擎系统中的唯一编号</td><td>String</td><td>targetTradeId</td><td>--</td></tr>
<tr><td>交易所真正成交ID</td><td>String</td><td>marketTradeId</td><td>--</td></tr>
<tr><td>成交方向</td><td>String</td><td>side</td><td>--</td></tr>
<tr><td>成交开平仓</td><td>String</td><td>effect</td><td>--</td></tr>
<tr><td>成交价格</td><td>Double</td><td>price</td><td>--</td></tr>
<tr><td>成交数量</td><td>Double</td><td>quantity</td><td>--</td></tr>
<tr><td>成交金额\成本金额</td><td>Double</td><td>amount</td><td>--</td></tr>
<tr><td>成交时间</td><td>Number</td><td>tradeTime</td><td>--</td></tr>
<tr><td>成交入库时间</td><td>Number</td><td>createTime</td><td>--</td></tr>
<tr><td>扩展字段</td><td>String</td><td>ext</td><td>--</td></tr>
<tr><td>成交类型</td><td>String</td><td>tradeType</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
def onTrade(context, data):
    qlog.info_f('触发onTrade,trade信息:{}',data)
</pre> 
<h3>●输出结果</h3>
<pre>
触发onTrade,trade信息:[{"channelCode":"CMDS","createTime":20200201165814,"delStatus":0,"financialTool":"U","id":673210517375946752,"orderId":673210517296254976,"price":2.725,"quantity":1000000.0,"runId":3450,"side":"S","symbol":"FR007_1Y","tradeTime":20190128100000,"userId":"1111"}]
</pre>

### onMonitor函数 (策略中必须添加的函数)
<h3>●函数定义</h3>
<pre>onMonitor(context,monitors)</pre>    
<h3>●函数说明</h3>
<h3>行情信息监控链路发生变化后触发onMonitor函数,将链路信息返回函数.</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>策略上下文</td><td>Object</td><td>context</td><td>--</td></tr>
<tr><td>链路</td><td>monitor</td><td>monitors</td><td>--</td></tr>
</table>
<table class="hovertable">
<tr><th>链路结构解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>链路状态</td><td>String</td><td>type</td><td>--</td></tr>
<tr><td>渠道名称</td><td>String</td><td>channelCode</td><td>--</td></tr>
<tr><td>监控对象</td><td>String</td><td>object</td><td>--</td></tr>
<tr><td>状态</td><td>String</td><td>state</td><td>--</td></tr>
<tr><td>描述</td><td>String</td><td>msg</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
def onMonitor(context, data):
    qlog.info_f('触发onMonitor,onMonitor:{}',data)
</pre>   
<h3>●输出结果</h3>
<pre>
触发onMonitor,onMonitor:
</pre>

### onTime函数 (策略中必须添加的函数)
<h3>●函数定义</h3>
<pre>onTime(context,name,time)</pre>
<h3>●函数说明</h3>
<h3>通过run_daily函数定义触发器,定时触发器触发后调用的函数,onTime函数触发</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>策略上下文</td><td>Object</td><td>context</td><td>--</td></tr>
<tr><td>name</td><td>String</td><td>定时触发器名称</td><td>--</td></tr>
<tr><td>time</td><td>String</td><td>定时任务触发时间</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
def onTime(context,name,timer):
    qlog.info_f('触发onTime,name:{},timer:{}',name,timer)
</pre> 
<h3>●输出结果</h3>
<pre>触发onTime,name:"roll_job",timer:"20190119033320"</pre>

### run_daily函数
<h3>●函数定义</h3>
<pre>run_daily(name,timer)</pre>    
<h3>●函数说明</h3>
<h3>在上下文中定义某个时间点的触发器,触发后调用onTime函数,将触发器信息内容在onTime中使用.</h3>
<h3>●输入参数定义</h3>
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>name</td><td>String</td><td>触发器名称</td><td>是</td><td>自定义触发器名称,同一个策略不允许有两个同名的触发器</td></tr>
<tr><td>timer</td><td>String</td><td>时间</td><td>是</td><td>定时任务时间,日期格式HHmmSS</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>●无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info("定义一个名字为roll_pos_job的定时器,每天16点触发onTime方法")
context.run_daily("roll_pos_job", "160000")
</pre>
<h3>●输出结果</h3>
<pre>无</pre>

### run_second函数
<h3>●函数定义</h3>
<pre>run_second(name,timer)</pre>    
<h3>●函数说明</h3>
<h3>在上下文中定义循环触发定时器,时间设置只支持60秒以内.触发onTime方法执行.</h3>
<h3>●输入参数定义</h3>
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>name</td><td>String</td><td>触发器名称</td><td>是</td><td>循环定时触发器名称,同一个策略不允许有两个同名的触发器</td></tr>
<tr><td>timer</td><td>String</td><td>时间</td><td>是</td><td>单位秒,只支持60秒范围以内的设置.</td></tr>
</table>
<h3>●输出字段解释</h3>
<h3>●无</h3>
<h3>●API执行范例</h3>
<pre>
qlog.info("定义一个名字为roll_job的循环触发器,每隔40秒触发onTime方法")
context.run_second("roll_job", '40')
</pre>
<h3>●输出结果</h3>
<pre>无</pre>

---
### param
<h3>●自定义参数函数</h3>
<pre>p.param(key)</pre>    
<h3>●函数说明</h3>
<h3>策略配置的参数获取函数,在策略维度使用客户端配置的启动参数.</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>key</td><td>String</td><td>参数名称</td><td>是</td><td>仅支持中英文大小写,下划线和中括号.</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>value</td><td>Object</td><td>value</td><td>仅支持中英文大小写,下划线和中括号.</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
策略参数配置一个(product="FR007")的配置,然后获取配置的值
'''
qlog.info(p.param('product'))
</pre>   
<h3>●输出结果</h3>
<pre>
text:"FR007"
</pre>

### matrix
<h3>●自定义参数函数</h3>
<pre>p.matrix(fileName)</pre>    
<h3>●函数说明</h3>
<h3>策略配置的参数获取函数,在策略维度使用客户端配置的ndarray启动参数,由固定Excel模板导入数据.仅支持中英文大小写,下划线和中括号.</h3>
<h3>●输入参数定义</h3>
<table class="hovertable">
<tr><th>输入参数名</th><th>数据类型</th><th>描述</th><th>是否必填</th><th>备注说明</th><tr>
<tr><td>fileName</td><td>String</td><td>名称</td><td>是</td><td>Excel中sheet页的名称</td></tr>
</table>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>value</td><td>ndarray</td><td>value</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
'''
策略参数配置一个sheet页为Matrix的名字,然后获取Matrix的ndarray结构数据
'''
ndarray = p.matrix("Matrix")
qlog.info(ndarray[0][0])
</pre>
<h3>●输出结果</h3>
<pre>
text:"1000000.00"
</pre>

### onBusinessDate函数 (策略中必须添加的函数)
<h3>●函数定义</h3>
<pre>onBusinessDate(context, data)</pre>    
<h3>●函数说明</h3>
<h3>系统切日批量执行触发函数onBusinessDate,系统现在存在三个日期切日动作,<br>凌晨4点国际贵金属切日,下午4点半国内贵金属切日.</h3>
<h3>●输入参数定义</h3>
<h3>无</h3>
<h3>●输出字段解释</h3>
<table class="hovertable">
<tr><th>字段名称解释</th><th>字段类型</th><th>字段名称</th><th>备注说明</th></tr>
<tr><td>策略上下文</td><td>Object</td><td>context</td><td>--</td></tr>
<tr><td>data</td><td>[CutingDate]</td><td>切日对象列表:sendDate(发送日期),<br>sendTime(发送时间),<br>instrument(产品类型BSTK-商品),<br>beforeDate(切日前日期)<br>afterDate(切日后日期)<br>nowTime(现在时间)</td><td>--</td></tr>
</table>
<h3>●API执行范例</h3>
<pre>
def onBusinessDate(context,data):
    qlog.info_f('触发onBusinessDate,data:{}',data)
</pre>
<h3>●输出结果</h3>
<pre>
触发onBusinessDate,data:[{"afterDate":20190119,"beforeDate":20190118,"instrument":"BSTK","nowTime":20190118163000}]
</pre>
# 量化系统API文档

### 策略使用及事项 api.md

### 策略及案例说明 api_example.md

### 贵金属数据模块 api_pm_data.md

### 贵金属交易模块 api_pm_trade.md

### 数据预处理模块 api_deal_data.md

### 工具类处理模块 api_tool.md


