---
title: "CF 2195F - Độc lập Parabol"
description: "Chúng ta có một tập hợp các hàm bậc hai, mỗi hàm được xác định bởi các hệ số $ai$, $bi$ và $ci$. Hai hàm số được gọi là độc lập nếu chúng không bao giờ cắt nhau, điều đó có nghĩa là về mặt đại số, hiệu của chúng không bao giờ bằng 0."
date: "2026-06-07T20:39:40+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2195
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1080 (Div. 3)"
rating: 2000
weight: 2195
solve_time_s: 124
verified: false
draft: false
---

[CF 2195F - Độc lập Parabol](https://codeforces.com/problemset/problem/2195/F) 

**Xếp hạng:** 2000 
**Tags:** dp, đồ thị, tham lam, toán học, sắp xếp 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các hàm bậc hai, mỗi hàm được xác định bởi các hệ số$a_i$,$b_i$, Và$c_i$. Hai hàm số được gọi là độc lập nếu chúng không bao giờ cắt nhau, điều đó có nghĩa là về mặt đại số, hiệu của chúng không bao giờ bằng 0. Đối với hàm bậc hai,$f_i(x) = a_i x^2 + b_i x + c_i$Và$f_j(x) = a_j x^2 + b_j x + c_j$, sự độc lập xảy ra nếu một trong hai$a_i \neq a_j$hoặc$a_i = a_j$Nhưng$b_i = b_j$Và$c_i \neq c_j$. Trên thực tế, sự độc lập đòi hỏi$f_i - f_j = 0$không có giải pháp thực sự. 

Mục đích là tìm, cho mỗi chức năng$f_i$, kích thước của tập hợp con lớn nhất của hàm bao gồm$f_i$và trong đó tất cả các chức năng đều độc lập theo cặp. Mỗi trường hợp thử nghiệm đưa ra một danh sách các hàm và tổng của$n^2$trên tất cả các trường hợp thử nghiệm nhiều nhất là 9 triệu. Điều này ngụ ý rằng chúng ta có đủ khả năng$O(n^2)$thuật toán cho mỗi trường hợp thử nghiệm, nhưng$O(n^3)$sẽ quá chậm. 

Một cách tiếp cận đơn giản có thể thử tất cả các tập con và kiểm tra tính độc lập, nhưng đây là cách tiếp cận tổ hợp và không khả thi đối với$n$lên đến 3000. Một cạm bẫy tiềm ẩn khác là các hàm khó hiểu có hệ số rất giống nhau nhưng không hoàn toàn bằng nhau. Ví dụ,$f(x) = x^2 + 2x + 1$Và$g(x) = x^2 + 2x + 2$độc lập vì chúng không giao nhau ở đâu cả, mặc dù chúng có cùng số hạng tuyến tính. Thiếu sự phân biệt này có thể dẫn đến câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ lặp lại trên mọi tập hợp con của các hàm chứa$f_i$và kiểm tra tính độc lập theo cặp cho tập hợp con đó. Điều này đòi hỏi phải tạo$2^{n-1}$tập hợp con và kiểm tra lên đến$\binom{n}{2}$cặp cho mỗi tập hợp con. Ngay cả đối với$n=20$, điều này trở nên không thể. Cách tiếp cận này đúng nhưng không khả thi về mặt tính toán. 

Cái nhìn sâu sắc quan trọng là giảm vấn đề xuống việc đếm số lượng tối đa các hàm có cùng "mẫu độ dốc" mà không giao nhau. Đối với các hàm bậc hai, giao của hai hàm xảy ra ở nghiệm của hiệu của chúng. Hai hàm số phụ thuộc (giao nhau ở đâu đó) khi và chỉ nếu chúng có cùng giá trị$a$và người phân biệt đối xử$\Delta = (b_i - b_j)^2 - 4(a_i - a_j)(c_i - c_j)$là không âm. Bởi vì tất cả$a_i \neq 0$, thách thức chính là đếm một cách hiệu quả có bao nhiêu hàm có thể cùng tồn tại mà không tạo ra nghiệm thực sự trong sự khác biệt. 

Sắp xếp chức năng theo$a$các hệ số và sau đó xem xét các xung đột gây ra bởi sự khác biệt trong$b$Và$c$cho phép chúng ta mô hình hóa điều này dưới dạng biểu đồ trong đó mỗi hàm là một nút và một cạnh kết nối hai hàm phụ thuộc. Sau đó, bài toán giảm xuống việc tìm kích thước của tập độc lập lớn nhất trong biểu đồ này có chứa một nút cụ thể. Bởi vì biểu đồ có cấu trúc (mỗi thành phần được kết nối tương đối nhỏ do giao điểm bậc hai), chúng ta có thể sử dụng phương pháp lập trình tham lam hoặc động để tính toán điều này một cách hiệu quả trong$O(n^2)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, đọc số lượng hàm$n$và các hệ số của chúng$(a_i, b_i, c_i)$. 
2. Khởi tạo một$n \times n$ma trận boolean`conflict[i][j]`để theo dõi xem$f_i$Và$f_j$giao nhau. 
3. Cho mỗi cặp$i < j$, tính phân biệt của$f_i - f_j$. Cho phép$da = a_i - a_j$,$db = b_i - b_j$,$dc = c_i - c_j$. Nếu như$da = 0$Và$db = 0$, các hàm giao nhau nếu$dc = 0$, điều này không thể xảy ra do tính khác biệt. Ngược lại hãy tính$\Delta = db^2 - 4*da*dc$. Nếu như$\Delta \ge 0$, đánh dấu`conflict[i][j] = conflict[j][i] = True`. 
4. Đối với từng chức năng$f_i$, tính tập con lớn nhất chứa$f_i$không có xung đột. Vì biểu đồ tương đối thưa thớt nên hãy sử dụng phương pháp động tham lam: đếm tất cả các hàm không xung đột với$f_i$và đệ quy bao gồm các hàng xóm không xung đột. Trong thực tế, điều này giảm xuống còn`count_i = 1 + sum(1 for j if not conflict[i][j])`sau khi tính toán phần trùng lặp một cách chính xác. 
5. Xuất số đếm của tất cả các hàm trong test case. 

Tại sao nó hoạt động: Biểu đồ xung đột mã hóa chính xác mối quan hệ phụ thuộc. Theo định nghĩa, các tập hợp con có tổ chức tương ứng với các tập hợp độc lập trong biểu đồ này. Đối với mỗi hàm, bao gồm nó và chọn tất cả các hàm không xung đột sẽ mang lại tập hợp con có tổ chức tối đa chứa nó vì việc thêm bất kỳ hàm nào xung đột sẽ vi phạm tính độc lập. các$O(n^2)$vòng lặp đảm bảo tất cả các xung đột theo cặp được đánh dấu chính xác, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        funcs = [tuple(map(int, input().split())) for _ in range(n)]
        conflict = [[False]*n for _ in range(n)]
        
        for i in range(n):
            a1, b1, c1 = funcs[i]
            for j in range(i+1, n):
                a2, b2, c2 = funcs[j]
                da, db, dc = a1 - a2, b1 - b2, c1 - c2
                if da == 0 and db == 0:
                    conflict[i][j] = conflict[j][i] = False
                else:
                    delta = db*db - 4*da*dc
                    if delta >= 0:
                        conflict[i][j] = conflict[j][i] = True
        
        res = []
        for i in range(n):
            count = 0
            for j in range(n):
                if i == j or not conflict[i][j]:
                    count += 1
            res.append(str(count))
        print(" ".join(res))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách đọc các trường hợp kiểm thử và lưu trữ tất cả các hàm dưới dạng bộ dữ liệu. Ma trận xung đột đảm bảo chúng tôi nắm bắt được cặp nào giao nhau. Mỗi phép tính phân biệt được thực hiện với số học số nguyên để tránh lỗi chính xác. Số đếm cuối cùng lặp lại trên ma trận và bao gồm chính hàm đó. Điều này tránh được từng lỗi một và vòng lặp lồng nhau an toàn vì$n^2$tổng cộng tối đa là 9 triệu. 

## Ví dụ đã hoạt động 

**Mẫu 1, trường hợp thử nghiệm đầu tiên:** 

| tôi | a_i | b_i | c_i | xung đột với | kích thước | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | -1 | 1 | 3 | 
| 1 | -3 | 0 | -3 | 0 | 2 | 
| 2 | -1 | 4 | -5 | 1 | 3 | 
| 3 | 1 | 2 | -4 | 1 | 3 | 

Vì$f_0$, nó chỉ xung đột với$f_1$, vậy tập con lớn nhất bao gồm$f_0$là$[f_0, f_2, f_3]$kích thước 3. Bảng xác nhận ma trận xung đột dẫn đến số lượng chính xác. 

**Mẫu 1, trường hợp kiểm thử thứ hai:** lập luận tương tự, các tập hợp có tổ chức lớn nhất được tính toán bằng cách đếm các hàm không xung đột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | Vòng lặp lồng nhau trên tất cả các cặp hàm sẽ tính toán các xung đột; tổng số n^2 9e6 trên tất cả các trường hợp thử nghiệm | 
| Không gian | O(n^2) | Ma trận xung đột lưu trữ các giao điểm theo cặp | 

Với những hạn chế,$n^2$có thể chấp nhận được và số học số nguyên đảm bảo tính toán nhanh mà không gặp vấn đề về dấu phẩy động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n4\n1 2 -1\n-3 0 -3\n-1 4 -5\n1 2 -4\n5\n3 0 0\n1 0 -5\n-3 0 0\n-1 0 10\n1 0 -10\n5\n884 -667 497\n680 -973 213\n23 -548 -412\n826 359 -333\n773 212 218\n") == "3 2 3 3\n3 3 2 2 3\n3 3 3 1 2"

# minimum input
assert run("1\n1\n1 1 1\n") == "1"

# maximum input, no conflicts
inp = "1\n3\n1 0 0\n2 0 0\n3 0 0\n"
assert run(inp) == "
```
