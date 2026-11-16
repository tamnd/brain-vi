---
title: "CF 1942A - Thử thách của anh nông dân John"
description: "Chúng ta cần xây dựng một mảng có độ dài n sao cho có chính xác k độ dịch chuyển theo chu kỳ của nó được sắp xếp theo thứ tự không giảm. Sự dịch chuyển theo chu kỳ chọn một số vị trí làm điểm bắt đầu mới của mảng và bao bọc các phần tử còn lại xung quanh."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1942
codeforces_index: "A"
codeforces_contest_name: "CodeTON Round 8 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 800
weight: 1942
solve_time_s: 97
verified: false
draft: false
---

[CF 1942A - Thử thách của nông dân John](https://codeforces.com/problemset/problem/1942/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng có độ dài`n`chính xác như vậy`k`các dịch chuyển theo chu kỳ của nó được sắp xếp theo thứ tự không giảm. 

Sự dịch chuyển theo chu kỳ chọn một số vị trí làm điểm bắt đầu mới của mảng và bao bọc các phần tử còn lại xung quanh. Ví dụ, sự dịch chuyển của`[1, 2, 3]`là`[1, 2, 3]`,`[2, 3, 1]`, Và`[3, 1, 2]`. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp`n`Và`k`. Chúng ta phải xuất ra bất kỳ mảng hợp lệ nào có số lần dịch chuyển tuần hoàn được sắp xếp chính xác`k`, hoặc đầu ra`-1`nếu không có mảng như vậy tồn tại. 

Những hạn chế là rất nhỏ. Tổng của tất cả`n`giá trị nhiều nhất là`1000`, vì vậy ngay cả việc kiểm tra tất cả các ca của một mảng cũng sẽ rẻ. Thách thức không phải là hiệu quả, mà là tìm ra một công trình tạo ra chính xác số ca được sắp xếp theo yêu cầu. 

Khó khăn chính đến từ việc hiểu cách thức hoạt động của các dịch chuyển theo chu kỳ được sắp xếp. Một nỗ lực ngây thơ có thể cố gắng điều chỉnh các giá trị riêng lẻ cho đến khi số đếm trở thành`k`, nhưng cấu trúc của các dịch chuyển tuần hoàn cứng nhắc hơn nhiều so với lần đầu tiên nó xuất hiện. 

Một trường hợp cạnh quan trọng là`k = n`. 

Ví dụ:```
n = 4, k = 4
```Mảng`[1, 1, 1, 1]`hoạt động vì mọi sự thay đổi theo chu kỳ đều giống hệt nhau và do đó được sắp xếp. 

Một trường hợp cạnh quan trọng khác là`1 < k < n`. 

Ví dụ:```
n = 3, k = 2
```Không có mảng hợp lệ tồn tại. Một cách tiếp cận bất cẩn có thể cố gắng xây dựng một cách trực tiếp, nhưng đặc tính cơ bản của sự dịch chuyển theo chu kỳ khiến điều này không thể thực hiện được. 

Trường hợp cạnh cuối cùng là`k = 1`. 

Ví dụ:```
n = 5, k = 1
```Một câu trả lời hợp lệ tồn tại. Chúng ta cần một mảng có đúng một phép quay được sắp xếp. Việc xây dựng phải tránh vô tình tạo thêm ca được sắp xếp. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực rất hữu ích cho việc khám phá mô hình. 

Giả sử chúng ta tạo ra một số mảng ứng cử viên. Chúng ta có thể đếm các dịch chuyển tuần hoàn đã được sắp xếp của nó bằng cách kiểm tra tất cả`n`các phép quay và đối với mỗi phép quay, hãy xác minh xem các phần tử của nó có giảm hay không. Điều này đòi hỏi`O(n^2)`làm việc trên mỗi mảng ứng viên. 

Vấn đề là lực lượng vũ phu không cho chúng ta biết cách xây dựng một mảng với số ca được sắp xếp chính xác theo yêu cầu. Không gian tìm kiếm là rất lớn. 

Quan sát quan trọng đến từ việc nghiên cứu khi nào có thể sắp xếp một sự thay đổi theo chu kỳ. 

Hãy xem xét một mảng không hoàn toàn không đổi. Nếu một phép quay nào đó được sắp xếp thì trong phép quay được sắp xếp đó có chính xác một vị trí mà chuỗi bao quanh từ các giá trị lớn nhất trở lại các giá trị nhỏ nhất. Điểm bọc đó xác định duy nhất vị trí bắt đầu của phép quay được sắp xếp. 

Kết quả là, một mảng không cố định có thể có nhiều nhất một phép dịch chuyển tuần hoàn được sắp xếp. 

Ví dụ:```
[2, 3, 4, 1]
```Chỉ vòng quay bắt đầu bằng`1`được sắp xếp:```
[1, 2, 3, 4]
```Tất cả các phép quay khác đều có sự giảm. 

Cách duy nhất để có được nhiều hơn một phép quay được sắp xếp là mọi phần tử phải bằng nhau. Trong trường hợp đó mọi phép quay đều giống hệt nhau, vì vậy tất cả`n`phép quay được sắp xếp. 

Điều này ngay lập tức mô tả tất cả các câu trả lời có thể có: 

Nếu`k = n`, xuất ra một mảng trong đó tất cả các giá trị đều bằng nhau. 

Nếu như`k = 1`, xuất ra bất kỳ mảng không cố định nào có chính xác một phép quay được sắp xếp. 

Nếu như`1 < k < n`, câu trả lời là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) mỗi ứng viên | O(n) | Không phải là công trình | 
| Tối ưu | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. 
2. Nếu`k = n`, đầu ra`n`số bằng nhau. 

