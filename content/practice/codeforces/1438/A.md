---
title: "CF 1438A - Hương vị đặc trưng của Andre"
description: "Bài toán yêu cầu chúng ta xây dựng các mảng có thuộc tính chia hết rất chặt chẽ. Để một mảng được gọi là hoàn hảo, mỗi mảng con, bất kể độ dài hay vị trí, đều phải có tổng chia hết cho độ dài của nó."
date: "2026-06-11T04:38:14+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1438
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 682 (Div. 2)"
rating: 800
weight: 1438
solve_time_s: 95
verified: false
draft: false
---

[CF 1438A - Sở thích cụ thể của Andre ](https://codeforces.com/problemset/problem/1438/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xây dựng các mảng có thuộc tính chia hết rất chặt chẽ. Để một mảng được gọi là hoàn hảo, mỗi mảng con, bất kể độ dài hay vị trí, đều phải có tổng chia hết cho độ dài của nó. Ràng buộc bổ sung duy nhất là mỗi phần tử của mảng phải nằm trong khoảng từ 1 đến 100. Chúng tôi được cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định độ dài mảng mong muốn`n`và chúng ta phải xuất ra bất kỳ mảng nào có độ dài thỏa mãn điều kiện. 

Nhìn vào những hạn chế,`n`nhiều nhất là 100 và số lượng ca kiểm thử cũng nhiều nhất là 100. Điều này có nghĩa là bất kỳ thuật toán nào hoạt động với khoảng O(n) hoặc O(n²) cho mỗi ca kiểm thử sẽ chạy hiệu quả vì tổng số thao tác sẽ nhiều nhất là khoảng 10⁴. Không cần cấu trúc dữ liệu nâng cao hoặc kỹ thuật tối ưu hóa. 

Phần quan trọng không rõ ràng của vấn đề là định nghĩa của một mảng hoàn hảo. Một nỗ lực ngây thơ có thể thử kiểm tra tất cả các chuỗi số có thể có hoặc xây dựng tổng của mọi mảng con, nhưng điều này là không cần thiết. Chúng ta cần chú ý một quy luật rõ ràng: nếu một mảng có tất cả các phần tử bằng nhau thì mỗi mảng con cũng bao gồm các số bằng nhau, làm cho tổng chia hết cho độ dài. Ví dụ,`[5,5,5]`hoạt động vì mỗi tổng mảng con là`5*k`đối với một số người`k`, chia hết cho`k`. 

Vỏ cạnh bao gồm nhỏ nhất`n = 1`, hoạt động bình thường với bất kỳ số nào từ 1 đến 100. Một trường hợp khác là`n = 100`, độ dài lớn nhất được phép, trong đó chúng ta phải đảm bảo tất cả các số đều nằm trong giới hạn và giống hệt nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra các mảng và kiểm tra mọi mảng con. Đối với một mảng có độ dài nhất định`n`, có`n*(n+1)/2`mảng con và kiểm tra từng tổng sẽ mất O(n) trong trường hợp xấu nhất. Điều này dẫn đến các hoạt động O(n³) cho mỗi trường hợp thử nghiệm. Với`n`lên tới 100, điều này có thể đạt tới 1 triệu thao tác cho mỗi trường hợp thử nghiệm, điều này khả thi đối với một thử nghiệm nhưng lãng phí và không cần thiết, đồng thời nếu được thực hiện một cách ngây thơ bằng cách tính tổng lặp đi lặp lại, nó có thể dễ dàng vượt quá giới hạn thời gian. 

Cách tiếp cận tối ưu xuất phát từ quan sát rằng nếu tất cả các phần tử của mảng đều bằng nhau thì mọi mảng con đều có tổng`element_value * length_of_subarray`. Vì tổng này rõ ràng là chia hết cho độ dài mảng con nên mảng đó tự động hoàn hảo. Vì giá trị phần tử có thể là số bất kỳ từ 1 đến 100 nên chúng ta có thể chọn`1`hoặc bất kỳ hằng số nào trong phạm vi đó và mảng sẽ thỏa mãn các điều kiện. 

Cách tiếp cận này làm giảm vấn đề tạo ra một mảng có độ dài`n`chứa đầy một số duy nhất. Đó là O(n) cho mỗi trường hợp thử nghiệm và thỏa mãn một cách tầm thường các giới hạn và tất cả các điều kiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm và không cần thiết | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, hãy đọc số nguyên`n`đại diện cho độ dài của mảng mong muốn. 
3. Tạo một mảng có độ dài`n`nơi mọi phần tử đều ở đó`1`. Bất kỳ số nào từ 1 đến 100 đều có tác dụng, nhưng`1`thật đơn giản. 
4. In mảng dưới dạng dòng cách nhau bằng dấu cách. 

Lý do điều này có tác dụng là vì bất kỳ mảng con nào có độ dài`k`sẽ có tổng`1*k`, chia hết cho`k`. Bất biến này đúng với mọi`k`từ 1 đến`n`. Không cần phải xem xét các giá trị hoặc kết hợp khác và điều này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        # perfect array: all elements equal, choose 1
        print(' '.join(['1'] * n))

if __name__ == "__main__":
    solve()
```Mỗi phần của mã tương ứng trực tiếp với thuật toán. Chúng ta đọc số lượng ca kiểm thử, sau đó với mỗi ca`n`tạo ra một mảng những cái đó. Việc sử dụng`' '.join(['1'] * n)`là một cách Pythonic để xây dựng đầu ra dưới dạng một chuỗi được phân tách bằng dấu cách, tránh các vòng lặp thủ công hoặc lỗi định dạng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
1
2
4
```| Bước | n | Mảng được tạo | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 1 | [1] | Yếu tố đơn lẻ, hoàn hảo một cách tầm thường | 
| 2 | 2 | [1,1] | Mảng con: [1], [1], [1,1] tổng chia hết cho độ dài | 
| 3 | 4 | [1,1,1,1] | Tất cả các tổng của mảng con chia hết cho độ dài: 1,2,3,4 | 

Điều này cho thấy rằng bất biến giữ bất kể độ dài`n`. 

Một ví dụ đầu vào khác:```
2
3
5
```| Bước | n | Mảng được tạo | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 3 | [1,1,1] | Mảng con có độ dài 1,2,3: tổng 1,2,3 chia hết cho độ dài | 
| 2 | 5 | [1,1,1,1,1] | Mảng con có độ dài 1,.5 đều chia hết | 

Điều này xác nhận giải pháp hoạt động cho các mảng cỡ trung bình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi trường hợp thử nghiệm tạo ra một mảng có độ dài n một lần | 
| Không gian | O(n) | Mảng cho một trường hợp thử nghiệm được lưu trữ trước khi in | 

Được cho`t <= 100`Và`n <= 100`, tổng số hoạt động tối đa là 10.000, nằm trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("3\n1\n2\n4\n") == "1\n1 1\n1 1 1 1", "sample 1"

# custom cases
assert run("1\n100\n") == ' '.join(['1']*100), "maximum size array"
assert run("2\n1\n3\n") == "1\n1 1 1", "minimum and small array"
assert run("1\n5\n") == "1 1 1 1 1", "all equal values"
assert run("1\n7\n") == "1 1 1 1 1 1 1", "odd length array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\n1\n2\n4 | 1\n1 1\n1 1 1 1 | chức năng cơ bản | 
| 1\n100 | 1 lần lặp lại 100 lần | xử lý tối đa n | 
| 2\n1\n3 | 1\n1 1 1 | nhiều trường hợp thử nghiệm | 
| 1\n5 | 1 1 1 1 1 | đầu ra chính xác cho mảng có độ dài trung bình | 
| 1\n7 | 1 1 1 1 1 1 1 | mảng có độ dài lẻ được xử lý | 

## Vỏ cạnh 

cho`n = 1`, đầu ra của thuật toán`[1]`. Bất kỳ mảng con nào cũng có độ dài 1 và tổng 1 chia hết cho 1.`n = 100`, đầu ra là`[1]*100`. Bất kỳ mảng con nào có độ dài`k`có tổng`k`, chia hết cho`k`. Lựa chọn 1 đảm bảo tất cả các giá trị đều nằm trong giới hạn 1..100. Không có vấn đề tràn riêng lẻ hoặc tràn tổng vì tất cả số học đều tầm thường.
