---
title: "CF 1530G - Thật là một sự đảo ngược"
description: "Chúng ta có hai chuỗi nhị phân a và b có cùng độ dài n và số nguyên k. Mục tiêu của chúng ta là biến đổi a thành b bằng cách đảo ngược liên tục các chuỗi con của a chứa chính xác k chuỗi con. Mỗi sự đảo ngược có thể liên quan đến bất kỳ số không nào."
date: "2026-06-10T16:55:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1530
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 733 (Div. 1 + Div. 2, based on VK Cup 2021 - Elimination (Engine))"
rating: 3300
weight: 1530
solve_time_s: 49
verified: false
draft: false
---

[CF 1530G - Thật là một sự đảo ngược](https://codeforces.com/problemset/problem/1530/G) 

**Đánh giá:** 3300 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi nhị phân,`a`Và`b`, có cùng độ dài`n`và một số nguyên`k`. Mục tiêu của chúng tôi là biến đổi`a`vào trong`b`bằng cách đảo ngược liên tục các chuỗi con của`a`chứa chính xác`k`những cái đó. Mỗi sự đảo ngược có thể liên quan đến bất kỳ số không nào. Đầu ra phải là một chuỗi đảo ngược hoặc`-1`nếu việc chuyển đổi là không thể trong giới hạn quy định. 

Vấn đề mang tính xây dựng nhưng nó có một hạn chế tinh tế: chúng ta không thể tùy tiện di chuyển các cái. Chúng ta chỉ có thể đảo ngược các khối chứa chính xác`k`những cái đó. Điều này có nghĩa là chúng ta cần suy nghĩ về vị trí của những người
