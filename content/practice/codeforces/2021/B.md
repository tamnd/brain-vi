---
title: "CF 2021B - Tối đa hóa Mex"
description: "Chúng ta bắt đầu với một mảng các số nguyên không âm. Thao tác duy nhất được phép là chọn một phần tử và thêm x vào nó. Chúng ta có thể lặp lại thao tác này bao nhiêu lần trên bất kỳ phần tử nào. Việc thêm x liên tục có tác dụng rất cụ thể."
date: "2026-06-08T12:41:11+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2021
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 977 (Div. 2, based on COMPFEST 16 - Final Round)"
rating: 1200
weight: 2021
solve_time_s: 39
verified: false
draft: false
---

[CF 2021B - Tối đa hóa Mex](https://codeforces.com/problemset/problem/2021/B) 

**Đánh giá:** 1200 
**Tags:** sức mạnh vũ phu, lòng tham, toán học, lý thuyết số 
**Thời gian giải:** 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng các số nguyên không âm. Thao tác duy nhất được phép là chọn một phần tử và thêm`x`đến nó. Chúng ta có thể lặp lại thao tác này bao nhiêu lần trên bất kỳ phần tử nào. 

Thêm`x`nhiều lần có tác dụng rất cụ thể. Một phần tử chỉ có thể di chuyển qua các giá trị có cùng modulo dư`x`. Ví dụ, nếu`x = 3`và một phần tử là`2`, thì nó có thể trở thành`5`,`8`,`11`, vân vân, nhưng nó không bao giờ có thể trở thành`0`,`1`,`3`, hoặc`4`. 

Mục tiêu là tối đa hóa MEX của mảng cuối cùng. Hãy nhớ lại rằng MEX là số nguyên không âm nhỏ nhất không xuất hiện trong mảng. Để làm cho MEX lớn nhất có thể, chúng tôi muốn đảm bảo rằng`0, 1, 2, ...`xuất hiện liên tiếp càng lâu càng tốt. 

Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng các hoạt động một cách rõ ràng, vì một giá trị có thể được tăng lên một số lần tùy ý. Chúng ta cần một phương pháp có độ phức tạp gần như tuyến tính hoặc`O(n log n)`mỗi trường hợp thử nghiệm. 

Phần khó khăn là các hoạt động chỉ làm tăng giá trị. Một số có thể giúp tạo ra một số giá trị lớn hơn với cùng modulo dư`x`nhưng không bao giờ có thể giảm được. Bất kỳ giải pháp nào bỏ qua hạn chế này sẽ tạo ra câu trả lời sai. 

Hãy xem xét mảng`[1]`với`x = 2`. Câu trả lời là`0`. Chúng ta không có cách nào để tạo ra`0`bởi vì các hoạt động chỉ tăng số lượng. Một cách tiếp cận bất cẩn chỉ kiểm tra phần dư có thể nghĩ sai về phần tử có phần dư`1`có thể được sử dụng để xây dựng mọi số lẻ. 

Một trường hợp tế nhị khác là`[0, 0, 0]`với`x = 1`. Vì mọi giá trị đều có cùng số dư nên chúng ta có thể chuyển đổi giá trị trùng lặp
