# Route Configuration

This repository is a lab for NCTU course "Introduction to Computer Networks 2018".

---
## Abstract

In this lab, we are going to write a Python program with Ryu SDN framework to build a simple software-defined network and compare the different between two forwarding rules.

---
## Objectives

1. Learn how to build a simple software-defined networking with Ryu SDN framework
2. Learn how to add forwarding rule into each OpenFlow switch

---
## Execution

> TODO:
> * How to run your program?
> * What is the meaning of the executing command (both Mininet and Ryu controller)?
> * Show the screenshot of using iPerf command in Mininet (both `SimpleController.py` and `controller.py`)
1.
2.
3.
![image](https://github.com/nctucn/lab3-isbecky27/blob/master/result1.jpg)
![image](https://github.com/nctucn/lab3-isbecky27/blob/master/result2.jpg)
---
## Description

### Tasks

> TODO:
> * Describe how you finish this work in detail

1. Environment Setup
   - 開啟PieTTY IP位址為140.113.195.69 port為13316(學號末5碼)
   - 登入 Login:root Password:0210 (改密碼 指令:passwd)
   - 輸入指令:git clone https://github.com/nctucn/lab2-isbecky27.git Route_Configuration 將檔案複製下來
   - 接著登入github Username for 'https://github.com': isbecky27 和 Password for 'https://isbecky27@github.com': 密碼
   - 試著執行 Mininet 使用指令:sudo mn 接下來若跑出錯誤訊息 如:You may wish to try "service openvswitch-switch start".
   - 則輸入指令:sudo service openvswitch-switch start 然後再執行一次 sudo mn 即可
   
2. Example of Ryu SDN
   - 打開第2個終端機 IP位址為140.113.195.69 port為13316(學號末5碼)
   - 登入 Login:root Password:0210
   - 在第一個終端機執行SimpleTopo.py 指令:sudo mn --custom SimpleTopo.py --topo topo --link tc --controller remote
   - 若出現錯誤"Exception: Error creating interface pair (s1-eth1,s2eth1): RTNETLINK answers: File exists"
   - 則輸入 mn -c 再重新執行即可
   - 若出現錯誤"Error setting resource limits. Mininet's performance may be affected."
   - 則不用理會 在這次的Lab中不影響
   - 接下來在另一個終端機執行SimpleController.py 指令:sudo ryu-manager SimpleController.py --observe-links
   - 若出現loading app controller.py ....則表示成功
   - 接著輸入exit離開第一個終端機(SimpleTopo.py) 再按Ctrl-z離開另一個終端機(SimpleController.py)
   - 最後輸入mn -c 確認"RTNETLINK"為乾淨的
   
3. Mininet Topology
   - 複製SimpleTopo.py至新檔案topo.py 指令:cp SimpleTopo.py topo.py
   - 並加入topo.png內有的條件(頻bandwidth、delay、loss)
   - 在第一個終端機執行topo.py 指令:sudo mn --custom SimpleTopo.py --topo topo --link tc --controller remote
   - 接著在另一台終端機內執行SimpleController.py 指令:sudo ryu-manager SimpleController.py –-observe-links 
   - 若出現錯誤"ImportError: No module named SimpleController.py" 表示controller的程式有問題
   - 若出現錯誤"SyntaxError: invalid syntax (SimpleTopo.py, line 19)" 表示topology的程式有問題
 
4. Ryu Controller
   - 複製SimpleController.py至新檔案controller.py 指令:cp SimpleController.py controller.py
   - 並修改switch_features_handler(self, ev)的部分(依據topo.png的資訊)

5. Measurement
   - 在第一個終端機執行topo.py 指令:sudo mn --custom topo.py --topo topo --link tc --controller remote
   - 接著在另一台終端機內執行SimpleController.py 指令:sudo ryu-manager SimpleController.py –-observe-links 
   - 輸入iPerf指令 測試頻寬mininet> h1 iperf -s -u -i 1 –p 5566 > ./out/result1 & 和 mininet> h2 iperf -c 10.0.0.1 -u –i 1 –p 5566
   - 執行後的結果result1會在出現在out資料夾中
   - 輸入:exit 離開mininet(topo.py) 再按Ctrl-z離開另一個終端機(SimpleController.py)
   - 最後輸入mn -c 確認"RTNETLINK"為乾淨的
   - 再來在第一個終端機執行topo.py 指令:sudo mn --custom topo.py --topo topo --link tc --controller remote
   - 接著在另一台終端機內執行controller.py 指令:sudo ryu-manager controller.py –-observe-links 
   - 輸入iPerf指令 測試頻寬mininet> h1 iperf -s -u -i 1 –p 5566 > ./out/result2 & 和 mininet> h2 iperf -c 10.0.0.1 -u –i 1 –p 5566
   - 輸入:exit 離開mininet(topo.py) 再按Ctrl-z離開另一個終端機(controller.py)
   - 最終的結果result2會出現在out資料夾中


### Discussion

> TODO:
> * Answer the following questions

1. Describe the difference between packet-in and packet-out in detail.
   
2. What is “table-miss” in SDN?
   
3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?
   
4. Explain the following code in `controller.py`.
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    ```

5. What is the meaning of “datapath” in `controller.py`?
   
6. Why need to set "`ip_proto=17`" in the flow entry?
   設定IP協定種類為UDP
   
7. Compare the differences between the iPerf results of `SimpleController.py` and `controller.py` in detail.
   
8. Which forwarding rule is better? Why?

---
## References

> TODO: 
> * Please add your references in the following

* **Ryu SDN**
    * [Ryubook Documentation](https://osrg.github.io/ryu-book/en/html/)
    * [Ryubook [PDF]](https://osrg.github.io/ryu-book/en/Ryubook.pdf)
    * [Ryu 4.30 Documentation](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
    * [Ryu Controller Tutorial](http://sdnhub.org/tutorials/ryu/)
    * [OpenFlow 1.3 Switch Specification](https://www.opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf)
    * [Ryubook 說明文件](https://osrg.github.io/ryu-book/zh_tw/html/)
    * [GitHub - Ryu Controller 教學專案](https://github.com/OSE-Lab/Learning-SDN/blob/master/Controller/Ryu/README.md)
    * [Ryu SDN 指南 – Pengfei Ni](https://feisky.gitbooks.io/sdn/sdn/ryu.html)
    * [OpenFlow 通訊協定](https://osrg.github.io/ryu-book/zh_tw/html/openflow_protocol.html)
* **Python**
    * [Python 2.7.15 Standard Library](https://docs.python.org/2/library/index.html)
    * [Python Tutorial - Tutorialspoint](https://www.tutorialspoint.com/python/)
* **Others**
    * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
    * [Vim Tutorial – Tutorialspoint](https://www.tutorialspoint.com/vim/index.htm)
    * [鳥哥的 Linux 私房菜 – 第九章、vim 程式編輯器](http://linux.vbird.org/linux_basic/0310vi.php)

---
## Contributors

* [Ching](https://github.com/isbecky27)
* [David Lu](https://github.com/yungshenglu)

---
## License

GNU GENERAL PUBLIC LICENSE Version 3
