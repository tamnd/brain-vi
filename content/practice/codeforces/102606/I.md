---
title: "CF 102606I - Mảng hậu tố ngu ngốc"
description: "Cách tiếp cận trực tiếp sẽ cố gắng xây dựng một chuỗi ứng viên, tạo ra tất cả các hậu tố của nó, sắp xếp chúng và kiểm tra xem hậu tố ban đầu có thứ hạng được yêu cầu hay không. Điều này đúng vì nó tuân theo chính xác định nghĩa của mảng hậu tố."
date: "2026-08-04T17:07:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102606
codeforces_index: "I"
codeforces_contest_name: "2020 ECNU Campus Online Invitational Contest"
rating: 0
weight: 102606
solve_time_s: 73
verified: true
draft: false
---

[CF 102606I - Mảng hậu tố ngu ngốc](https://codeforces.com/problemset/problem/102606/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xây dựng một chuỗi ứng viên, tạo ra tất cả các hậu tố của nó, sắp xếp chúng và kiểm tra xem hậu tố ban đầu có thứ hạng được yêu cầu hay không. Điều này đúng vì nó tuân theo chính xác định nghĩa của mảng hậu tố. Tuy nhiên, nó không đưa ra cách hữu ích nào để tìm kiếm trong không gian khổng lồ của các chuỗi có thể. có`n`hậu tố và việc tạo chúng có thể đã yêu cầu`O(n^2)`bộ nhớ vì tổng chiều dài của chúng là bậc hai. Vì`n = 100000`, trường hợp xấu nhất sẽ liên quan đến khoảng năm tỷ vị trí ký tự chỉ để lưu trữ tất cả các hậu tố. 

Quan sát chính là chúng ta không cần xây dựng một mảng hậu tố. Chúng ta chỉ cần bắt buộc so sánh giữa các hậu tố. 

Giả sử ký tự đầu tiên của câu trả lời là`'b'`. Bất kỳ hậu tố nào bắt đầu bằng`'a'`sẽ tự động nhỏ hơn, trong khi bất kỳ hậu tố nào bắt đầu bằng ký tự lớn hơn`'b'`sẽ tự động lớn hơn. Điều này đưa ra một cách đơn giản để quyết định thứ hạng. 

Chúng ta có thể đặt chính xác`k - 1`hậu tố sau vị trí đầu tiên bắt đầu bằng`'a'`. Những hậu tố đó sẽ xuất hiện trước toàn bộ chuỗi. Sau đó, chúng tôi làm cho mọi hậu tố còn lại bắt đầu bằng`'c'`, vì vậy tất cả chúng đều đứng sau toàn bộ chuỗi. 

Chuỗi trở thành:```
b + (k-1 times 'a') + (n-k times 'c')
```Các hậu tố bắt đầu bên trong`a`khối nhỏ hơn vì ký tự đầu tiên của chúng là`'a'`. Các hậu tố bắt đầu bên trong`c`khối lớn hơn vì ký tự đầu tiên của chúng là`'c'`. Toàn bộ chuỗi là hậu tố duy nhất bắt đầu bằng`'b'`, vì vậy nó nằm chính xác sau`k-1`hậu tố nhỏ hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. Ký tự đầu tiên của câu trả lời sẽ là`'b'`, bởi vì chúng tôi muốn tách các hậu tố nhỏ hơn khỏi các hậu tố lớn hơn bằng cách sử dụng thứ tự từ điển. 
2. Nối thêm`k - 1`bản sao của`'a'`. Mỗi hậu tố bắt đầu bên trong khối này bắt đầu bằng`'a'`, do đó nó nhỏ hơn chuỗi đầy đủ, có ký tự đầu tiên là`'b'`. Đây chính xác là những hậu tố sẽ xuất hiện trước câu trả lời. 
3. Nối thêm`n - k`bản sao của`'c'`. Mọi hậu tố bắt đầu bên trong khối này đều bắt đầu bằng`'c'`, vì vậy nó lớn hơn chuỗi đầy đủ. Những hậu tố này sẽ xuất hiện sau câu trả lời. 
4. Xuất chuỗi đã xây dựng. 

Tại sao nó hoạt động: các hậu tố được chia thành ba nhóm. Nhóm đầu tiên gồm có`k - 1`hậu tố bắt đầu bằng`a`khối và mỗi khối đều nhỏ hơn chuỗi đầy đủ. Nhóm thứ hai là hậu tố bắt đầu từ chỉ mục`0`, bắt đầu bằng`b`. Nhóm thứ ba bao gồm các hậu tố bắt đầu từ`c`khối, và tất cả chúng đều lớn hơn. Vì không có hậu tố nào khác bắt đầu bằng`b`, chuỗi đầy đủ có chính xác`k - 1`hậu tố đứng trước nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    ans = ['b']
    ans.append('a' * (k - 1))
    ans.append('c' * (n - k))
    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Phần bổ sung đầu tiên tạo ra hậu tố mà chúng tôi muốn xếp hạng. Phần bổ sung tiếp theo sẽ thêm chính xác số lượng hậu tố nhỏ hơn mà dữ liệu đầu vào yêu cầu. Phần bổ sung cuối cùng sẽ lấp đầy các vị trí còn lại bằng các ký tự được đảm bảo làm cho hậu tố của chúng lớn hơn. 

Việc tính toán`k - 1`là chi tiết ranh giới quan trọng. Bản thân chuỗi đầy đủ không được tính trong số các hậu tố trước nó, vì vậy chỉ những hậu tố sau vị trí 0 mới có thể đóng góp vào số lượng nhỏ hơn. 

Khi`k = 1`, khối ở giữa có độ dài bằng 0 và câu trả lời là a`b`theo sau là`c`nhân vật. Khi`k = n`, khối cuối cùng biến mất và tất cả các hậu tố sau này đều bắt đầu bằng`a`, làm cho chuỗi đầy đủ trở thành hậu tố lớn nhất. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,`n = 4`,`k = 2`. 

| Bước | Phần thi công | Chuỗi hiện tại | 
| --- | --- | --- | 
| Bắt đầu | ký tự đầu tiên |`b`| 
| Thêm vào`k-1 = 1`bản sao của`a`| thêm nguồn hậu tố nhỏ hơn |`ba`| 
| Thêm vào`n-k = 2`bản sao của`c`| thêm nguồn hậu tố lớn hơn |`bacc`| 

Các hậu tố là: 

| Vị trí hậu tố | Hậu tố | So sánh | 
| --- | --- | --- | 
| 0 |`bacc`| hậu tố mục tiêu | 
| 1 |`acc`| nhỏ hơn | 
| 2 |`cc`| lớn hơn | 
| 3 |`c`| lớn hơn | 

Chỉ có một hậu tố nhỏ hơn nên toàn bộ chuỗi có thứ hạng`2`. 

Đối với mẫu 2,`n = 8`,`k = 3`. 

| Bước | Phần thi công | Chuỗi hiện tại | 
| --- | --- | --- | 
| Bắt đầu | ký tự đầu tiên |`b`| 
| Thêm vào`k-1 = 2`bản sao của`a`| thêm nguồn hậu tố nhỏ hơn |`baa`| 
| Thêm vào`n-k = 5`bản sao của`c`| thêm nguồn hậu tố lớn hơn |`baaccccc`| 

Các hậu tố bắt đầu ở vị trí`1`Và`2`là`aaccccc`Và`accccc`, cả hai đều nhỏ hơn chuỗi đầy đủ. Tất cả các hậu tố sau này đều bắt đầu bằng`c`, vì vậy chúng lớn hơn. Chuỗi đầy đủ có chính xác hai hậu tố trước nó, cho biết thứ hạng`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Thuật toán tạo ra chính xác`n`nhân vật. | 
| Không gian | O(n) | Chuỗi đầu ra yêu cầu lưu trữ tuyến tính. | 

Đầu vào lớn nhất có thể có`100000`các ký tự, do đó, việc xây dựng tuyến tính dễ dàng phù hợp với giới hạn. Không có hậu tố được lưu trữ hoặc so sánh. 

## Trường hợp thử nghiệm```python
import sys
import io

def make_solution(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n, k = map(int, sys.stdin.readline().split())
    ans = ['b']
    ans.append('a' * (k - 1))
    ans.append('c' * (n - k))
    print(''.join(ans))

    result = sys.stdout.getvalue().strip()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def rank_of_string(s):
    suffixes = sorted(s[i:] for i in range(len(s)))
    return suffixes.index(s) + 1

assert rank_of_string(make_solution("4 2")) == 2, "sample 1"
assert rank_of_string(make_solution("8 3")) == 3, "sample 2"

assert rank_of_string(make_solution("1 1")) == 1, "minimum size"
assert rank_of_string(make_solution("5 5")) == 5, "largest rank"
assert rank_of_string(make_solution("10 1")) == 1, "smallest rank"
assert rank_of_string(make_solution("100000 50000")) == 50000, "large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`| bất kỳ chuỗi một ký tự nào | Xử lý trường hợp hậu tố đơn | 
|`5 5`| một chuỗi có bốn hậu tố nhỏ hơn | Kiểm tra thứ hạng lớn nhất có thể | 
|`10 1`| một chuỗi không có hậu tố nhỏ hơn | Kiểm tra thứ hạng nhỏ nhất có thể | 
|`100000 50000`| thứ hạng`50000`| Khẳng định thang đo xây dựng tuyến tính | 

## Vỏ cạnh 

cho`n = 5, k = 5`, thuật toán tạo ra`baaaa`. Các hậu tố sau vị trí đầu tiên là`aaaa`,`aaa`,`aa`, Và`a`. Tất cả chúng đều nhỏ hơn vì chúng bắt đầu bằng`a`. Có bốn hậu tố nhỏ hơn nên chuỗi đầy đủ là thứ năm. 

Vì`n = 4, k = 1`, thuật toán tạo ra`bccc`. Các hậu tố là`bccc`,`ccc`,`cc`, Và`c`. Mọi hậu tố sau hậu tố đầu tiên đều bắt đầu bằng`c`, lớn hơn`b`, vì vậy chuỗi đầy đủ sẽ ở đầu tiên. 

Vì`n = 1, k = 1`, thuật toán tạo ra`b`. Không có hậu tố nào khác nên hậu tố duy nhất có hạng một. Việc xây dựng xử lý việc này một cách tự nhiên vì cả hai khối lặp lại đều có độ dài bằng 0.