Mọi sự dịch chuyển theo chu kỳ đều giống hệt nhau, vì vậy tất cả`n`ca làm việc được sắp xếp. 
3. Nếu`k = 1`, đầu ra:```
1 2 3 ... n
```Mảng này đang tăng dần nên dạng ban đầu của nó được sắp xếp. 
4. Quan sát rằng mọi sự thay đổi theo chu kỳ khác đều bắt đầu ở đâu đó sau phần tử đầu tiên và bao quanh`1`ở cuối. 

Vì giá trị lớn hơn xuất hiện trước giá trị nhỏ hơn nên không thể sắp xếp những thay đổi đó. 
5. Nếu`1 < k < n`, đầu ra`-1`. 

Không có mảng không cố định nào có thể có nhiều hơn một phép quay được sắp xếp, trong khi một mảng không đổi có chính xác`n`. 

### Tại sao nó hoạt động 

Chỉ có hai số ca có thể được sắp xếp theo chu kỳ. 

Nếu tất cả các phần tử đều bằng nhau thì mọi phép quay đều giống hệt nhau và tất cả`n`phép quay được sắp xếp. 

Ngược lại, hãy xem xét mọi phép quay được sắp xếp. Trong mảng hình tròn phải có một vị trí duy nhất trong đó chuỗi bao bọc từ giá trị lớn hơn trở lại giá trị nhỏ hơn. Bắt đầu ngay sau vị trí đó sẽ tạo ra phép quay được sắp xếp duy nhất có thể. Do đó, một mảng không cố định có thể có nhiều nhất một phép dịch chuyển tuần hoàn được sắp xếp. 

Điều này có nghĩa là các giá trị có thể đạt được của`k`chính xác là`1`Và`n`. Việc xây dựng đưa ra một mảng hợp lệ cho những trường hợp đó và báo cáo chính xác khả năng không thể thực hiện được đối với tất cả các trường hợp khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())

    if k == n:
        print(*([1] * n))
    elif k == 1:
        print(*range(1, n + 1))
    else:
        print(-1)
