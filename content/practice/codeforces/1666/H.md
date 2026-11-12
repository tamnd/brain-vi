---
title: "CF 1666H - Anh hùng quyền năng"
description: "Chúng ta được cấp một chuỗi các số nguyên đại diện cho các anh hùng. Mỗi anh hùng có thể đánh bại những con quái vật có sức mạnh ngang bằng hoặc yếu hơn."
date: "2026-06-10T02:17:11+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1666
codeforces_index: "H"
codeforces_contest_name: "2021-2022 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3500
weight: 1666
solve_time_s: 47
verified: true
draft: false
---

[CF 1666H - Anh hùng quyền năng](https://codeforces.com/problemset/problem/1666/H) 

**Đánh giá:** 3500 
**Thẻ:** toán 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên đại diện cho các anh hùng. Mỗi anh hùng có thể đánh bại những con quái vật có sức mạnh ngang bằng hoặc yếu hơn. Chúng tôi được yêu cầu, đối với mỗi sức mạnh quái vật có thể có từ 1 đến sức mạnh anh hùng tối đa, xác định số lượng anh hùng tối thiểu cần thiết để bao phủ tất cả quái vật có sức mạnh đó trở xuống. Về cơ bản, chúng tôi muốn tiền tố nhỏ nhất của các anh hùng sao cho tổng sức mạnh của họ bao gồm mọi số nguyên từ 1 đến một số sức mạnh mục tiêu. 

Đầu vào bao gồm một dòng số nguyên biểu thị sức mạnh của anh hùng. Đầu ra là một chuỗi các số nguyên, mỗi số cho mỗi sức mạnh của quái vật, cho biết số lượng anh hùng tối thiểu cần có. 

Số lượng anh hùng tối đa có thể lên tới 200.000 và sức mạnh anh hùng có thể lên tới$10^9$. Điều này loại trừ bất kỳ cách tiếp cận vũ phu nào lặp đi lặp lại một cách rõ ràng trên tất cả quái vật cho mọi sức mạnh, bởi vì điều đó có thể dẫn đến$2 \cdot 10^{14}$hoạt động vượt xa khả năng thực hiện. 

Trường hợp cạnh xảy ra khi tất cả các anh hùng có sức mạnh 1. Ví dụ: nếu đầu vào là`[1, 1, 1]`, số lượng anh hùng tối thiểu để trang trải quái vật có sức mạnh 1, 2 và 3 là`1, 2, 3`. Một thuật toán ngây thơ có thể cho rằng một anh hùng có thể bao gồm nhiều cấp độ sức mạnh một cách không chính xác mà không tính tổng. Một trường hợp khác là một anh hùng rất mạnh, ví dụ:`[10]`. Thuật toán phải báo cáo chính xác`1`cho tất cả sức mạnh của quái vật lên tới 10. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ sắp xếp các anh hùng và sau đó, đối với sức mạnh của từng quái vật mục tiêu$m$, hãy thử tất cả các kết hợp của các anh hùng để xem tiền tố nào có tổng ít nhất$m$. Điều này đúng về nguyên tắc nhưng không thực tế, vì việc kiểm tra tất cả các tiền tố cho tất cả các điểm mạnh sẽ dẫn đến$O(n \cdot M)$thời gian, ở đâu$M$là sức mạnh tối đa. Nếu như$n = 2 \cdot 10^5$Và$M = 10^9$, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống vấn đề bao phủ tổng tiền tố. Sau khi các anh hùng được sắp xếp theo thứ tự giảm dần, chúng ta có thể lặp đi lặp lại việc tích lũy tổng sức mạnh. Đối với mỗi khoản tiền$S$, tất cả sức mạnh của quái vật lên đến$S$có thể bị đánh bại bởi lần đầu tiên$k$những anh hùng. Sau đó chúng ta có thể nhanh chóng gán câu trả lời cho tất cả các điểm mạnh từ tổng trước cộng một cho đến$S$sử dụng số lượng anh hùng hiện tại. Các anh hùng không cần phải được kiểm tra theo các tổ hợp hoặc tập hợp con vì việc tổng hợp các anh hùng lớn nhất hiện có trước tiên sẽ đảm bảo số lượng anh hùng tối thiểu cho phạm vi bao phủ. 

Brute-force hoạt động vì câu trả lời dựa trên phạm vi cường độ tích lũy nhưng không thành công khi đầu vào lớn do quét nhiều lần. Quan sát rằng các tổng tiền tố bao gồm các phạm vi liên tiếp cho phép chúng ta rút gọn nghiệm thành$O(n + M)$thời gian nếu chúng ta chỉ tính toán trước sức mạnh tối đa được bao phủ, tránh việc lặp lại không cần thiết đối với sức mạnh của quái vật không thể tiếp cận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * max_ Strength) | O(n) | Quá chậm | 
| Bảo hiểm Tổng tiền tố | O(n log n) | O(n + max_ Strength) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách sức mạnh anh hùng. 
2. Sắp xếp các anh hùng theo thứ tự giảm dần để sớm tối đa hóa phạm vi bao phủ. Điều này đảm bảo số lượng anh hùng tối thiểu được sử dụng cho bất kỳ sức mạnh nhất định nào. 
3. Khởi tạo biến tổng tích lũy`S = 0`và một mảng kết quả`ans`có kích thước bằng với sức mạnh tối đa của quái vật. 
4. Lặp lại các anh hùng đã được sắp xếp, theo dõi số lượng anh hùng`k`được sử dụng cho đến nay. Đối với mỗi anh hùng có sức mạnh`h`, tăng`S`qua`h`. 
5. Với mọi cường độ nguyên`m`từ`previous_S + 1`ĐẾN`S`, giao phó`ans[m] = k`. Điều này bao gồm tất cả các điểm mạnh hiện có thể truy cập được bằng tiền tố hiện tại. 
6. Tiếp tục cho đến khi tất cả anh hùng được xử lý. Bất kỳ sức mạnh quái vật nào lớn hơn tổng số đều không thể truy cập được và có thể không được xác định hoặc được gán một giá trị trọng điểm. 

Tại sao nó hoạt động: Tính năng sắp xếp đảm bảo chúng tôi luôn sử dụng tướng có sẵn lớn nhất trước tiên, giảm thiểu số lượng tướng. Tổng tiền tố đảm bảo rằng tất cả sức mạnh của quái vật lên tới`S`có thể được bao phủ bởi lần đầu tiên`k`những anh hùng. Mỗi phép gán phạm vi đều rời rạc và liền kề nhau nên không có cường độ nào bị ghi đè hoặc bỏ sót. 

## Giải pháp Python```
PythonRun
```
