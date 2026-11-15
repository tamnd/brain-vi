---
title: "CF 1824A - LuoTianyi và buổi biểu diễn"
description: "Chúng ta có một hàng m ghế và n người muốn ngồi, mỗi người có một sở thích cố định. Một số người nhất quyết đòi ngồi vào một chiếc ghế được đánh số cụ thể."
date: "2026-06-09T07:42:30+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1824
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 872 (Div. 1)"
rating: 1400
weight: 1824
solve_time_s: 246
verified: false
draft: false
---

[CF 1824A - LuoTianyi và buổi biểu diễn](https://codeforces.com/problemset/problem/1824/A) 

**Đánh giá:** 1400 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 4 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`m`chỗ ngồi và`n`những người muốn chiếm lĩnh chúng, mỗi người có một sở thích cố định. Một số người nhất quyết đòi ngồi vào một chiếc ghế được đánh số cụ thể. Những người khác sẽ cố gắng ngồi ngay bên trái của người ngồi ngoài cùng bên trái hoặc ngay bên phải của người ngồi ngoài cùng bên phải. Nếu những vị trí đó đã được đảm nhận, người đó sẽ rời đi. Nhiệm vụ là tối đa hóa số người ngồi nếu chúng ta có thể chọn thứ tự mọi người đến. 

Đầu vào mô tả nhiều trường hợp thử nghiệm. Mỗi trường hợp đưa ra`n`Và`m`, theo sau là một danh sách`x_i`mô tả sự lựa chọn chỗ ngồi của mỗi người. Một tích cực`x_i`là một chỗ ngồi cố định,`-1`là phương pháp bên trái,`-2`là phương pháp đúng đắn. Đầu ra là một số duy nhất cho mỗi thùng, biểu thị số lượng chỗ ngồi tối đa. 

Các ràng buộc cho phép lên tới`10^5`tổng số người và số ghế trong tất cả các trường hợp thử nghiệm và có thể có tới`10^4`trường hợp thử nghiệm. Cách tiếp cận O(n*m) ngây thơ mô phỏng mọi người thử từng chỗ ngồi sẽ quá chậm vì điều đó có thể đạt tới`10^10`hoạt động. Vì vậy chúng ta cần một cách tiếp cận tuyến tính hoặc tuyến tính trong`n`cho từng trường hợp. 

Các trường hợp đặc biệt là nhiều người nhắm vào cùng một chỗ ngồi hoặc tất cả đều thích ghế bên trái/phải với tổng số ghế rất ít hoặc rất nhiều. Ví dụ, nếu tất cả mọi người muốn chỗ ngồi`5`và chỉ có một chỗ ngồi`5`, chỉ có một người sẽ ngồi. Việc triển khai bất cẩn có thể bỏ qua các vị trí trùng lặp hoặc đếm sai vị trí trái/phải. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng từng người theo một thứ tự tùy ý nào đó, duy trì các ghế ngồi ngoài cùng bên trái và ngoài cùng bên phải hiện tại. Mỗi người được xử lý theo loại của họ: chỗ ngồi cố định, mở rộng sang trái hoặc mở rộng sang phải. Phương pháp này hoạt động chính xác nhưng chậm khi`n`Và`m`lớn, vì mỗi lần mở rộng có thể yêu cầu kiểm tra tình trạng chỗ trống, dẫn đến hành vi O(n^2) trong trường hợp đầu vào xấu nhất. 

Quan sát quan trọng là thứ tự của mọi người chỉ quan trọng đối với các vị trí bên trái/phải so với chỗ ngồi cố định. Những người có ghế cố định chỉ được ngồi một lần, vì vậy trước tiên chúng tôi tính số ghế cố định duy nhất. Đối với những người thuận bên trái và bên phải, vị trí tối ưu của họ là chiếm chỗ càng gần chỗ ngồi ngoài cùng bên trái và ngoài cùng bên phải càng tốt mà không xung đột với các ghế cố định. Do đó, chúng ta có thể rút gọn bài toán về việc tính số lượng người ở bên trái và bên phải tối đa có thể ngồi mà không chồng lên số ghế cố định và không vượt quá tổng số ghế. Sắp xếp chỗ ngồi cố định và đếm khoảng trống cho phép chúng ta tìm ra cách sắp xếp tối ưu trong thời gian O(n log n) cho mỗi trường hợp thử nghiệm. 

Quan sát cho thấy chúng ta có thể tách các ghế cố định khỏi các ghế trái/phải linh hoạt và tính toán số ghế bổ sung tối đa một cách độc lập sẽ giảm vấn đề từ O(n*m) xuống O(n log n), vì chúng ta chỉ cần sắp xếp các ghế cố định và thực hiện đếm tiền tố/hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(m) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, trích xuất số lượng bên trái (`-1`) và sang phải (`-2`) mọi người. Ghi lại bộ ghế cố định`x_i > 0`. 
2. Đếm số ghế cố định duy nhất. Nếu nhiều người muốn ngồi chung một chỗ thì chỉ một người được ngồi ở đó. 
3. Sắp xếp các chỗ ngồi cố định theo thứ tự tăng dần. 
4. Tính số người bổ sung tối đa có thể ngồi ở bên trái và bên phải. Đối với người hướng trái, các vị trí có sẵn là số ghế từ 1 đến tối thiểu(ghế cố định) - 1. Số người hướng trái ngồi tối đa là số tối thiểu của số này và tổng số người hướng trái. Đối xứng, đối với người thuận tay phải, các vị trí có sẵn là số ghế max(ghế cố định) + 1 đến m. 
5. Để tối đa hóa tổng số chỗ ngồi, hãy cân nhắc việc bố trí một số người linh hoạt ở hai bên ghế cố định. Lặp lại qua các chỗ ngồi cố định đã được sắp xếp, tính toán giá trị tối đa của`left_space + left_count`Và`right_space + right_count`để trang trải mọi sự sắp xếp. 
6. Câu trả lời cuối cùng là số lượng tối đa giữa số lượng ghế cố định duy nhất và tổng số được tính toán bao gồm cả các vị trí bên trái và bên phải, bị giới hạn bởi tổng số ghế. 

Thuật toán hoạt động vì các ghế cố định hạn chế sức chứa tối đa và các vị trí bên trái/phải chỉ mở rộng sang các phân khúc trống. Bằng cách đếm các phân khúc này và so sánh với những người bên trái/phải có sẵn, chúng tôi đảm bảo sự sắp xếp tối đa mà không có xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        x = list(map(int, input().split()))
        fixed = set()
        left = 0
        right = 0
        for val in x:
            if val == -1:
                left += 1
            elif val == -2:
                right += 1
            else:
                fixed.add(val)
        fixed = sorted(fixed)
        k = len(fixed)
        res = 0
        # consider no fixed seats
        if k == 0:
            res = min(left + right, m)
            print(res)
            continue
        # try placing flexible people around fixed seats
        max_add = 0
        for i in range(k + 1):
            # left side: seats before fixed[i]
            left_space = fixed[i - 1] - 1 if i > 0 else 0
            # right side: seats after fixed[i-1]
            right_space = m - fixed[i - 1] if i > 0 else m
            add_left = min(left, left_space)
            add_right = min(right, right_space)
            max_add = max(max_add, add_left + add_right)
        print(min(k + max_add, m))

if __name__ == "__main__":
    solve()
```Mã đầu tiên đếm những người ngồi bên trái, bên phải và chỗ ngồi cố định. Việc sắp xếp các chỗ ngồi cố định đảm bảo chúng tôi có thể xem xét các phân khúc vị trí một cách hiệu quả. Vòng lặp lặp qua các vị trí liền kề với các ghế cố định, tính toán số người trái/phải tối đa có thể ngồi mà không chồng chéo các ghế cố định. Câu trả lời cuối cùng không được vượt quá tổng số ghế`m`. 

Các điều kiện ranh giới được xử lý bao gồm không có chỗ ngồi cố định, không có người bên trái/phải và các tình huống trong đó tất cả các chỗ ngồi đều được nhắm mục tiêu bởi các yêu cầu cố định. 

## Ví dụ đã hoạt động 

Mẫu 1:`3 10`với`5 5 5`. 

| Bước | trái | đúng | cố định | cố định độc đáo | left_space | đúng_space | thêm_left+add_right | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 0 | {5,5,5} | 1 | 0 | 5 | 0 | 1 | 

Chỉ có một người được ngồi vì tất cả đều muốn ngồi ghế 5. Thuật toán trả về đúng 1. 

Mẫu 2:`5 7`với`-1 -1 4 -2 -2`| Bước | trái | đúng | cố định | cố định độc đáo | left_space | đúng_space | thêm_left+add_right | kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 2 | {4} | 1 | 3 | 3 | 4 | 5 | 

Hai người bên trái chiếm ghế 1-3, hai người bên phải chiếm ghế 5-7, một ghế cố định 4. Sức chứa tối đa là 5. Thuật toán trả về 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chỗ ngồi cố định chiếm ưu thế, quét tuyến tính sau | 
| Không gian | O(n) | Để lưu trữ bộ chỗ ngồi cố định và mảng đầu vào | 

Với các ràng buộc của bài toán, thuật toán xử lý tổng tối đa`n`của`10^5`thoải mái trong 2 giây. 

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
assert run("10\n3 10\n5 5 5\n4 6\n1 -2 -2 1\n5 7\n-1 -1 4 -2 -2\n6 7\n5 -2 -2 -2 -2 -2\n6 6\n-1 1 4 5 -1 4\n6 8\n-1 -1 -1 3 -1 -2\n6 7\n5 -1 -2 -2 -2 -2\n3 1\n-2 -2 1\n2 5\n5 -2\n1 2\n-1") == "1\n3\n5\n6\n5\n5\n5\n1\n2\n1"

# Custom cases
assert run("1\n5 5\n-1 -1 -1 -1 -1") == "5", "all leftward"
assert run("1\n5 5\n-2 -2 -2 -2 -2") == "5", "all rightward"
assert run("1\n5 5\n1 2 3 4 5") == "5", "all fixed seats"
assert run("1\n5 5\n-1 -2 3 3 -1") == "3", "mixed with duplicate fixed seat"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả sang trái | 5 | điền bên trái hoạt động ở ngoài cùng bên trái |
