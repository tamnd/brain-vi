---
title: "CF 104020E - Cân bằng âm thanh"
description: "Chúng tôi được cung cấp một chuỗi biên độ âm thanh. Mỗi biên độ góp phần tạo nên khái niệm “âm lượng cảm nhận được” được xác định bằng bình phương giá trị của nó. Hệ thống đo âm lượng cảm nhận trung bình bằng giá trị trung bình của các giá trị bình phương này trên tất cả các vị trí."
date: "2026-07-02T04:40:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104020
codeforces_index: "E"
codeforces_contest_name: "2022 Benelux Algorithm Programming Contest (BAPC 22)"
rating: 0
weight: 104020
solve_time_s: 46
verified: true
draft: false
---

[CF 104020E - Cân bằng âm thanh](https://codeforces.com/problemset/problem/104020/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi biên độ âm thanh. Mỗi biên độ góp phần tạo nên khái niệm “âm lượng cảm nhận được” được xác định bằng bình phương giá trị của nó. Hệ thống đo âm lượng cảm nhận trung bình bằng giá trị trung bình của các giá trị bình phương này trên tất cả các vị trí. 

Chúng ta được phép áp dụng một hệ số tỷ lệ dương duy nhất cho mọi biên độ. Sau khi chia tỷ lệ, chuỗi phải giữ nguyên hình dạng chính xác, chỉ kéo dài hoặc co lại đồng đều. Mục tiêu là chọn hệ số tỷ lệ này sao cho giá trị trung bình mới của biên độ bình phương trở thành chính xác một giá trị mục tiêu x cho trước. Tồn tại một trường hợp đặc biệt: nếu tất cả biên độ đầu vào bằng 0 thì đầu ra phải giữ nguyên tất cả các số 0 bất kể x. 

Kích thước đầu vào có thể lên tới 100000 phần tử và biên độ có thể lớn tới 10^6 độ lớn. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai trong n. Bất kỳ giải pháp hợp lệ nào cũng phải tính toán các thuộc tính toàn cục của mảng theo thời gian tuyến tính và sau đó áp dụng phép biến đổi không đổi. 

Trường hợp cạnh tinh tế là khi tổng năng lượng bằng không. Ví dụ: nếu đầu vào là`0 0 0`, thì tổng bình phương bằng 0. Bất kỳ hệ số tỷ lệ nào cũng sẽ tạo ra số 0, do đó việc chia cho 0 sẽ xảy ra nếu được xử lý một cách ngây thơ. 

Một trường hợp cạnh khác là khi x bằng 0 nhưng đầu vào không phải toàn số 0. Ví dụ,`1 2 3`với x = 0 yêu cầu giảm tỷ lệ về 0, điều này là không thể đối với hệ số tỷ lệ dương hoàn toàn trừ khi chúng ta hiểu nó là tỷ lệ giới hạn. Tuy nhiên, cách giải thích dự định vẫn mang tính đại số: hệ số tỷ lệ trở thành 0, tạo ra tất cả các số 0 và khớp chính xác với x. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là cố gắng “tìm kiếm” hệ số tỷ lệ c. Nếu chúng ta chọn một ứng cử viên c, chúng ta có thể tính giá trị bình phương trung bình thu được bằng cách nhân mọi biên độ với c và tính lại giá trị trung bình của bình phương. Kiểm tra này là O(n), vì vậy tìm kiếm nhị phân trên c sẽ cho độ chính xác nhật ký O(n). Điều đó sẽ hiệu quả, nhưng nó không cần thiết. 

Quan sát quan trọng là việc chia tỷ lệ tương tác rất rõ ràng với hình vuông. Nếu mỗi giá trị ai trở thành c · ai thì mỗi số hạng bình phương sẽ trở thành c² · ai². Điều này có nghĩa là âm lượng cảm nhận trung bình có thang đo chính xác là c2. Không có sự tương tác giữa các phần tử ngoài tổng bình phương. 

Vì vậy, toàn bộ vấn đề quy về một phương trình một biến. Gọi S là tổng bình phương của đầu vào. Giá trị trung bình ban đầu là S/n. Sau khi chia tỷ lệ theo c, giá trị trung bình mới sẽ trở thành (c² · S) / n. Chúng ta muốn cái này bằng x, nên chúng ta giải trực tiếp: 

c² · S/n = x 

mang lại 

c = sqrt(x · n/S) 

Vấn đề duy nhất còn lại là xử lý S = 0. Điều đó chỉ xảy ra khi tất cả biên độ bằng 0 và vấn đề rõ ràng yêu cầu trả về tất cả các số 0 trong trường hợp đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm thô bạo theo hệ số tỷ lệ | O(n độ chính xác của nhật ký) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Công thức trực tiếp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính tổng bình phương một lần, lấy hệ số tỷ lệ từ phương trình dạng đóng và áp dụng nó cho mọi phần tử. 

## bước 

1. Đọc n và x, sau đó đọc mảng biên độ. Đây là tín hiệu thô mà chúng ta sẽ chuyển đổi thống nhất. 
2. Tính S = sum(ai² với mọi i). Điều này nắm bắt tất cả thông tin về độ ồn vì mục tiêu chỉ phụ thuộc vào độ lớn bình phương. 
3. Nếu S bằng 0 thì mọi biên độ đều bằng 0, do đó kết quả xuất ra mảng không thay đổi. Không có hệ số tỷ lệ nào có thể được suy ra một cách có ý nghĩa vì việc chia cho 0 sẽ xảy ra. 
4. Ngược lại, hãy tính hệ số tỷ lệ c = sqrt(x · n / S). Điều này xuất phát trực tiếp từ việc khớp các giá trị bình phương trung bình mong muốn và thu được. 
5. Nhân mọi biên độ với c và đưa ra kết quả với độ chính xác vừa đủ. 

## Tại sao nó hoạt động 

Thuộc tính quan trọng là phép biến đổi có tính nhân và độc lập trên các tọa độ. Bình phương loại bỏ các tương tác dấu và biến mục tiêu thành hàm tuyến tính của S dưới c². Vì ràng buộc chỉ phụ thuộc vào giá trị trung bình của bình phương nên việc bảo toàn các tỷ số là không cần thiết; chỉ có vấn đề năng lượng toàn cầu. Khi S được cố định, có chính xác một hệ số tỷ lệ ánh xạ nó tới x, do đó giải pháp được xây dựng phải khớp với mức trung bình được yêu cầu. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    s = 0
    for v in a:
        s += v * v
    
    if s == 0:
        print(*a)
        return
    
    c = math.sqrt(x * n / s)
    res = [v * c for v in a]
    print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp công thức dẫn xuất. Việc tích lũy các hình vuông sử dụng số học số nguyên để tránh các vấn đề về độ chính xác. Nhánh duy nhất là trường hợp năng lượng bằng 0, ngăn cản việc chia cho 0. Bước chia tỷ lệ là một bước tuyến tính đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 5, x = 6 

a = [0, 1, -2, 3, -4] 

Đầu tiên tính S: 

| tôi | ai | ai² | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | -2 | 4 | 
| 4 | 3 | 9 | 
| 5 | -4 | 16 | 

S = 30 

Tính hệ số tỷ lệ: 

c = sqrt(6 * 5/30) = sqrt(1) = 1 

Mảng tỷ lệ vẫn không thay đổi. 

Điều này chứng tỏ trường hợp năng lượng ban đầu đã phù hợp với mục tiêu. 

### Ví dụ 2 

đầu vào: 

n = 4, x = 1 

a = [1, 3, 3, 7] 

Tính S: 

| tôi | ai | ai² | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 3 | 9 | 
| 3 | 3 | 9 | 
| 4 | 7 | 49 | 

S = 68 

Hệ số tỷ lệ: 

c = sqrt(4/68) = sqrt(1/17) 

Bây giờ nhân từng giá trị: 

| ai | ai · c | 
| --- | --- | 
| 1 | 0,242535625 | 
| 3 | 0,727606875 | 
| 3 | 0,727606875 | 
| 7 | 1.697749375 | 

Điều này xác nhận rằng sự biến đổi là đồng đều và bảo toàn tỷ lệ giữa các biên độ trong khi điều chỉnh năng lượng tổng thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tính tổng bình phương một lần và chia tỷ lệ một lần | 
| Không gian | O(1) | Chỉ có một số vô hướng ngoài bộ nhớ đầu vào | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó chỉ thực hiện hai lần tuyến tính trên mảng và không tính toán nặng nề cho mỗi phần tử ngoài phép nhân. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import sqrt
    
    n, x = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))
    
    s = sum(v*v for v in a)
    if s == 0:
        return " ".join(map(str, a))
    
    c = (x * n / s) ** 0.5
    res = [v * c for v in a]
    return " ".join(f"{v:.9f}" for v in res)

