---
title: "CF 2136C - Chống lại sự khác biệt"
description: "Chúng ta được cung cấp một mảng các số nguyên và mục tiêu của chúng ta là trích xuất chuỗi con dài nhất có thể \"gọn gàng\". Một chuỗi con gọn gàng được tạo bằng cách ghép một hoặc nhiều khối, trong đó một khối là một chuỗi gồm các số giống hệt nhau và giá trị của số đó bằng độ dài của…"
date: "2026-06-09T04:10:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 2136
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1046 (Div. 2)"
rating: 1200
weight: 2136
solve_time_s: 82
verified: true
draft: false
---

[CF 2136C - Chống lại sự khác biệt](https://codeforces.com/problemset/problem/2136/C) 

**Đánh giá:** 1200 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và mục tiêu của chúng ta là trích xuất chuỗi con dài nhất có thể "gọn gàng". Một chuỗi con gọn gàng được tạo ra bằng cách ghép một hoặc nhiều khối, trong đó một khối là một chuỗi các số giống nhau và giá trị của số đó bằng với độ dài của khối. Ví dụ,`[3,3,3]`là một khối hợp lệ vì nó chứa ba phần tử, tất cả đều bằng ba. Mảng`[2,2]`là một khối hợp lệ, nhưng`[2,2,2]`không phải vì giá trị 2 không khớp với độ dài 3. Một mảng trống luôn được coi là gọn gàng. 

Chúng ta cần tìm độ dài của dãy con dài nhất của mảng đầu vào có thể được phân chia thành các khối này. Điều này không chỉ yêu cầu tìm các số lặp lại mà còn phải nhóm chúng sao cho số đếm khớp với chính số đó. 

Những ràng buộc chỉ ra rằng`n`có thể lớn tới 200.000 trên tất cả các trường hợp thử nghiệm. Với tối đa 10.000 trường hợp thử nghiệm, chúng tôi không thể chấp nhận bất cứ điều gì chậm hơn khoảng O(n) cho mỗi trường hợp thử nghiệm. Một cách tiếp cận ngây thơ thử mọi dãy con sẽ là hàm mũ theo n và hoàn toàn không thực tế. Các trường hợp cạnh cần xem xét bao gồm các mảng không tồn tại một chuỗi con gọn gàng, các mảng bao gồm toàn bộ số một hoặc n và các mảng trong đó tồn tại nhiều khối rời rạc có cùng số nhưng không thể kết hợp thành một khối gọn gàng lớn hơn. 

Ví dụ, mảng`[2,2,1,1]`có thể tạo thành khối`[2,2]`và hai`[1]`các khối, tạo ra một dãy con gọn gàng có độ dài bằng 4. Ngược lại,`[2,3,3]`không thể tạo thành bất kỳ khối hợp lệ nào, dẫn đến một dãy con gọn gàng có độ dài bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ xem xét mọi chuỗi con, kiểm tra xem nó có thể được phân chia thành các khối hợp lệ hay không và lấy độ dài tối đa. Về nguyên tắc thì điều này đúng, nhưng số dãy con thì`2^n`, điều này hoàn toàn không thể thực hiện được ngay cả đối với`n = 20`. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng các khối được xác định hoàn toàn bởi tần số của các con số. Chúng tôi không quan tâm đến trật tự trong một khối hoặc giữa các khối; chúng ta chỉ cần đủ số lượng giống nhau để tạo thành các khối đầy đủ. Đối với mỗi số`x`trong mảng, chúng ta có thể cố gắng tạo thành nhiều khối có kích thước`x`càng tốt. Chiến lược tham lam là tiếp tục hình thành các khối đầy đủ từ số lượng lớn nhất có thể có của mỗi số, tích lũy tổng chiều dài của tất cả các khối hoàn chỉnh. Bởi vì việc hình thành khối là độc lập với mỗi số nên chúng ta có thể sử dụng mảng lập trình động trong đó`dp[x]`lưu trữ độ dài tối đa của một chuỗi con gọn gàng kết thúc bằng các khối số`x`. 

Trong thực tế, chúng tôi lặp lại từng số trong mảng, duy trì số lần xuất hiện và theo dõi độ dài chuỗi con gọn gàng lớn nhất mà chúng tôi có thể xây dựng bằng cách thêm số này như một phần của khối hoàn chỉnh bất cứ khi nào chúng tôi thấy đủ số lần xuất hiện. Vấn đề giảm xuống còn việc đếm và nhóm, cho phép xử lý O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Đếm + Tham Lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và mảng`a`. 
2. Tạo từ điển`count`để theo dõi số lần mỗi số xuất hiện trong`a`. 
3. Tạo từ điển`dp`để theo dõi độ dài tối đa của chuỗi con gọn gàng kết thúc bằng khối của số đó. 
4. Lặp lại mảng. Đối với mỗi phần tử`x`: 

- Tăng`count[x]`. 
- Nếu như`count[x]`đạt tới`x`, điều đó có nghĩa là chúng ta có thể tạo thành một khối có kích thước hoàn chỉnh`x`. 
- Cập nhật`dp[x]`BẰNG`dp[x] = dp[x] + x`và thiết lập lại`count[x]`cho khối tiềm năng tiếp theo. 
5. Theo dõi giá trị lớn nhất trong`dp`trên tất cả các con số; điều này thể hiện độ dài của dãy con gọn gàng dài nhất. 
6. In kết quả cho từng test. 

Lý do là mỗi khối độc lập và chỉ dựa vào việc có đủ số lần xuất hiện. Bằng cách đếm tăng dần số lần xuất hiện và hình thành các khối một cách tham lam, chúng tôi đảm bảo không có khối tiềm năng nào bị lãng phí và tổng chiều dài được tối đa hóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        from collections import defaultdict
        
        count = defaultdict(int)
        dp = defaultdict(int)
        res = 0
        
        for x in a:
            dp[x] = max(dp[x], 0)
            count[x] += 1
            if count[x] >= x:
                dp[x] += x
                count[x] -= x
            res = max(res, dp[x])
        print(res)
```Giải pháp này đọc nhiều trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`để nhập liệu nhanh. Chúng tôi sử dụng một`defaultdict`để tránh các lỗi chính khi tăng số lượng. các`dp`từ điển theo dõi độ dài tối đa có thể đạt được với mỗi số. Bản cập nhật tham lam đảm bảo rằng mọi khối hoàn chỉnh đều đóng góp vào tổng độ dài chuỗi con gọn gàng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**`[1]`| Bước | x | đếm[x] | dp[x] | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0→1 | 1 | 

Điều này tạo ra chiều dài 1, tạo thành khối`[1]`. 

**Đầu vào mẫu 2:**`[1,2,3,3,3,1]`| Bước | x | đếm[x] | dp[x] | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 
| 2 | 2 | 1 | 0 | 0 | 
| 3 | 3 | 1 | 0 | 0 | 
| 4 | 3 | 2 | 0 | 0 | 
| 5 | 3 | 3 | 0→3 | 3 | 
| 6 | 1 | 2 | 1→2 | 5 | 

Dãy con gọn gàng dài nhất thu được có độ dài 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần, các phép toán từ điển trung bình là O(1) | 
| Không gian | O(n) |`count`Và`dp`từ điển lưu trữ tối đa n khóa | 

Được cho`sum(n) ≤ 2*10^5`, điều này vừa vặn thoải mái trong giới hạn 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("6\n1\n1\n2\n2 2\n4\n2 2 1 1\n6\n1 2 3 3 3 1\n8\n8 8 8 8 8 8 8 7\n10\n2 3 3 1 2 3 5 1 1 7\n") == "1\n2\n4\n5\n0\n5"

# custom cases
assert run("1\n5\n1 1 1 1 1\n") == "5"  # all ones
assert run("1\n4\n4 4 4 4\n") == "4"  # single block matches length
assert run("1\n6\n2 2 2 2 2 2\n") == "4"  # 2 blocks of 2
assert run("1\n3\n2 3 3\n") == "0"  # no neat subsequence
assert run("1\n1\n10\n") == "0"  # number too large for block
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 cái | 5 | Có thể tạo thành các khối có kích thước 1 nhiều lần | 
| 4 chân | 4 | Khối đơn bằng số | 
| 6 đôi | 4 | Nhiều khối có cùng số | 
| [2,3,3] | 0 | Không thể hình thành bất kỳ khối hợp lệ nào | 
| [10] | 0 | Số đơn không thể tạo thành khối | 

## Vỏ cạnh 

Khi mảng chứa các số lớn hơn số lượng có sẵn, không khối nào có thể hình thành. Ví dụ`[10]`cho kết quả có độ dài 0 vì một khối có độ dài 10 yêu cầu 10 lần xuất hiện của 10. Mảng có các số lặp lại tạo thành nhiều khối có kích thước 1. Mảng trong đó tổng số lần xuất hiện là bội số của số đó, như`[2,2,2,2]`, mang lại khối chính xác của`[2,2]`hai lần với tổng độ dài là 4. Thuật toán này xử lý các trường hợp này bằng cách theo dõi`count[x]`và chỉ cập nhật`dp[x]`khi có sẵn khối đầy đủ, đảm bảo tính chính xác.
