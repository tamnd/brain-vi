---
title: "CF 1493E - XOR khổng lồ"
description: "Chúng ta có hai số nguyên không âm, l và r, được biểu diễn dưới dạng chuỗi nhị phân có độ dài bằng nhau. Nhiệm vụ là xem xét tất cả các phạm vi liền kề [x, y] trong đó l ≤ x ≤ y ≤ r và tính toán XOR theo bit của tất cả các số nguyên trong phạm vi đó. Đặt g(x, y) biểu thị XOR này."
date: "2026-06-10T22:15:58+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "greedy", "math", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1493
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 705 (Div. 2)"
rating: 2600
weight: 1493
solve_time_s: 51
verified: false
draft: false
---

[CF 1493E - XOR khổng lồ](https://codeforces.com/problemset/problem/1493/E) 

**Đánh giá:** 2600 
**Tags:** bitmasks, thuật toán xây dựng, tham lam, toán học, chuỗi, hai con trỏ 
**Thời gian giải:** 51s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên không âm,`l`Và`r`, được biểu diễn dưới dạng chuỗi nhị phân có độ dài bằng nhau. Nhiệm vụ là xem xét tất cả các phạm vi liền kề`[x, y]`Ở đâu`l ≤ x ≤ y ≤ r`và tính toán XOR theo bit của tất cả các số nguyên trong phạm vi đó. Cho phép`g(x, y)`biểu thị XOR này. Chúng ta được yêu cầu tìm giá trị lớn nhất có thể có của`g(x, y)`trên tất cả các phạm vi hợp lệ. 

Nói cách khác, chúng ta đang tìm một tập con các số bên trong`[l, r]`có XOR, được coi là số nguyên, càng lớn càng tốt. Đầu ra phải ở dạng nhị phân không có số 0 đứng đầu. 

Những ràng buộc cho phép`n`lên đến 10^6, nghĩa là`l`Và`r`có thể dài tới một triệu bit. Điều này loại trừ bất kỳ cách tiếp cận ngây thơ nào liệt kê các số giữa`l`Và`r`hoặc kiểm tra mọi phạm vi con có thể có: số lượng phạm vi như vậy tăng bậc hai trong trường hợp xấu nhất và thậm chí việc lưu trữ các số có độ dài 10^6 trong một mảng đơn giản là không thực tế. 

Sự tinh tế nằm trong biểu diễn nhị phân. Những khác biệt nhỏ ở những bit quan trọng nhất giữa`l`Và`r`có thể cho phép XOR của một số phạm vi đạt được một số với tất cả các số có bit cao. Các trường hợp cạnh bao gồm khi`l`bằng`r`, hoặc khi`l`bằng không và`r`là lũy thừa lớn của hai trừ một. Một cách tiếp cận bất cẩn mà XOR bit độc lập hoặc giả sử các thuộc tính số đơn giản sẽ không thành công trên các đầu vào này. Ví dụ, nếu`l = 0011`Và`r = 0110`, XOR tối đa là`0111`, không`0110`, vì XOR một dải con được chọn cẩn thận có thể lật bit cao nhất trong đó`l`Và`r`khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp đi lặp lại tất cả những gì có thể`x`Và`y`TRONG`[l, r]`, tính XOR`g(x, y)`và theo dõi mức tối đa. Tính toán XOR của một phạm vi con`[x, y]`có thể được tối ưu hóa bằng cách sử dụng mảng XOR tiền tố`px[i] = 0 ⊕ 1 ⊕ ... ⊕ i`, với`g(x, y) = px[y] ⊕ px[x-1]`. Tuy nhiên,`x`Và`y`mỗi cái có thể lên tới 2^n và n có thể lên tới 10^6. Ngay cả với tiền tố XOR, chúng ta sẽ cần phải lặp lại trên một số lượng lớn các phạm vi. Vì vậy, vũ lực là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc phân tích cấu trúc nhị phân. Hãy xem xét các biểu diễn nhị phân của`l`Và`r`. Cho phép`k`là vị trí của bit quan trọng nhất trong đó`l`Và`r`khác nhau. Để tối đa hóa XOR, chúng tôi muốn chọn`x`Và`y`sao cho kết quả XOR có cái này`k`- tập bit thứ. Khi chúng ta đặt bit này, chúng ta có thể tự do đặt tất cả các bit thấp hơn thành một để tối đa hóa giá trị. Về cơ bản, XOR tối ưu là một số trong đó tất cả các bit từ bit khác biệt quan trọng nhất đến bit ít quan trọng nhất đều là một. 

Quan sát này làm giảm vấn đề thành một phép tính duy nhất: tìm bit quan trọng nhất trong đó`l`Và`r`khác nhau và đặt tất cả các bit từ vị trí đó trở xuống. Điều này có thể được thực hiện trong thời gian tuyến tính tương ứng với độ dài bit của các số, O(n). Không cần liệt kê các phạm vi vì giá trị XOR chỉ phụ thuộc vào bit khác nhau cao nhất. 

| Tiếp cận | Thời Cô
