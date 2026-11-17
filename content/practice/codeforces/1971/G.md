---
title: "CF 1971G - XOUR"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và một điều kiện hoán đổi đặc biệt: hai phần tử có thể hoán đổi cho nhau nếu XOR theo bit của các giá trị của chúng nhỏ hơn 4. Mục tiêu là tạo ra mảng nhỏ nhất về mặt từ điển có thể bằng cách thực hiện bất kỳ số lượng hoán đổi nào như vậy."
date: "2026-06-08T17:22:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1971
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 944 (Div. 4)"
rating: 1700
weight: 1971
solve_time_s: 67
verified: false
draft: false
---

[CF 1971G - XOUR](https://codeforces.com/problemset/problem/1971/G) 

**Đánh giá:** 1700 
**Thẻ:** cấu trúc dữ liệu, dsu, sắp xếp 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và một điều kiện hoán đổi đặc biệt: hai phần tử có thể hoán đổi cho nhau nếu XOR theo bit của các giá trị của chúng nhỏ hơn 4. Mục tiêu là tạo ra mảng nhỏ nhất về mặt từ điển có thể bằng cách thực hiện bất kỳ số lượng hoán đổi nào như vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có độ dài`n`và các phần tử mảng. Đầu ra là mảng được chuyển đổi cho từng trường hợp thử nghiệm. 

Những ràng buộc ngụ ý rằng`n`có thể lên tới 2×10^5 trên tất cả các trường hợp thử nghiệm. Điều này có nghĩa là chúng tôi không thể thử kiểm tra tất cả các chuỗi hoán đổi. Chúng ta phải dựa vào một phương pháp xác định nhanh chóng những yếu tố nào có thể được sắp xếp lại giữa chúng. Các trường hợp cạnh không rõ ràng phát sinh khi mảng chứa số lượng lớn có XOR ≥4. Ví dụ, một mảng như`[16, 4, 1, 64]`không thể thay đổi vì tất cả các XOR đều quá lớn, do đó đầu ra chính là mảng đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là quét liên tục mảng và hoán đổi hai phần tử bất kỳ có XOR nhỏ hơn 4 cho đến khi không thể hoán đổi được nữa. Điều này đảm bảo tính chính xác nhưng cực kỳ chậm, vì trong trường hợp xấu nhất, nó có thể yêu cầu hoán đổi O(n^2) cho mỗi lần lặp và nhiều lần lặp. 

Cách tiếp cận tối ưu nhận thấy rằng điều kiện hoán đổi xác định **các thành phần được kết nối**: các phần tử có XOR theo cặp nhỏ hơn 4 tạo thành một nhóm trong đó có thể thực hiện bất kỳ hoán vị nào. Tất cả các giao dịch hoán đổi bên ngoài một thành phần đều bị cấm. Vì vậy, chúng ta chỉ cần: 

1. Xác định tất cả các thành phần liên thông của các phần tử trong quan hệ XOR<4. 
2. Sắp xếp từng thành phần một cách độc lập. 
3. Đặt các giá trị đã sắp xếp trở lại vị trí ban đầu. 

Điều này đảm bảo mảng trở nên nhỏ nhất về mặt từ điển vì mỗi nhóm được sắp xếp nội bộ và các phần tử không thể hoán đổi với nhau vẫn giữ nguyên vị trí so với các nhóm khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Các thành phần được kết nối + Sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Khởi tạo cấu trúc Disjoint Set Union (DSU) cho`n`các phần tử. Điều này sẽ theo dõi những chỉ số nào có thể được hoán đổi với nhau. 
4. Với mỗi cặp chỉ số`(i, j)`, nếu như`a[i] XOR a[j] < 4`, hợp nhất của họ
