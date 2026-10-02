---
title: "CF 104872H - Số xe tay ga"
description: "Chúng ta được cho một số nguyên cố định $n$. Nhiệm vụ là xem xét mọi cách viết $n$ dưới dạng tổng của các số nguyên dương trong đó thứ tự không quan trọng, do đó mỗi biểu diễn là một dãy không giảm. Mỗi biểu diễn như vậy được coi là một tập hợp nhiều phần."
date: "2026-06-28T10:27:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104872
codeforces_index: "H"
codeforces_contest_name: "2023-2024 Russia Team Open, High School Programming Contest (VKOSHP XXIV)"
rating: 0
weight: 104872
solve_time_s: 22
verified: false
draft: false
---

[CF 104872H - Số xe tay ga](https://codeforces.com/problemset/problem/104872/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên cố định$n$. Nhiệm vụ là xem xét mọi cách viết$n$dưới dạng tổng của các số nguyên dương trong đó thứ tự không quan trọng, vì vậy mỗi biểu diễn là một dãy không giảm. Mỗi biểu diễn như vậy được coi là một tập hợp nhiều phần. 

Đối với mỗi nhiều tập hợp, chúng tôi tính toán mex của nó, được định nghĩa là số nguyên dương nhỏ nhất không xuất hiện trong số các phần tử của nó. Sau khi tính toán giá trị này cho mọi phân vùng nhiều tập hợp lệ của$n$, chúng ta tính tổng tất cả các giá trị mex và đưa ra kết quả theo modulo$10^9+7$. 

Vì vậy, đầu vào xác định kích thước$n$và đầu ra tổng hợp một hàm trên tất cả các phân vùng số nguyên của$n$, trong đó hàm chỉ phụ thuộc vào số nguyên nhỏ nào xuất hiện trong phân vùng chứ không phụ thuộc vào thứ tự của chúng. 

Ràng buộc$n \le 1000$ngay lập tức loại trừ việc liệt kê tất cả các phân vùng. Số lượng phân vùng tăng lên gần như$e^{\Theta(\sqrt{n})}$, vốn đã lớn ở$n=1000$. Việc liệt kê lực lượng vũ phu sẽ yêu cầu tạo mọi phân vùng và tính toán mex cho mỗi phân vùng, điều này là không khả thi. 

Một trường hợp phức tạp là khi các phân vùng bị chi phối bởi các phần lớn. Ví dụ, phân vùng$[n]$luôn đóng góp mex$1$. Một thái cực khác là$[1,1,\dots,1]$, mex ở đâu$2$. Những thái cực này cho thấy mex phụ thuộc vào sự hiện diện của các số nguyên nhỏ hơn là cấu trúc đầy đủ của phân vùng. 

Một cách tiếp cận đơn giản sẽ đếm quá mức hoặc tính toán lại mex không hiệu quả nếu nó cố gắng tạo các phân vùng một cách rõ ràng. Một chế độ lỗi khác là tính toán lại mex cho từng phân vùng theo thời gian tuyến tính, điều này sẽ nhân chi phí liệt kê theo cấp số nhân. 

## Phương pháp tiếp cận 

Một phương pháp bạo lực liệt kê tất cả các phân vùng của$n$, lưu trữ từng multiset, tính toán mex của nó bằng cách kiểm tra các số nguyên bắt đầu từ$1$, và tính tổng kết quả. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, số lượng phân vùng của$1000$ở xung quanh$2.4 \times 10^{31}$, vì vậy ngay cả việc tạo ra chúng cũng không thể thực hiện được trong thời gian giới hạn. 

Quan sát quan trọng là mex chỉ phụ thuộc vào việc mỗi số nguyên có$1,2,3,\dots$xuất hiện trong phân vùng. Nếu chúng ta sửa một giá trị$k$, thì một phân vùng có mex chính xác$k$nếu nó chứa tất cả các số$1$bởi vì$k-1$ít nhất một lần và không chứa$k$, trong khi có thể chứa các số lớn hơn$k$. 

Vì vậy, vấn đề trở thành đếm phân vùng của$n$với các ràng buộc bao gồm hạn chế. Chúng ta quy đổi tổng toàn cầu thành tổng ov
