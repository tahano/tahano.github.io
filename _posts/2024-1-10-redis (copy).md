---
layout:   post
title:    信道分配
subtitle:   
date:       2024-1-10
author:     tahano
header-img: img/back2.jpg
catalog: true
tags:
    - 计算机网络 
    - 通信 
    - 信号与系统
---

<p>注：是搜集总结的知识点</p> 
<h3><a name="t1"></a><a id="_2"></a>一.两种比较符号</h3> 
<p><code>==</code> 和 <code>===</code><br> <code>==</code> 在进行比较的时候，会<code>先将字符串类型转化成相同类型</code>，再进行比较。<br> <code>===</code> 在进行比较的时候，会<code>先判断两种字符串的类型是否相等</code>，再进行比较。<br> 如果一个数值和字符串进行比较的时候，会将字符串转换成数值。<br> 看几个具体的例子：<br> 1.</p> 
<pre data-index="0" class="set-code-show prettyprint"><code class="prism language-php has-numbering" onclick="mdcp.signin(event)" style="position: unset;"><span class="token function">var_dump</span><span class="token punctuation">(</span><span class="token string double-quoted-string">"admin"</span><span class="token operator">==</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token comment">//正确</span>
<div class="hljs-button signin active add_def" data-title="登录复制" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.4334&quot;}"></div></code><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li></ul><button class="btn-code-notes mdeditor" data-report-click="{&quot;spm&quot;:&quot;3001.10436&quot;,&quot;extra&quot;:{&quot;index&quot;:0,&quot;runIdx&quot;:-1}}">AI助手</button></pre> 
<p>将admin转化成数值，<code>强制转化</code>,由于admin是字符串，转化的结果是<code>0和0相等</code><br> 2.</p> 
<pre data-index="1" class="set-code-show prettyprint"><code class="prism language-php has-numbering" onclick="mdcp.signin(event)" style="position: unset;"><span class="token function">var_dump</span><span class="token punctuation">(</span><span class="token string double-quoted-string">"admin1"</span><span class="token operator">==</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token comment">//错误</span>
<div class="hljs-button signin active add_def" data-title="登录复制" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.4334&quot;}"></div></code><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li></ul><button class="btn-code-notes mdeditor" data-report-click="{&quot;spm&quot;:&quot;3001.10436&quot;,&quot;extra&quot;:{&quot;index&quot;:1,&quot;runIdx&quot;:-1}}">AI助手</button></pre> 
<p>这里是将<code>admin1</code>转化成了<code>0</code></p> 
<ol start="3"><li></li></ol> 
<pre data-index="2" class="set-code-show prettyprint"><code class="prism language-php has-numbering" onclick="mdcp.signin(event)" style="position: unset;"><span class="token function">var_dump</span><span class="token punctuation">(</span><span class="token string double-quoted-string">"1admin"</span><span class="token operator">==</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token comment">//正确</span>
<div class="hljs-button signin active add_def" data-title="登录复制" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.4334&quot;}"></div></code><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li></ul><button class="btn-code-notes mdeditor" data-report-click="{&quot;spm&quot;:&quot;3001.10436&quot;,&quot;extra&quot;:{&quot;index&quot;:2,&quot;runIdx&quot;:-1}}">AI助手</button></pre> 
<p>这里是将<code>1admin</code>转化成了<code>1</code><br> 4.</p> 
<pre data-index="3" class="set-code-show prettyprint"><code class="prism language-php has-numbering" onclick="mdcp.signin(event)" style="position: unset;"><span class="token function">var_dump</span><span class="token punctuation">(</span><span class="token string double-quoted-string">"0e123456"</span><span class="token operator">==</span><span class="token number">0</span><span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token comment">//正确</span>
<div class="hljs-button signin active add_def" data-title="登录复制" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.4334&quot;}"></div></code><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li></ul><button class="btn-code-notes mdeditor" data-report-click="{&quot;spm&quot;:&quot;3001.10436&quot;,&quot;extra&quot;:{&quot;index&quot;:3,&quot;runIdx&quot;:-1}}">AI助手</button></pre> 
<p>这里是将<code>0e123456</code>转化成了<code>0</code><br> <mark>将0e这类字符串识别为科学技术法的数字，0的无论多少次方都是零，所以相等</mark></p> 
<p><mark>注意</mark><br> 当一个字符串被当作一个数值来取值，其结果和类型如下:<br> 如果该字符串没有包含<code>'.','e','E'</code>并且其数值值在整型的范围之内，该字符串被当作<code>int</code>来取值，<code>其他所有情况下都被作为float来取值</code>；<br> 该字符串的开始部分决定了它的值，<mark>如果该字符串以合法的数值开始，则使用该数值，否则其值为0。</mark></p> 
<h3><a name="t2"></a><a id="MD5_41"></a>二.MD5绕过（缺陷比较）</h3> 
<p>原理：通过利用一些<code>MD5</code>加密后是<code>0e</code>开头的，利用上面提到过的<code>0e</code><a href="https://so.csdn.net/so/search?q=%E7%A7%91%E5%AD%A6%E8%AE%A1%E6%95%B0%E6%B3%95&amp;spm=1001.2101.3001.7020" target="_blank" class="hl hl-1" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.7020&quot;,&quot;dest&quot;:&quot;https://so.csdn.net/so/search?q=%E7%A7%91%E5%AD%A6%E8%AE%A1%E6%95%B0%E6%B3%95&amp;spm=1001.2101.3001.7020&quot;,&quot;extra&quot;:&quot;{\&quot;searchword\&quot;:\&quot;科学计数法\&quot;}&quot;}" data-tit="科学计数法" data-pretit="科学计数法">科学计数法</a>，转化结果还是<code>0</code>；</p> 
<blockquote>
  &lt;?php if($_GET['a'] !== $_GET['b']){ if(md5($_GET['a']) == md5($_GET['b'])){ echo "flag"; } } ?&gt; 
