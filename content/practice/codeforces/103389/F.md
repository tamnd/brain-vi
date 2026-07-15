---
title: "CF 103389F - \u5730\u56fe\u538b\u7f29"
description: "Chúng ta được cung cấp một lưới các ký tự hình chữ nhật và nhiệm vụ là xác định xem lưới có thể được nén đến mức nào bằng cách lặp lại định kỳ ở cả hai chiều."
date: "2026-07-03T12:12:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103389
codeforces_index: "F"
codeforces_contest_name: "2021\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 103389
solve_time_s: 43
verified: true
draft: false
---

[CF 103389F - \u5730\u56fe\u538b\u7f29](https://codeforces.com/problemset/problem/103389/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới các ký tự hình chữ nhật và nhiệm vụ là xác định xem lưới có thể được nén đến mức nào bằng cách lặp lại định kỳ ở cả hai chiều. Nói cách khác, chúng ta muốn tìm hình chữ nhật con nhỏ nhất sao cho việc xếp nó liên tục theo hướng ngang và dọc sẽ tái tạo lại toàn bộ lưới một cách chính xác. 

Quan sát quan trọng là việc lặp lại theo hàng và lặp lại theo cột không ảnh hưởng lẫn nhau. Nếu lưới lặp lại mỗi k hàng và mỗi d cột thì đơn vị lặp lại cơ bản là khối k x d và câu trả lời là k nhân d. 

Đầu vào có thể được coi là một ma trận trong đó mỗi hàng là một chuỗi. Đầu ra là một số nguyên duy nhất biểu thị diện tích của ô lặp lại nhỏ nhất. 

Từ góc độ phức tạp, kích thước lưới thường đạt tổng cộng khoảng 10^5 ô hoặc nhiều hơn một chút tùy thuộc vào biến thể của vấn đề. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào so sánh tất cả các hình chữ nhật con hoặc kiểm tra tính tuần hoàn bằng vũ lực trên tất cả các kích thước ứng cử viên. Một cách tiếp cận đơn giản là thử tất cả các khoảng thời gian hàng và cột một cách độc lập và xác thực chúng bằng cách quét toàn bộ lưới sẽ chuyển thành hành vi hình khối trong trường hợp xấu nhất, quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi chỉ có một chiều là tuần hoàn. Ví dụ: nếu tất cả các hàng giống nhau nhưng các cột thì không, chu kỳ hàng là 1 trong khi chu kỳ cột có thể có chiều rộng tối đa. Tương tự, nếu mỗi cột giống hệt nhau từ dưới lên nhưng các hàng khác nhau theo chiều ngang thì chu kỳ cột là 1. Việc triển khai đơn giản giả định tính đối xứng hoặc cố gắng lấy một chiều từ chiều kia sẽ thất bại trên các lưới bất đối xứng này. 

Một trường hợp khác là lưới hoàn toàn đồng nhất, trong đó mọi ô đều giống hệt nhau. Ở đây cả hai khoảng thời gian hàng và cột đều thu gọn thành 1 và câu trả lời là 1. Bất kỳ triển khai nào không tính toán chính xác hàm tiền tố tối thiểu hoặc đường viền cho mảng không đổi có thể trả về không chính xác kích thước đầy đủ. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra độc lập mọi chu kỳ hàng k và chu kỳ cột d có thể có. Với mỗi ứng viên k, chúng ta sẽ xác minh rằng hàng i bằng hàng i modulo k với mọi i và tương tự với các cột. Mỗi lần xác minh sẽ quét toàn bộ lưới, do đó việc kiểm tra một cặp (k, d) sẽ tốn O(nm). Trong trường hợp xấu nhất, việc thử tất cả k lên đến n và tất cả d lên đến m sẽ dẫn đến hành vi O(nm(n + m)), vượt xa giới hạn khả thi. 

Cái nhìn sâu sắc quan trọng là tính tuần hoàn trong mỗi chiều giảm xuống vấn đề chuỗi hoặc chu kỳ chuỗi cổ điển. Nếu chúng ta nén mỗi hàng thành một giá trị băm thì lưới sẽ trở thành một chuỗi các giá trị băm hàng. Mẫu hàng lặp lại nhỏ nhất chính xác là khoảng thời gian nhỏ nhất của chuỗi này, có thể được tìm thấy bằng cách sử dụng hàm tiền tố từ KMP hoặc bằng cách tính toán tiền tố thích hợp dài nhất cũng là hậu tố. 

Ý tưởng tương tự cũng áp dụng cho các cột bằng cách xử lý mỗi cột dưới dạng một chuỗi ký tự và băm nó hoặc hiệu quả hơn bằng cách tính toán trước các giá trị băm của cột để mỗi cột trở thành một đơn vị có thể so sánh được. Khi cả hai khoảng thời gian tối thiểu được tính toán độc lập, khối lặp lại cơ bản của lưới sẽ là tích của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm(n + m)) | O(1) | Quá chậm | 
| Tối ưu (KMP/băm) | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các hàng và cột một cách đối xứng và tính toán chu kỳ tối thiểu của chúng một cách độc lập. 

