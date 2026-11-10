---
title: "CF 1534A - Tô màu lá cờ"
description: "Đầu ra được yêu cầu 10 cho \"0?10\" thực sự là không thể. Tổng cộng chỉ có 10 chuỗi con của một chuỗi có độ dài 4 và hai trong số đó không đẹp: - \"0?10\" không thể xen kẽ được. - \"?10\" không thể thực hiện luân phiên được. Vậy đáp án đúng là 8."
date: "2026-06-10T15:58:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1534
codeforces_index: "A"
codeforces_contest_name: "Codeforces LATOKEN Round 1 (Div. 1 + Div. 2)"
rating: 800
weight: 1534
solve_time_s: 85
verified: false
draft: false
---

[CF 1534A - Tô màu lá cờ](https://codeforces.com/problemset/problem/1534/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
Đầu ra được yêu cầu`10`vì`"0?10"`thực sự là không thể. Chỉ có tổng cộng 10 chuỗi con của một chuỗi có độ dài 4 và hai trong số đó không đẹp: 

-`"0?10"`không thể thực hiện luân phiên được. 
-`"?10"`không thể thực hiện luân phiên được. 

Vậy câu trả lời đúng là`8`. 

Nguyên nhân sâu xa là việc triển khai không duy trì đúng ranh giới xung đột. Giải pháp tiêu chuẩn được chấp nhận cho Codeforces 1535C theo dõi vị trí gần đây nhất khiến một trong hai mẫu xen kẽ không thể thực hiện được. 

Hãy nghĩ đến hai mẫu xen kẽ hợp lệ: 

- Mẫu A:`010101...`- Mẫu B:`101010...`Đối với mỗi vị trí`i`, một ký tự cố định (`0`hoặc`1`) có thể mâu thuẫn với một trong những mẫu này. Chúng tôi ghi lại vị trí mâu thuẫn mới nhất cho từng mẫu. 

Cho phép:

-`last0`= vị trí mới nhất trong đó Mẫu A không thể thực hiện được. 
-`last1`= vị trí mới nhất trong đó Mẫu B không thể thực hiện được. 

Khi đó mọi chuỗi con đẹp đều kết thúc ở vị trí`i`phải bắt đầu hoàn toàn sau quan điểm trước đó của hai quan điểm mâu thuẫn đó. Do đó số lượng chuỗi con hợp lệ kết thúc tại`i`là:```
PythonRun
```Lỗi thường gặp nhất là cập nhật sai biến khi xử lý`0`Và`1`. 

Đối với mẫu A (`0101...`), chức vụ`i`mong đợi`i % 2`. 

Đối với Mẫu B (`1010...`), chức vụ`i`mong đợi`1 - (i % 2)`. 

Nếu ký tự thực tế không đồng ý với Mẫu A, hãy cập nhật`last0 = i`. 

Nếu ký tự thực tế không đồng ý với Mẫu B, hãy cập nhật`last1 = i`. 

Việc thực hiện được chấp nhận là:```
PythonRun
```Theo dõi nhanh mẫu bị lỗi`"0?10"`: 

| tôi | ch | cuối cùng0 | cuối cùng1 | thêm | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | -1 | 0 | 1 | 
| 1 | ? | -1 | 0 | 2 | 
| 2 | 1 | -1 | 2 | 3 | 
| 3 | 0 | -1 | 3 | 4 | 

Tổng cộng =`1 + 2 + 3 + 4 = 10`. 

Thoạt nhìn, điều này dường như tạo ra kết quả sai, điều này cho chúng ta biết một điều quan trọng: đoạn mã trên chỉ đúng khi`last0`Và`last1`được hiểu là sự không khớp mới nhất của hai mẫu và việc cập nhật được thực hiện chính xác như trong giải pháp được chấp nhận. Thực tế là dấu vết của bạn vẫn cho điểm 10 có nghĩa là việc triển khai bạn đã thử nghiệm không phải là công thức được chấp nhận tiêu chuẩn. 

Thay vào đó, giải pháp Codeforces được chấp nhận sẽ theo dõi vị trí mới nhất phá vỡ từng phép gán chẵn lẻ:```
PythonRun
```Đây là giải pháp được chấp nhận theo quy luật cho CF 1535C và tạo ra:```

```cho đầu vào mẫu.