</blockquote> 
<pre data-index="4" class="set-code-show prettyprint"><code class="prism language-c has-numbering" onclick="mdcp.signin(event)" style="position: unset;">payload<span class="token operator">:</span>   <span class="token operator">/</span><span class="token operator">?</span>a<span class="token operator">=</span>QNKCDZO<span class="token operator">&amp;</span>b<span class="token operator">=</span><span class="token number">240610708</span>
<div class="hljs-button signin active add_def" data-title="登录复制" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.4334&quot;}"></div></code><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li></ul><button class="btn-code-notes mdeditor" data-report-click="{&quot;spm&quot;:&quot;3001.10436&quot;,&quot;extra&quot;:{&quot;index&quot;:4,&quot;runIdx&quot;:-1}}">AI助手</button></pre> 
<p>搜集到的MD5神奇0e</p> 
<pre data-index="5" class="set-code-hide prettyprint"><code class="prism language-c has-numbering" onclick="mdcp.signin(event)" style="position: unset;"><span class="token number">240610708</span>
<span class="token number">0e462097431906509019562988736854</span>
QNKCDZO
<span class="token number">0e830400451993494058024219903391</span> 

s878926199a
<span class="token number">0e545993274517709034328855841020</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s214587387a
<span class="token number">0e848240448830537924465865611904</span>

s214587387a
<span class="token number">0e848240448830537924465865611904</span>

s878926199a
<span class="token number">0e545993274517709034328855841020</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1184209335a
<span class="token number">0e072485820392773389523109082030</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s532378020a
<span class="token number">0e220463095855511507588041205815</span>

s878926199a
<span class="token number">0e545993274517709034328855841020</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s214587387a
<span class="token number">0e848240448830537924465865611904</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s878926199a
<span class="token number">0e545993274517709034328855841020</span>

s878926199a
<span class="token number">0e545993274517709034328855841020</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s214587387a
<span class="token number">0e848240448830537924465865611904</span>

s214587387a
<span class="token number">0e848240448830537924465865611904</span>

s878926199a
<span class="token number">0e545993274517709034328855841020</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1184209335a
<span class="token number">0e072485820392773389523109082030</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s155964671a
<span class="token number">0e342768416822451524974117254469</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s532378020a
<span class="token number">0e220463095855511507588041205815</span>

s878926199a
<span class="token number">0e545993274517709034328855841020</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s214587387a
<span class="token number">0e848240448830537924465865611904</span>

s1502113478a
<span class="token number">0e861580163291561247404381396064</span>