```Nhánh đầu tiên xử lý trường hợp mọi ca làm việc theo chu kỳ phải được sắp xếp. Làm cho tất cả các phần tử bằng nhau đảm bảo rằng mọi vòng quay đều giống hệt nhau. 

Nhánh thứ hai xử lý trường hợp cần chính xác một ca được sắp xếp. Một mảng tăng dần đã có một phép quay được sắp xếp, đó là chính nó. Bất kỳ phép quay nào khác sẽ bao quanh và đặt số lớn hơn trước số nhỏ hơn. 

Nhánh cuối cùng bao gồm tất cả các giá trị còn lại của`k`. Bằng chứng trên cho thấy không có mảng nào có thể thực hiện được số đếm như vậy. 

Việc thực hiện ngắn gọn vì đặc tính toán học hoàn toàn xác định câu trả lời. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, k = 2
```| Bước | n | k | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | Kiểm tra`k == n`| 
| 2 | 2 | 2 | đầu ra`[1, 1]`| 

Các phép quay là:```
[1, 1]
[1, 1]
```Cả hai đều được sắp xếp, vì vậy số lượng là`2`. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 1
```| Bước | n | k | Hành động | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | Kiểm tra`k == 1`| 
| 2 | 3 | 1 | đầu ra`[1, 2, 3]`| 

Vòng quay:```
[1, 2, 3]  sorted
[2, 3, 1]  not sorted
[3, 1, 2]  not sorted
```Chính xác một vòng quay được sắp xếp. 

Ví dụ này chứng minh thuộc tính chính là một mảng tăng nghiêm ngặt có sự dịch chuyển theo chu kỳ được sắp xếp duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc in mảng đã xây dựng chiếm ưu thế trong công việc | 
| Không gian | O(1) thêm | Chỉ có một vài biến được lưu trữ | 

Vì tổng của tất cả`n`giá trị nhiều nhất là`1000`, nghiệm dễ dàng nằm trong giới hạn. Thuật toán chỉ thực hiện các kiểm tra đơn giản và đưa ra kết quả xây dựng trực tiếp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())

        if k == n:
            ans.append(" ".join(["1"] * n))
        elif k == 1:
            ans.append(" ".join(map(str, range(1, n + 1))))
        else:
            ans.append("-1")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run("3\n2 2\n3 1\n3 2\n") == "1 1\n1 2 3\n-1"

# minimum size
assert run("1\n1 1\n") == "1"

# impossible middle value
assert run("1\n5 3\n") == "-1"

# all rotations sorted
assert run("1\n4 4\n") == "1 1 1 1"

# exactly one sorted rotation
assert run("1\n5 1\n") == "1 2 3 4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Ví dụ nhỏ nhất có thể | 
|`5 3`|`-1`| Trường hợp không thể xảy ra với`1 < k < n`| 
|`4 4`|`1 1 1 1`| Mỗi vòng quay phải được sắp xếp | 
|`5 1`|`1 2 3 4 5`| Chính xác một vòng quay được sắp xếp | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
```Thuật toán đi vào`k == n`nhánh vì cả hai giá trị đều là`1`. Nó xuất ra:```
1
```Có chính xác một sự thay đổi theo chu kỳ và nó được sắp xếp. 

Bây giờ hãy xem xét:```
1
3 2
```Thuật toán đi đến nhánh cuối cùng và đưa ra:```
-1
```Tại sao điều này đúng? Nếu tất cả các phần tử đều bằng nhau thì sẽ có`3`phép quay được sắp xếp. Nếu mảng không phải là hằng số thì có thể có nhiều nhất một phép quay được sắp xếp. một số lượng`2`là không thể. 

Cuối cùng hãy xem xét:```
1
5 5
```Đầu ra là:```
1 1 1 1 1
```Mỗi phép dịch chuyển theo chu kỳ vẫn giữ nguyên cùng một mảng, vì vậy tất cả năm phép quay đều được sắp xếp. Số lượng yêu cầu đạt được chính xác.
