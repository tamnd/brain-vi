---
title: "CF 1493D - GCD của mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần xử lý một chuỗi truy vấn trong đó mỗi truy vấn nhân một phần tử cụ thể của mảng với một thừa số nhất định. Sau mỗi truy vấn, chúng ta phải tính ước số chung lớn nhất (GCD) của toàn bộ mảng theo modulo $10^9+7$."
date: "2026-06-10T22:15:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "hashing", "implementation", "math", "number-theory", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1493
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 705 (Div. 2)"
rating: 2100
weight: 1493
solve_time_s: 52
verified: false
draft: false
---

[CF 1493D - GCD của một mảng](https://codeforces.com/problemset/problem/1493/D) 

**Đánh giá:** 2100 
**Tags:** brute Force, cấu trúc dữ liệu, băm, triển khai, toán học, lý thuyết số, sắp xếp, hai con trỏ 
**Thời gian giải:** 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần xử lý một chuỗi truy vấn trong đó mỗi truy vấn nhân một phần tử cụ thể của mảng với một thừa số nhất định. Sau mỗi truy vấn, chúng ta phải tính ước số chung lớn nhất (GCD) của toàn bộ mảng theo modulo$10^9+7$. Mảng đầu vào có thể lên tới$2 \cdot 10^5$các phần tử và có thể có nhiều truy vấn. Các con số liên quan, bao gồm cả số nhân, cũng lên tới$2 \cdot 10^5$. 

Một giải pháp đơn giản sẽ tính toán lại GCD từ đầu sau mỗi truy vấn, yêu cầu lặp lại toàn bộ mảng mỗi lần. Với$n$Và$q$có khả năng cả hai$2 \cdot 10^5$, điều này sẽ dẫn đến$O(nq) \approx 4 \cdot 10^{10}$hoạt động, vượt xa giới hạn 2 giây. Do đó, chúng tôi cần một cách nhanh hơn để tránh phải tính toán lại toàn bộ GCD nhiều lần. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó một số phần tử bằng 1 hoặc các mảng trong đó nhiều phần tử chỉ chia sẻ một phần hệ số nguyên tố của chúng. Ví dụ, nếu mảng là$[2, 3, 5]$và chúng tôi nhân phần tử đầu tiên với 3, GCD thay đổi từ 1 thành 1. Việc triển khai đơn giản có thể thất bại nếu nó cho rằng việc nhân bất kỳ phần tử nào luôn làm tăng GCD. 

Chúng ta cũng phải xem xét số học modulo một cách cẩn thận. Bản thân GCD được tính toán trên các số nguyên, nhưng đầu ra cuối cùng được yêu cầu theo modulo$10^9+7$. Việc áp dụng modulo bất cẩn trước khi tính toán đóng góp của thừa số nguyên tố có thể mang lại kết quả sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại qua mảng sau mỗi truy vấn, tính toán GCD của tất cả các phần tử. Điều này đúng vì GCD có tính kết hợp và việc lặp lại tất cả các phần tử sẽ cho kết quả chính xác. Tuy nhiên, với$n$Và$q$lên đến$2 \cdot 10^5$, số lượng hoạt động trong trường hợp xấu nhất là$O(nq) = 4 \cdot 10^{10}$, quá chậm trong 2 giây. 

Quan sát quan trọng là GCD của một tập hợp số được xác định hoàn toàn bằng hệ số nguyên tố của chúng. Chúng tôi có thể đại diện cho