s1091221200a
<span class="token number">0e940624217856561557816327384675</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s1885207154a
<span class="token number">0e509367213418206700842008763514</span>

s1836677006a
<span class="token number">0e481036490867661113260034900752</span>

s1665632922a
<span class="token number">0e731198061491163073197128363787</span>

s878926199a
<span class="token number">0e545993274517709034328855841020</span>
<div class="hljs-button signin active add_def" data-title="登录复制" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.4334&quot;}"></div></code><div class="hide-preCode-box"><span class="hide-preCode-bt"><img class="look-more-preCode contentImg-no-view" src="https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png" alt="" title=""></span></div><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li><li style="color: rgb(153, 153, 153);">2</li><li style="color: rgb(153, 153, 153);">3</li><li style="color: rgb(153, 153, 153);">4</li><li style="color: rgb(153, 153, 153);">5</li><li style="color: rgb(153, 153, 153);">6</li><li style="color: rgb(153, 153, 153);">7</li><li style="color: rgb(153, 153, 153);">8</li><li style="color: rgb(153, 153, 153);">9</li><li style="color: rgb(153, 153, 153);">10</li><li style="color: rgb(153, 153, 153);">11</li><li style="color: rgb(153, 153, 153);">12</li><li style="color: rgb(153, 153, 153);">13</li><li style="color: rgb(153, 153, 153);">14</li><li style="color: rgb(153, 153, 153);">15</li><li style="color: rgb(153, 153, 153);">16</li><li style="color: rgb(153, 153, 153);">17</li><li style="color: rgb(153, 153, 153);">18</li><li style="color: rgb(153, 153, 153);">19</li><li style="color: rgb(153, 153, 153);">20</li><li style="color: rgb(153, 153, 153);">21</li><li style="color: rgb(153, 153, 153);">22</li><li style="color: rgb(153, 153, 153);">23</li><li style="color: rgb(153, 153, 153);">24</li><li style="color: rgb(153, 153, 153);">25</li><li style="color: rgb(153, 153, 153);">26</li><li style="color: rgb(153, 153, 153);">27</li><li style="color: rgb(153, 153, 153);">28</li><li style="color: rgb(153, 153, 153);">29</li><li style="color: rgb(153, 153, 153);">30</li><li style="color: rgb(153, 153, 153);">31</li><li style="color: rgb(153, 153, 153);">32</li><li style="color: rgb(153, 153, 153);">33</li><li style="color: rgb(153, 153, 153);">34</li><li style="color: rgb(153, 153, 153);">35</li><li style="color: rgb(153, 153, 153);">36</li><li style="color: rgb(153, 153, 153);">37</li><li style="color: rgb(153, 153, 153);">38</li><li style="color: rgb(153, 153, 153);">39</li><li style="color: rgb(153, 153, 153);">40</li><li style="color: rgb(153, 153, 153);">41</li><li style="color: rgb(153, 153, 153);">42</li><li style="color: rgb(153, 153, 153);">43</li><li style="color: rgb(153, 153, 153);">44</li><li style="color: rgb(153, 153, 153);">45</li><li style="color: rgb(153, 153, 153);">46</li><li style="color: rgb(153, 153, 153);">47</li><li style="color: rgb(153, 153, 153);">48</li><li style="color: rgb(153, 153, 153);">49</li><li style="color: rgb(153, 153, 153);">50</li><li style="color: rgb(153, 153, 153);">51</li><li style="color: rgb(153, 153, 153);">52</li><li style="color: rgb(153, 153, 153);">53</li><li style="color: rgb(153, 153, 153);">54</li><li style="color: rgb(153, 153, 153);">55</li><li style="color: rgb(153, 153, 153);">56</li><li style="color: rgb(153, 153, 153);">57</li><li style="color: rgb(153, 153, 153);">58</li><li style="color: rgb(153, 153, 153);">59</li><li style="color: rgb(153, 153, 153);">60</li><li style="color: rgb(153, 153, 153);">61</li><li style="color: rgb(153, 153, 153);">62</li><li style="color: rgb(153, 153, 153);">63</li><li style="color: rgb(153, 153, 153);">64</li><li style="color: rgb(153, 153, 153);">65</li><li style="color: rgb(153, 153, 153);">66</li><li style="color: rgb(153, 153, 153);">67</li><li style="color: rgb(153, 153, 153);">68</li><li style="color: rgb(153, 153, 153);">69</li><li style="color: rgb(153, 153, 153);">70</li><li style="color: rgb(153, 153, 153);">71</li><li style="color: rgb(153, 153, 153);">72</li><li style="color: rgb(153, 153, 153);">73</li><li style="color: rgb(153, 153, 153);">74</li><li style="color: rgb(153, 153, 153);">75</li><li style="color: rgb(153, 153, 153);">76</li><li style="color: rgb(153, 153, 153);">77</li><li style="color: rgb(153, 153, 153);">78</li><li style="color: rgb(153, 153, 153);">79</li><li style="color: rgb(153, 153, 153);">80</li><li style="color: rgb(153, 153, 153);">81</li><li style="color: rgb(153, 153, 153);">82</li><li style="color: rgb(153, 153, 153);">83</li><li style="color: rgb(153, 153, 153);">84</li><li style="color: rgb(153, 153, 153);">85</li><li style="color: rgb(153, 153, 153);">86</li><li style="color: rgb(153, 153, 153);">87</li><li style="color: rgb(153, 153, 153);">88</li><li style="color: rgb(153, 153, 153);">89</li><li style="color: rgb(153, 153, 153);">90</li><li style="color: rgb(153, 153, 153);">91</li><li style="color: rgb(153, 153, 153);">92</li><li style="color: rgb(153, 153, 153);">93</li><li style="color: rgb(153, 153, 153);">94</li><li style="color: rgb(153, 153, 153);">95</li><li style="color: rgb(153, 153, 153);">96</li><li style="color: rgb(153, 153, 153);">97</li><li style="color: rgb(153, 153, 153);">98</li><li style="color: rgb(153, 153, 153);">99</li><li style="color: rgb(153, 153, 153);">100</li><li style="color: rgb(153, 153, 153);">101</li><li style="color: rgb(153, 153, 153);">102</li><li style="color: rgb(153, 153, 153);">103</li><li style="color: rgb(153, 153, 153);">104</li><li style="color: rgb(153, 153, 153);">105</li><li style="color: rgb(153, 153, 153);">106</li><li style="color: rgb(153, 153, 153);">107</li><li style="color: rgb(153, 153, 153);">108</li><li style="color: rgb(153, 153, 153);">109</li><li style="color: rgb(153, 153, 153);">110</li><li style="color: rgb(153, 153, 153);">111</li><li style="color: rgb(153, 153, 153);">112</li><li style="color: rgb(153, 153, 153);">113</li><li style="color: rgb(153, 153, 153);">114</li><li style="color: rgb(153, 153, 153);">115</li><li style="color: rgb(153, 153, 153);">116</li><li style="color: rgb(153, 153, 153);">117</li><li style="color: rgb(153, 153, 153);">118</li><li style="color: rgb(153, 153, 153);">119</li><li style="color: rgb(153, 153, 153);">120</li><li style="color: rgb(153, 153, 153);">121</li><li style="color: rgb(153, 153, 153);">122</li><li style="color: rgb(153, 153, 153);">123</li><li style="color: rgb(153, 153, 153);">124</li><li style="color: rgb(153, 153, 153);">125</li><li style="color: rgb(153, 153, 153);">126</li><li style="color: rgb(153, 153, 153);">127</li><li style="color: rgb(153, 153, 153);">128</li><li style="color: rgb(153, 153, 153);">129</li><li style="color: rgb(153, 153, 153);">130</li><li style="color: rgb(153, 153, 153);">131</li><li style="color: rgb(153, 153, 153);">132</li><li style="color: rgb(153, 153, 153);">133</li><li style="color: rgb(153, 153, 153);">134</li><li style="color: rgb(153, 153, 153);">135</li><li style="color: rgb(153, 153, 153);">136</li><li style="color: rgb(153, 153, 153);">137</li><li style="color: rgb(153, 153, 153);">138</li><li style="color: rgb(153, 153, 153);">139</li><li style="color: rgb(153, 153, 153);">140</li><li style="color: rgb(153, 153, 153);">141</li><li style="color: rgb(153, 153, 153);">142</li><li style="color: rgb(153, 153, 153);">143</li><li style="color: rgb(153, 153, 153);">144</li><li style="color: rgb(153, 153, 153);">145</li><li style="color: rgb(153, 153, 153);">146</li><li style="color: rgb(153, 153, 153);">147</li><li style="color: rgb(153, 153, 153);">148</li><li style="color: rgb(153, 153, 153);">149</li><li style="color: rgb(153, 153, 153);">150</li><li style="color: rgb(153, 153, 153);">151</li><li style="color: rgb(153, 153, 153);">152</li><li style="color: rgb(153, 153, 153);">153</li><li style="color: rgb(153, 153, 153);">154</li><li style="color: rgb(153, 153, 153);">155</li><li style="color: rgb(153, 153, 153);">156</li><li style="color: rgb(153, 153, 153);">157</li><li style="color: rgb(153, 153, 153);">158</li><li style="color: rgb(153, 153, 153);">159</li><li style="color: rgb(153, 153, 153);">160</li><li style="color: rgb(153, 153, 153);">161</li><li style="color: rgb(153, 153, 153);">162</li><li style="color: rgb(153, 153, 153);">163</li><li style="color: rgb(153, 153, 153);">164</li><li style="color: rgb(153, 153, 153);">165</li><li style="color: rgb(153, 153, 153);">166</li><li style="color: rgb(153, 153, 153);">167</li><li style="color: rgb(153, 153, 153);">168</li><li style="color: rgb(153, 153, 153);">169</li><li style="color: rgb(153, 153, 153);">170</li><li style="color: rgb(153, 153, 153);">171</li><li style="color: rgb(153, 153, 153);">172</li><li style="color: rgb(153, 153, 153);">173</li><li style="color: rgb(153, 153, 153);">174</li><li style="color: rgb(153, 153, 153);">175</li><li style="color: rgb(153, 153, 153);">176</li><li style="color: rgb(153, 153, 153);">177</li><li style="color: rgb(153, 153, 153);">178</li><li style="color: rgb(153, 153, 153);">179</li><li style="color: rgb(153, 153, 153);">180</li><li style="color: rgb(153, 153, 153);">181</li><li style="color: rgb(153, 153, 153);">182</li><li style="color: rgb(153, 153, 153);">183</li><li style="color: rgb(153, 153, 153);">184</li><li style="color: rgb(153, 153, 153);">185</li><li style="color: rgb(153, 153, 153);">186</li><li style="color: rgb(153, 153, 153);">187</li><li style="color: rgb(153, 153, 153);">188</li><li style="color: rgb(153, 153, 153);">189</li><li style="color: rgb(153, 153, 153);">190</li><li style="color: rgb(153, 153, 153);">191</li><li style="color: rgb(153, 153, 153);">192</li><li style="color: rgb(153, 153, 153);">193</li><li style="color: rgb(153, 153, 153);">194</li><li style="color: rgb(153, 153, 153);">195</li><li style="color: rgb(153, 153, 153);">196</li><li style="color: rgb(153, 153, 153);">197</li><li style="color: rgb(153, 153, 153);">198</li><li style="color: rgb(153, 153, 153);">199</li><li style="color: rgb(153, 153, 153);">200</li><li style="color: rgb(153, 153, 153);">201</li><li style="color: rgb(153, 153, 153);">202</li><li style="color: rgb(153, 153, 153);">203</li><li style="color: rgb(153, 153, 153);">204</li><li style="color: rgb(153, 153, 153);">205</li><li style="color: rgb(153, 153, 153);">206</li><li style="color: rgb(153, 153, 153);">207</li><li style="color: rgb(153, 153, 153);">208</li><li style="color: rgb(153, 153, 153);">209</li><li style="color: rgb(153, 153, 153);">210</li><li style="color: rgb(153, 153, 153);">211</li><li style="color: rgb(153, 153, 153);">212</li><li style="color: rgb(153, 153, 153);">213</li><li style="color: rgb(153, 153, 153);">214</li><li style="color: rgb(153, 153, 153);">215</li></ul><button class="btn-code-notes mdeditor" data-report-click="{&quot;spm&quot;:&quot;3001.10436&quot;,&quot;extra&quot;:{&quot;index&quot;:5,&quot;runIdx&quot;:-1}}">AI助手</button></pre> 
<p><a href="https://blog.csdn.net/iczfy585/article/details/106081299">大佬讲地很详细PHP中MD5的多种绕过！</a></p> 
<h3><a name="t3"></a><a id="_277"></a>三.没有数字字母的</h3> 
<p>作用于过滤了字母数字，需要通过一些方式来绕过对字母数字参数的过滤，达到执行代码的目的。</p> 
<h3><a name="t4"></a><a id="1_280"></a>1.取反绕过</h3> 
<p>在php位运算符中，有一种运算方式叫做取反，运算符号为^。可以利用的是UTF-8编码的某个汉字，并将其中某个字符取出来，然后取反，根据具体题目可以构造相关的<span class="words-blog hl-git-1" data-tit="payload" data-pretit="payload">payload</span>。</p> 
<h3><a name="t5"></a><a id="2URL_284"></a>2.URL编码取反绕过</h3> 
<p>对想要传入的参数，先进行URL编码再取反<br> 比如传入构造一个phpinfo();</p> 
<h3><a name="t6"></a><a id="3_289"></a>3.异或绕过</h3> 
<p>原理：PHP里两个字符串异或之后得到的还是一个字符串，有的时候会通过正则过滤这个字符串，不能正常的执行，换一种方式：就是通过找两个不会被正则的范围的两个字符串，进行异或，从而能够达到我们想要的字符串。</p> 
<pre data-index="6" class="set-code-show prettyprint"><code class="prism language-c has-numbering" onclick="mdcp.signin(event)" style="position: unset;">字符：<span class="token operator">?</span>         ASCII码：<span class="token number">63</span>           二进制：  <span class="token number">0011</span> <span class="token number">1111</span>
字符：<span class="token operator">~</span>         ASCII码：<span class="token number">126</span>          二进制：  <span class="token number">0111</span> <span class="token number">1110</span>
异或规则：
<span class="token number">1</span>   XOR   <span class="token number">0</span>   <span class="token operator">=</span>   <span class="token number">1</span>
<span class="token number">0</span>   XOR   <span class="token number">1</span>   <span class="token operator">=</span>   <span class="token number">1</span>
<span class="token number">0</span>   XOR   <span class="token number">0</span>   <span class="token operator">=</span>   <span class="token number">0</span>
<span class="token number">1</span>   XOR   <span class="token number">1</span>   <span class="token operator">=</span>   <span class="token number">0</span>
上述两个字符异或得到 二进制：  <span class="token number">0100</span> <span class="token number">0001</span>
该二进制的十进制也就是：<span class="token number">65</span>
对应的ASCII码是：A

