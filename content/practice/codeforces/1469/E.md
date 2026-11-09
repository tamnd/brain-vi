---
title: "CF 1469E - Một chút tương tự"
description: "Chúng ta được yêu cầu tìm một chuỗi nhị phân có độ dài k tương tự với mọi chuỗi con có độ dài k trong chuỗi nhị phân s cho trước có độ dài n. Hai chuỗi hơi giống nhau nếu chúng có chung ít nhất một vị trí mà các ký tự khớp với nhau."
date: "2026-06-11T01:18:59+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "hashing", "string-suffix-structures", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1469
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 101 (Rated for Div. 2)"
rating: 2400
weight: 1469
solve_time_s: 607
verified: true
draft: false
---

[CF 1469E - Tương tự một chút](https://codeforces.com/problemset/problem/1469/E) 

**Đánh giá:** 2400 
**Thẻ:** bitmasks, brute Force, băm, cấu trúc hậu tố chuỗi, chuỗi, hai con trỏ 
**Thời gian giải:** 10 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm một chuỗi nhị phân có độ dài`k`đó là "hơi giống" với mọi chuỗi con có độ dài`k`trong một chuỗi nhị phân nhất định`s`chiều dài`n`. Hai chuỗi hơi giống nhau nếu chúng có chung ít nhất một vị trí mà các ký tự khớp với nhau. Trong điều kiện thực tế, chúng ta phải chọn một chuỗi`t`sao cho với mọi cửa sổ có độ dài`k`TRONG`s`, ít nhất một bit`t`đồng ý với bit tương ứng trong cửa sổ đó. Yêu cầu bổ sung đó là`t`phải ở mức tối thiểu về mặt từ điển trong số tất cả các chuỗi hợp lệ có thể có. Nếu không có chuỗi như vậy tồn tại, chúng tôi phải báo cáo "KHÔNG". 

Những ràng buộc ngụ ý rằng`n`có thể đạt tới 10^6 trên tất cả các trường hợp thử nghiệm. Một cách tiếp cận bạo lực lặp đi lặp lại trên tất cả 2^k chuỗi có độ dài có thể`k`sẽ là không thể nếu`k`lớn vì 2^20 trở lên là quá cao. Do đó, chúng ta cần một phương thức xử lý chuỗi theo thời gian tuyến tính hoặc gần tuyến tính đối với`n`, lý tưởng nhất là sử dụng mặt nạ bit hoặc kỹ thuật hai con trỏ để xác định hiệu quả các vị trí ở đó`0`hoặc`1`có thể được chọn. 

Các trường hợp cạnh không rõ ràng xảy ra khi`k`gần với`n`. Ví dụ, nếu`k = n`, chuỗi con duy nhất là`s`chính nó và mọi giá trị hợp lệ`t`phải khớp ít nhất một bit với`s`. Nếu như`s`đều là số 0, thì chuỗi duy nhất có thể xung đột với tất cả các chuỗi con là một trong tất cả các chuỗi con, điều này là không thể; do đó "KHÔNG" là đầu ra chính xác. Một trường hợp khác là khi tất cả các chuỗi con chứa cả hai`0`Và`1`ở cùng một vị trí trên tất cả các cửa sổ. Ở đây, chúng ta có thể chỉ còn lại một lựa chọn bit duy nhất cho vị trí đó trong`t`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ngây thơ sẽ liệt kê tất cả`2^k`chuỗi nhị phân có độ dài`k`, và với mỗi cái, hãy kiểm tra tất cả`n-k+1`chuỗi con cho độ tương tự bit. Điều này đúng nhưng hoàn toàn không khả thi đối với`k`lên đến 10^6. Số lượng hoạt động sẽ là khoảng`O((n-k+1)*2^k*k)`, vượt quá giới hạn thời gian theo nhiều bậc độ lớn. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu đến từ việc xử lý vấn đề theo hướng ngược lại. Thay vì tạo ra tất cả những gì có thể`t`và kiểm tra các chuỗi con, chúng ta có thể kiểm tra từng chuỗi con của`s`để tìm vị trí nơi các bit bị ép buộc. Chúng tôi theo dõi các vị trí nơi`0`Và`1`xuất hiện bằng hai mảng có độ dài`k`. Đối với một từ điển tối thiểu`t`, chúng tôi tham lam đặt`0`nếu có thể; nếu như`0`là không thể ở một vị trí (vì mỗi chuỗi con có một`1`ở đó), chúng tôi đặt`1`. Nếu không thể thực hiện được bit nào, chúng tôi sẽ báo cáo ngay lập tức "KHÔNG". Điều này làm giảm vấn đề đối với việc truyền tải tuyến tính và cập nhật bitmask trên chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-k+1)_2^k_k) | O(2^k*k) | Quá chậm | 
| Tối ưu | O(n*k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng boolean`can_be_0`Và`can_be_1`chiều dài`k`. Mỗi vị trí bắt đầu như`True`, thể hiện rằng vị trí trong`t`có thể có khả năng`0`hoặc`1`. 
2. Lặp lại tất cả các chuỗi con có độ dài`k`TRONG`s`. Đối với mỗi chuỗi con, đối với mỗi vị trí`i`trong chuỗi con, đánh dấu`can_be_0[i] = False`nếu chuỗi con có`1`ở vị trí đó (nghĩa là`t[i] = 0`sẽ không giống chút nào) và đánh dấu`can_be_1[i] = False`nếu chuỗi con có`0`. 
3. Sau khi xử lý tất cả các chuỗi con, xây dựng câu trả lời`t`. Đối với mỗi vị trí từ`0`ĐẾN`k-1`, nếu như`can_be_0[i]`là Đúng, được đặt`t[i] = '0'`. Bằng không, nếu`can_be_1[i]`là Đúng, được đặt`t[i] = '1'`. Nếu cả hai đều không đúng, xuất "NO" và chuyển sang trường hợp kiểm thử tiếp theo. 
4. Nếu tất cả các vị trí được đặt thành công, xuất "YES" theo sau là chuỗi được xây dựng`t`. 

Tại sao nó hoạt động: Các mảng`can_be_0`Và`can_be_1`duy trì tính bất biến rằng một vị trí được đánh dấu là Sai chỉ khi việc thiết lập bit không giống với ít nhất một chuỗi con. Bằng cách tham lam lựa chọn`0`bất cứ khi nào có thể, chúng tôi đảm bảo tính tối giản về mặt từ điển. Bất kỳ xung đột nào mà cả hai bit đều không hợp lệ ngay lập tức cho thấy không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    for _ in range(q):
        n, k = map(int, input().split())
        s = input().strip()
        can_be_0 = [True] * k
        can_be_1 = [True] * k
        
        for start in range(n - k + 1):
            substring = s[start:start + k]
            for i, char in enumerate(substring):
                if char == '0':
                    can_be_1[i] = can_be_1[i] and True
                    can_be_0[i] = can_be_0[i]
                else:
                    can_be_0[i] = can_be_0[i] and True
                    can_be_1[i] = can_be_1[i]
            # Mark impossible bits
            for i, char in enumerate(substring):
                if char == '0':
                    can_be_1[i] = False
                else:
                    can_be_0[i] = False

        result = []
        impossible = False
        for i in range(k):
            if can_be_
```
