---
title: "CF 104921C - Lời trên giấy"
description: "Chúng tôi được cung cấp một số lưới 8 x 8 ký tự độc lập. Mỗi lưới hầu hết chứa đầy các dấu chấm, nhưng đâu đó bên trong nó ẩn một từ duy nhất. Từ được viết thẳng vào đúng một cột, chiếm các hàng liên tiếp không bị gián đoạn."
date: "2026-06-28T08:09:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104921
codeforces_index: "C"
codeforces_contest_name: "Easy_Training"
rating: 0
weight: 104921
solve_time_s: 66
verified: true
draft: false
---

[CF 104921C - Lời trên giấy](https://codeforces.com/problemset/problem/104921/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số lưới 8 x 8 ký tự độc lập. Mỗi lưới hầu hết chứa đầy các dấu chấm, nhưng đâu đó bên trong nó ẩn một từ duy nhất. Từ được viết thẳng vào đúng một cột, chiếm các hàng liên tiếp không bị gián đoạn. Mỗi ô khác trong lưới là một dấu chấm. 

Nhiệm vụ của mỗi lưới là khôi phục từ dọc bị ẩn đó bằng cách đọc các chữ cái trong cột đúng từ trên xuống dưới. 

Các ràng buộc là nhỏ và cố định. Mỗi trường hợp thử nghiệm chứa chính xác 64 ký tự được sắp xếp thành khối 8 x 8 và có tối đa 1000 trường hợp thử nghiệm như vậy. Điều này có nghĩa là tổng kích thước đầu vào được giới hạn bởi khoảng 64.000 ký tự, do đó, bất kỳ phương pháp nào quét từng lưới với số lần không đổi đều dễ dàng đủ nhanh. Ngay cả việc quét toàn bộ lặp đi lặp lại cho mỗi trường hợp thử nghiệm vẫn có thể được chấp nhận, nhưng không cần thiết. 

Không có trường hợp cạnh thuật toán thực sự nào về mặt hiệu suất, nhưng có những bẫy về tính chính xác trong việc diễn giải bố cục. Một sai lầm ngây thơ là cho rằng từ đó có thể bị phân tán hoặc yêu cầu ghép nhiều cột lại với nhau. Một sai lầm khác là cố đọc theo hàng hoặc dừng lại ở chữ cái đầu tiên gặp phải mà không đảm bảo bạn ở cùng một cột. 

Ví dụ: hãy xem xét một lưới trong đó từ đó`"lost"`trong một cột:```
........
....l...
....o...
....s...
....t...
........
........
........
```Đầu ra đúng là`"lost"`. Một cách tiếp cận thiếu sót có thể quét từng hàng và chọn các chữ cái ở bất cứ nơi nào chúng xuất hiện, cách này vẫn hoạt động ở đây nhưng sẽ thất bại nếu nhiều cột chứa các chữ cái trong các bài toán khác có kiểu tương tự. Ràng buộc xác định là chính xác một cột chứa tất cả các chữ cái của từ đó. 

Một trường hợp khác là khi từ là một ký tự đơn, chẳng hạn như:```
........
........
....a...
........
........
........
........
........
```Đầu ra phải là`"a"`. Bất kỳ logic nào giả định ít nhất hai chữ cái hoặc cố gắng phát hiện điểm bắt đầu và điểm kết thúc đều có thể bị hỏng ở đây. 

Thực tế cấu trúc quan trọng là chính xác một cột chứa các ký tự không có dấu chấm và các ký tự đó xuất hiện liền kề nhau từ trên xuống dưới. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ là kiểm tra từng cột và kiểm tra xem nó có chứa bất kỳ chữ cái nào không. Đối với mỗi cột, chúng tôi có thể quét tất cả 8 hàng và thu thập các ký tự không phải là dấu chấm. Nếu danh sách kết quả không trống thì cột đó chứa từ đó và chúng tôi xuất từ ​​đó. 

Vì kích thước lưới được cố định ở mức 8 x 8, ngay cả một cách tiếp cận thử mọi cột có thể và quét mọi hàng đều có hiệu quả là thời gian không đổi. Trường hợp xấu nhất cho mỗi trường hợp kiểm thử là kiểm tra 64 ô và với tối đa 1000 trường hợp kiểm thử thì đây chỉ là 64.000 thao tác, một điều không đáng kể. 

Quan sát trực tiếp hơn là chúng ta không cần phải “tìm kiếm” gì cả. Vì từ nằm hoàn toàn trong một cột nên mỗi hàng chứa tối đa một ký tự không có dấu chấm và tất cả các ký tự đó đều thuộc cùng một cột. Vì vậy, chúng ta có thể chỉ cần quét từng hàng, trích xuất ký tự không có dấu chấm đầu tiên trên mỗi hàng nếu nó tồn tại và nối chúng lại. Điều này trực tiếp xây dựng lại từ theo thứ tự. 

Brute-force hoạt động vì nó xác minh rõ ràng từng cột. Cách tiếp cận được tối ưu hóa sẽ loại bỏ nhu cầu chọn cột bằng cách khai thác đảm bảo tính duy nhất: chỉ một cột có thể chứa các chữ cái, do đó, mọi trích xuất theo hàng đều tự động nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cột quét vũ phu | O(8 × 8 × t) | O(1) | Đã chấp nhận | 
| Trích xuất theo hàng | O(8 × t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`, vì mỗi lưới là độc lập và phải được xử lý riêng. 
2. Với mỗi test, đọc 8 chuỗi đại diện cho 8 hàng của lưới. Mỗi chuỗi có độ dài 8, vì vậy chúng ta có thể coi nó như một mảng ký tự cố định. 
3. Khởi tạo một danh sách trống (hoặc trình tạo chuỗi) để tích lũy các ký tự của từ ẩn. 
4. Lặp lại từng hàng trong số 8 hàng. Đối với một hàng nhất định, hãy quét 8 cột của nó từ trái sang phải cho đến khi chúng tôi tìm thấy ký tự không phải`'.'`. 

Khi chúng ta tìm thấy một ký tự như vậy, hãy thêm nó vào câu trả lời và chuyển sang hàng tiếp theo ngay lập tức. Chúng tôi không tiếp tục quét hàng vì mỗi hàng chứa tối đa một ký tự có liên quan theo cấu trúc. 
5. Sau khi xử lý tất cả 8 hàng, xuất chuỗi tích lũy. 

Lựa chọn thiết kế quan trọng là quét theo hàng thay vì cố gắng xác định cột trước. Vì từ được căn chỉnh theo chiều dọc nên mỗi hàng đóng góp chính xác một ký tự từ cùng một cột, do đó việc trích xuất theo hàng sẽ tự động giữ nguyên thứ tự. 

### Tại sao nó hoạt động 

Mỗi lưới chứa chính xác một cột có các chữ cái và trong cột đó, các chữ cái xuất hiện thành các hàng liên tiếp tạo thành từ. Tất cả các ô khác là dấu chấm. Do đó, mỗi hàng chứa 0 chữ cái hoặc chính xác một chữ cái và tất cả các hàng không trống đều tương ứng với cùng một chỉ mục cột. Bằng cách trích xuất ký tự không phải dấu chấm đầu tiên từ mỗi hàng, chúng tôi xây dựng lại chuỗi các chữ cái theo thứ tự từ trên xuống dưới. Không có cột nào khác có thể đóng góp một lá thư, do đó không có sự mơ hồ nào phát sinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        grid = [input().strip() for _ in range(8)]
        word = []

        for r in range(8):
            for c in range(8):
                if grid[r][c] != '.':
                    word.append(grid[r][c])
                    break

        out.append("".join(word))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo chiến lược trích xuất theo hàng một cách trực tiếp. Đối với mỗi hàng, nó quét từ trái sang phải và dừng ở ký tự không có dấu chấm đầu tiên. các`break`là cần thiết vì mỗi hàng chỉ đóng góp tối đa một ký tự và việc tiếp tục sẽ là dư thừa. 

Việc sử dụng`strip()`đảm bảo chúng tôi không vô tình đưa các ký tự dòng mới vào biểu diễn lưới. Các câu trả lời cuối cùng được tích lũy trong một danh sách và được in cùng một lúc để tránh lặp lại chi phí I/O. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Lưới đầu vào:```
........
....l...
....o...
....s...
....t...
........
........
........
```| Hàng | Kết quả quét | Đã thêm ký tự | Từ cho đến nay | 
| --- | --- | --- | --- | 
| 0 | không | - | "" | 
| 1 | tôi | tôi | "tôi" | 
| 2 | o | o | "lo" | 
| 3 | s | s | "thua" | 
| 4 | t | t | "mất" | 
| 5-7 | không | - | "mất" | 

Đầu ra là`"lost"`, khớp với phép nối dọc của cột hoạt động duy nhất. 

### Ví dụ 2 

Lưới đầu vào:```
........
........
..a.....
........
........
........
........
........
```| Hàng | Kết quả quét | Đã thêm ký tự | Từ cho đến nay | 
| --- | --- | --- | --- | 
| 0-1 | không | - | "" | 
| 2 | một | một | "một" | 
| 3-7 | không | - | "một" | 

Điều này xác nhận thuật toán xử lý chính xác một từ có một ký tự mà không cần giả định bất kỳ độ dài tối thiểu nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(8 × 8 × t) | Mỗi ô được kiểm tra tối đa một lần mỗi lần quét hàng | 
| Không gian | O(1) | Chỉ lưu trữ 8 hàng và chuỗi đầu ra cho mỗi bài kiểm tra | 

Tổng công việc tối đa là khoảng 64.000 ký tự kiểm tra kích thước đầu vào tối đa, nằm trong giới hạn thoải mái đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # inline solution
        input = _sys.stdin.readline

        t = int(input())
        res = []

        for _ in range(t):
            grid = [input().strip() for _ in range(8)]
            word = []
            for r in range(8):
                for c in range(8):
                    if grid[r][c] != '.':
                        word.append(grid[r][c])
                        break
            res.append("".join(word))

        print("\n".join(res))

    return out.getvalue().strip()

# provided samples (conceptual placeholders; actual formatting may vary)
# assert run("...") == "..."

# custom cases

# single letter
assert run(
"1\n"
"........\n........\n........\n....x...\n........\n........\n........\n........\n"
) == "x"

# full column word
assert run(
"1\n"
".a......\n.b......\n.c......\n.d......\n.e......\n.f......\n.g......\n.h......\n"
) == "abcdefgh"

# word in last column
assert run(
"1\n"
".......k\n.......i\n.......t\n.......e\n.......n\n.......s\n.......u\n.......n\n"
) == "kitensun"

# all dots except one column
assert run(
"1\n"
"........\n........\n........\n....z...\n....a...\n....p...\n........\n........\n"
) == "zap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới chữ cái đơn | x | độ dài từ tối thiểu | 
| cột dọc đầy đủ | abcdefgh | trường hợp liền kề bình thường | 
| vị trí cột cuối cùng | nhà bếp | xử lý cạnh phải | 
| các chấm xung quanh thưa thớt | zap | bỏ qua các ô không liên quan | 

## Vỏ cạnh 

Một từ có một ký tự được xử lý vì quá trình quét theo hàng vẫn tìm thấy chính xác một ô không có dấu chấm về tổng thể. Đối với đầu vào chỉ tồn tại một chữ cái, chẳng hạn như:```
........
........
........
....x...
........
........
........
........
```thuật toán truy cập từng hàng, chỉ nối thêm`"x"`, và tạo ra`"x"`theo yêu cầu. 

Một từ nằm ở cột cuối cùng sẽ được xử lý mà không cần bất kỳ logic đặc biệt nào. Ví dụ:```
.......k
.......i
.......t
.......e
.......n
.......s
.......u
.......n
```Mỗi lần quét hàng sẽ đến cột 7 và tìm thấy chữ cái ở đó. Thứ tự vẫn đúng vì các hàng được xử lý từ trên xuống dưới, duy trì trình tự dọc. 

Lưới có nhiều dấu chấm bên ngoài từ không ảnh hưởng đến tính chính xác vì vòng lặp bên trong dừng ở ký tự không phải dấu chấm đầu tiên trên mỗi hàng. Ngay cả khi các biến thể trong tương lai có các ký tự nhiễu ở nơi khác, thì tính bất biến chỉ có một cột chứa các chữ cái sẽ đảm bảo không có trích xuất sai.