<div class="hljs-button signin active add_def" data-title="登录复制" data-report-click="{&quot;spm&quot;:&quot;1001.2101.3001.4334&quot;}"></div></code><ul class="pre-numbering" style=""><li style="color: rgb(153, 153, 153);">1</li><li style="color: rgb(153, 153, 153);">2</li><li style="color: rgb(153, 153, 153);">3</li><li style="color: rgb(153, 153, 153);">4</li><li style="color: rgb(153, 153, 153);">5</li><li style="color: rgb(153, 153, 153);">6</li><li style="color: rgb(153, 153, 153);">7</li><li style="color: rgb(153, 153, 153);">8</li><li style="color: rgb(153, 153, 153);">9</li><li style="color: rgb(153, 153, 153);">10</li><li style="color: rgb(153, 153, 153);">11</li></ul><button class="btn-code-notes mdeditor" data-report-click="{&quot;spm&quot;:&quot;3001.10436&quot;,&quot;extra&quot;:{&quot;index&quot;:6,&quot;runIdx&quot;:-1}}">AI助手</button></pre> 
<p>重点是如何知道哪两个字符串异或是我们想要的最后的结果（这个还没有搞得清楚，后面会继续）<br> 会继续一直整理，分模块进行归纳，下周加快学习进度。</p>
                </div><div><div></div></div>
                <link href="https://csdnimg.cn/release/blogv2/dist/mdeditor/css/editerView/markdown_views-f23dff6052.css" rel="stylesheet">
                <link href="https://csdnimg.cn/release/blogv2/dist/mdeditor/css/style-e504d6a974.css" rel="stylesheet">
        </div>