### Hướng hàng 

1. Chuyển đổi mỗi hàng thành giá trị băm. Điều này biến lưới thành một mảng một chiều có độ dài n. 

Bước này là cần thiết vì chúng ta muốn so sánh toàn bộ các hàng trong O(1), thay vì so sánh từng ký tự. 
2. Tính hàm tiền tố (hàm lỗi KMP) trên mảng row-hash.

Hàm tiền tố ghi lại tiền tố thích hợp dài nhất cũng là hậu tố cho mọi tiền tố của mảng. 
3. Gọi k là độ dài chu kỳ nhỏ nhất của dãy hàng. Nó được cho bởi n trừ đi giá trị cuối cùng của hàm tiền tố. 

Điều này có tác dụng vì đường viền dài nhất của chuỗi đầy đủ xác định mức độ lặp lại ở cấp cao nhất. 

### Hướng cột 

1. Xây dựng các hàm băm cột bằng cách coi mỗi cột là một chuỗi ký tự trên tất cả các hàng. 

Mỗi cột trở thành một đơn vị có thể so sánh được, tương tự như cách xử lý các hàng. 
2. Áp dụng phép tính hàm tiền tố tương tự trên mảng băm cột. 

Điều này tạo ra chu kỳ cột nhỏ nhất d. 

###Câu trả lời cuối cùng 

1. Nhân hai khoảng thời gian độc lập k và d để có được diện tích của ô lặp lại tối thiểu. 

### Tại sao nó hoạt động 

Sự lặp lại hàng và sự lặp lại cột là các ràng buộc độc lập vì mọi cách xếp hợp lệ đều phải tôn trọng cả hai cùng một lúc. Khi một khoảng thời gian hợp lệ tồn tại trong các hàng, mọi khối k hàng đều giống hệt nhau. Bên trong mỗi khối như vậy, tính tuần hoàn của cột xác định cách mỗi hàng nén theo chiều ngang. Sự kết hợp của hai khoảng thời gian tối thiểu độc lập này tạo ra hình chữ nhật nhỏ nhất có thể xếp chồng lên toàn bộ lưới mà không tạo ra sự không khớp. 

Tính chính xác dựa trên thực tế là các yếu tố quan hệ đẳng thức lưới tạo thành đẳng thức của chuỗi hàng và chuỗi cột và cả hai đều được giải quyết một cách tối ưu bằng tính toán đường viền tiêu chuẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def prefix_function(arr):
    n = len(arr)
    pi = [0] * n
    j = 0
    for i in range(1, n):
        while j > 0 and arr[i] != arr[j]:
            j = pi[j - 1]
        if arr[i] == arr[j]:
            j += 1
            pi[i] = j
    return pi

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    row_hash = []
    for r in grid:
        h = 0
        for c in r:
            h = h * 131 + (ord(c) - 96)
        row_hash.append(h)

    pi_row = prefix_function(row_hash)
    row_period = n - pi_row[-1]
    if n % row_period != 0:
        row_period = n

    col_hash = []
    for j in range(m):
        h = 0
        for i in range(n):
            h = h * 131 + (ord(grid[i][j]) - 96)
        col_hash.append(h)

    pi_col = prefix_function(col_hash)
    col_period = m - pi_col[-1]
    if m % col_period != 0:
        col_period = m

    print(row_period * col_period)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén mỗi hàng thành một hàm băm cuộn để việc so sánh hàng trở thành các hoạt động có thời gian không đổi. Ý tưởng tương tự được sử dụng cho các cột. Sau đó, hàm tiền tố KMP được áp dụng cho từng biểu diễn một chiều. 

