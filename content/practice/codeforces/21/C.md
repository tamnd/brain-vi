---
title: "CF 21C - Sọc 2"
description: "Chúng ta được cho một dải gồm n ô vuông, mỗi ô chứa một số nguyên. Nhiệm vụ là cắt sọc này thành ba đoạn liền kề nhau, không trống sao cho tổng các số ở mỗi đoạn bằng nhau. Đầu ra là số cách hợp lệ để thực hiện những lần cắt này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 21
codeforces_index: "C"
codeforces_contest_name: "Codeforces Alpha Round 21 (Codeforces format)"
rating: 2000
weight: 21
solve_time_s: 86
verified: true
draft: false
---
[CF 21C - Sọc 2](https://codeforces.com/problemset/problem/21/C) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, dp, sắp xếp 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một sọc bao gồm`n`hình vuông, mỗi hình chứa một số nguyên. Nhiệm vụ là cắt sọc này thành ba đoạn liền kề nhau, không trống sao cho tổng các số ở mỗi đoạn bằng nhau. Đầu ra là số cách hợp lệ để thực hiện những lần cắt này. Mỗi lần cắt xảy ra giữa các hình vuông và mỗi đoạn phải chứa ít nhất một hình vuông. 

Các ràng buộc đầu vào cho chúng ta biết`n`có thể lớn tới 100.000 và số của mỗi ô vuông có thể lớn tới 10.000 về giá trị tuyệt đối. Với kích thước này, bất kỳ phương pháp nào kiểm tra rõ ràng tất cả các bộ ba vị trí cắt đều không khả thi. Giải pháp O(n²) sẽ yêu cầu khoảng 10 tỷ phép tính trong trường hợp xấu nhất, vượt quá giới hạn thời gian 1 giây, vì vậy chúng ta cần một giải pháp tuyến tính hoặc toán học tuyến tính. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tổng số sọc không chia hết cho ba. Ví dụ: hãy xem xét đầu vào:```
3
1 2 3
```Tổng số là 6, chia hết cho 3, nhưng chúng ta cần các đoạn có độ dài dương. Việc cắt giảm duy nhất có thể sẽ tạo ra các phân đoạn`[1]`,`[2]`,`[3]`với các tổng 1, 2, 3. Vì các tổng không bằng nhau nên kết quả đầu ra đúng là 0. Một giải pháp đơn giản chỉ kiểm tra khả năng chia hết mà không có độ dài phân đoạn có thể báo cáo giải pháp không chính xác. Một trường hợp khác là khi tất cả các số đều bằng 0:```
5
0 0 0 0 0
```Ở đây, bất kỳ hai vết cắt nào cũng tạo ra các phân đoạn có tổng bằng 0, do đó tồn tại nhiều cặp vết cắt hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ xem xét từng cặp vị trí cắt`(i, j)`Ở đâu`i < j`, tính tổng của ba phân đoạn thu được và kiểm tra xem chúng có bằng nhau không. Điều này hiệu quả vì nó trực tiếp thực hiện định nghĩa vấn đề, nhưng đúng lúc là O(n²). Vì`n = 10^5`, điều này dẫn đến khoảng 5 * 10^9 kiểm tra, quá chậm. 

Quan sát quan trọng để tối ưu hóa là chúng tôi chỉ quan tâm đến tổng phân khúc chứ không quan tâm đến nội dung cụ thể của từng phân khúc. Cho phép`total`là tổng của tất cả các số. Nếu như`total`chia hết cho 3 chẳng hạn`total = 3*S`, thì mỗi đoạn phải có tổng bằng`S`. Sau đó chúng ta có thể lặp lại từ đầu, duy trì tổng tiền tố. Mỗi lần tổng tiền tố bằng`S`, nó thể hiện điểm kết thúc tiềm năng của đoạn đầu tiên. Sau đó, số cách hợp lệ để đặt đường cắt thứ hai phụ thuộc vào số lần tổng tiền tố đạt đến`2*S`sau vị trí này, vì đoạn cuối cùng tự động tổng hợp thành`S`. Bằng cách đếm những lần xuất hiện này trong một lượt, chúng tôi giảm độ phức tạp xuống O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng các số trong dãy. Nếu tổng này không chia hết cho 3 thì xuất ra 0. Ngược lại, không thể tồn tại ba đoạn có tổng bằng nhau. 
2. Hãy để`S = total / 3`. Khởi tạo hai biến:`count_s`để đếm số lần xuất hiện của tổng tiền tố bằng`S`Và`ways`để tích lũy số lượng các phép chia hợp lệ. 
3. Lặp lại từng sọc trong khi vẫn duy trì tổng tiền tố đang chạy. Mỗi phần tử được thêm vào tổng tiền tố khi chúng ta tiến về phía trước. 
4. Khi tổng tiền tố bằng`2*S`, nó cho biết điểm cuối tiềm năng của đoạn thứ hai. Thêm vào`count_s`ĐẾN`ways`bởi vì mỗi lần xuất hiện trước đó của tổng tiền tố bằng`S`đại diện cho phân đoạn đầu tiên hợp lệ. 
5. Nếu tổng tiền tố bằng`S`, tăng`count_s`để ghi lại điểm cuối tiềm năng của đoạn đầu tiên này. 
6. Sau khi xử lý tất cả các phần tử (không bao gồm ô vuông cuối cùng, không thể bắt đầu phân đoạn thứ hai hoặc thứ ba),`ways`chứa tổng số phần chia hợp lệ. 

Tại sao nó hoạt động: bất biến là vậy`count_s`theo dõi số điểm cắt đoạn đầu tiên trước chỉ mục hiện tại. Khi tổng tiền chạy đạt`2*S`, số lần cắt đầu tiên có thể xảy ra trước nó sẽ trực tiếp xác định số cách hợp lệ để thực hiện lần cắt thứ hai. Điều này tránh bất kỳ sự kết hợp nào hoặc bị thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

total = sum(a)
if total % 3 != 0:
    print(0)
    sys.exit()

S = total // 3
prefix_sum = 0
count_s = 0
ways = 0

# iterate until the second-to-last element
for i in range(n - 1):
    prefix_sum += a[i]
    if prefix_sum == 2 * S:
        ways += count_s
    if prefix_sum == S:
        count_s += 1

print(ways)
```Đầu tiên, mã kiểm tra khả năng chia hết cho 3 và kết thúc sớm nếu không thể. Vòng lặp loại trừ ô vuông cuối cùng vì nó không thể bắt đầu một đoạn sau ô vuông đó. Chúng tôi duy trì tổng tiền tố đang chạy và hai bộ đếm, cập nhật chúng theo thứ tự chính xác để tránh lỗi riêng lẻ. Tăng dần`ways`trước khi cập nhật`count_s`đảm bảo chúng ta không coi hình vuông hiện tại vừa là hình cắt thứ nhất vừa là hình cắt thứ hai. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
1 2 3 3
```| tôi | một [tôi] | tiền tố_sum | đếm_s | cách | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 0 | 
| 1 | 2 | 3 | 1 | 1 | 
| 2 | 3 | 6 | 1 | 1 | 

Đây,`total = 9`,`S = 3`. Tổng tiền tố đầu tiên của 3 xảy ra ở chỉ số 1, vì vậy`count_s = 1`. Tại chỉ số 2,`prefix_sum = 6 = 2*S`, vì vậy chúng tôi thêm`count_s`ĐẾN`ways`, cho kết quả 1. Điều này xác nhận đầu ra chính xác. 

Đầu vào tùy chỉnh:```
5
0 0 0 0 0
```| tôi | một [tôi] | tiền tố_sum | đếm_s | cách | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 0 | 
| 1 | 0 | 0 | 2 | 1 | 
| 2 | 0 | 0 | 3 | 3 | 
| 3 | 0 | 0 | 4 | 6 | 

Tổng số tiền là 0,`S = 0`. Tồn tại nhiều vết cắt hợp lệ và thuật toán đếm chính xác 6 cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần để tính tổng và số tiền tố, tất cả các phép toán O(1) cho mỗi phần tử | 
| Không gian | O(1) | Chỉ có một vài bộ đếm và tổng chạy, không cần thêm mảng | 

Giải pháp xử lý`n`lên tới 100.000 một cách thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    total = sum(a)
    if total % 3 != 0:
        return "0"
    S = total // 3
    prefix_sum = 0
    count_s = 0
    ways = 0
    for i in range(n - 1):
        prefix_sum += a[i]
        if prefix_sum == 2 * S:
            ways += count_s
        if prefix_sum == S:
            count_s += 1
    return str(ways)

assert run("4\n1 2 3 3\n") == "1", "sample 1"
assert run("5\n0 0 0 0 0\n") == "6", "all zeros"
assert run("3\n1 2 3\n") == "0", "sum not divisible by 3"
assert run("3\n1 1 1\n") == "1", "minimal equal segments"
assert run("6\n1 2 3 0 3 0\n") == "2", "multiple valid splits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n1 2 3 3 | 1 | Mẫu chuẩn | 
| 5\n0 0 0 0 0 | 6 | Tất cả các số không, nhiều phần chia tách | 
| 3\n1 2 3 | 0 | Tổng số chia hết cho 3 nhưng các đoạn không bằng nhau | 
| 3\n1 1 1 | 1 | Số lượng hình vuông tối thiểu tạo thành các đoạn bằng nhau | 
| 6\n1 2 3 0 3 0 | 2 | Nhiều lần cắt hợp lệ, kiểm tra logic đếm | 

## Vỏ cạnh 

Nếu`total`không chia hết cho 3, như`[1, 2, 3]`, thuật toán đưa ra 0 ngay lập tức, tránh tính toán không cần thiết. 

Đối với một sọc có tất cả số không`[0, 0, 0, 0, 0]`, thuật toán đếm nhiều lần xuất hiện của`S = 0`
