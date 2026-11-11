---
title: "CF 1575L - Giải cấu trúc mảng dài nhất"
description: "Chúng ta bắt đầu với một mảng. Chúng tôi có thể xóa nhiều lần các phần tử tùy ý và sau mỗi lần xóa, các phần tử còn lại sẽ khít lại với nhau. Đối với bất kỳ mảng kết quả nào, hãy xác định điểm của nó là số vị trí trong đó giá trị bằng chỉ số dựa trên 1 hiện tại của nó."
date: "2026-06-10T11:00:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1575
codeforces_index: "L"
codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1575
solve_time_s: 154
verified: false
draft: false
---

[CF 1575L - Giải cấu trúc mảng dài nhất](https://codeforces.com/problemset/problem/1575/L) 

**Đánh giá:** 2100 
**Thẻ:** cấu trúc dữ liệu, phân chia và chinh phục, dp, sắp xếp 
**Thời gian giải:** 2m 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng. Chúng tôi có thể xóa nhiều lần các phần tử tùy ý và sau mỗi lần xóa, các phần tử còn lại sẽ khít lại với nhau. 

Đối với bất kỳ mảng kết quả nào, hãy xác định điểm của nó là số vị trí trong đó giá trị bằng chỉ số dựa trên 1 hiện tại của nó. Chúng ta muốn chọn một dãy con của mảng ban đầu, giữ nguyên thứ tự tương đối, sao cho trong mảng kết quả có càng nhiều vị trí càng tốt thỏa mãn$$b_i=i.$$Nhiệm vụ là tính số điểm tối đa có thể đạt được. 

Quan sát chính là việc xóa các phần tử chỉ thay đổi chỉ số. Thứ tự tương đối của các phần tử được giữ không bao giờ thay đổi. Bất kỳ mảng cuối cùng nào cũng chỉ đơn giản là một dãy con của mảng ban đầu. 

Những hạn chế là lớn. Độ dài mảng có thể đạt tới$2 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào xem xét rõ ràng các tập hợp con, khoảng hoặc lập trình động trên các cặp vị trí. Thậm chí$O(n^2)$sẽ yêu cầu đại khái$4 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa thời hạn. Chúng ta cần một cái gì đó gần gũi$O(n \log n)$. 

Một số tình huống rất dễ bị hiểu sai. 

Coi như:```
3
1 1 1
```Câu trả lời là 1. Chúng ta có thể giữ một phần tử duy nhất và thu được`[1]`, có vị trí đầu tiên phù hợp. Một giải pháp bất cẩn có thể đếm tất cả các lần xuất hiện của giá trị 1 và trả về sai 3. 

Hãy xem xét:```
4
4 4 4 4
```Câu trả lời là 1. Giữ tất cả các phần tử chỉ cho một kết quả khớp ở vị trí 4. Chúng ta cũng có thể giữ một phần tử duy nhất, nhưng khi đó nó nằm ở vị trí 1 và không còn khớp nữa. Chỉ mục sau khi xóa có vấn đề. 

Coi như:```
5
2 3 4 5 6
```Câu trả lời là 5. Không có phần tử nào ban đầu khớp với vị trí của nó, nhưng việc giữ nguyên toàn bộ mảng sẽ mang lại vị trí```
1 2 3 4 5
```chống lại các giá trị```
2 3 4 5 6
```mà vẫn không khớp. Tuy nhiên, nếu chúng ta nghĩ về các vị trí dãy con cuối cùng, mọi phần tử đều có thể trở thành một phần tử khớp vì phần tử được giữ đầu tiên trở thành vị trí 1, phần tử thứ hai trở thành vị trí 2, v.v. Một giải pháp chỉ nhìn vào chỉ số gốc sẽ bỏ lỡ điều này. 

## Phương pháp tiếp cận 

Một quan điểm vũ phu là đơn giản. Mỗi mảng cuối cùng là một dãy con của dãy ban đầu, vì vậy chúng ta có thể liệt kê tất cả các dãy con, tính điểm của chúng và tính toán
