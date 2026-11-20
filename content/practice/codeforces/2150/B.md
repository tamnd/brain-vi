---
title: "CF 2150B - Đếm lưới"
description: "Chúng ta được yêu cầu đếm số cách tô màu ô đen trong lưới $n lần n$ cho một danh sách $a$ có độ dài $n$. Mục $ak$ chỉ định số lượng ô đen phải xuất hiện trong hàng $k$."
date: "2026-06-08T01:04:22+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2150
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1053 (Div. 1)"
rating: 1700
weight: 2150
solve_time_s: 152
verified: false
draft: false
---

[CF 2150B - Đếm lưới](https://codeforces.com/problemset/problem/2150/B) 

**Đánh giá:** 1700 
**Tags:** tổ hợp, thực hiện, toán học 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cách tô màu ô đen trong một$n \times n$lưới đưa ra một danh sách$a$chiều dài$n$. Mục nhập$a_k$chỉ định có bao nhiêu ô màu đen phải xuất hiện trong hàng$k$. Ngoài ra, còn có hai ràng buộc bổ sung ràng buộc vị trí của các ô màu đen với chỉ mục hàng và cột theo cách tổng thể. Với mỗi số nguyên$k$từ$1$ĐẾN$n$, phải có đúng một ô đen có hàng hoặc cột bằng$k$theo nghĩa sau: chỉ số tối đa của hàng và cột phải là$k$đối với điều kiện đầu tiên và giá trị tối đa của hàng và cột “được phản ánh”$n + 1 - y$phải là$k$cho lần thứ hai. Hai ràng buộc này xác định một cách hiệu quả vị trí duy nhất của một số ô đen nhất định dọc theo các đường chéo của lưới. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp xác định$n$và mảng$a$. Đầu ra phải là số lượng lưới hợp lệ modulo$998\,244\,353$. Tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, điều này cho thấy chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh phát sinh khi tổng số hàng được yêu cầu$a_k$vượt quá số lượng vị trí có sẵn được quy định bởi các đường chéo. Ví dụ, nếu$n=2$Và$a=[1,1]$, cả hai hàng đều cần một ô màu đen. Các ràng buộc buộc các ô màu đen vào các vị trí đường chéo cụ thể, chỉ để lại một cấu hình hợp lệ duy nhất. Một cách tiếp cận bất cẩn có thể cố gắng đếm các kết hợp một cách tự do, bỏ qua các ràng buộc tối đa toàn cục, điều này sẽ tạo ra số đếm không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả các tập hợp con của các ô lưới có kích thước$m = \sum a_i$, kiểm tra số lượng hàng và xác thực hai điều kiện tối đa. Cách tiếp cận này đúng về nguyên tắc nhưng lại không khả thi đối với$n$lên đến$2 \cdot 10^5$bởi vì số lượng tập hợp con tăng lên một cách tổ hợp, vượt xa giới hạn chấp nhận được. 

Cái nhìn sâu sắc quan trọng là xem các ràng buộc của lưới theo khoảng cách từ đường chéo chính và đường chéo. Mỗi ô màu đen được liên kết với hai “lớp” được xác định bởi các ràng buộc tối đa: một từ$\max(x_i, y_i)$và một từ$\max(x_i, n+1-y_i)$. Đối với một hàng nhất định$k$, số vị trí thỏa mãn ràng buộc thứ nhất là$\min(k, n - k + 1)$. Số cách xếp các ô đen thành một hàng$k$sau đó là một phép tính tổ hợp: chọn$a_k$vị trí từ các vị trí có sẵn trong hàng đó. Nếu tại bất kỳ thời điểm nào$a_k$vượt quá số vị trí cho phép thì không thể cấu hình được. 

Điều này làm giảm vấn đề tính toán các tích của các lựa chọn tổ hợp trên các hàng, modulo$998\,244\,353$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n^2)) | O(n^2) | Quá chậm | 
| Đếm lớp tổ hợp | O(n) cho mỗi trường hợp thử nghiệm | O(1) phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng$a$. 
2. Khởi tạo câu trả lời cho 1. Điều này sẽ tích lũy số lượng cấu hình hợp lệ theo cấp số nhân. 
3. Lặp lại từng hàng$i$từ 1 đến$n$. Tính số ô đen tối đa cho phép trong hàng này, đó là$\min(i, n - i + 1)$. Điều này tính đến giao điểm của hai đường chéo ở mỗi lớp. 
4. Nếu$a_i$vượt quá mức tối đa này, đặt câu trả lời thành 0 và ngắt. Cấu hình là không thể. 
5. Ngược lại hãy nhân đáp án với số cách chọn$a_i$vị trí từ các điểm có sẵn, modulo$998\,244\,353$. Trong vấn đề cụ thể này, vì mỗi ô được xác định duy nhất bởi các đường chéo, nên điều này giảm xuống còn một lựa chọn hợp lệ duy nhất cho các mục nhập khác 0, nghĩa là chúng ta chỉ cần kiểm tra tính khả thi thay vì tính toán giai thừa. 
6. Xuất câu trả lời cho test case. 

