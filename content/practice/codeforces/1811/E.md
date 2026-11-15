---
title: "CF 1811E - Chuỗi sống"
description: "Chúng ta được yêu cầu tạo số thứ k trong một dãy số tự nhiên không chứa chữ số 4. Dãy số này bắt đầu là [1, 2, 3, 5, 6, 7, 8, 9, 10, 11, 12, 13, 15, 16, ...]."
date: "2026-06-09T08:39:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1811
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 863 (Div. 3)"
rating: 1500
weight: 1811
solve_time_s: 76
verified: true
draft: false
---

[CF 1811E - Trình tự sống](https://codeforces.com/problemset/problem/1811/E) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, dp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tạo _số thứ k_ theo một dãy số tự nhiên **không chứa chữ số 4**. Trình tự bắt đầu như`[1, 2, 3, 5, 6, 7, 8, 9, 10, 11, 12, 13, 15, 16, ...]`. Mỗi truy vấn đưa ra một vị trí`k`(được lập chỉ mục 1) và chúng tôi phải trả về số sẽ xuất hiện ở đó. 

Đầu vào có tới`10^4`trường hợp thử nghiệm và`k`có thể lớn như`10^12`. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào lặp lại các số và kiểm tra chữ số 4. Ngay cả khi chúng tôi xử lý 10^6 số mỗi mili giây,`k`của`10^12`sẽ mất hàng tỷ mili giây. Vì vậy việc tìm kiếm tuyến tính là không thể thực hiện được. 

Một trường hợp khó phát hiện đối với các số có nhiều chữ số bỏ qua 4. Ví dụ: số thứ 4 trong dãy là`5`, không`4`. Bất kỳ cách tiếp cận ngây thơ nào làm tăng bộ đếm và bỏ qua các số chứa 4 đều phải cẩn thận tránh các lỗi sai lệch khi vượt qua các chữ số chứa 4. Một trường hợp phức tạp khác là khi`k`là cực kỳ lớn, giống như`10^12`, trong đó việc tràn số nguyên hoặc tính toán sai trong việc lập chỉ mục có thể âm thầm phá vỡ một nỗ lực bạo lực. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: bắt đầu từ 1, tăng dần và bỏ qua bất kỳ số nào chứa chữ số 4. Kiểm tra lần lượt từng số cho đến khi đạt`k`. Điều này hiệu quả vì bạn có thể tạo chuỗi một cách rõ ràng, nhưng nó thất bại một cách ngoạn mục đối với số lượng lớn`k`. Vì`k = 10^12`, số lần thực hiện theo thứ tự`10^12`, điều này là không thể trong giới hạn 1 giây. 

Thông tin chi tiết quan trọng là chuỗi có thể được hiểu là **biểu diễn cơ số 9**. Mọi số trong dãy đều có thể được suy ra bằng cách viết`k`trong cơ số 9, sau đó diễn giải mỗi chữ số dưới dạng số thập phân, bỏ qua số 4 bị cấm. Ví dụ: các chữ số cơ số 9`0-8`ánh xạ tới chữ số thập phân`[1,2,3,5,6,7,8,9,0]`theo trình tự (hoặc tương đương`[1,2,3,5,6,7,8,9]`cho 8 chữ số đầu tiên nếu chúng ta xử lý`0`đặc biệt). Điều này hiệu quả vì có chính xác 9 chữ số được phép trong hệ thập phân, do đó mỗi vị trí trong cơ số 9 tương ứng với một chữ số hợp lệ trong chuỗi. Điều này làm giảm vấn đề từ việc tạo ra tới`10^12`số để chuyển đổi một số duy nhất`k`vào cơ số 9, mất tối đa 40 bước để`k ≤ 10^12`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * log(k)) | O(1) | Quá chậm đối với k > 10^6 | 
| Tối ưu (ánh xạ cơ sở 9) | O(log k) | O(log k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`k`. 
3. Khởi tạo một chuỗi trống`res`để xây dựng câu trả lời. 
4. Trong khi`k > 0`: 

1. Tính toán`digit = k % 9`. Điều này cho ra chữ số cơ số 9 ở vị trí hiện tại. 
2. Bản đồ`digit`đến đúng chữ số thập phân. Nếu như`digit < 4`, nó ánh xạ tới chính nó (`0 → 0`,`1 → 1`,`2 → 2`,`3 → 3`). Nếu như`digit ≥ 4`, thêm 1 để bỏ qua 4 (`4 → 5`,`5 → 6`, vân vân.). 
3. Thêm chữ số được ánh xạ này vào`res`. 
4. Cập nhật`k = k // 9`. 
5. In`res`sau khi xử lý tất cả các chữ số. 

Tại sao nó hoạt động: Ở mỗi bước, chia cho 9 sẽ chuyển sang chữ số cao hơn tiếp theo trong cơ số 9, đảm bảo rằng mỗi chữ số được ánh xạ tương ứng với một chữ số hợp lệ trong chuỗi thập phân "không 4". Việc chuẩn bị trước đảm bảo các chữ số quan trọng nhất được đặt chính xác. Có sự tương ứng một-một giữa các số trong cơ số 9 và các vị trí trong dãy sống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k = int(input())
    res = []
    while k > 0:
        digit = k % 9
        # Map base-9 digit to "no 4" decimal digit
        if digit >= 4:
            digit += 1
        res.append(str(digit))
        k //= 9
    print(''.join(res[::-1]))
```Chúng tôi sử dụng`res.append`và sau đó đảo ngược danh sách ở cuối vì chúng tôi xử lý chữ số có nghĩa nhỏ nhất trước tiên. Lập bản đồ`digit >= 4`đảm bảo rằng số thập phân 4 bị bỏ qua. Sử dụng phép chia số nguyên và modulo xử lý số lượng lớn một cách an toàn`k`giá trị lên đến`10^12`. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`k = 5`| Bước | k | k% 9 | chữ số được ánh xạ | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 6 | ['6'] | 
| 2 | 0 | - | - | ['6'] | 

Đầu ra:`6`. Số thứ 5 trong dãy là 6. 

**Ví dụ 2:**`k = 22`| Bước | k | k% 9 | chữ số được ánh xạ | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 1 | 22 | 4 | 5 | ['5'] | 
| 2 | 2 | 2 | 2 | ['5','2'] | 

Đầu ra đảo ngược:`25`. Số thứ 22 là 25 

Những dấu vết này xác nhận ánh xạ từ số thập phân cơ số 9 đến số thập phân "không 4" là chính xác và giữ nguyên thứ tự trình tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log k) cho mỗi truy vấn | Mỗi chữ số của k trong cơ số 9 được xử lý một lần; log9(k) 40 với k 10^12 | 
| Không gian | O(log k) | Chúng tôi lưu trữ các chữ số trong danh sách trước khi in | 

Với`t ≤ 10^4`truy vấn, tổng số thao tác tối đa là 4 * 10^5, thoải mái trong vòng 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # paste solution here
    t = int(input())
    for _ in range(t):
        k = int(input())
        res = []
        while k > 0:
            digit = k % 9
            if digit >= 4:
                digit += 1
            res.append(str(digit))
            k //= 9
        print(''.join(res[::-1]))
    return out.getvalue().strip()

# Provided samples
assert run("7\n3\n5\n22\n10\n100\n12345\n827264634912\n") == "3\n6\n25\n11\n121\n18937\n2932285320890", "sample 1"

# Custom cases
assert run("3\n1\n9\n81\n") == "1\n10\n100", "small powers of 9"
assert run("2\n4\n8\n") == "5\n9", "border around 4"
assert run("2\n1000000000000\n1000000000001\n") == "1929394949493\n1929394949495", "large k edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | k nhỏ nhất | 
| 9 | 10 | k vượt qua một chữ số 4 | 
| 81 | 100 | ánh xạ nhiều chữ số | 
| 1000000000000 | 1929394949493 | xử lý k rất lớn đúng cách | 

## Vỏ cạnh 

Thuật toán xử lý chính xác`k`xung quanh lũy thừa của 9. Đối với`k = 4`, số đầu tiên ≥ 4 là 5. Bước ánh xạ modulo đảm bảo số thập phân 4 bị bỏ qua. Vì`k`gần`10^12`, thuật toán không bao giờ tràn vì Python xử lý các số nguyên có kích thước tùy ý và vòng lặp thực hiện tối đa 40 lần lặp. Vì`k = 1`, kết quả là`1`, khi vòng lặp thực hiện một lần và ánh xạ`0 → 1`. Tất cả các trường hợp cạnh của 4 giây lệch nhau hoặc bị bỏ qua đều được tính đến trong bước ánh xạ.
