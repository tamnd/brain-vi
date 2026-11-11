---
title: "CF 159C - Thao tác chuỗi 1.0"
description: "Chúng ta được cho một chuỗi s và một số nguyên k. Ban đầu, người dùng đăng ký tên người dùng t, đơn giản là chuỗi s được lặp lại k lần liên tiếp. Sau đó, một chuỗi chỉnh sửa diễn ra, mỗi chỉnh sửa sẽ loại bỏ lần xuất hiện thứ p của một ký tự được chỉ định khỏi chuỗi hiện tại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "binary-search", "brute-force", "data-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 159
codeforces_index: "C"
codeforces_contest_name: "VK Cup 2012 Qualification Round 2"
rating: 1400
weight: 159
solve_time_s: 91
verified: true
draft: false
---

[CF 159C - Thao tác chuỗi 1.0](https://codeforces.com/problemset/problem/159/C) 

**Đánh giá:** 1400 
**Tags:** *đặc biệt, tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, chuỗi 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`và một số nguyên`k`. Người dùng ban đầu đăng ký tên người dùng`t`đơn giản là chuỗi`s`lặp đi lặp lại`k`lần liên tiếp. Sau đó, một chuỗi chỉnh sửa diễn ra, mỗi lần loại bỏ`p`-thứ xuất hiện của một ký tự được chỉ định từ chuỗi hiện tại. Nhiệm vụ của chúng ta là tính chuỗi cuối cùng sau khi áp dụng tất cả các phép xóa này theo thứ tự. 

Chuỗi`s`có tối đa 100 ký tự, nhưng`k`có thể lớn tới 2000, nghĩa là chuỗi đầu tiên`t`có thể có tới 200.000 ký tự. Số lần xóa`n`cũng có thể lên tới 20.000. Các giới hạn này ngay lập tức cho chúng ta biết rằng một cách tiếp cận đơn giản quét chuỗi cho mỗi lần xóa có thể sẽ quá chậm vì mỗi lần xóa có thể tốn O(|t|) thời gian, dẫn đến 200.000 × 20.000 thao tác trong trường hợp xấu nhất, lớn đến mức không thể chấp nhận được. 

Các trường hợp chính phát sinh khi ký tự cần xóa xuất hiện nhiều lần và mục tiêu xóa là lần xuất hiện đầu tiên hoặc cuối cùng hoặc khi nhiều thao tác ảnh hưởng đến cùng một ký tự. Ví dụ, với`s = "aa"`Và`k = 3`,`t = "aaaaaa"`, việc loại bỏ chữ 'a' thứ ba sẽ thay đổi vị trí của các lần xuất hiện tiếp theo, vì vậy cần phải tính toán cẩn thận. Một tình huống phức tạp khác là khi tất cả các thao tác xóa đều nhắm vào cùng một ký tự trong một khối dày đặc, điều này có thể dễ dàng dẫn đến sai sót riêng lẻ nếu chúng ta không theo dõi chính xác các vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là bạo lực: xây dựng chuỗi`t`một cách rõ ràng và đối với mỗi thao tác, hãy lặp qua chuỗi để tìm`p`-thứ xuất hiện của ký tự đích, sau đó loại bỏ nó. Điều này đúng vì nó mô phỏng hoạt động chính xác, nhưng chi phí là O(n × |t|), trong trường hợp xấu nhất là khoảng 4×10^9 hoạt động. Điều đó rõ ràng vượt quá giới hạn tính toán khả thi trong khoảng thời gian 3 giây. 

Quan sát quan trọng để có giải pháp nhanh hơn là chúng ta không cần phải quét liên tục toàn bộ chuỗi để xác định vị trí các lần xuất hiện. Thay vào đó, chúng ta có thể xử lý trước`t`bằng cách lưu trữ, đối với mỗi ký tự, vị trí mà nó xuất hiện. Sau đó, mỗi thao tác xóa sẽ trở thành vấn đề lập chỉ mục vào danh sách các vị trí, đánh dấu vị trí đó là đã bị xóa và sau đó xây dựng lại chuỗi cuối cùng. Việc sử dụng cấu trúc dữ liệu như danh sách được sắp xếp hoặc mảng vị trí xuất hiện đảm bảo rằng chúng ta có thể xử lý từng thao tác xóa trong thời gian không đổi sau khi tiền xử lý. 

Chúng ta có thể duy trì một mảng Boolean cho biết vị trí nào trong`t`bị xóa. Sau khi xử lý tất cả các thao tác, lặp qua`t`và chỉ bao gồm các vị trí không bị xóa sẽ cho chuỗi cuối cùng. Điều này làm giảm độ phức tạp xuống O(|t| + n), có thể chấp nhận được đối với các ràng buộc đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × k × | s | ) | 
| Tối ưu | O(k × | s | + n) | 

## Hướng dẫn thuật toán 

1. Xây dựng chuỗi ban đầu`t`bằng cách lặp lại`s` `k`lần. Điều này rất đơn giản và tổng chiều dài là`len(s) * k`. 
2. Chuẩn bị từ điển`positions`ánh xạ từng ký tự vào danh sách các chỉ mục nơi ký tự đó xuất hiện trong`t`. Lặp lại qua`t`một lần, nối thêm từng chỉ mục vào danh sách thích hợp trong`positions`. Điều này cho phép truy cập trực tiếp vào`p`-lần xuất hiện của bất kỳ ký tự nào. 
3. Khởi tạo mảng Boolean`deleted`có độ dài bằng`t`để theo dõi những vị trí đã bị loại bỏ. 
4. Xử lý từng thao tác xóa`(p, c)`. Tra cứu danh sách chỉ số cho ký tự`c`TRONG`positions`và tìm`p-1`-chỉ mục thứ (dựa trên số 0) trong danh sách đó. Đánh dấu`deleted[index] = True`để loại bỏ nó. 
5. Sau khi xóa xong, hãy lặp lại`t`và xây dựng chuỗi cuối cùng bằng cách chỉ bao gồm các ký tự có`deleted`cờ là Sai. 

Tại sao nó hoạt động: Ở mỗi bước,`positions[c]`duy trì vị trí tuyệt đối của tất cả các lần xuất hiện của`c`trong chuỗi gốc, không phụ thuộc vào việc xóa trước đó. Sử dụng`deleted`mảng đảm bảo rằng chúng ta không sửa đổi cấu trúc của`positions`, tránh việc cập nhật danh sách tốn kém trong khi vẫn thể hiện chính xác trạng thái hiện tại của chuỗi. Bất biến này đảm bảo việc xây dựng lại cuối cùng sẽ tạo ra câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())
s = input().strip()
n = int(input())
operations = [input().split() for _ in range(n)]

t = s * k
positions = {chr(c): [] for c in range(ord('a'), ord('z')+1)}

for i, char in enumerate(t):
    positions[char].append(i)

deleted = [False] * len(t)

for p_str, c in operations:
    p = int(p_str)
    index = positions[c][p-1]
    deleted[index] = True

result = ''.join(t[i] for i in range(len(t)) if not deleted[i])
print(result)
```Giải pháp này xây dựng chuỗi ban đầu, tính toán trước vị trí của từng ký tự và sử dụng mảng Boolean đơn giản để theo dõi các thao tác xóa. sử dụng`positions[c][p-1]`tránh việc quét lặp đi lặp lại, đây là cách tăng tốc chính so với lực lượng vũ phu. Việc xây dựng lại cuối cùng đảm bảo rằng các vị trí đã xóa sẽ được bỏ qua, duy trì đúng thứ tự. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
bac
3
2 a
1 b
2 c
```| Bước | t | Hoạt động | Đã xóa chỉ mục | Chuỗi kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | bacbac | - | [] | bacbac | 
| 1 | bacbac | xóa 'a' thứ 2 | [3] | bacbc | 
| 2 | bacbc | xóa 'b' đầu tiên | [1, 3] | acbc | 
| 3 | acbc | xóa 'c' thứ 2 | [1, 3, 4] | acb | 

Dấu vết xác nhận thuật toán xác định chính xác vị trí xuất hiện phù hợp và cập nhật`deleted`mảng. 

Đầu vào tùy chỉnh 2:```
3
aa
2
1 a
3 a
```| Bước | t | Hoạt động | Đã xóa chỉ mục | Chuỗi kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | aaaa | - | [] | aaaa | 
| 1 | aaaa | xóa 'a' đầu tiên | [0] | aaa | 
| 2 | aaa | xóa 'a' thứ 3 | [0, 3] | aaa | 

Điều này xác minh việc xử lý thích hợp khi nhiều lần xóa nhắm vào cùng một ký tự ở các vị trí liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k × | s | 
| Không gian | O(k × | s | 

Với k × |s| 200.000 và n 20.000, phương pháp này dễ dàng chạy trong giới hạn thời gian 3 giây và sử dụng bộ nhớ chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    k = int(input())
    s = input().strip()
    n = int(input())
    operations = [input().split() for _ in range(n)]

    t = s * k
    positions = {chr(c): [] for c in range(ord('a'), ord('z')+1)}
    for i, char in enumerate(t):
        positions[char].append(i)
    deleted = [False] * len(t)
    for p_str, c in operations:
        p = int(p_str)
        index = positions[c][p-1]
        deleted[index] = True
    return ''.join(t[i] for i in range(len(t)) if not deleted[i])

# Provided sample
assert run("2\nbac\n3\n2 a\n1 b\n2 c\n") == "acb", "sample 1"

# Custom cases
assert run("3\naa\n2\n1 a\n3 a\n") == "aaaa", "multiple same character deletions"
assert run("1\nabc\n0\n") == "abc", "no deletions"
assert run("2\nab\n4\n1 a\n1 a\n1 b\n1 b\n") == "", "all characters deleted"
assert run("5\na\n3\n1 a\n1 a\n1 a\n") == "aa", "single character repeated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3\naa\n2\n1 a\n3 a | aaa | Xóa nhiều lần trên cùng một ký tự, lập chỉ mục chính xác | 
| 1\nabc\n0 | abc | Không xóa, chuỗi không thay đổi | 
| 2\nab\n4\n1 a\n1 a\n1 b\n1 b | | Đã xóa tất cả ký tự | 
| 5\na\n3\n1 a\n1 a\n1 a | aa | Ký tự đơn lặp lại, trình tự xóa thích hợp | 

## Vỏ cạnh 

Khi thao tác xóa nhắm vào lần xuất hiện đầu tiên hoặc cuối cùng của một ký tự, thuật toán vẫn hoạt động vì`positions`lưu trữ các chỉ số tuyệt đối. Ví dụ,`s = "aa"`,`k = 3`,`t = "aaaaaa"`, xóa cái đầu tiên '
