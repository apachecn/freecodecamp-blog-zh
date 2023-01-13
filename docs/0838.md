# 家庭网络安全——如何使用 Suricata、RaspberryPI4 和 Python 来确保网络安全

> 原文：<https://www.freecodecamp.org/news/home-network-security-with-suricata-raspberrypi4-python/>

在[之前的一篇文章](https://www.freecodecamp.org/news/wireless-security-using-raspberry-pi-4-kismet-and-python/)中，我展示了如何使用 [Kismet](https://www.kismetwireless.net/) 保护你的无线家庭网络。

Kismet 非常适合检测异常和某些类型的攻击，但如果我想分析流量并寻找异常模式或可能指示攻击的模式，该怎么办？

而[入侵检测系统](https://en.wikipedia.org/wiki/Intrusion_detection_system) ( ****IDS**** )是:

> ...一种设备或软件应用程序，用于监控网络或系统是否存在恶意活动或违反策略的情况。

我过去用过一个很好的 IDS 叫做 [Snort V2](https://snort.org/) ，我意识到 Snort 3 已经过时了。但是有一个[非常明确的警告](https://snort.org/documents/snort-supported-oss)关于在没有多少内存的机器上运行它:

> 虽然 Snort 几乎可以在所有基于*nix 的机器上编译，但是不建议您在低功耗或低 RAM 的机器上编译 Snort。Snort 需要内存来运行，并正确地分析尽可能多的流量。

和

> Snort 不正式支持任何特定的操作系统。

*这不完全是不喜欢它的理由*，但是当供应商告诉我我的操作系统不在他们支持的平台列表中时，我会更有信心。我最近也有过使用开源工具 [Suricata、](https://suricata.io/download/)进行设置的经验，所以我决定更认真地尝试监视我的本地网络，如果检测到任何可疑活动，就提醒我。

四处逛了逛，我发现对于我的本地网络，8 GB 的 RAM 和我的 Linux 发行版就足够了:

```
josevnz@raspberrypi:~$ lsb_release --release
Release:	20.04 
```

我的 Ubuntu 版本*支持开箱即用*。

选择权在你。对我来说，使用 Suricata 比使用 Snort 感觉更好。像往常一样，您需要围绕您的硬件、您的用例以及工具提供的特性(包括商业支持)进行规划。

# **目录**

*   [快速安装](#quick-installation)
*   [你应该把你的树莓 Pi 4 和 Suricata 连接在哪里](#where-you-should-connect-your-raspberrypi-4-with-suricata)
*   [如何设置 Suricata](#how-to-set-up-suricata)
*   [如何调整苏里卡塔](#how-to-tune-up-suricata)
*   [理解所有警报](#making-sense-of-all-the-alerts)
*   我们学到了什么，接下来是什么？

# **快速安装**

这里详细解释了安装[，所以我只在这里放我在我的机器上使用的](https://redmine.openinfosecfoundation.org/projects/suricata/wiki/Ubuntu_Installation_-_Personal_Package_Archives_%28PPA%29)[快速安装步骤](https://www.youtube.com/playlist?list=PLFqw30a25lWRIhAnQNb7ZaPpexPYgxhVv):

```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata 
```

## **苏里卡塔是一只复杂的野兽**

您可以使用 Suricata 来检测网络流量(IDS)中的异常并向您发出警报，或者在入侵防御系统( ****IPS**** )中工作时主动丢弃可疑连接。

它还可以捕获网络流量，并以 PCAP 格式存储，以供以后分析(小心，因为你可以吃你的磁盘空间相当快)。

在本教程中，我们将保持简单，现在将采取更被动的方法，并在检测到入侵时获得警报(坚持 IDS 模式)。

# **您应该在哪里连接 RaspBerryPI 4 和 Suricata？**

理想情况下，您应该将 Suricata 传感器放在靠近家用路由器的地方。一种方法是将所有设备(包括您的家庭路由器)连接到一台公共交换机，然后将进出家庭路由器的流量镜像到交换机的一个端口。苏里卡塔将连接到那个端口，监听所有的流量。

如果您想将 Suricata 作为 IPS 运行，那么连接必须不同，但这不是本教程的预期用途。

# **如何设置 Suricata**

理想情况下，放置 Suricata 的最佳位置是在防火墙和家庭网络中的其他服务器之间。

在这种情况下，让我们假设这是不可能的，因为没有防火墙(好吧，这将是你的 ISP 路由器，但你不能在那里运行 Suricata)。所以下一个最好的东西是连接到它的有线网络接口(在我的例子中是 eth0)。

/etc/suricata/suricata.yaml 文件包含默认值。我将在这里展示我覆盖的内容:

```
root@raspberrypi:~# grep -in1 af-p /etc/suricata/suricata.yaml 
580-# Linux high speed capture support
581:af-packet:
582-  - interface: eth0
root@raspberrypi:~# grep -in 'HOME_NET: "' /etc/suricata/suricata.yaml |grep -v '#'
15:    HOME_NET: "[192.168.1.0/24]" 
```

苏里南启动:

```
root@raspberrypi:~# systemctl start suricata.service
root@raspberrypi:~# systemctl status suricata.service
● suricata.service - LSB: Next Generation IDS/IPS
     Loaded: loaded (/etc/init.d/suricata; generated)
     Active: active (running) since Sun 2022-04-10 23:49:00 UTC; 24h ago
       Docs: man:systemd-sysv-generator(8)
      Tasks: 10 (limit: 9257)
     CGroup: /system.slice/suricata.service
             └─1834983 /usr/bin/suricata -c /etc/suricata/suricata.yaml --pidfile /var/run/suricata.pid --af-packet -D -vvv

Apr 10 23:49:00 raspberrypi systemd[1]: Starting LSB: Next Generation IDS/IPS...
Apr 10 23:49:00 raspberrypi suricata[1834973]: Starting suricata in IDS (af-packet) mode... done.
Apr 10 23:49:00 raspberrypi systemd[1]: Started LSB: Next Generation IDS/IPS. 
```

重要的细节放在文件“/var/log/suricata/eve.json”中。在苏里卡塔开始后，我的成长速度惊人:

```
{"timestamp":"2022-04-10T23:49:32.527488+0000","event_type":"stats","stats":{"uptime":32,"capture":{"kernel_packets":113,"kernel_drops":0,"errors":0},"decoder":{"pkts":126,"bytes":17986,"invalid":0,"ipv4":30,"ipv6":74,"ethernet":126,"chdlc":0,"raw":0,"null":0,"sll":0,"tcp":4,"udp":30,"sctp":0,"icmpv4":0,"icmpv6":70,"ppp":0,"pppoe":0,"geneve":0,"gre":0,"vlan":0,"vlan_qinq":0,"vxlan":0,"vntag":0,"ieee8021ah":0,"teredo":0,"ipv4_in_ipv6":0,"ipv6_in_ipv6":0,"mpls":0,"avg_pkt_size":142,"max_pkt_size":392,"max_mac_addrs_src":0,"max_mac_addrs_dst":0,"erspan":0,"event":{"ipv4":{"pkt_too_small":0,"hlen_too_small":0,"iplen_smaller_than_hlen":0,"trunc_pkt":0,"opt_invalid":0,"opt_invalid_len":0,"opt_malformed":0,"opt_pad_required":0,"opt_eol_required":0,"opt_duplicate":0,"opt_unknown":0,"wrong_ip_version":0,"icmpv6":0,"frag_pkt_too_large":0,"frag_overlap":0,"frag_ignored":0},"icmpv4":{"pkt_too_small":0,"unknown_type":0,"unknown_code":0,"ipv4_trunc_pkt":0,"ipv4_unknown_ver":0},"icmpv6":{"unknown_type":0,"unknown_code":0,"pkt_too_small":0,"ipv6_unknown_version":0,"ipv6_trunc_pkt":0,"mld_message_with_invalid_hl":0,"unassigned_type":0,"experimentation_type":0},"ipv6":{"pkt_too_small":0,"trunc_pkt":0,"trunc_exthdr":0,"exthdr_dupl_fh":0,"exthdr_useless_fh":0,"exthdr_dupl_rh":0,"exthdr_dupl_hh":0,"exthdr_dupl_dh":0,"exthdr_dupl_ah":0,"exthdr_dupl_eh":0,"exthdr_invalid_optlen":0,"wrong_ip_version":0,"exthdr_ah_res_not_null":0,"hopopts_unknown_opt":0,"hopopts_only_padding":0,"dstopts_unknown_opt":0,"dstopts_only_padding":0,"rh_type_0":0,"zero_len_padn":21,"fh_non_zero_reserved_field":0,"data_after_none_header":0,"unknown_next_header":0,"icmpv4":0,"frag_pkt_too_large":0,"frag_overlap":0,"frag_invalid_length":0,"frag_ignored":0,"ipv4_in_ipv6_too_small":0,"ipv4_in_ipv6_wrong_version":0,"ipv6_in_ipv6_too_small":0,"ipv6_in_ipv6_wrong_version":0},"tcp":{"pkt_too_small":0,"hlen_too_small":0,"invalid_optlen":0,"opt_invalid_len":0,"opt_duplicate":0},"udp":{"pkt_too_small":0,"hlen_too_small":0,"hlen_invalid":0},"sll":{"pkt_too_small":0},"ethernet":{"pkt_too_small":0},"ppp":{"pkt_too_small":0,"vju_pkt_too_small":0,"ip4_pkt_too_small":0,"ip6_pkt_too_small":0,"wrong_type":0,"unsup_proto":0},"pppoe":{"pkt_too_small":0,"wrong_code":0,"malformed_tags":0},"gre":{"pkt_too_small":0,"wrong_version":0,"version0_recur":0,"version0_flags":0,"version0_hdr_too_big":0,"version0_malformed_sre_hdr":0,"version1_chksum":0,"version1_route":0,"version1_ssr":0,"version1_recur":0,"version1_flags":0,"version1_no_key":0,"version1_wrong_protocol":0,"version1_malformed_sre_hdr":0,"version1_hdr_too_big":0},"vlan":{"header_too_small":0,"unknown_type":0,"too_many_layers":0},"ieee8021ah":{"header_too_small":0},"vntag":{"header_too_small":0,"unknown_type":0},"ipraw":{"invalid_ip_version":0},"ltnull":{"pkt_too_small":0,"unsupported_type":0},"sctp":{"pkt_too_small":0},"mpls":{"header_too_small":0,"pkt_too_small":0,"bad_label_router_alert":0,"bad_label_implicit_null":0,"bad_label_reserved":0,"unknown_payload_type":0},"vxlan":{"unknown_payload_type":0},"geneve":{"unknown_payload_type":0},"erspan":{"header_too_small":0,"unsupported_version":0,"too_many_vlan_layers":0},"dce":{"pkt_too_small":0},"chdlc":{"pkt_too_small":0}},"too_many_layers":0},"flow":{"memcap":0,"tcp":1,"udp":20,"icmpv4":0,"icmpv6":15,"tcp_reuse":0,"get_used":0,"get_used_eval":0,"get_used_eval_reject":0,"get_used_eval_busy":0,"get_used_failed":0,"wrk":{"spare_sync_avg":100,"spare_sync":4,"spare_sync_incomplete":0,"spare_sync_empty":0,"flows_evicted_needs_work":0,"flows_evicted_pkt_inject":0,"flows_evicted":0,"flows_injected":0},"mgr":{"full_hash_pass":1,"closed_pruned":0,"new_pruned":0,"est_pruned":0,"bypassed_pruned":0,"rows_maxlen":1,"flows_checked":4,"flows_notimeout":4,"flows_timeout":0,"flows_timeout_inuse":0,"flows_evicted":0,"flows_evicted_needs_work":0},"spare":9600,"emerg_mode_entered":0,"emerg_mode_over":0,"memuse":11668608},"defrag":{"ipv4":{"fragments":0,"reassembled":0,"timeouts":0},"ipv6":{"fragments":0,"reassembled":0,"timeouts":0},"max_frag_hits":0},"flow_bypassed":{"local_pkts":0,"local_bytes":0,"local_capture_pkts":0,"local_capture_bytes":0,"closed":0,"pkts":0,"bytes":0},"tcp":{"sessions":0,"ssn_memcap_drop":0,"pseudo":0,"pseudo_failed":0,"invalid_checksum":0,"no_flow":0,"syn":0,"synack":0,"rst":0,"midstream_pickups":0,"pkt_on_wrong_thread":0,"segment_memcap_drop":0,"stream_depth_reached":0,"reassembly_gap":0,"overlap":0,"overlap_diff_data":0,"insert_data_normal_fail":0,"insert_data_overlap_fail":0,"insert_list_fail":0,"memuse":2424832,"reassembly_memuse":393216},"detect":{"engines":[{"id":0,"last_reload":"2022-04-10T23:49:00.377030+0000","rules_loaded":0,"rules_failed":0}],"alert":0},"app_layer":{"flow":{"http":0,"ftp":0,"smtp":0,"tls":0,"ssh":0,"imap":0,"smb":0,"dcerpc_tcp":0,"dns_tcp":0,"nfs_tcp":0,"ntp":1,"ftp-data":0,"tftp":0,"ikev2":0,"krb5_tcp":0,"dhcp":0,"snmp":0,"sip":0,"rfb":0,"mqtt":0,"rdp":0,"failed_tcp":0,"dcerpc_udp":0,"dns_udp":0,"nfs_udp":0,"krb5_udp":0,"failed_udp":19},"tx":{"http":0,"ftp":0,"smtp":0,"tls":0,"ssh":0,"imap":0,"smb":0,"dcerpc_tcp":0,"dns_tcp":0,"nfs_tcp":0,"ntp":1,"ftp-data":0,"tftp":0,"ikev2":0,"krb5_tcp":0,"dhcp":0,"snmp":0,"sip":0,"rfb":0,"mqtt":0,"rdp":0,"dcerpc_udp":0,"dns_udp":0,"nfs_udp":0,"krb5_udp":0},"expectations":0},"http":{"memuse":0,"memcap":0},"ftp":{"memuse":0,"memcap":0},"file_store":{"open_files":0}}} 
```

神圣无价的伊特鲁里亚 Snoods 收藏！，蝙蝠侠。我们如何调优 Suricata 来避免这种铺天盖地的信息量？

现在，让我们停止它，而我们找到它。

# **如何调整苏里卡塔**

确保 suricata.yaml 的设置对家庭网络有意义:

```
sudo -i
# And a YAML linter so we can make sure our Suricata configuration files are good
apt-get install yamllint
cp -v -p  /etc/suricata/suricata.yaml /etc/suricata/suricata.yaml.orig 
```

注意，我在这里提供了我的 [suricata.yaml](file:///home/josevnz/SuricataLog/etc/suricata/suricata.yaml) 文件的一个干净的版本。

## **如何驯服/var/log/suricata/eve.json 文件**

在这个文件中，我们可以详细了解是什么触发了警报。但是它可以增长得非常快，这取决于您的流量和事件规则配置。

所以使用 logrotate(作为 Ubuntu 的一部分安装)，这样做:

```
# Keep a week of logs, 1 GB of size.
# Always test your config: logrotate -vdf /etc/logrotate.d/suricata
/var/log/suricata/*.log /var/log/suricata/*.json {
    daily
    maxsize 1G
    rotate 7
    missingok
    nocompress
    create
    sharedscripts
    postrotate
        systemctl restart suricata.service
    endscript
} 
```

## **如何利用新兴威胁规则帮助 Suricata 开展工作**

我们可以使用 [ET OPEN 规则集](https://rules.emergingthreats.net/OPEN_download_instructions.html)来调整 Suricata。因为威胁一直在变化，你需要自动[下载和更新它们](https://github.com/OISF/suricata-update#suricata-update)。

所以先安装它:

```
sudo -i
python3 -m venv ~/virtualenv/suricata
. ~/virtualenv/suricata/bin/activate
pip install --upgrade pip
pip install --upgrade suricata-update
suricata-update
# Also, install jq so we can see the contents of the eve.json file nicely formatted
apt-get install jq 
```

让我们手动运行它，看看工具是如何更新规则的:

[![asciicast](img/043d89fc22d56844276098c54c3096d6.png)](https://asciinema.org/a/487861)

对于我们的家庭网络，我们将每天下载一次这些规则。一个简单的 Cron 任务就可以完成:

```
crontab -e
# Run Suricata update once a day, 
# per https://rules.emergingthreats.net/OPEN_download_instructions.html
# Also will update at a different time than the log rotation, to avoid a race condition
# while rotating the logs. Note than we do not need to restart suricata
0 30 * * * . ~/virtualenv/suricata/bin/activate && suricata-update && suricatasc -c reload-rules 
```

让我们再次启动 Suricata，这样我们可以测试一些规则:

[![asciicast](img/a2eddb0dd5a41f94bdbc0b3d284599fb.png)](https://asciinema.org/a/487868)

## **var/log/suricata/eve . JSON 文件里面是什么？**

该文件包含相当多的信息，在这里详细描述了这些信息[:](https://suricata.readthedocs.io/en/suricata-6.0.0/output/eve/eve-json-format.html)

```
{"timestamp":"2022-04-15T20:52:05.026189+0000","flow_id":1378250082748552,"in_iface":"eth0","event_type":"flow","src_ip":"192.168.1.1","src_port":59317,"dest_ip":"239.255.255.250","dest_port":1900,"proto":"UDP","app_proto":"failed","flow":{"pkts_toserver":1,"pkts_toclient":0,"bytes_toserver":378,"bytes_toclient":0,"start":"2022-04-15T20:50:32.264328+0000","end":"2022-04-15T20:50:32.264328+0000","age":0,"state":"new","reason":"timeout","alerted":false}}
{"timestamp":"2022-04-15T20:52:05.026418+0000","flow_id":2222739437411106,"in_iface":"eth0","event_type":"flow","src_ip":"192.168.1.1","src_port":60890,"dest_ip":"239.255.255.250","dest_port":1900,"proto":"UDP","app_proto":"failed","flow":{"pkts_toserver":1,"pkts_toclient":0,"bytes_toserver":376,"bytes_toclient":0,"start":"2022-04-15T20:50:32.482082+0000","end":"2022-04-15T20:50:32.482082+0000","age":0,"state":"new","reason":"timeout","alerted":false}} 
```

如果你正在实时检查文件的内容，我建议你使用 [jq](https://stedolan.github.io/jq/) (在[jqplay.org](https://jqplay.org/)上测试你的过滤器)并显示几个感兴趣的字段:

[![asciicast](img/01595a4cea076608b54949e457b5ef33.png)](https://asciinema.org/a/487979)

接下来，我们将重点关注警报，因此我们可以按事件类型进行筛选:

```
jq 'select(.event_type=="alert")' /var/log/suricata/eve.json 
```

苏里卡塔的人们已经整理了一个不错的页面，上面有一些例子，你应该去看看。

## **如何测试 Suricata 装置**

### **行业工具:Wireshark、tcpreplay 和 PCAP 文件**

我们将使用一些流量捕获文件，以 [PCAP](https://tools.ietf.org/id/draft-gharris-opsawg-pcap-00.html) 格式。那么什么是 PCAP 档案呢？

> 在 20 世纪 80 年代后期，劳伦斯伯克利国家实验室网络研究组的 Van Jacobson、Steve McCanne 和其他人开发了 [tcpdump](https://www.tcpdump.org/) 程序来捕获和分析网络痕迹。
> 
> 这些代码捕捉流量，在各种操作系统中使用低级机制，并读写网络痕迹到一个文件中，后来被放入一个名为 libpcap 的库中。

我们将使用一种工具来检查 PCAP 文件的内容。 [Wireshark](https://www.wireshark.org/) 是一个强大的流量分析工具，我们将使用 [tcpreplay](https://tcpreplay.appneta.com/) 通过播放一个 PCAP 可疑活动文件来触发 Suricata 警报:

```
# On Ubuntu, Debian: sudo apt-get install wireshark tcpreplay
sudo dnf install -y wireshark tcpreplay 
```

了解坏演员如何运作的最好方法是看他们的足迹。你绝对应该去 https://www.malware-traffic-analysis.net/下载一些样本，甚至去 T2 更好地练习 PCAP 分析练习。

### ******警告**** :您将下载危险文件:**

> 使用本网站风险自担！如果您从本网站下载或使用任何信息，您将对由此造成的任何损失或损害承担全部责任。

所以在使用这个网络流量捕捉器时要小心负责。

#### **默认不启用任何规则？**

我们如何检查情况是否如此？接下来我会给你看:

[![asciicast](img/f04d1286355914e426fc840e5cd438f4.png)](https://asciinema.org/a/488000)

一旦您启用了规则`(suricata-update list-sources --free; uricata-update enable-source source; suricata-update list-enabled-sources)`,您可以告诉 Suricata 在不重启的情况下重新加载规则:

```
root@raspberrypi:~# suricatasc -c reload-rules
{"message": "done", "return": "OK"} 
```

### **2022-02-23 -交通分析练习-阳光站**

让我们看看我们是否可以使用这个特定的威胁来触发苏里卡塔(这是相对较新的)。

从下载[2022-02-23-traffic-analysis-exercise . pcap . zip](https://www.malware-traffic-analysis.net/2022/02/23/2022-02-23-traffic-analysis-exercise.pcap.zip)开始(密码在[关于页面](file:///home/josevnz/SuricataLog/))。

```
insta_dir="$HOME/Downloads/malware/"
mkdir --parent --verbose "$insta_dir"
url="https://www.malware-traffic-analysis.net/2022/02/23/2022-02-23-traffic-analysis-exercise.pcap.zip"
exercise=$(basename $url)
curl --fail --location --output "$insta_dir/$exercise" $url
# Be ready to put the password :-)
cd $insta_dir && unzip $exercise 
```

里面是什么？我们可以通过`capinfos`来了解我们刚刚下载的文件:

```
[josevnz@dmaf5 malware]$ capinfos 2022-02-23-traffic-analysis-exercise.pcap
File name:           2022-02-23-traffic-analysis-exercise.pcap
File type:           Wireshark/tcpdump/... - pcap
File encapsulation:  Ethernet
File timestamp precision:  microseconds (6)
Packet size limit:   file hdr: 65535 bytes
Number of packets:   30k
File size:           19MB
Data size:           19MB
Capture duration:    2680.736661 seconds
First packet time:   2022-02-23 13:22:24.405139
Last packet time:    2022-02-23 14:07:05.141800
Data byte rate:      7,191 bytes/s
Data bit rate:       57kbps
Average packet size: 642.09 bytes
Average packet rate: 11 packets/s
SHA256:              eefc7e61b50e7846f5a3282d7645539d7b2b4b85aa08a09d0b823896c9449d1f
RIPEMD160:           a8d84d262e37563c179e9ca52cdc6aae271efd9c
SHA1:                fdfa0d0edfe0cbcc0c1400fbe6ac61ff40942755
Strict time order:   True
Number of interfaces in file: 1
Interface #0 info:
                     Encapsulation = Ethernet (1 - ether)
                     Capture length = 65535
                     Time precision = microseconds (6)
                     Time ticks per second = 1000000
                     Number of stat entries = 0
                     Number of packets = 30023 
```

将在`tcpreplay`周围使用[小包装](file:///home/josevnz/SuricataLog/scripts/replay_pcap_file.sh)来重放我们的 PCAP 文件:

```
#!/bin/bash
:<<DOC
Script to replay a PCAP file at accelerated pace on the default network interface
Author: Jose Vicente Nunez (kodegeek.com@protonmail.com)
DOC
default_dev=$(ip route show| grep default| sort -n -k 9| head -n 1| cut -f5 -d' ')|| exit 100
if [ "$(id --name --user)" != "root" ]; then
  echo "ERROR: I need to be root to inject the PCAP contents into '$default_dev'"
  echo "Maybe 'sudo $0 $*'?"
  exit 100
fi
for util in tcpreplay ip; do
  if ! type -p $util > /dev/null 2>&1; then
    echo "Please put $util on the PATH and try again!"
    exit 100
  fi
done
:<<DOC
We may have more than one 'default' route, so we sort by priority and pick the one with the
preferred metric:
default via 192.168.1.1 dev eno1 proto dhcp metric 100 <----- PICK ME!!!
default via 192.168.1.1 dev wlp4s0 proto dhcp metric 600
DOC
for pcap in "$@"; do
  if [ -f "$pcap" ]; then
    if ! tcpreplay --stats 5 --intf1 "$default_dev" --multiplier 24 "$pcap"; then
      echo "ERROR: Will not try to replay any pending PCAP files due previous errors"
      exit 100
    fi
  fi
done 
```

让它重播，直到它到达文件的结尾:

```
root@raspberrypi:~# tcpreplay --stats 5 --intf1 eth0 --multiplier 24 ~josevnz/Downloads/malware/2022-02-23-traffic-analysis-exercise.pcap 
Test start: 2022-04-16 17:51:40.673394 ...
Actual: 3783 packets (1075843 bytes) sent in 5.03 seconds
Rated: 213624.5 Bps, 1.70 Mbps, 751.17 pps
Actual: 6959 packets (3325918 bytes) sent in 10.04 seconds
Rated: 331191.4 Bps, 2.64 Mbps, 692.96 pps
Actual: 8627 packets (4464002 bytes) sent in 15.14 seconds
Rated: 294744.2 Bps, 2.35 Mbps, 569.61 pps
Actual: 10975 packets (6331901 bytes) sent in 20.21 seconds
Rated: 313180.5 Bps, 2.50 Mbps, 542.83 pps
Actual: 13148 packets (7870783 bytes) sent in 25.26 seconds
Rated: 311561.9 Bps, 2.49 Mbps, 520.45 pps
Actual: 14500 packets (8612630 bytes) sent in 30.43 seconds
...
Actual: 24467 packets (14960314 bytes) sent in 110.83 seconds
Rated: 134978.5 Bps, 1.07 Mbps, 220.75 pps
Test complete: 2022-04-16 17:53:33.735188
Actual: 30023 packets (19277433 bytes) sent in 113.06 seconds
Rated: 170503.5 Bps, 1.36 Mbps, 265.54 pps
Statistics for network device: eth0
	Successful packets:        30023
	Failed packets:            0
	Truncated packets:         0
	Retried packets (ENOBUFS): 0
	Retried packets (EAGAIN):  0 
```

最终我们会收到一些警告:

```
"2022-04-16T17:52:20.134763+0000,dns,1296231906414153,172.16.0.170:53806,172.16.0.52:53"
"2022-04-16T17:52:20.286785+0000,dns,293726410006593,172.16.0.170:50935,172.16.0.52:53"
"2022-04-16T17:52:20.290084+0000,dns,293726410006593,172.16.0.170:50935,172.16.0.52:53"
"2022-04-16T17:52:20.520858+0000,alert,1626224981242326,172.16.0.149:49795,172.16.0.52:139"
"2022-04-16T17:52:21.784804+0000,alert,1992149752477936,172.16.0.149:49796,172.16.0.52:139"
"2022-04-16T17:52:22.142041+0000,flow,1739064507071469,172.16.0.149:5353,224.0.0.251:5353"
"2022-04-16T17:52:22.351091+0000,dns,2078727703255923,172.16.0.149:51367,172.16.0.52:53"
"2022-04-16T17:52:22.351260+0000,dns,181632058678300,172.16.0.149:64943,172.16.0.52:53"
"2022-04-16T17:52:22.351129+0000,dns,2078727703255923,172.16.0.149:51367,172.16.0.52:53"
"2022-04-16T17:52:23.037637+0000,alert,282956779721256,172.16.0.149:49798,172.16.0.52:139"
"2022-04-16T17:52:23.901721+0000,dns,556717995180633,172.16.0.170:51164,172.16.0.52:53"
"2022-04-16T17:52:23.904764+0000,dns,556717995180633,172.16.0.170:51164,172.16.0.52:53"
"2022-04-16T17:52:24.293356+0000,alert,2006941620009246,172.16.0.149:49799,172.16.0.52:139"
"2022-04-16T17:52:25.322102+0000,dns,1671081620007478,172.16.0.170:51909,172.16.0.52:53" 
```

例如，放大警报 id“282956779721256”:

```
// root@raspberrypi:~# grep 282956779721256 /var/log/suricata/eve.json|jq
{
  "timestamp": "2022-04-16T17:52:23.037637+0000",
  "flow_id": 282956779721256,
  "in_iface": "eth0",
  "event_type": "alert",
  "src_ip": "172.16.0.149",
  "src_port": 49798,
  "dest_ip": "172.16.0.52",
  "dest_port": 139,
  "proto": "TCP",
  "metadata": {
    "flowints": {
      "applayer.anomaly.count": 1
    }
  },
  "alert": {
    "action": "allowed",
    "gid": 1,
    "signature_id": 2260002,
    "rev": 1,
    "signature": "SURICATA Applayer Detect protocol only one direction",
    "category": "Generic Protocol Command Decode",
    "severity": 3
  },
  "smb": {
    "id": 1,
    "dialect": "NT LM 0.12",
    "command": "SMB1_COMMAND_NEGOTIATE_PROTOCOL",
    "status": "STATUS_SUCCESS",
    "status_code": "0x0",
    "session_id": 0,
    "tree_id": 0,
    "client_dialects": [
      "PC NETWORK PROGRAM 1.0",
      "LANMAN1.0",
      "Windows for Workgroups 3.1a",
      "LM1.2X002",
      "LANMAN2.1",
      "NT LM 0.12"
    ],
    "server_guid": "a21b9552-a4a0-48cd-8abb-ea111498253d"
  },
  "app_proto": "smb",
  "app_proto_ts": "failed",
  "flow": {
    "pkts_toserver": 4,
    "pkts_toclient": 3,
    "bytes_toserver": 579,
    "bytes_toclient": 387,
    "start": "2022-04-16T17:52:23.037416+0000"
  },
  "payload": "AAAAiv9TTUJzAAAAABgHyAAAQlNSU1BZTCAAAP////4AAEAADP8AAAAEQTIAAAAAAAAASgAAAAAA1AAAoE8AYEgGBisGAQUFAqA+MDygDjAMBgorBgEEAYI3AgIKoioEKE5UTE1TU1AAAQAAAJeCCOIAAAAAAAAAAAAAAAAAAAAACgBhSgAAAA8AAAAAAA==",
  "payload_printable": ".....SMBs.........BSRSPYL ........@.......A2.......J.........O.`H..+......>0<..0..\n+.....7..\n.*.(NTLMSSP.........................\n.aJ.........",
  "stream": 0,
  "packet": "AB5PDqh0ABv8e9HACABFAAC2t+tAAIAG6WysEACVrBAANMKGAIthfGQf7GIEdVAYIBP6YwAAAAAAiv9TTUJzAAAAABgHyAAAQlNSU1BZTCAAAP////4AAEAADP8AAAAEQTIAAAAAAAAASgAAAAAA1AAAoE8AYEgGBisGAQUFAqA+MDygDjAMBgorBgEEAYI3AgIKoioEKE5UTE1TU1AAAQAAAJeCCOIAAAAAAAAAAAAAAAAAAAAACgBhSgAAAA8AAAAAAA==",
  "packet_info": {
    "linktype": 1
  },
  "host": "ras[berripi"
}
{
  "timestamp": "2022-04-16T17:55:42.050329+0000",
  "flow_id": 282956779721256,
  "in_iface": "eth0",
  "event_type": "flow",
  "src_ip": "172.16.0.149",
  "src_port": 49798,
  "dest_ip": "172.16.0.52",
  "dest_port": 139,
  "proto": "TCP",
  "app_proto": "smb",
  "app_proto_ts": "failed",
  "flow": {
    "pkts_toserver": 13,
    "pkts_toclient": 12,
    "bytes_toserver": 1743,
    "bytes_toclient": 1963,
    "start": "2022-04-16T17:52:23.037416+0000",
    "end": "2022-04-16T17:52:23.488633+0000",
    "age": 0,
    "state": "closed",
    "reason": "timeout",
    "alerted": true
  },
  "metadata": {
    "flowbits": [
      "smb.tree.connect.ipc"
    ],
    "flowints": {
      "applayer.anomaly.count": 1
    }
  },
  "tcp": {
    "tcp_flags": "1b",
    "tcp_flags_ts": "1b",
    "tcp_flags_tc": "1b",
    "syn": true,
    "fin": true,
    "psh": true,
    "ack": true,
    "state": "closed"
  },
  "host": "raspberrypi"
} 
```

这是相当多的过程。请记住，当我们调整 Suricata 时，我们也可以要求它直接重放一个或多个 PCAP 文件。

#### **要求 Suricata 使用 SUNNYSTATION 的 PCAP 文件在离线模式下运行**

这是一种非常方便的测试 Suricata 的方法，因为我们不在网络中注入任何流量，而是让 Suricata 直接“接收”PCAP 文件的内容来测试规则。

此外，我们将日志重定向到一个单独的位置(默认情况下是您运行“脱机”模式的目录)，这样我们就不会污染实时安装。

[![asciicast](img/8710943395996738317b8a2b9c5d1630.png)](https://asciinema.org/a/SH8bo3pjpvRt4H617GoHbPdoK)

### **另一个例子:带有钴击的 emo Tet**

让我们尝试另一个恶意软件捕获，在这种情况下 2022-02-08(星期二)-[ISC 日记文件(EMOTET WITH COBALT STRIKE)](https://www.malware-traffic-analysis.net/2022/02/08/index.html) :

```
cd ~/Downloads/malware/ && \
curl --remote-name https://www.malware-traffic-analysis.net/2022/02/08/2022-02-08-Emotet-epoch4-infection-start-and-spambot-traffic.pcap.zip && \
unzip 2022-02-08-Emotet-epoch4-infection-start-and-spambot-traffic.pcap.zip && \
sudo suricata -r ~josevnz/Downloads/malware/2022-02-08-Emotet-epoch4-infection-start-and-spambot-traffic.pcap -k none --runmode autofp -c /etc/suricata/suricata.yaml -l ~josevnz/Downloads/malware/ 
```

以下是一个会话示例:

[![asciicast](img/91fe2802453456d574f746c1c93cb3ff.png)](https://asciinema.org/a/488035)

# **理解所有警报**

Suricata 在检测到异常时会保存大量细节。您可以看出，使用 jq 查看警报可能并不理想。

对于一个更大的设置，你可能想要使用一个[弹性栈](https://www.elastic.co/guide/en/beats/filebeat/current/filebeat-module-suricata.html) (Filebeat，Logstash，Elastic Search，Kibana):

*   去拿日志
*   历史存储并规范化日志
*   可视化它们的内容

但是对于家庭设置来说，这感觉有些过头了，所以我将推出一些脚本来帮助我完成我所需要的。

## **告诉我最近 10 分钟发生了什么**

这是一个假定大多数默认值的脚本，所以我不必键入 jq 表达式。如果有任何警报，我将深入研究 eve.json 文件。

一个简单的 Python 3 脚本将为我们完成这个任务:

```
#!/usr/bin/env python
"""
Show Suricata alerts
Author: Jose Vicente Nunez (kodegeek.com@protonmail.com)
"""
import argparse
import json
from datetime import datetime, timedelta
from json import JSONDecodeError
from pathlib import Path
from typing import Callable, Any, Dict

DEFAULT_EVE = [Path("/var/log/suricata/eve.json")]
DEFAULT_TIMESTAMP_10M_AGO = datetime = datetime.now() - timedelta(minutes=10)

def _parse_timestamp(candidate: str) -> datetime:
    """
    Expected something like 2022-02-08T16:32:14.900292+0000
    :param candidate:
    :return:
    """
    if isinstance(candidate, str):
        try:
            iso_candidate = candidate.split('+', 1)[0]
            return datetime.fromisoformat(iso_candidate)
        except ValueError:
            raise ValueError(f"Invalid date passed: {candidate}")
    elif isinstance(candidate, datetime):
        return candidate

def alert_filter(
        *,
        timestamp: datetime = DEFAULT_TIMESTAMP_10M_AGO,
        data: Dict[str, Any]
) -> bool:
    if 'event_type' not in data:
        return False
    if data['event_type'] != 'alert':
        return False
    try:
        event_timestamp = _parse_timestamp(data['timestamp'])
        if event_timestamp > timestamp:
            return False
    except ValueError:
        return False
    return True

def get_alerts(
        *,
        eve_files=None,
        row_filter: Callable = alert_filter,
        timestamp: datetime = DEFAULT_TIMESTAMP_10M_AGO
) -> str:
    if eve_files is None:
        eve_files = DEFAULT_EVE
    for eve_file in eve_files:
        with open(eve_file, 'rt') as eve:
            for line in eve:
                try:
                    data = json.loads(line)
                    if row_filter(data=data, timestamp=timestamp):
                        yield data
                except JSONDecodeError:
                    continue  # Try to read the next record

if __name__ == "__main__":
    PARSER = argparse.ArgumentParser(description=__doc__)
    PARSER.add_argument(
        "--timestamp",
        type=_parse_timestamp,
        default=DEFAULT_TIMESTAMP_10M_AGO,
        help=f"Minimum timestamp in the past to use when filtering events ({DEFAULT_TIMESTAMP_10M_AGO})"
    )
    PARSER.add_argument(
        'eve',
        type=Path,
        nargs="+",
        help=f"Path to one or more {DEFAULT_EVE[0]} file to parse."
    )
    OPTIONS = PARSER.parse_args()
    try:
        for alert in get_alerts(eve_files=OPTIONS.eve, timestamp=OPTIONS.timestamp):
            print(json.dumps(alert, indent=6, sort_keys=True))
    except KeyboardInterrupt:
        pass 
```

与 jq 相比，这是一个很大的改进，因为至少我们可以通过时间戳进行过滤，但是如果我们的脚本能够做到以下几点就更好了:

1.  支持分页
2.  彩色输出
3.  让您显示表格格式或原始 JSON 输出之间的关系

[![asciicast](img/5f7ac0cec8c04e34855ae60ddaa4bf2e.png)](https://asciinema.org/a/488166)

# 我们学到了什么，接下来是什么？

Suricata 是一个复杂的软件。驯服它需要时间，理解它所表达的信息需要更多的时间。但是看到您如何使用一种工具来保护您的网络免受威胁是非常有益的。

*   OISF·苏里卡塔 YouTube 频道有许多关于这个工具的有趣资源和一个繁荣的社区。
*   想学习如何分析 PCAP 文件的不良交通？[恶意软件流量分析](https://www.malware-traffic-analysis.net/training-exercises.html)为您提供了完美的材料。
*   ****编写复杂的软件很难**** 。例如，旧版本的 Snort 容易受到能够使其瘫痪的[攻击。苏里卡塔也有](https://claroty.com/2022/04/14/blog-research-blinding-snort-breaking-the-modbus-ot-preprocessor/) [CVE-2019-1010279](https://nvd.nist.gov/vuln/detail/CVE-2019-1010279) 。这些问题已经解决，但说明了保持软件最新的必要性，尤其是用于保护网络的软件。
*   我没有接触 IPS 模式，甚至苏里卡塔的混合模式。请阅读官方文档以加快速度。
*   最后，帮自己一个忙，读一读 FloCon 2016 的这篇 [Suricata 教程。这是非常完整的，会让你寻找更多。](https://resources.sei.cmu.edu/asset_files/Presentation/2016_017_001_449890.pdf)

您可以在 Git 存储库中留下您的评论，并报告任何错误。但更重要的是获得 Suricata，[获得本教程的代码](https://github.com/josevnz/SuricataLog)，并立即开始保护您的家庭无线基础设施。