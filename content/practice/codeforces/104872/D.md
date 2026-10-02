---
title: "CF 104872D - Chuỗi a, ab, ba"
description: "Chúng tôi đang duy trì một chuỗi nhị phân chỉ gồm các ký tự a và b, với hai thao tác được áp dụng trực tuyến. Thao tác đầu tiên lật một vị trí duy nhất, biến a thành b hoặc b thành a."
date: "2026-06-28T10:25:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104872
codeforces_index: "D"
codeforces_contest_name: "2023-2024 Russia Team Open, High School Programming Contest (VKOSHP XXIV)"
rating: 0
weight: 104872
solve_time_s: 35
verified: false
draft: false
---

[CF 104872D - Chuỗi a, ab, ba](https://codeforces.com/problemset/problem/104872/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một chuỗi nhị phân chỉ gồm các ký tự`a`Và`b`, với hai thao tác được áp dụng trực tuyến. Thao tác đầu tiên lật một vị trí, xoay`a`vào trong`b`hoặc`b`vào trong`a`. Thao tác thứ hai hỏi liệu một chuỗi con đã cho có thể được phân chia hoàn toàn thành các khối liền kề hay không, trong đó mỗi khối chính xác là một trong ba dạng được phép: một ký tự đơn`a`, hoặc một cặp`ab`, hoặc một cặp`ba`. 

Vì vậy, mọi phân tách hợp lệ là một chuỗi con được sắp xếp bằng cách sử dụng các ô có độ dài 1 hoặc 2, nhưng các ô có độ dài 2 bị hạn chế: chúng phải xen kẽ các ký tự. 

Khó khăn chính là chúng ta phải trả lời tới 100.000 bản cập nhật và 100.000 truy vấn trên một chuỗi có độ dài lên tới 100.000, do đó, bất kỳ giải pháp nào quét lại chuỗi con cho mỗi truy vấn đều ngay lập tức quá chậm. Việc kiểm tra trực tiếp từng chuỗi con truy vấn sẽ tốn O(n) cho mỗi truy vấn trong trường hợp xấu nhất, dẫn đến O(nq), điều này hoàn toàn không khả thi. 

Điều tinh tế là việc ốp lát được phép không hề tùy tiện. Ví dụ: một chuỗi con như`aa`luôn hợp lệ vì nó có thể được chia thành hai`a`gạch lát. Nhưng`aaa`cũng hợp lệ. Trong khi đó`aba`có giá trị như`ab | a`, trong khi`aab`có giá trị như`a | ab`. Trở ngại thực sự là khi chúng ta buộc phải đặt một ô có chiều dài 2 nhưng các ràng buộc về tính chẵn lẻ cục bộ và tính liền kề lại xung đột. 

Một sự ngây thơ