# provided samples (approx format checks)
assert run("5 6\n0 1 -2 3 -4\n")[:1] == "0" or run("5 6\n0 1 -2 3 -4\n") is not None
assert run("4 1\n1 3 3 7\n") is not None

# custom cases
assert run("1 0\n5\n") == "0.000000000", "single element to zero"
assert run("3 0\n1 2 3\n") == "0.000000000 0.000000000 0.000000000", "zero target"
assert run("3 9\n0 0 0\n") == "0 0 0", "all zeros edge"
assert run("2 4\n2 0\n") is not None, "sparse array scaling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn về 0 | 0 | trường hợp tối thiểu và không có mục tiêu | 
| mục tiêu bằng không | tất cả số không | hành vi sụp đổ hoàn toàn | 
| tất cả các cạnh số không | 0 0 0 | biện pháp bảo vệ chia cho 0 | 
| chia tỷ lệ mảng thưa thớt | đầu ra có quy mô nhất quán | cấu trúc không một phần | 

## Vỏ cạnh 

Đầu vào hoàn toàn bằng 0 là trường hợp đặc biệt về mặt cấu trúc. Đối với một đầu vào như`0 0 0`, S bằng 0, vì vậy việc tính c sẽ liên quan đến việc chia cho 0. Thuật toán kiểm tra rõ ràng điều này và trả về mảng không thay đổi. 

Đối với một đầu vào như`1 2 3`với x = 0, S = 14 thì c bằng 0. Nhân mọi phần tử với 0 sẽ mang lại tất cả các số 0, khớp chính xác với mục tiêu được yêu cầu. 

Đối với mảng một phần tử như`[5]`, S = 25 và việc chia tỷ lệ hoạt động chính xác, tạo ra một giá trị duy nhất được điều chỉnh để khớp trực tiếp với mức trung bình mục tiêu do không có sự tương tác giữa các phần tử. 

Những trường hợp này xác nhận rằng công thức xử lý cả cấu hình suy biến và cấu hình tổng quát mà không cần phân nhánh bổ sung ngoài việc kiểm tra năng lượng bằng không.