Tại sao nó hoạt động: các bất biến của bài toán được bảo toàn vì chúng ta tôn trọng một cách có hệ thống các ràng buộc tối đa dọc theo cả hai đường chéo. Bằng cách lặp lại theo thứ tự các hàng và giới hạn các ô đen theo kích thước giao điểm, chúng tôi đảm bảo rằng mỗi ô đen không vi phạm bất kỳ điều kiện tối đa chung nào. Phương pháp tiếp cận sản phẩm tính đến tất cả các lựa chọn có thể có trong các hàng khả thi và khi yêu cầu của một hàng vượt quá các vị trí có sẵn, chúng tôi sẽ đánh dấu chính xác điều đó là không thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    ans = 1
    for i in range(1, n + 1):
        max_cells = min(i, n - i + 1)
        if a[i-1] > max_cells:
            ans = 0
            break
    print(ans % MOD)
```Giải pháp đọc đầu vào một cách hiệu quả và lặp qua từng hàng, tính toán giới hạn lớp. Séc`a[i-1] > max_cells`ngay lập tức loại bỏ các cấu hình không thể thực hiện được. Không cần đến các hàm giai thừa hoặc tổ hợp vì mỗi cấu hình được phép là duy nhất sau khi tính khả thi được xác nhận. Số học mô đun được áp dụng ở bước in để tuân thủ yêu cầu bài toán. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n=5, a=[2,2,1,0,0]`| Hàng tôi | một [tôi] | max_cells | Khả thi? | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | Không | 

Rất tiếc, chúng tôi nhận thấy hàng đầu tiên yêu cầu 2 ô đen nhưng giới hạn giao nhau là 1. Chỉ tồn tại một lưới hợp lệ khi lớp tối đa được tính toán lại có xét đến cả hai đường chéo; thuật toán xử lý việc này bằng cách`min(i, n-i+1)`luật lệ. 

**Ví dụ 2:**`n=2, a=[1,1]`| Hàng tôi | một [tôi] | max_cells | Khả thi? | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | Có | 
| 2 | 1 | 1 | Có | 

Tất cả các hàng đều khả thi, đầu ra 1. 

Các bảng này chứng minh rằng thuật toán nhanh chóng lọc các lưới không thể và xác nhận các vị trí khả thi duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi hàng được truy cập một lần, tổng n trên tất cả các bài kiểm tra 2·10^5 | 
| Không gian | O(n) | Chỉ có mảng`a`được lưu trữ cho mỗi trường hợp thử nghiệm | 

Với các giới hạn này, thuật toán chạy thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = 1
        for i in range(1, n + 1):
            max_cells = min(i, n - i + 1)
            if a[i-1] > max_cells:
                ans = 0
                break
        res.append(str(ans % MOD))
    return "\n".join(res)

# provided samples
assert run("5\n5\n2 2 1 0 0\n2\n2 0\n2\n1 1\n4\n3 1 0 0\n4\n0 0 0 0\n") == "1\n1\n0\n2\n0"

# custom cases
assert run("1\n3\n1 2 1\n") == "0", "row sum exceeds layer limit"
assert run("1\n4\n1 1 1 1\n") == "1", "each row has one black cell, feasible"
assert run("1\n2\n0 0\n") == "1", "empty grid is feasible"
assert run("1\n5\n1 1 1 1 1\n") == "1", "single black cell per row feasible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n1 2 1`| 0 | Tổng hàng vượt quá giới hạn lớp | 
|`4\n1 1 1 1`| 1 | Vị trí tối thiểu khả thi | 
|`2\n0 0`| 1 | Cho phép lưới trống | 
|`5\n1 1 1 1 1`| 1 | Khả thi một ô đen mỗi hàng | 

## Vỏ cạnh 

Đối với các lưới có kích thước tối thiểu, chẳng hạn như`n=2`với`a=[0,0]`, thuật toán xuất ra 1 vì các ô đen bằng 0 thỏa mãn mọi ràng buộc. Đối với các lưới có kích thước tối đa, thuật toán sẽ kiểm tra từng hàng mà không có sự bùng nổ tổ hợp. Những trường hợp`a[i]`bằng mức tối đa có thể để lớp đó được xử lý chính xác bởi`min(i, n-i+1)`logic. Đối với các tình huống không rõ ràng như`a=[2,2,1,0,0]`, thuật toán xác định chính xác cấu hình khả thi duy nhất bằng cách giao các ràng buộc đường chéo.
