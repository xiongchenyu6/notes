---
title: "Tencent"
id: "85DD05BD-8404-441E-B6D7-B575B532FB82"
date: "2022-02-22 Tue 21:44"
---

# Presentations

# OKR

1.  外部账单新需求接入到收敛周期从1周到2周缩减到3天到1周
2.  账单调度模块使用go重写。
3.  至少有10个以上 海外外部对账 使用对账平台

python3 main.py -l debug upload --cluster singapore~cros~ --project t~billingcentercheckstatisexternal~=t~billingcentercheckstatisgroupexternal~,t~billingcentercheckfaileditemexternal~=t~billingcentercheckfaileditemgroupexternal~ --column group~id~ postgres --port "5432" --user <REDACTED> --password <REDACTED> --db teg~settdb~ <REDACTED_HOST>
