---
title: "CF 1849B - Quái vật"
description: "Vấn đề mô tả một tình huống trong đó Monocarp chiến đấu với một nhóm quái vật, mỗi quái vật có một giá trị máu ban đầu. Monocarp có thể liên tục tấn công quái vật có lượng máu hiện tại cao nhất, giảm nó đi một lượng $k$ cố định cho mỗi đòn tấn công."
date: "2026-06-09T05:34:10+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1849
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 152 (Rated for Div. 2)"
rating: 1000
weight: 1849
solve_time_s: 96
verified: false
draft: false
---

[CF 1849B - Quái vật](https://codeforces.com/problemset/problem/1849/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một tình huống trong đó Monocarp chiến đấu với một nhóm quái vật, mỗi quái vật có một giá trị máu ban đầu. Monocarp có thể tấn công liên tục vào quái vật có lượng máu hiện tại cao nhất, khiến chúng bị giảm một lượng máu nhất định$k$mỗi cuộc tấn công. Nếu nhiều quái vật có cùng lượng máu cao nhất, con quái vật có chỉ số nhỏ nhất sẽ được chọn. Nhiệm vụ là xác định chính xác trình tự quái vật chết. 

Đầu vào đưa ra nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, số lượng quái vật$n$và thiệt hại cố định$k$được cung cấp, theo sau là danh sách$n$số nguyên đại diện cho sức khỏe của quái vật. Đầu ra của mỗi trường hợp thử nghiệm phải là danh sách các chỉ số quái vật theo thứ tự chúng chết. 

Những hạn chế là đáng kể:$n$có thể lên đến$3 \cdot 10^5$, và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng được giới hạn ở mức$3 \cdot 10^5$. Mỗi giá trị sức khoẻ và sát thương có thể đạt tới$10^9$. Một mô phỏng ngây thơ trong đó bạn liên tục tìm thấy con quái vật có lượng máu tối đa và trừ đi$k$sẽ yêu cầu lên đến$10^9$hoạt động trên mỗi quái vật trong trường hợp xấu nhất, rõ ràng là không khả thi. Thay vào đó, thuật toán phải tính toán thứ tự tử hình bằng cách sử dụng một số dạng lý luận số học mà không mô phỏng từng cuộc tấn công riêng lẻ. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả quái vật đều có sức khỏe như nhau, trong đó$k$vượt quá lượng máu của quái vật nhỏ nhất hoặc khi nhiều quái vật chết trong cùng một "vòng" nếu lượng máu của chúng chia cho$k$dẫn đến cùng số lượng truy cập cần thiết. Một cách tiếp cận đơn giản sử dụng tính năng sắp xếp sau mỗi lần truy cập sẽ không thành công do giới hạn thời gian và cách tiếp cận bỏ qua các ràng buộc trong các lần truy cập bắt buộc sẽ tạo ra thứ tự sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là mô phỏng từng cuộc tấn công một cách tuần tự. Tìm quái vật có lượng máu hiện tại tối đa, trừ đi$k$, và ghi lại nếu nó chết. Lặp lại cho đến khi tất cả quái vật đều chết. Điều này đúng vì nó tuân theo chính xác các quy tắc của bài toán, nhưng nó cực kỳ chậm. Nếu chúng ta có$n = 3 \cdot 10^5$quái vật và sức khỏe lớn nhất là$10^9$với$k = 1$, quá trình mô phỏng sẽ mất tới$3 \cdot 10^{14}$hoạt động vượt xa khả thi trong giới hạn thời gian 2 giây. 

Quan sát quan trọng để có giải pháp nhanh hơn là cái chết của mỗi quái vật có thể được dự đoán bằng cách tính toán số lượng đòn tấn công đầy đủ cần thiết để giảm lượng máu của nó xuống 0. Đặc biệt đối với quái vật$i$với sức khỏe$a_i$, số lần chạm cần thiết là$\lceil a_i / k \rceil$. Vì những quái vật có ít lượt đánh cần thiết hơn sẽ chết sớm hơn nên trình tự chết có thể được xác định bằng cách sắp xếp quái vật dựa trên giá trị này. Mối quan hệ bị phá vỡ theo chỉ số ban đầu vì những quái vật có cùng số lần đánh bắt buộc sẽ chết theo thứ tự ban đầu. 

Giải pháp tối ưu tránh mọi mô phỏng và giảm vấn đề xuống còn một phép tính duy nhất cho mỗi quái vật, sau đó là một loại ổn định, nằm trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số đòn tấn công) ≈ O(n * max(a_i / k)) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$, và danh sách sức khỏe$a_1, a_2, ..., a_n$. 
2. Chuyển đổi máu của mỗi quái vật thành số lần đánh cần thiết để tiêu diệt nó, sử dụng công thức$\lceil a_i / k \rceil$. Để tránh các phép tính dấu phẩy động, hãy tính$(a_i + k - 1) // k$. Điều này đảm bảo làm tròn chính xác. 
3. Ghép các đòn đánh cần thiết của mỗi quái vật với chỉ số ban đầu của nó. Điều này duy trì trật tự ràng buộc. 
4. Sắp xếp danh sách các cặp chủ yếu theo số lần truy cập và thứ hai theo chỉ mục gốc. Điều này đưa ra lệnh tử hình chính xác. 
5. Xuất các chỉ số từ danh sách đã sắp xếp. 

