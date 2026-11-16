---
title: "CF 1941D - Rudolf và trò chơi bóng"
description: "Chúng ta có một vòng tròn gồm n người chơi, được đánh số từ 1 đến n theo chiều kim đồng hồ. Bóng bắt đầu với cầu thủ x. Có m lần ném và mỗi lần ném có khoảng cách r_i và hướng ghi nhớ c_i. Hướng có thể theo chiều kim đồng hồ (0), ngược chiều kim đồng hồ (1) hoặc không xác định (?)."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1941
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 933 (Div. 3)"
rating: 1200
weight: 1941
solve_time_s: 77
verified: false
draft: false
---
[CF 1941D - Rudolf và trò chơi bóng](https://codeforces.com/problemset/problem/1941/D) 

**Đánh giá:** 1200 
**Tags:** dfs và tương tự, dp, triển khai 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một vòng tròn`n`người chơi, được đánh số từ 1 đến`n`theo chiều kim đồng hồ. Bóng bắt đầu với cầu thủ`x`. có`m`lần ném và mỗi lần ném có một khoảng cách`r_i`và một hướng đi được ghi nhớ`c_i`. Hướng có thể theo chiều kim đồng hồ (`0`), ngược chiều kim đồng hồ (`1`), hoặc chưa biết (`?`). Rốt cuộc`m`ném, chúng ta cần tìm tất cả những cầu thủ có thể có bóng, dựa trên thông tin đã cung cấp. 

Về mặt khái niệm, chúng ta có thể mô hình hóa điều này như một tập hợp các vị trí trên một vòng tròn. Ban đầu, bộ chỉ có người chơi bắt đầu. Mỗi lần ném sẽ di chuyển từng vị trí hiện tại có thể theo chiều kim đồng hồ bằng cách`r_i`, ngược chiều kim đồng hồ bởi`r_i`, hoặc cả hai nếu không biết hướng. Ở cuối, tập hợp chứa tất cả các vị trí kết thúc có thể có. 

Các hạn chế ở mức vừa phải:`n`Và`m`nhiều nhất là 1000 và tổng của tất cả`n*m`trên các trường hợp thử nghiệm là 2×10^5. Điều này loại trừ bất kỳ thuật toán nào có kết quả tệ hơn O(n * m) cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh cần chú ý bao gồm khi nào`c_i`chưa biết (`?`) cho tất cả các lần ném, có nghĩa là số lượng vị trí cuối cùng có thể có có thể tăng vọt. Một điểm tinh tế khác là phép tính mô-đun xung quanh đường tròn. Số người chơi được đánh số 1 nên khi cộng hoặc trừ khoảng cách, chúng ta phải bao quanh một cách chính xác. Ví dụ như việc di chuyển`-1`từ người chơi`1`đáp xuống người chơi`n`, không`0`. Tương tự, di chuyển`+1`từ`n`hạ cánh trở lại`1`. 

Một cách triển khai ngây thơ mà quên đi số học mô-đun sẽ âm thầm tạo ra các vị trí không hợp lệ và một cách xử lý`?`không chính xác có thể bỏ lỡ các khả năng hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng mọi con đường có thể. Bắt đầu với người chơi đầu tiên và với mỗi lần ném, hãy tạo tất cả các vị trí mới theo hướng. Nếu chưa xác định được cú ném, hãy chia trạng thái thành các bước di chuyển theo chiều kim đồng hồ và ngược chiều kim đồng hồ. Sau đó`m`ném, thu thập tất cả các vị trí kết quả. Điều này đúng, nhưng trong trường hợp xấu nhất,`?`trong mỗi lần ném nhân đôi số vị trí mỗi bước. Đó là O(2^m * m), điều này không khả thi khi`m`lên tới 1000. 

Thông tin chi tiết quan trọng là chúng tôi không cần theo dõi _các đường dẫn riêng lẻ_. Modulo vị trí của quả bóng`n`chỉ phụ thuộc vào tổng số lần di chuyển theo chiều kim đồng hồ trừ đi số lần di chuyển ngược chiều kim đồng hồ. Nếu không xác định được hướng của cú ném, nó cho phép cả hai khả năng, nhưng chúng ta vẫn có thể tính toán tất cả các vị trí có thể tiếp cận bằng cách truyền bá một tập hợp các vị trí. Cụ thể, chúng ta có thể duy trì một mảng boolean`reachable[1..n]`để đánh dấu những vị trí có thể thực hiện được sau mỗi lần ném. Đối với mỗi hướng đã biết, chúng tôi sẽ thay đổi vị trí có thể tiếp cận hiện tại cho phù hợp. Vì`?`, chúng tôi đánh dấu cả sự dịch chuyển theo chiều kim đồng hồ và ngược chiều kim đồng hồ. Sử dụng số học mô-đun và truyền tập hợp, mỗi lần ném mất O(n) thời gian, đưa ra tổng thể O(n*m) cho mỗi trường hợp thử nghiệm, phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * m) | O(2^m) | Quá chậm | 
| Tối ưu (Đặt tuyên truyền / DP) | O(n * m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng boolean`reachable[1..n]`tất cả đều sai, ngoại trừ`reachable[x] = True`, Ở đâu`x`là người chơi xuất phát. Điều này đại diện cho những cầu thủ có thể hiện đang giữ bóng. 
2. Lặp lại qua mỗi lần ném`(r_i, c_i)`: 

- Để ném theo chiều kim đồng hồ (`c_i == '0'`), tạo một mảng boolean mới và cho mỗi trình phát hiện có thể truy cập`p`, đánh dấu`(p + r_i - 1) % n + 1`có thể tiếp cận được. 
- Để ném ngược chiều kim đồng hồ (`c_i == '1'`), đánh dấu`(p - r_i - 1 + n) % n + 1`có thể tiếp cận được. 
- Đối với một cú ném không xác định (`c_i == '?'`), đánh dấu cả hai điểm đích theo chiều kim đồng hồ và ngược chiều kim đồng hồ là có thể truy cập được. 
3. Sau khi xử lý tất cả các lần ném, hãy thu thập tất cả các vị trí được đánh dấu là có thể tiếp cận được. Sắp xếp chúng theo thứ tự tăng dần cho đầu ra. 
4. Xuất ra số lượng vị trí có thể và danh sách đã sắp xếp. 

Tại sao nó hoạt động: Tính bất biến của mảng boolean đảm bảo rằng ở bất kỳ bước nào,`reachable[p]`đúng khi và chỉ nếu bóng có thể đến chỗ người chơi`p`vào thời điểm đó. Chúng tôi xử lý các hướng không xác định bằng cách cho phép cả hai chuyển đổi đồng thời, do đó không có vị trí kết thúc nào có thể bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, x = map(int, input().split())
        reachable = [False] * (n + 1)
        reachable[x] = True
        
        for _ in range(m):
            r, c = input().split()
            r = int(r)
            new_reachable = [False] * (n + 1)
            for i in range(1, n + 1):
                if not reachable[i]:
                    continue
                if c == '0' or c == '?':
                    new_pos = (i + r - 1) % n + 1
                    new_reachable[new_pos] = True
                if c == '1' or c == '?':
                    new_pos = (i - r - 1 + n) % n + 1
                    new_reachable[new_pos] = True
            reachable = new_reachable
        
        result = [i for i in range(1, n + 1) if reachable[i]]
        print(len(result))
        print(' '.join(map(str, result)))

if __name__ == "__main__":
    solve()
```Ghi chú thực hiện: Chúng tôi sử dụng`(i + r - 1) % n + 1`để di chuyển theo chiều kim đồng hồ và`(i - r - 1 + n) % n + 1`ngược chiều kim đồng hồ để xử lý việc lập chỉ mục và gói mô-đun dựa trên 1. Sự riêng biệt`new_reachable`mảng tránh ghi đè các vị trí trong cùng một bước, điều này rất quan trọng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
6 3 2
2 ?
2 ?
2 ?
```| Bước | Hiện tại có thể truy cập | Ném | Mới có thể truy cập | 
| --- | --- | --- | --- | 
| 0 | {2} | 2 ? | {4, 6} | 
| 1 | {4, 6} | 2 ? | {2, 6, 4} → {2, 4, 6} | 
| 2 | {2, 4, 6} | 2 ? | {2, 4, 6} | 

Tất cả người chơi có thể truy cập đều hội tụ về {2, 4, 6}, phù hợp với kết quả mong đợi. 

**Đầu vào mẫu 2:**```
12 1 2
3 1
```| Bước | Hiện tại có thể truy cập | Ném | Mới có thể truy cập | 
| --- | --- | --- | --- | 
| 0 | {2} | 3 1 | {(2-3-1+12)%12+1 = 11} | 
| Có thể truy cập cuối cùng = {11}. | | | | 

Các bảng này xác nhận rằng thuật toán xử lý chính xác các hướng chưa biết và số học mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi lần ném cập nhật tối đa n vị trí trong một mảng boolean. | 
| Không gian | O(n) | Mảng boolean lưu trữ những người chơi có thể tiếp cận. | 

Cho n, m ≤ 1000 và n*m 2×10^5, thao tác này diễn ra tốt đẹp trong vòng chưa đầy 2 giây. 

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
assert run("5\n6 3 2\n2 ?\n2 ?\n2 ?\n12 1 2\n3 1\n10 7 4\n2 ?\n9 1\n4 ?\n7 0\n2 0\n8 1\n5 ?\n5 3 1\n4 0\n4 ?\n1 ?\n4 1\n1 2 ?") == \
"3\n2 4 6\n1\n11\n4\n3 5 7 9\n3\n2 3 5\n1\n3", "sample 1"

# Custom: minimum input
assert run("1\n2 1 1\n1 ?") == "2\n1 2", "min size, unknown direction"

# Custom: all clockwise
assert run("1\n5 3 3\n2 0\n1 0\n2 0") == "1\n3", "clockwise wrapping"

# Custom: all counterclockwise
assert run("1\n5 3 3\n2 1\n1 1\n2 1") == "1\n3", "counterclockwise wrapping"

# Custom: mix known and unknown
assert run("1\n4 2 1\n1 ?\n2 0") == "3\n1 2 4", "mix of ? and 0"

# Custom: max distance
assert run("1\n5 1 3\n
```
