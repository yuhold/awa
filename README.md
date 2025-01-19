# awa
 一个探针项目

部署步骤：

创建 runyun_probe 目录，例如 /opt/runyun_probe/。

将 config.ini, probe.py, utils.py 和 uploader.py 保存到 /opt/runyun_probe/。

将 runyun_probe.service 保存到 /etc/systemd/system/。

修改 runyun_probe.service 文件中的 WorkingDirectory 和 ExecStart，替换成你的实际路径。

修改 config.ini 中的配置，替换成你自己的配置。

启用并启动 systemd 服务：

BASH
sudo systemctl enable runyun_probe.service
sudo systemctl start runyun_probe.service

检查服务状态：

BASH
sudo systemctl status runyun_probe.service
查看日志：

BASH
sudo tail -f /var/log/runyun_probe.log

代码解释：


config.ini： 配置文件，管理所有参数。
utils.py： 包含加载配置和发送邮件的工具函数。

probe.py：
使用 APScheduler 创建后台调度器。

probe_task 函数负责网站探测，并在连续失败3次时发送邮件通知。

trigger_uploader 函数负责触发被测端程序。

主程序启动调度器，并使用 time.sleep 来保持程序运行。

uploader.py: 被测端触发程序，可以替换成你自己的上传逻辑。

runyun_probe.service： systemd 服务文件，用于守护 probe.py。

关键点:

模块化: 代码结构更加模块化，易于扩展和维护。

配置化: 所有参数都从 config.ini 文件读取，方便修改。

定时任务: 使用 APScheduler 实现定时任务调度。

进程守护: 使用 systemd 守护探针程序。