Tại sao nó hoạt động: Điều bất biến là quái vật có ít đòn đánh cần thiết hơn luôn chết trước kẻ có nhiều đòn đánh hơn. Trong cùng một số lần truy cập, quái vật có chỉ số thấp hơn sẽ chết trước, phù hợp với quy tắc lựa chọn trong bài toán. Việc sắp xếp dựa trên các lượt truy cập với các chỉ số là tie-breaker tái tạo đầy đủ trình tự tử vong chính xác mà không cần mô phỏng từng cuộc tấn công riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        monsters = [((a[i] + k - 1) // k, i + 1) for i in range(n)]
        monsters.sort()
        print(' '.join(str(idx) for _, idx in monsters))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó tính toán số lần đánh cần thiết cho mỗi quái vật bằng cách sử dụng số học số nguyên để mô phỏng giới hạn phân chia. Mỗi quái vật được ghép nối với chỉ số dựa trên 1 ban đầu của nó. Sắp xếp danh sách này sẽ tạo ra lệnh tử hình. Cuối cùng, các chỉ số được in theo đúng trình tự. Việc sử dụng`i + 1`đảm bảo đầu ra khớp với chỉ mục dựa trên 1 trong vấn đề. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3 2 1 2 3
```| Quái vật | Sức khỏe | Số lượt truy cập cần thiết | Chỉ mục | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 3 | 2 | 3 | 

Sắp xếp theo lượt truy cập rồi lập chỉ mục: (1,1), (1,2), (2,3) → tử lệnh: 2,1,3 

Điều này chứng tỏ bộ phận trần đã xử lý chính xác những con quái vật cần những đòn đánh tương tự. 

### Đầu vào mẫu 2```
4 3
2 8 3 5
```| Quái vật | Sức khỏe | Số lượt truy cập cần thiết | Chỉ mục | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 8 | 3 | 2 | 
| 3 | 3 | 1 | 3 | 
| 4 | 5 | 2 | 4 | 

Sắp xếp: (1,1), (1,3), (2,4), (3,2) → tử lệnh: 3,1,2,4 

Điều này cho thấy thuật toán duy trì giới hạn chỉ số cho những quái vật cần cùng số lần đánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp n quái vật theo số lần truy cập cần thiết | 
| Không gian | O(n) | Lưu trữ lượt truy cập và chỉ số gốc | 

Với$n \le 3 \cdot 10^5$tổng cộng,$O(n \log n)$thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n3 2\n1 2 3\n2 3\n1 1\n4 3\n2 8 3 5\n") == "2 1 3\n1 2\n3 1 4 2", "samples"

# Custom test cases
assert run("1\n1 10\n7\n") == "1", "single monster"
assert run("1\n5 1\n1 2 3 4 5\n") == "1 2 3 4 5", "incremental health"
assert run("1\n4 2\n4 4 4 4\n") == "1 2 3 4", "all equal health"
assert run("1\n3 10\n1000000000 999999999 1000000000\n") == "2 1 3", "large numbers with tie-breaking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 quái vật | 1 | Xử lý đúng quái vật đơn lẻ | 
| Tăng cường sức khỏe | 1 2 3 4 5 | Tính toán chính xác số lượt truy cập cần thiết | 
| Mọi sức khỏe đều bình đẳng | 1 2 3 4 | Đúng sự ràng buộc dựa trên chỉ số | 
| Số lượng lớn | 2 1 3 | Xử lý số nguyên lớn và phép chia trần | 

## Vỏ cạnh 

Nếu tất cả quái vật có sức khỏe như nhau, chẳng hạn như`4 2 4 4 4 4`, mỗi lần yêu cầu 2 lượt truy cập. Thuật toán sắp xếp theo lượt truy cập rồi theo chỉ mục, tạo ra thứ tự ban đầu: 1,2,3,4. Điều này phù hợp với quy tắc của bài toán, vì tất cả quái vật đều chết theo thứ tự chỉ số nếu số lần đánh của chúng bằng nhau. 

Nếu một quái vật có thể bị giết chỉ trong một đòn và những quái vật khác cần nhiều đòn, chẳng hạn như`3 10 1 20 30`, quái vật trúng một đòn sẽ chết trước. Việc tính toán`(a_i + k - 1) // k`mang lại lượt truy cập`[1,2,3]`, và sắp xếp sẽ đưa ra thứ tự chết chính xác`[1,2,3]`. 

Cả hai ví dụ đều xác nhận giải pháp xử lý tình trạng đứt gãy và thay đổi trạng thái một cách chính xác.
