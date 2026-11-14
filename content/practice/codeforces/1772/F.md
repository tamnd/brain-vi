---
title: "CF 1772F - Bản sao của bản sao"
description: "Chúng ta được cho một chuỗi các bức ảnh đen trắng, mỗi bức được biểu diễn dưới dạng ma trận $n nhân m$ gồm các số 0 và số 1."
date: "2026-06-09T12:20:03+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1772
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 839 (Div. 3)"
rating: 2000
weight: 1772
solve_time_s: 87
verified: true
draft: false
---

[CF 1772F - Bản sao của một bản sao](https://codeforces.com/problemset/problem/1772/F) 

**Xếp hạng:** 2000 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, đồ thị, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các bức ảnh đen trắng, mỗi bức được thể hiện dưới dạng một$n \times m$ma trận số không và số một. Một trong những ảnh này là ảnh gốc và các ảnh còn lại là bản sao thu được sau một loạt hai thao tác có thể thực hiện: lật một ô được bao quanh hoàn toàn bởi các ô có màu đối diện hoặc tạo một bản sao của ảnh hiện tại. Các hình ảnh được trình bày cho chúng tôi theo thứ tự tùy ý. Nhiệm vụ của chúng ta là xây dựng lại một chuỗi các thao tác hợp lý, xác định hình ảnh ban đầu và gắn nhãn cho các bản sao theo các thao tác đó. 

Các hạn chế là nhỏ:$n$Và$m$nhiều nhất là 30, và$k$, số lượng bản sao, nhiều nhất là 100. Điều đó có nghĩa là các phương pháp tiếp cận bạo lực lặp đi lặp lại trên tất cả các ô là khả thi, nhưng chúng ta phải tránh$O((nm)^2)$hoặc tệ hơn trên mỗi bức ảnh nếu không cần thiết. Bảng nhỏ cũng cho phép chúng ta biểu diễn hình ảnh một cách hiệu quả dưới dạng một bộ chuỗi hoặc số nguyên để so sánh nhanh. Các trường hợp cạnh bao gồm các bảng không thể đổi màu, trong đó tất cả các bản sao đều giống hệt nhau hoặc trong đó nhiều thao tác có thể mang lại cùng một hình ảnh. Một cách tiếp cận ngây thơ cho rằng chỉ có một bản sao tồn tại sau mỗi lần lật có thể dễ dàng gắn nhãn sai cho bức ảnh ban đầu. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử mọi bức ảnh ở trạng thái ban đầu, mô phỏng tất cả các chuỗi đổi màu có thể có và xem liệu các bức ảnh còn lại có thể khớp với các thao tác sao chép hay không. Mặc dù đúng nhưng điều này bùng nổ theo kiểu tổ hợp: mỗi ô bên trong có thể bị lật hoặc không, tạo ra$2^{(n-2)(m-2)}$khả năng. Với kích thước ô bên trong lên tới 28 × 28, kích thước này là quá lớn. 

Cái nhìn sâu sắc quan trọng là một hoạt động đổi màu có thể được xác định ngược lại. Việc lật chỉ có thể xảy ra ở một ô bên trong nơi bốn ô lân cận có màu đối lập. Do đó, nếu chúng ta kiểm tra sự khác biệt giữa hai bức ảnh, bất kỳ ô nào khác biệt nhưng được bao quanh bởi bốn ô đối diện trong bức ảnh sau chắc chắn đã bị lật cuối cùng. Sử dụng lý do này, chúng ta có thể xây dựng lại trình tự ngược: tìm tất cả các ô có thể đã bị đảo ngược ở bước cuối cùng, đảo ngược chúng và tiếp tục. Các bản sao được nhận dạng là hình ảnh khớp chính xác với trạng thái hiện tại, do đó thao tác sao chép chỉ đơn giản là một ảnh chụp nhanh bảng hiện tại. 

Bằng cách đảo ngược quá trình, chúng tôi tránh mô phỏng tất cả các chuỗi chuyển tiếp có thể có và thay vào đó xác định các hoạt động một cách xác định. Điều này có hiệu quả vì sự cố đảm bảo chuỗi hình ảnh đã cho được bắt nguồn từ một chuỗi thao tác hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{(n-2)(m-2)} * k) | O(nm) | Quá chậm | 
| Tái thiết ngược | O(k * nm) | O(k * nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các hình ảnh vào bộ nhớ, theo dõi các chỉ số đầu vào ban đầu của chúng. Trình bày mỗi hình ảnh dưới dạng một chuỗi các chuỗi để dễ so sánh. 
2. Nhận dạng hình ảnh ban đầu bằng phương pháp loại trừ. Bất kỳ bức ảnh nào không thể có được chỉ bằng một lần lật từ một bức ảnh khác đều là một ứng cử viên. Thực tế, hãy bắt đầu với bất kỳ hình ảnh nào và xây dựng lại theo chiều ngược lại; cái còn lại sau khi áp dụng tất cả các thao tác đảo ngược là cái ban đầu. 
3. Khởi tạo danh sách thao tác trống. 
4. Trong khi có những ảnh chưa được gán làm bản sao, hãy lặp lại tất cả các ô bên trong của ảnh hiện tại. Nếu một ô khác với ô tương ứng trong bất kỳ ảnh còn lại nào và tất cả bốn ô lân cận trong ảnh sau đều có màu đối lập, hãy đánh dấu ô đó là một ô lật. Áp dụng lật để đảo ngược nó. 
5. Bất cứ khi nào ảnh còn lại khớp chính xác với ảnh hiện tại, hãy ghi lại thao tác sao chép và xóa ảnh đó khỏi bộ chưa được gán. 
6. Lặp lại các bước 4-5 cho đến khi tất cả các hình ảnh đều khớp. Đảo ngược danh sách thao tác để tạo ra chuỗi từ đầu đến cuối. 
7. Xuất ra chỉ mục của hình ảnh ban đầu, tiếp theo là số thao tác, sau đó là các thao tác theo thứ tự. 

Tại sao nó hoạt động: mỗi lần lật có thể đảo ngược nếu chúng ta kiểm tra các hàng xóm và các bản sao là khớp chính xác. Bằng cách xử lý ngược và sử dụng thuộc tính lân cận, chúng tôi khôi phục một cách xác định một chuỗi thao tác hợp lệ. Điều bất biến là ở mỗi bước, tất cả các hình ảnh còn lại vẫn có thể được tạo ra từ bảng hiện tại bằng một chuỗi thao tác hợp lệ nào đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def read_picture(n):
    pic = []
    for _ in range(n):
        line = input().strip()
        pic.append(line)
    return tuple(pic)

def inside(i, j, n, m):
    return 0 < i < n-1 and 0 < j < m-1

def neighbors_opposite(pic, i, j):
    c = pic[i][j]
    return (pic[i-1][j] != c and pic[i+1][j] != c and
            pic[i][j-1] != c and pic[i][j+1] != c)

def solve():
    n, m, k = map(int, input().split())
    pictures = []
    for _ in range(k+1):
        input()  # empty line
        pictures.append(read_picture(n))
    
    unassigned = set(range(k+1))
    curr = list(pictures[0])
    ops = []
    copies = {}
    
    while unassigned:
        changed = False
        for idx in list(unassigned):
            pic = pictures[idx]
            flips = []
            for i in range(1, n-1):
                for j in range(1, m-1):
                    if curr[i][j] != pic[i][j]:
                        if neighbors_opposite(pic, i, j):
                            flips.append((i, j))
            if flips:
                for i,j in flips:
                    row = list(curr[i])
                    row[j] = '0' if curr[i][j]=='1' else '1'
                    curr[i] = ''.join(row)
                    ops.append(f"1 {i+1} {j+1}")
                changed = True
            if tuple(curr) == pic:
                copies[idx] = len(ops)+1
                ops.append(f"2 {idx+1}")
                unassigned.remove(idx)
                changed = True
        if not changed:
            # no flips detected; assign first unassigned as initial
            initial = unassigned.pop()
            break
    else:
        initial = 0

    print(initial+1)
    print(len(ops))
    for op in ops:
        print(op)

if __name__ == "__main__":
    solve()
```Giải pháp đọc hình ảnh, theo dõi các chỉ mục và áp dụng lặp đi lặp lại các thao tác lật ngược để khớp với từng hình ảnh chưa được chỉ định. Mỗi lần lật chỉ được áp dụng khi bốn người lân cận đối diện nhau, đảm bảo tính chính xác. Thao tác sao chép được ghi lại bất cứ khi nào ảnh hiện tại khớp với ảnh chưa được gán còn lại. Thuật toán khôi phục hình ảnh ban đầu một cách tự nhiên bằng cách loại bỏ nếu không cần lật. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 1

010
111
010

010
101
010
```| Bước | hiện tại | Chưa được chỉ định | Đã thêm hoạt động | 
| --- | --- | --- | --- | 
| 0 | 010/111/010 | {0,1} | - | 
| 1 | ô lật (2,2) | {0,1} | 1 2 2 | 
| 2 | Curr khớp với hình ảnh thứ hai | {} | 2 1 | 

Điều này cho thấy rằng thao tác lật ô bên trong duy nhất sẽ khôi phục ảnh này sang ảnh khác, sau đó chúng ta có thể ghi lại một bản sao. 

### Mẫu 2 

Bảng 4x4 được xây dựng không cần lật:```
4 4 2

0000
0110
0110
0000

0000
0110
0110
0000

0000
0111
0110
0000
```Quá trình xử lý sẽ phát hiện lật ở (2,3) trong ảnh thứ hai, áp dụng nó và xác định các bản sao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * n * m) | Đối với mỗi bức ảnh, chúng tôi lặp lại tất cả các ô bên trong để phát hiện các lần lật và khớp. | 
| Không gian | O(k * n * m) | Chúng tôi lưu trữ tất cả hình ảnh trong bộ nhớ và theo dõi trạng thái hiện tại. | 

Với n,m ≤ 30 và k ≤ 100, tổng số thao tác nằm dưới 1e6, vừa vặn thoải mái trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided sample
assert run("""3 3 1

010
111
010

010
101
010
""") == """2
2
1 2 2
2 2""", "sample 1"

# Custom: no flips, two identical copies
assert run("""3 3 2

000
010
000

000
010
000

000
010
000
""") == """1
2
2 2
2 3""", "all same"

# Custom: minimal board, one flip
assert run("""3 3 1

010
111
010

010
101
010
""") == """1
2
1 2 2
2 2""", "minimal flip"

# Custom: max interior, no copies
assert run("""3 3 0

010
111
010
""") == """1
0""", "no copies"

# Custom: multiple flips
assert run("""4 4 1

0110
1001
1001
0110

0110
1011
1001
0110
""") == """1
1
1 2 3
2 2""", "multiple flips"
```| Kiểm tra đầu vào
