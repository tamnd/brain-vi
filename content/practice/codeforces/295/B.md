---
title: "CF 295B - Greg và đồ thị"
description: "Chúng ta được cho một đồ thị có hướng có trọng số hoàn chỉnh với n đỉnh, được biểu diễn dưới dạng ma trận kề. Mỗi mục a[i][j] là trọng số của cạnh từ đỉnh i đến đỉnh j. Đồ thị hoàn chỉnh nên mỗi cặp đỉnh phân biệt đều có cạnh theo cả hai hướng."
date: "2026-06-05T17:44:31+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 295
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 179 (Div. 1)"
rating: 1700
weight: 295
solve_time_s: 85
verified: true
draft: false
---

[CF 295B – Greg và Đồ thị](https://codeforces.com/problemset/problem/295/B) 

**Đánh giá:** 1700 
**Tags:** dp, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng có trọng số hoàn chỉnh với`n`đỉnh được biểu diễn dưới dạng ma trận kề. Mỗi mục`a[i][j]`là trọng số của cạnh tính từ đỉnh`i`đến đỉnh`j`. Đồ thị hoàn chỉnh nên mỗi cặp đỉnh phân biệt đều có cạnh theo cả hai hướng. Greg chơi một trò chơi trong đó anh ấy lần lượt xóa các đỉnh khỏi biểu đồ và trước khi xóa từng đỉnh, anh ấy muốn biết tổng khoảng cách đường đi ngắn nhất giữa tất cả các cặp đỉnh còn lại. 

Đầu vào chỉ định thứ tự xóa dưới dạng một chuỗi các chỉ số đỉnh. Đối với mỗi lần xóa, chúng ta phải xuất ra một số nguyên duy nhất: tổng các đường đi ngắn nhất giữa tất cả các cặp đỉnh vẫn còn trong biểu đồ tại thời điểm đó. Đường đi ngắn nhất được tính chỉ dựa trên các đỉnh còn lại và các cạnh của chúng. Khoảng cách bản thân bằng 0 và không đóng góp vào tổng. 

Đồ thị có thể có tới 500 đỉnh, do đó, bất kỳ thuật toán nào lặp lại trên tất cả các bộ ba đỉnh nhiều lần đều khả thi nếu nó chạy trong`O(n^3)`thời gian. Một thuật toán đơn giản tính toán lại các đường đi ngắn nhất từ ​​đầu sau mỗi lần xóa sẽ có độ phức tạp về thời gian là`O(n^4)`, quá chậm. Các trường hợp cạnh bao gồm biểu đồ nhỏ nhất`n=1`, trong đó đầu ra gần như bằng 0 và các trường hợp có trọng số cạnh lớn trong đó việc xử lý số nguyên bất cẩn có thể tràn. 

Một cách tiếp cận ngây thơ cũng có thể thất bại nếu chúng ta không tính đến thứ tự xóa ngược một cách khéo léo. Ví dụ: việc loại bỏ các đỉnh trong một chuỗi mà không cập nhật đường đi một cách hiệu quả có thể đưa ra tổng sai vì các đỉnh bị loại bỏ trước đó có thể đã rút ngắn một số đường đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại thông qua trình tự xóa. Đối với mỗi lần loại bỏ đỉnh, chúng tôi tạo một sơ đồ con gồm các đỉnh còn lại và tính toán lại các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng Floyd-Warshall. Chi phí là`O(n^4)`trong trường hợp xấu nhất kể từ khi chúng tôi làm`n`xóa và mỗi yêu cầu`O(n^3)`công việc. Điều này đúng nhưng quá chậm đối với`n=500`. 

Cách tiếp cận tối ưu khai thác quan sát cho thấy chúng ta có thể đảo ngược vấn đề. Thay vì loại bỏ các đỉnh, chúng ta có thể xem xét **thêm lại chúng theo thứ tự ngược lại**. Nếu chúng ta xử lý việc xóa ngược, bắt đầu từ một biểu đồ trống và thêm các đỉnh theo thứ tự xóa ngược, chúng ta có thể duy trì ma trận đường đi ngắn nhất tất cả các cặp động một cách hiệu quả. Mỗi lần chúng ta thêm một đỉnh`k`, chúng tôi cập nhật khoảng cách bằng bước cập nhật Floyd-Warshall cho đỉnh`k`:```
d[i][j] = min(d[i][j], d[i][k] + d[k][j])
```Sau mỗi lần cộng, chúng tôi tính tổng khoảng cách giữa tất cả các đỉnh hiện đang hoạt động. Điều này làm giảm độ phức tạp`O(n^3)`về tổng thể, bởi vì mỗi phép cộng đỉnh chỉ yêu cầu một`n x n`cập nhật trong ma trận khoảng cách. Thông tin chi tiết quan trọng là việc đảo ngược thứ tự xóa sẽ chuyển "vấn đề xóa" thành "vấn đề bổ sung gia tăng", cho phép các bản cập nhật Floyd-Warshall tích lũy dần dần các đường đi ngắn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại tất cả các cặp sau mỗi lần xóa) | O(n^4) | O(n^2) | Quá chậm | 
| Phép cộng ngược với Floyd-Warshall | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận kề`a`và trình tự xóa`x`. Đảo ngược`x`để mô phỏng việc thêm các đỉnh trở lại thay vì xóa chúng. 
2. Khởi tạo ma trận khoảng cách`d`như một bản sao của`a`. Điều này sẽ giữ khoảng cách đường đi ngắn nhất động. 
3. Duy trì mảng boolean`active`để theo dõi những đỉnh nào đã được "thêm" trở lại. 
4. Khởi tạo danh sách trống`answer`để lưu trữ tổng các đường đi ngắn nhất sau mỗi lần thêm đỉnh. 
5. Lặp lại trình tự xóa ngược: 