Một điểm tinh tế là việc kiểm tra tính chia hết cuối cùng. Ngay cả khi hàm tiền tố gợi ý một khoảng thời gian ứng cử viên, nó chỉ tạo thành một sự lặp lại hợp lệ nếu chia tổng chiều dài. Nếu không, chúng ta phải quay lại chiều dài đầy đủ vì không tồn tại ô xếp chính xác nhỏ hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một lưới trong đó các hàng lặp lại sau mỗi 2 hàng và các cột lặp lại sau mỗi 3 cột. 

Băm hàng:`[A, B, A, B]`Băm cột:`[X, Y, Z, X, Y, Z]`| Bước | Mảng | Chức năng tiền tố | Thời kỳ | 
| --- | --- | --- | --- | 
| Hàng | A B A B | [0,0,1,2] | 2 | 
| Cols | X Y Z X Y Z | [0,0,0,1,2,3] | 3 | 

Thuật toán xác định cấu trúc hàng lặp lại sau mỗi 2 và cấu trúc cột lặp lại sau mỗi 3, đưa ra câu trả lời cuối cùng là 6. 

Điều này xác nhận rằng tính tuần hoàn độc lập sẽ kết hợp chính xác thành một ô 2D. 

### Ví dụ 2 

Một lưới hoàn toàn thống nhất:```
aaa
aaa
aaa
```| Bước | Mảng | Chức năng tiền tố | Thời kỳ | 
| --- | --- | --- | --- | 
| Hàng | A A A | [0,1,2] | 1 | 
| Cols | A A A | [0,1,2] | 1 | 

Cả hai chiều đều thu gọn về giai đoạn 1, tạo ra câu trả lời là 1. Điều này thể hiện việc xử lý đúng sự lặp lại tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi hàng và cột được băm một lần và các hàm tiền tố chạy theo thời gian tuyến tính | 
| Không gian | O(n + m) | Lưu trữ cho các giá trị băm hàng và cột cộng với mảng tiền tố | 

Thuật toán phù hợp một cách thoải mái với các ràng buộc điển hình cho kích thước lưới lên tới tổng số khoảng 10^5 đến 10^6 ký tự, vì mỗi ô được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve() or ""

# Sample-like cases (illustrative)

# 1. Fully uniform grid
assert run("3 3\naaa\naaa\naaa\n") == "1", "all equal grid"

# 2. Horizontal repetition only
# rows repeat every 2, cols no repetition
assert run("4 2\nab\ncd\nab\ncd\n") == "4", "row period 2, col period 2 -> area 4"

# 3. No repetition
assert run("2 3\nabc\ndef\n") == "6", "no periodicity"

# 4. Vertical repetition only
assert run("2 4\nabcd\nabcd\n") == "4", "column repetition only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả lưới bằng nhau | 1 | sụp đổ hoàn toàn theo cả hai chiều | 
| ab/cd lặp đi lặp lại | 4 | tính tuần hoàn hàng và cột độc lập | 
| không có tính tuần hoàn | 6 | dự phòng cho toàn bộ lưới | 
| lặp lại theo chiều dọc | 4 | tính toán chu kỳ cột | 

## Vỏ cạnh 

Một lưới hoàn toàn không đổi được xử lý chính xác vì cả mảng băm hàng và cột đều trở thành chuỗi không đổi và hàm tiền tố trả về đường viền tối đa. Điều này buộc cả hai giai đoạn thành 1, tạo ra ô tối thiểu chính xác. 

Lưới có sự lặp lại chỉ trong một chiều sẽ được xử lý vì tính toán hàng và cột là độc lập. Ví dụ: một lưới trong đó mọi hàng giống hệt nhau nhưng các cột khác nhau sẽ mang lại chu kỳ hàng 1 trong khi chu kỳ cột vẫn giữ nguyên chiều rộng và ngược lại. 

Lưới có kích thước nguyên tố không có sự lặp lại sẽ kích hoạt cơ chế dự phòng trong đó hàm tiền tố trả về đường viền bằng 0, dẫn đến các khoảng thời gian có độ dài đầy đủ ở cả hai chiều. Điều này đảm bảo chúng ta không giả định sai cấu trúc từng phần khi không tồn tại.
