---
title: "CF 1458A - Hàng GCD"
description: "Bài toán đưa ra cho chúng ta hai dãy số nguyên: dãy đầu tiên a chứa n phần tử và dãy thứ hai b chứa m phần tử."
date: "2026-06-11T02:33:38+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1458
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 691 (Div. 1)"
rating: 1600
weight: 1458
solve_time_s: 264
verified: true
draft: false
---

[CF 1458A - Hàng GCD](https://codeforces.com/problemset/problem/1458/A) 

**Đánh giá:** 1600 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra cho chúng ta hai dãy số nguyên: dãy đầu tiên`a`chứa`n`các phần tử và chuỗi thứ hai`b`chứa`m`các phần tử. Đối với mỗi phần tử`b_j`TRONG`b`, chúng ta được yêu cầu tính ước số chung lớn nhất (GCD) của dãy được tạo thành bằng cách thêm`b_j`tới mọi phần tử trong`a`. Nói cách khác, chúng ta phải tính GCD của`a_1 + b_j, a_2 + b_j, ..., a_n + b_j`cho mỗi`b_j`. 

Các ràng buộc rất lớn:`n`Và`m`mỗi phần tử có thể lên tới 2 × 10^5 và các phần tử riêng lẻ có thể lên tới 10^18. Điều này loại trừ bất kỳ phương pháp nào tính toán rõ ràng GCD của tất cả các phần tử cho mỗi phần tử.`b_j`trong một vòng lặp lồng nhau đơn giản, bởi vì điều đó sẽ yêu cầu tính toán GCD O(n × m), có thể lên tới 4 × 10^10 phép tính. Bất kỳ cách tiếp cận nào như vậy sẽ vượt quá giới hạn thời gian hợp lý. 

Một điểm tinh tế là tất cả các con số đều dương và có khả năng rất lớn. Điều này có nghĩa là tràn số nguyên là mối lo ngại trong các ngôn ngữ có số nguyên có chiều rộng cố định, nhưng trong Python, chúng ta có thể dựa vào số học có độ chính xác tùy ý. Một trường hợp cạnh khác là khi`n = 1`, trong trường hợp đó GCD của một số chính là số đó. Việc triển khai bất cẩn có thể cố gắng tính toán sự khác biệt giữa các phần tử mà không xử lý vấn đề này đúng cách. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là lặp lại mọi`b_j`rồi tính GCD của`a_i + b_j`cho tất cả`i`TRONG`a`. Điều này có thể đúng, vì GCD của một tập hợp số có thể được tính toán lặp đi lặp lại, nhưng độ phức tạp của nó là O(n × m), quá chậm so với các ràng buộc đã cho. 

Cái nhìn sâu sắc quan trọng là đối với bất kỳ chuỗi nào`x_1, x_2, ..., x_n`, GCD của dãy giống với GCD của phần tử đầu tiên cộng với những khác biệt so với phần tử đầu tiên. chính thức,```

```Áp dụng điều này cho vấn đề của chúng tôi:```

```Sự khác biệt`a_i - a_1`đừng phụ thuộc vào`b_j`, vì vậy chúng tôi có thể tính toán trước GCD của họ một lần, mà chúng tôi gọi là`g = GCD(a_2 - a_1, a_3 - a_1, ..., a_n - a_1)`. Sau đó với mỗi`b_j`, câu trả lời đơn giản là`GCD(a_1 + b_j, g)`. Điều này làm giảm độ phức tạp xuống O(n + m), có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n×m) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`, sau đó đọc trình tự`a`Và`b`. 
2. Nếu`n == 1`, GCD cho mỗi`b_j`chỉ là`a[0] + b_j`, vì GCD của một số chính là chính nó. 
3. Mặt khác, tính GCD của chênh lệch so với phần tử đầu tiên:```

```Điều này mang lại cho GCD tất cả sự khác biệt`a_i - a_1`. 
4. Đối với mỗi`b_j`TRONG`b`, tính toán`GCD(a[0] + b_j, g)`và lưu trữ kết quả. 
5. Xuất kết quả theo thứ tự. 

Tại sao nó hoạt động: Điều bất biến là việc thêm một hằng số`b_j`đối với tất cả các phần tử sẽ dịch chuyển tất cả các số một lượng như nhau. Sự khác biệt giữa các phần tử vẫn giữ nguyên, do đó GCD có thể được phân tích bằng cách sử dụng phần tử đầu tiên và GCD được tính toán trước của sự khác biệt. Điều này đảm bảo tính đúng đắn cho tất cả`b_j`mà không cần tính toán lại sự khác biệt mỗi lần. 

## Giải pháp Python```
PythonRun
```### Giải thích 

Phần đầu tiên xử lý trường hợp đặc biệt`n = 1`, nơi mà câu trả lời là tầm thường. Sự khác biệt`a[i] - a[0]`được tính toán trước cho tất cả`i > 0`và GCD của họ được lưu trữ trong`g`. Bước này đảm bảo rằng cấu trúc của chuỗi liên quan đến phần tử đầu tiên được nắm bắt. Sau đó với mỗi phần tử`b_j`trong chuỗi thứ hai, chúng tôi tính toán`gcd(a[0] + b_j, g)`, thế là đủ vì tất cả sự khác biệt đã được tính đến. 

## Ví dụ đã hoạt động 

đầu vào:```

```Tính toán sự khác biệt từ phần tử đầu tiên:```

```GCD của sự khác biệt:`gcd(24, 120) = 24`,`gcd(24, 168) = 24`→`g = 24`Tính toán đáp án: 

| b_j | a[0] + b_j | gcd(a[0] + b_j, g) | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | gcd(2, 24) | 2 | 
| 2 | 3 | gcd(3, 24) | 3 | 
| 7 | 8 | gcd(8, 24) | 8 | 
| 23 | 24 | gcd(24, 24) | 24 | 

Đầu ra:`2 3 8 24`→ phù hợp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Tính toán trước GCD của chênh lệch trong O(n), sau đó tính GCD cho mỗi`b_j`trong O(1) × m | 
| Không gian | O(n + m) | Lưu trữ trình tự`a`Và`b`, danh sách đầu ra | 

Điều này phù hợp với giới hạn vì n + m 4 × 10^5, thấp hơn nhiều so với giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```
PythonRun
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3\n7\n2 3 5\n`|`9 10 12`| Chuỗi phần tử đơn a | 
|`3 2\n5 5 5\n1 2\n`|`6 7`| Tất cả các phần tử bằng nhau trong | 
|`2 2\n10^18 10^18+12\n10^18 10^18-1\n`|`12 13`| Xử lý số lượng rất lớn | 
|`1 1\n42\n58\n`|`100`| Tối thiểu |