1. Đánh dấu đỉnh hiện tại`k`hoạt động như thế nào. 
2. Với mọi cặp đỉnh`i`Và`j`, cập nhật`d[i][j] = min(d[i][j], d[i][k] + d[k][j], d[i][k] + d[k][j])`chỉ xem xét các đỉnh hoạt động. 
3. Tính tổng của`d[i][j]`cho tất cả các cặp`(i,j)`nơi cả hai`i`Và`j`đang hoạt động. 
4. Thêm số tiền này vào`answer`. 
6. Sau khi xử lý tất cả các đỉnh, đảo ngược`answer`khớp với thứ tự xóa ban đầu và in ra. 

**Tại sao nó hoạt động:** Điều bất biến là sau khi thêm từng đỉnh,`d[i][j]`chứa khoảng cách đường đi ngắn nhất giữa`i`Và`j`chỉ xem xét các đỉnh hoạt động cho đến nay. Việc đảo ngược thứ tự xóa đảm bảo rằng mọi cập nhật đường dẫn đều phản ánh chính xác các lối tắt tiềm năng mà đỉnh mới được thêm vào đưa vào. Bằng cách chỉ tính tổng khoảng cách trên các đỉnh đang hoạt động, chúng tôi sao chép số tiền cần thiết trước mỗi lần xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = [list(map(int, input().split())) for _ in range(n)]
x = list(map(int, input().split()))
x = [v-1 for v in x][::-1]  # zero-based and reversed

d = [row[:] for row in a]
active = [False] * n
answer = []

for k in x:
    active[k] = True
    for i in range(n):
        if not active[i]:
            continue
        for j in range(n):
            if not active[j]:
                continue
            d[i][j] = min(d[i][j], d[i][k] + d[k][j])
    total = 0
    for i in range(n):
        if not active[i]:
            continue
        for j in range(n):
            if not active[j]:
                continue
            total += d[i][j]
    answer.append(total)

print(' '.join(map(str, answer[::-1])))
```Ma trận khoảng cách`d`chỉ được cập nhật cho các đỉnh hiện đang hoạt động. Phải cẩn thận chỉ lặp lại trên các đỉnh đang hoạt động để tránh tính khoảng cách cho các đỉnh chưa có trong biểu đồ. Việc đảo ngược trình tự xóa đảm bảo kết quả đầu ra tương ứng với tổng trước mỗi lần xóa. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
1
0
1
```| Bước | Đang hoạt động | ma trận d | Tổng hợp | 
| --- | --- | --- | --- | 
| thêm 1 | [Đúng] | [[0]] | 0 | 

Đầu ra là`0`, như mong đợi. 

**Ví dụ tùy chỉnh** 

đầu vào:```
3
0 1 4
1 0 2
4 2 0
3 1 2
```Thứ tự xóa đảo ngược: 2,1,3 → cộng 3,1,2. 

| Bước | Đang hoạt động | Ma trận d được cập nhật | Tổng hợp | 
| --- | --- | --- | --- | 
| thêm 2 | [Sai, Đúng, Sai] | khoảng cách cho đỉnh 2 | 0 | 
| thêm 1 | [Đúng, Đúng, Sai] | d[0][1]=1, d[1][0]=1 | 2 | 
| thêm 3 | [Đúng, Đúng, Đúng] | d[0][2]=3, d[2][0]=3, d[1][2]=2, d[2][1]=2 | 12 | 

Bảng xác nhận tổng khớp với trình tự dự kiến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Mỗi đỉnh trong số n đỉnh được thêm vào yêu cầu cập nhật n x n mục trong ma trận khoảng cách. | 
| Không gian | O(n^2) | Ma trận khoảng cách`d`và ma trận kề`a`yêu cầu`n^2`không gian. | 

Với`n ≤ 500`,`n^3 = 125,000,000`hoạt động được chấp nhận trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]
    x = list(map(int, input().split()))
    x = [v-1 for v in x][::-1]
    d = [row[:] for row in a]
    active = [False]*n
    answer = []
    for k in x:
        active[k] = True
        for i in range(n):
            if not active[i]:
                continue
            for j in range(n):
                if not active[j]:
                    continue
                d[i][j] = min(d[i][j], d[i][k] + d[k][j])
        total = sum(d[i][j] for i in range(n) for j in range(n) if active[i] and active[j])
        answer.append(total)
    return ' '.join(map(str, answer[::-1]))

# provided sample
assert run("1\n0\n1\n") == "0", "sample 1"

# custom: 3 vertices
assert run("3\n0 1 4\n1 0 2\n4 2 0\n3 1 2\n") == "12 2 0", "custom small"

# custom: all equal weights
assert run("2\n0 5\n5 0\n1 2\n") == "10 0", "all-equal"

# custom: n=1
assert run("1\n0\n1\n") == "0", "minimum n"

# custom: n=4, increasing sequence
assert run("4\n0 1 2 3\n1 0 4 5\n2 4 0 6\n3 5 6 0\n4 3 2 1\n
```
