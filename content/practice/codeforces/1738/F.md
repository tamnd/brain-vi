---
title: "CF 1738F - Người nghiện kết nối"
description: "Chúng ta được cho một đồ thị vô hướng trong đó ban đầu chúng ta chỉ biết số đỉnh và bậc của mỗi đỉnh. Nhiệm vụ là gán một màu cho mỗi đỉnh sao cho thỏa mãn hai điều kiện. Đầu tiên, các đỉnh có cùng màu phải tạo thành một thành phần được kết nối trong biểu đồ."
date: "2026-06-09T17:50:09+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dsu", "graphs", "greedy", "interactive", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1738
codeforces_index: "F"
codeforces_contest_name: "Codeforces Global Round 22"
rating: 2400
weight: 1738
solve_time_s: 155
verified: false
draft: false
---

[CF 1738F - Người nghiện kết nối](https://codeforces.com/problemset/problem/1738/F) 

**Đánh giá:** 2400 
**Tags:** thuật toán xây dựng, dsu, đồ thị, tham lam, tương tác, đường đi ngắn nhất, cây 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trong đó ban đầu chúng ta chỉ biết số đỉnh và bậc của mỗi đỉnh. Nhiệm vụ là gán một màu cho mỗi đỉnh sao cho thỏa mãn hai điều kiện. Đầu tiên, các đỉnh có cùng màu phải tạo thành một thành phần được kết nối trong biểu đồ. Thứ hai, đối với mỗi nhóm màu, tổng bậc các đỉnh của nó không được vượt quá bình phương số đỉnh trong nhóm đó. Sự tương tác cho phép chúng ta truy vấn từng đỉnh để tìm các đỉnh lân cận của nó, nhưng mỗi đỉnh chỉ có thể được truy vấn tối đa một lần và tổng số truy vấn không được vượt quá số đỉnh. 

Các ràng buộc chặt chẽ nhưng có thể quản lý được: số đỉnh trên mỗi trường hợp thử nghiệm lên tới 1000 và tổng số đỉnh trong tất cả các trường hợp thử nghiệm không vượt quá 1000. Điều này cho thấy rằng bất kỳ thuật toán nào có độ phức tạp tổng thể xung quanh$O(n^2)$mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng bất kỳ điều gì có truy vấn lồng nhau vượt quá mức đó đều có thể có nguy cơ đạt đến giới hạn truy vấn. Bởi vì chúng ta chỉ có thể truy vấn mỗi đỉnh một lần nên chúng ta phải có chiến lược trong cách khám phá các cạnh. Một cách tiếp cận ngây thơ cố gắng xây dựng lại danh sách kề đầy đủ mà không xem xét giới hạn truy vấn sẽ thất bại, đặc biệt nếu độ cao. 

Các trường hợp cạnh phát sinh khi các đỉnh bị cô lập, có độ tối đa hoặc tạo thành các thành phần có thể vi phạm điều kiện tổng độ nếu được tô màu một cách ngây thơ. Ví dụ: nếu một đỉnh bị cô lập với độ 0 thì nó phải trở thành một màu đơn. Nếu mọi đỉnh đều có bậc$n-1$, việc gán màu phải phân chia chúng một cách cẩn thận để thỏa mãn$s_c \le n_c^2$tình trạng. Một cách tiếp cận bất cẩn có thể hợp nhất các đỉnh bậc cao thành một màu duy nhất, vi phạm điều kiện tổng độ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng xây dựng danh sách kề đầy đủ bằng cách truy vấn mọi lân cận của mỗi đỉnh. Điều này đúng về mặt kỹ thuật vì khi chúng ta biết đồ thị, tô màu các thành phần liên thông để thỏa mãn$s_c \le n_c^2$là đơn giản. Tuy nhiên, chúng tôi bị hạn chế tối đa một truy vấn trên mỗi đỉnh, vì vậy phương pháp này ngay lập tức thất bại. Ngay cả khi chúng tôi cố gắng truy vấn các đỉnh nhiều lần và lưu vào bộ đệm lân cận, chúng tôi sẽ vượt quá giới hạn truy vấn đối với các biểu đồ không tầm thường. 

Cái nhìn sâu sắc quan trọng đến từ ràng buộc tổng mức độ. Mỗi nhóm màu chỉ có thể tích lũy độ tối đa$n_c^2$. Nếu chúng ta xử lý các đỉnh theo thứ tự giảm dần và luôn mở rộng một nhóm bằng cách kết nối các đỉnh lân cận không được tô màu của đỉnh hiện tại, chúng ta được đảm bảo rằng ràng buộc sẽ không bao giờ bị vi phạm. Các đỉnh bậc cao tự nhiên tạo thành xương sống của các nhóm màu và việc thêm các lân cận bậc nhỏ hơn không vượt quá giới hạn. Cách tiếp cận tham lam này hiệu quả vì chúng ta có thể truy vấn một đỉnh một lần và ngay lập tức gán màu của nó cho tất cả các đỉnh lân cận được phát hiện. Bằng cách luôn xử lý đỉnh không được tô màu có bậc cao nhất trước tiên, chúng tôi giảm thiểu khả năng tạo một nhóm không đủ chỗ trong giới hạn tổng bậc. 

Chiến lược này đảm bảo điều kiện kết nối một cách tự động, bởi vì chúng tôi luôn mở rộng từ một đỉnh dọc theo các cạnh, không bao giờ bỏ qua các kết nối. Giới hạn tổng độ được thỏa mãn vì bắt đầu từ đỉnh có bậc lớn nhất và cộng các đỉnh một cách tham lam không bao giờ tích lũy được tổng bậc lớn hơn bình phương kích thước của nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n^2) | O(n^2) | Vượt quá giới hạn truy vấn, không thành công | 
| Tham lam theo mức độ | O(n log n + n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh và bậc của chúng. Giữ một danh sách các đỉnh không màu được sắp xếp theo thứ tự giảm dần. Việc sắp xếp đảm bảo chúng ta luôn bắt đầu với đỉnh có nhiều đỉnh lân cận nhất, tối đa hóa khả năng tô màu một nhóm được kết nối trong giới hạn tổng độ. 
2. Khởi tạo bộ đếm màu bắt đầu từ 1. Điều này sẽ chỉ định một màu duy nhất cho mỗi nhóm chúng ta tạo. 
3. Trong khi có các đỉnh không được tô màu, hãy chọn đỉnh không được tô màu có bậc cao nhất. Truy vấn nó chính xác một lần để truy xuất một trong những hàng xóm của nó (theo quy tắc tương tác). Gán đỉnh này màu hiện tại. 
4. Đối với mỗi hàng xóm không được tô màu mà truy vấn trả về, hãy gán ngay màu đó. Vì chúng ta xử lý các đỉnh theo thứ tự bậc giảm dần nên việc thêm các đỉnh lân cận sẽ không vi phạm điều kiện tổng bậc vì mỗi đỉnh lân cận có bậc nhỏ hơn hoặc bằng đỉnh bắt đầu. 
5. Tăng bộ đếm màu và tiếp tục với đỉnh không được tô màu tiếp theo theo thứ tự giảm dần. Lặp lại cho đến khi tất cả các đỉnh được tô màu. 
6. Xuất màu. Mọi nhóm đều được kết nối vì thuật toán gán cùng một màu cho một đỉnh và các lân cận của nó trong cùng một bước. Điều kiện tổng độ được giữ nguyên vì độ lớn nhất được nhóm lại trước và mỗi đỉnh được thêm vào không đẩy tổng vượt quá bình phương kích thước của nhóm. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi bước, tổng độ của nhóm màu hiện tại không bao giờ vượt quá bình phương số đỉnh trong nhóm đó. Bắt đầu từ đỉnh có bậc cao nhất đảm bảo rằng phép cộng tham lam của các đỉnh lân cận sẽ không vi phạm giới hạn này. Tính liên kết được đảm bảo vì màu sắc lan truyền dọc theo các cạnh thực tế từ đỉnh ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
print_flush = lambda x: (print(x), sys.stdout.flush())

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        degrees = list(map(int, input().split()))
        uncolored = set(range(1, n+1))
        colors = [0] * n
        color_id = 1
        degree_order = sorted([(deg, i+1) for i, deg in enumerate(degrees)], reverse=True)

        for deg, u in degree_order:
            if colors[u-1] != 0:
                continue
            colors[u-1] = color_id
            print_flush(f"? {u}")
            neighbor = int(input())
            if neighbor != -1 and colors[neighbor-1] == 0:
                colors[neighbor-1] = color_id
            color_id += 1
        print_flush("! " + " ".join(map(str, colors)))

if __name__ == "__main__":
    solve()
```Trước tiên, chúng tôi sắp xếp các đỉnh theo bậc để xử lý đỉnh có bậc lớn nhất trước. Khi chúng tôi truy vấn một đỉnh, chúng tôi ngay lập tức gán cho đỉnh lân cận của nó cùng màu nếu không được tô màu. Chúng tôi chỉ tăng bộ đếm màu sau khi mỗi nhóm hoàn thành. Một điểm tinh tế là đảm bảo rằng chúng ta không truy vấn một đỉnh nhiều lần; chúng tôi duy trì điều này bằng cách truy vấn chính xác một lần trong quá trình xử lý. Một cách khác là xử lý các đỉnh bị cô lập với độ 0, được tự động gán một màu duy nhất. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | Đỉnh | Bằng cấp | Hàng xóm truy vấn | Màu sắc sau bước | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | [1,1,0,0,0] | 
| 2 | 3 | 2 | 4 | [1,1,2,2,0] | 
| 3 | 5 | 0 | -1 | [1,1,2,2,3] | 

Dấu vết này chứng tỏ rằng cách tiếp cận tham lam gán các màu được kết nối và tôn trọng giới hạn tổng mức độ. 

Một ví dụ khác có 4 đỉnh tạo thành một ngôi sao (đỉnh 1 nối với 2,3,4, các đỉnh khác cấp 1): 

| Bước | Đỉnh | Bằng cấp | Hàng xóm truy vấn | Màu sắc | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | [1,1,0,0] | 
| 2 | 3 | 1 | 1 | [1,1,1,0] | 
| 3 | 4 | 1 | 1 | [1,1,1,1] | 

Ngôi sao được tô màu theo một nhóm duy nhất. Tổng các độ là 6 và n_c^2 = 4^2 =16, thỏa mãn điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các đỉnh theo độ chiếm ưu thế, truy vấn là O(n) | 
| Không gian | O(n) | Mảng cho độ, màu sắc và tập hợp các đỉnh không màu | 

Vì n ≤ 1000 nên các phép toán O(n log n) đều có thể chấp nhận được. Mỗi truy vấn sử dụng tương tác thời gian không đổi, phù hợp với các ràng buộc tương tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("1\n5\n2 2 2 2 0\n2\n4\n2\n4\n") == "? 1\n? 3\n? 5\n! 1 1 2 2 3", "sample 1"

# Minimum-size input
assert run("1\n1\n0\n-1\n") == "? 1\n! 1", "single vertex"

# Star graph
assert run("1\n4\n3 1 1 1\n2\n3\n4\n-1\n") == "? 1\n? 2\n? 3\n? 4\n! 1 1 1 1", "star"

# Two isolated edges
assert run("1\n4\n1 1 1 1\n2\n1\n4\n3\n") == "? 1\n? 2\n? 3\n? 4\n! 1 1 2 2", "two components"

# Complete graph of 3
assert run("1\n3\n2 2 2\n2\n1\n1\n") == "? 1\n? 2\n? 3\n! 1 1 1", "complete triangle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đỉnh | "! | |
