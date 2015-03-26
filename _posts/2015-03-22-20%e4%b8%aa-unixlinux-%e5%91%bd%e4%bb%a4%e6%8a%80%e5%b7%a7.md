---
title: 20个 Unix/Linux 命令技巧
author: huahua
layout: post
permalink: /?p=78
categories:
  - 未分类
---
<table class="vwtb" cellspacing="0" cellpadding="0">
  <tr>
    <td id="article_content">
      让我们用<strong>这些Unix/Linux命令技巧</strong>开启新的一年，提高在终端下的生产力。我已经找了很久了，现在就与你们分享。</p> 
      
      <p>
        <img src="http://img.linux.net.cn/data/attachment/album/201503/21/193317jgu1112xvxktg1xk.jpg" alt="" />
      </p>
      
      <p>
        <a id="3_286" class="target-fix"></a>
      </p>
      
      <h3 id="toc_1">
        删除一个大文件
      </h3>
      
      <p>
        我在生产服务器上有一个很大的200GB的日志文件需要删除。我的rm和ls命令已经崩溃，我担心这是由于巨大的磁盘IO造成的，要删除这个大文件，输入：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pun">&gt;&lt;/span> &lt;span class="str">/path/&lt;/span>&lt;span class="pln">to&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">file&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">log&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="com"># 或使用如下格式&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pun">:&lt;/span> &lt;span class="pun">&gt;&lt;/span> &lt;span class="str">/path/&lt;/span>&lt;span class="pln">to&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">file&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">log&lt;/span></code>
        </li>
        <li class="L3">
          <code></code>
        </li>
        <li class="L4">
          <code>&lt;span class="com"># 然后删除它 &lt;/span></code>
        </li>
        <li class="L5">
          <code>&lt;span class="pln">rm &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">path&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">to&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">file&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">log&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_709" class="target-fix"></a>
      </p>
      
      <h3 id="toc_2">
        如何记录终端输出？
      </h3>
      
      <p>
        试试使用script命令行工具来为你的终端输出创建输出记录。
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">script &lt;/span>&lt;span class="kwd">my&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">terminal&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">sessio&lt;/span></code>
        </li>
      </ol>
      
      <p>
        输入命令：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">ls&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">date&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pln">sudo service foo stop&lt;/span></code>
        </li>
      </ol>
      
      <p>
        要退出（结束script会话），输入 <em>exit</em> 或者 <em>logout</em> 或者按下 <em>control-D</em>。
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="kwd">exit&lt;/span></code>
        </li>
      </ol>
      
      <p>
        要浏览输入：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">more &lt;/span>&lt;span class="kwd">my&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">terminal&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">session&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">less &lt;/span>&lt;span class="kwd">my&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">terminal&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">session&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pln">cat &lt;/span>&lt;span class="kwd">my&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">terminal&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">session&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_1382" class="target-fix"></a>
      </p>
      
      <h3 id="toc_3">
        还原被删除的 /tmp 文件夹
      </h3>
      
      <p>
        我在文章<a href="http://www.cyberciti.biz/tips/my-10-unix-command-line-mistakes.html">Linux和Unix shell，我犯了一些错误</a>。我意外地删除了/tmp文件夹。要还原它，我需要这么做：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">mkdir &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">tmp&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">chmod &lt;/span>&lt;span class="lit">1777&lt;/span> &lt;span class="pun">/&lt;/span>&lt;span class="pln">tmp&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pln">chown root&lt;/span>&lt;span class="pun">:&lt;/span>&lt;span class="pln">root &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">tmp&lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="pln">ls &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">ld &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">tmp&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_1777" class="target-fix"></a>
      </p>
      
      <h3 id="toc_4">
        锁定一个文件夹
      </h3>
      
      <p>
        为了我的数据隐私，我想要锁定我文件服务器下的/downloads文件夹。因此我运行了：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">chmod &lt;/span>&lt;span class="lit">0000&lt;/span> &lt;span class="pun">/&lt;/span>&lt;span class="pln">downloads&lt;/span></code>
        </li>
      </ol>
      
      <p>
        root用户仍旧可以访问，而ls和cd命令则不工作。要还原它用：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">chmod &lt;/span>&lt;span class="lit">0755&lt;/span> &lt;span class="pun">/&lt;/span>&lt;span class="pln">downloads&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_2183" class="target-fix"></a>
      </p>
      
      <h3 id="toc_5">
        在vim中用密码保护文件
      </h3>
      
      <p>
        害怕root用户或者其他人偷窥你的个人文件么？尝试在vim中用密码保护，输入：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">vim &lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">X filename&lt;/span></code>
        </li>
      </ol>
      
      <p>
        或者，在退出vim之前使用:X 命令来加密你的文件，vim会提示你输入一个密码。
      </p>
      
      <p>
        <a id="3_2530" class="target-fix"></a>
      </p>
      
      <h3 id="toc_6">
        清除屏幕上的乱码
      </h3>
      
      <p>
        只要输入：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">reset&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_2662" class="target-fix"></a>
      </p>
      
      <h3 id="toc_7">
        易读格式
      </h3>
      
      <p>
        传递<em>-h</em>或者<em>-H</em>（和其他选项）选项给GNU或者BSD工具来获取像ls、df、du等命令以易读的格式输出：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">ls &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">lh&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="com"># 以易读的格式 (比如： 1K 234M 2G)&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pln">df &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">h&lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="pln">df &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">k&lt;/span></code>
        </li>
        <li class="L4">
          <code>&lt;span class="com"># 以字节、KB、MB 或 GB 输出： &lt;/span></code>
        </li>
        <li class="L5">
          <code>&lt;span class="pln">free &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">b&lt;/span></code>
        </li>
        <li class="L6">
          <code>&lt;span class="pln">free &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">k&lt;/span></code>
        </li>
        <li class="L7">
          <code>&lt;span class="pln">free &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">m&lt;/span></code>
        </li>
        <li class="L8">
          <code>&lt;span class="pln">free &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">g&lt;/span></code>
        </li>
        <li class="L9">
          <code>&lt;span class="com"># 以易读的格式输出 (比如 1K 234M 2G)&lt;/span></code>
        </li>
        <li class="L0">
          <code>&lt;span class="pln">du &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">h&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="com"># 以易读的格式显示文件系统权限&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pln">stat &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">c &lt;/span>&lt;span class="pun">%&lt;/span>&lt;span class="pln">A &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">boot&lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="com"># 比较易读的数字&lt;/span></code>
        </li>
        <li class="L4">
          <code>&lt;span class="pln">sort &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">h &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">a file&lt;/span></code>
        </li>
        <li class="L5">
          <code>&lt;span class="com"># 在Linux上以易读的形式显示cpu信息&lt;/span></code>
        </li>
        <li class="L6">
          <code>&lt;span class="pln">lscpu&lt;/span></code>
        </li>
        <li class="L7">
          <code>&lt;span class="pln">lscpu &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">e&lt;/span></code>
        </li>
        <li class="L8">
          <code>&lt;span class="pln">lscpu &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">e&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="pln">cpu&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">node&lt;/span></code>
        </li>
        <li class="L9">
          <code>&lt;span class="com"># 以易读的形式显示每个文件的大小&lt;/span></code>
        </li>
        <li class="L0">
          <code>&lt;span class="pln">tree &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">h&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">tree &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">h &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">boot&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_3364" class="target-fix"></a>
      </p>
      
      <h3 id="toc_8">
        在Linux系统中显示已知的用户信息
      </h3>
      
      <p>
        只要输入：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="com">## linux 版本 ##&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">lslogins&lt;/span></code>
        </li>
        <li class="L2">
          <code></code>
        </li>
        <li class="L3">
          <code>&lt;span class="com">## BSD 版本 ##&lt;/span></code>
        </li>
        <li class="L4">
          <code>&lt;span class="pln">logins&lt;/span></code>
        </li>
      </ol>
      
      <p>
        示例输出：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">UID USER PWD&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">LOCK PWD&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">DENY LAST&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">LOGIN GECOS&lt;/span></code>
        </li>
        <li class="L1">
          <code> &lt;span class="lit">0&lt;/span>&lt;span class="pln"> root &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">0&lt;/span> &lt;span class="lit">22&lt;/span>&lt;span class="pun">:&lt;/span>&lt;span class="lit">37&lt;/span>&lt;span class="pun">:&lt;/span>&lt;span class="lit">59&lt;/span>&lt;span class="pln"> root&lt;/span></code>
        </li>
        <li class="L2">
          <code> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> bin &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> bin&lt;/span></code>
        </li>
        <li class="L3">
          <code> &lt;span class="lit">2&lt;/span>&lt;span class="pln"> daemon &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> daemon&lt;/span></code>
        </li>
        <li class="L4">
          <code> &lt;span class="lit">3&lt;/span>&lt;span class="pln"> adm &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> adm&lt;/span></code>
        </li>
        <li class="L5">
          <code> &lt;span class="lit">4&lt;/span>&lt;span class="pln"> lp &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> lp&lt;/span></code>
        </li>
        <li class="L6">
          <code> &lt;span class="lit">5&lt;/span>&lt;span class="pln"> sync &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> sync&lt;/span></code>
        </li>
        <li class="L7">
          <code> &lt;span class="lit">6&lt;/span>&lt;span class="pln"> shutdown &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="lit">2014&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="typ">Dec17&lt;/span>&lt;span class="pln"> shutdown&lt;/span></code>
        </li>
        <li class="L8">
          <code> &lt;span class="lit">7&lt;/span>&lt;span class="pln"> halt &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> halt&lt;/span></code>
        </li>
        <li class="L9">
          <code> &lt;span class="lit">8&lt;/span>&lt;span class="pln"> mail &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> mail&lt;/span></code>
        </li>
        <li class="L0">
          <code> &lt;span class="lit">10&lt;/span>&lt;span class="pln"> uucp &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> uucp&lt;/span></code>
        </li>
        <li class="L1">
          <code> &lt;span class="lit">11&lt;/span> &lt;span class="kwd">operator&lt;/span> &lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="kwd">operator&lt;/span></code>
        </li>
        <li class="L2">
          <code> &lt;span class="lit">12&lt;/span>&lt;span class="pln"> games &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> games&lt;/span></code>
        </li>
        <li class="L3">
          <code> &lt;span class="lit">13&lt;/span>&lt;span class="pln"> gopher &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> gopher&lt;/span></code>
        </li>
        <li class="L4">
          <code> &lt;span class="lit">14&lt;/span>&lt;span class="pln"> ftp &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> FTP &lt;/span>&lt;span class="typ">User&lt;/span></code>
        </li>
        <li class="L5">
          <code> &lt;span class="lit">27&lt;/span>&lt;span class="pln"> mysql &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="typ">MySQL&lt;/span> &lt;span class="typ">Server&lt;/span></code>
        </li>
        <li class="L6">
          <code> &lt;span class="lit">38&lt;/span>&lt;span class="pln"> ntp &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span></code>
        </li>
        <li class="L7">
          <code> &lt;span class="lit">48&lt;/span>&lt;span class="pln"> apache &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="typ">Apache&lt;/span></code>
        </li>
        <li class="L8">
          <code> &lt;span class="lit">68&lt;/span>&lt;span class="pln"> haldaemon &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> HAL daemon&lt;/span></code>
        </li>
        <li class="L9">
          <code> &lt;span class="lit">69&lt;/span>&lt;span class="pln"> vcsa &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="kwd">virtual&lt;/span>&lt;span class="pln"> console memory owner&lt;/span></code>
        </li>
        <li class="L0">
          <code> &lt;span class="lit">72&lt;/span>&lt;span class="pln"> tcpdump &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span></code>
        </li>
        <li class="L1">
          <code> &lt;span class="lit">74&lt;/span>&lt;span class="pln"> sshd &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="typ">Privilege&lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">separated SSH&lt;/span></code>
        </li>
        <li class="L2">
          <code> &lt;span class="lit">81&lt;/span>&lt;span class="pln"> dbus &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="typ">System&lt;/span>&lt;span class="pln"> message bus&lt;/span></code>
        </li>
        <li class="L3">
          <code> &lt;span class="lit">89&lt;/span>&lt;span class="pln"> postfix &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span></code>
        </li>
        <li class="L4">
          <code> &lt;span class="lit">99&lt;/span>&lt;span class="pln"> nobody &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="typ">Nobody&lt;/span></code>
        </li>
        <li class="L5">
          <code>&lt;span class="lit">173&lt;/span>&lt;span class="pln"> abrt &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span></code>
        </li>
        <li class="L6">
          <code>&lt;span class="lit">497&lt;/span>&lt;span class="pln"> vnstat &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> vnStat user&lt;/span></code>
        </li>
        <li class="L7">
          <code>&lt;span class="lit">498&lt;/span>&lt;span class="pln"> nginx &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span>&lt;span class="pln"> nginx user&lt;/span></code>
        </li>
        <li class="L8">
          <code>&lt;span class="lit">499&lt;/span>&lt;span class="pln"> saslauth &lt;/span>&lt;span class="lit">0&lt;/span> &lt;span class="lit">1&lt;/span> &lt;span class="str">"Saslauthd user"&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_5117" class="target-fix"></a>
      </p>
      
      <h3 id="toc_9">
        我如何删除意外在当前文件夹下解压的文件？
      </h3>
      
      <p>
        我意外在/var/www/html/而不是/home/projects/www/current下解压了一个tarball。它搞乱了/var/www/html下的文件，你甚至不知道哪些是误解压出来的。最简单修复这个问题的方法是：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">cd &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="kwd">var&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">www&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">html&lt;/span>&lt;span class="pun">/&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="str">/bin/&lt;/span>&lt;span class="pln">rm &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">f &lt;/span>&lt;span class="str">"$(tar ztf /path/to/file.tar.gz)"&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_5547" class="target-fix"></a>
      </p>
      
      <h3 id="toc_10">
        对top命令的输出感到疑惑？
      </h3>
      
      <p>
        正经地说，你应该试一下用htop代替top：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">sudo htop&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_5733" class="target-fix"></a>
      </p>
      
      <h3 id="toc_11">
        想要再次运行相同的命令
      </h3>
      
      <p>
        只需要输入!!。比如：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="str">/myhome/&lt;/span>&lt;span class="pln">dir&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">script&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">name arg1 arg2&lt;/span></code>
        </li>
        <li class="L1">
          <code></code>
        </li>
        <li class="L2">
          <code>&lt;span class="com"># 要再次运行相同的命令 &lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="pun">!!&lt;/span></code>
        </li>
        <li class="L4">
          <code></code>
        </li>
        <li class="L5">
          <code>&lt;span class="com">## 以root用户运行最后运行的命令&lt;/span></code>
        </li>
        <li class="L6">
          <code>&lt;span class="pln">sudo &lt;/span>&lt;span class="pun">!!&lt;/span></code>
        </li>
      </ol>
      
      <p>
        !!会运行最近使用的命令。要运行最近运行的以“foo”开头命令：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pun">!&lt;/span>&lt;span class="pln">foo&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="com"># 以root用户运行上一次以“service”开头的命令&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pln">sudo &lt;/span>&lt;span class="pun">!&lt;/span>&lt;span class="pln">service&lt;/span></code>
        </li>
      </ol>
      
      <p>
        !$用于运行带上最后一个参数的命令：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="com"># 编辑 nginx.conf&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">sudo vi &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">etc&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">nginx&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">nginx&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">conf&lt;/span></code>
        </li>
        <li class="L2">
          <code></code>
        </li>
        <li class="L3">
          <code>&lt;span class="com"># 测试 nginx.conf&lt;/span></code>
        </li>
        <li class="L4">
          <code>&lt;span class="pun">/&lt;/span>&lt;span class="pln">sbin&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">nginx &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">t &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">c &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">etc&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">nginx&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">nginx&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">conf&lt;/span></code>
        </li>
        <li class="L5">
          <code></code>
        </li>
        <li class="L6">
          <code>&lt;span class="com"># 测试完 "/sbin/nginx -t -c /etc/nginx/nginx.conf"你可以用vi再次编辑这个文件了&lt;/span></code>
        </li>
        <li class="L7">
          <code>&lt;span class="pln">sudo vi &lt;/span>&lt;span class="pun">!&lt;/span>&lt;span class="pln">$&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_6604" class="target-fix"></a>
      </p>
      
      <h3 id="toc_12">
        在终端上提醒你必须得走了
      </h3>
      
      <p>
        如果你需要提醒离开你的终端，输入下面的命令：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">leave &lt;/span>&lt;span class="pun">+&lt;/span>&lt;span class="pln">hhmm&lt;/span></code>
        </li>
      </ol>
      
      <p>
        这里：
      </p>
      
      <ul>
        <li>
          <strong>hhmm</strong> &#8211; 时间是以hhmm的形式，hh表示小时（12时制或者24小时制），mm代表分钟。所有的时间都转化成12时制，并且假定发生在接下来的12小时。
        </li>
      </ul>
      
      <p>
        <a id="3_7047" class="target-fix"></a>
      </p>
      
      <h3 id="toc_13">
        甜蜜的家
      </h3>
      
      <p>
        想要进入刚才进入的地方？运行：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">cd &lt;/span>&lt;span class="pun">-&lt;/span></code>
        </li>
      </ol>
      
      <p>
        需要快速地回到你的家目录？输入：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">cd&lt;/span></code>
        </li>
      </ol>
      
      <p>
        变量<em>CDPATH</em>定义了目录的搜索路径：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="kwd">export&lt;/span>&lt;span class="pln"> CDPATH&lt;/span>&lt;span class="pun">=&lt;/span>&lt;span class="str">/var/&lt;/span>&lt;span class="pln">www&lt;/span>&lt;span class="pun">:/&lt;/span>&lt;span class="pln">nas10&lt;/span></code>
        </li>
      </ol>
      
      <p>
        现在，不用输入cd */var/www/html/ 这样长了，我可以直接输入下面的命令进入 /var/www/html：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">cd html&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_7649" class="target-fix"></a>
      </p>
      
      <h3 id="toc_14">
        在less浏览时编辑文件
      </h3>
      
      <p>
        要编辑一个正在用less浏览的文件，可以按下v。你就可以用变量$EDITOR所指定的编辑器来编辑了：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">less &lt;/span>&lt;span class="pun">*.&lt;/span>&lt;span class="pln">c&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">less foo&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">html&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="com">## 按下v键来编辑文件 ##&lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="com">## 退出编辑器后，你可以继续用less浏览了 ##&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_8008" class="target-fix"></a>
      </p>
      
      <h3 id="toc_15">
        列出你系统中的所有文件和目录
      </h3>
      
      <p>
        要看到你系统中的所有目录，运行：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">find &lt;/span>&lt;span class="pun">/&lt;/span> &lt;span class="pun">-&lt;/span>&lt;span class="pln">type d &lt;/span>&lt;span class="pun">|&lt;/span>&lt;span class="pln"> less&lt;/span></code>
        </li>
        <li class="L1">
          <code></code>
        </li>
        <li class="L2">
          <code>&lt;span class="com"># 列出$HOME 所有目录&lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="pln">find $HOME &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">type d &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">ls &lt;/span>&lt;span class="pun">|&lt;/span>&lt;span class="pln"> less&lt;/span></code>
        </li>
      </ol>
      
      <p>
        要看到所有的文件，运行：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">find &lt;/span>&lt;span class="pun">/&lt;/span> &lt;span class="pun">-&lt;/span>&lt;span class="pln">type f &lt;/span>&lt;span class="pun">|&lt;/span>&lt;span class="pln"> less&lt;/span></code>
        </li>
        <li class="L1">
          <code></code>
        </li>
        <li class="L2">
          <code>&lt;span class="com"># 列出 $HOME 中所有的文件&lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="pln">find $HOME &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">type f &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">ls &lt;/span>&lt;span class="pun">|&lt;/span>&lt;span class="pln"> less&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_8460" class="target-fix"></a>
      </p>
      
      <h3 id="toc_16">
        用一条命令构造目录树
      </h3>
      
      <p>
        你可以用mkdir加上-p选项一次创建一颗目录树：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">mkdir &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">p &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">jail&lt;/span>&lt;span class="pun">/{&lt;/span>&lt;span class="pln">dev&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">bin&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">sbin&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">etc&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">usr&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">lib&lt;/span>&lt;span class="pun">,&lt;/span>&lt;span class="pln">lib64&lt;/span>&lt;span class="pun">}&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">ls &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">l &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">jail&lt;/span>&lt;span class="pun">/&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_8701" class="target-fix"></a>
      </p>
      
      <h3 id="toc_17">
        将文件复制到多个目录中
      </h3>
      
      <p>
        不必运行：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">cp &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">path&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">to&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">file &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">usr&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">dir1&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">cp &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">path&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">to&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">file &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="kwd">var&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">dir2&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="pln">cp &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">path&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">to&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">file &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">nas&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">dir3&lt;/span></code>
        </li>
      </ol>
      
      <p>
        运行下面的命令来复制文件到多个目录中：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">echo &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">usr&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">dir1 &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="kwd">var&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">dir2 &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">nas&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">dir3 &lt;/span>&lt;span class="pun">|&lt;/span>&lt;span class="pln"> xargs &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">n &lt;/span>&lt;span class="lit">1&lt;/span>&lt;span class="pln"> cp &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">v &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">path&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">to&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">file&lt;/span></code>
        </li>
      </ol>
      
      <p>
        留下<a href="http://bash.cyberciti.biz/guide/Writing_your_first_shell_function">创建一个shell函数</a>作为读者的练习。
      </p>
      
      <p>
        <a id="3_9253" class="target-fix"></a>
      </p>
      
      <h3 id="toc_18">
        快速找出两个目录的不同
      </h3>
      
      <p>
        diff命令会按行比较文件。但是它也可以比较两个目录：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">ls &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">l &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">tmp&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">r&lt;/span></code>
        </li>
        <li class="L1">
          <code>&lt;span class="pln">ls &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">l &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">tmp&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">s&lt;/span></code>
        </li>
        <li class="L2">
          <code>&lt;span class="com"># 使用 diff 比较两个文件夹&lt;/span></code>
        </li>
        <li class="L3">
          <code>&lt;span class="pln">diff &lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">tmp&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">r&lt;/span>&lt;span class="str">/ /&lt;/span>&lt;span class="pln">tmp&lt;/span>&lt;span class="pun">/&lt;/span>&lt;span class="pln">s&lt;/span>&lt;span class="pun">/&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a href="http://www.cyberciti.biz/open-source/command-line-hacks/20-unix-command-line-tricks-part-i/attachment/differences-between-folders/"><img src="http://img.linux.net.cn/data/attachment/album/201503/21/193319hl87wuvkvgk6l7hk.jpg" alt="Fig. : Finding differences between folders" /></a>
      </p>
      
      <p>
        <em>图片： 找出目录之间的不同</em>
      </p>
      
      <p>
        <a id="3_9887" class="target-fix"></a>
      </p>
      
      <h3 id="toc_19">
        文本格式化
      </h3>
      
      <p>
        你可以用fmt命令重新格式化每个段落。在本例中，我要用分割超长的行并且填充短行：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">fmt file&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">txt&lt;/span></code>
        </li>
      </ol>
      
      <p>
        你也可以分割长的行，但是不重新填充，也就是说分割长行，但是不填充短行：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">fmt &lt;/span>&lt;span class="pun">-&lt;/span>&lt;span class="pln">s file&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">txt&lt;/span></code>
        </li>
      </ol>
      
      <p>
        <a id="3_10303" class="target-fix"></a>
      </p>
      
      <h3 id="toc_20">
        可以看见输出并将其写入到一个文件中
      </h3>
      
      <p>
        如下使用tee命令在屏幕上看见输出并同样写入到日志文件my.log中：
      </p>
      
      <ol class="linenums">
        <li class="L0">
          <code>&lt;span class="pln">mycoolapp arg1 arg2 input&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">file &lt;/span>&lt;span class="pun">|&lt;/span>&lt;span class="pln"> tee &lt;/span>&lt;span class="kwd">my&lt;/span>&lt;span class="pun">.&lt;/span>&lt;span class="pln">log&lt;/span></code>
        </li>
      </ol>
      
      <p>
        tee可以保证你同时在屏幕上看到mycoolapp的输出并写入文件  my.log。</td> </tr> </tbody> </table>