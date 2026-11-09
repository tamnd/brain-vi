---
title: "CF 1467B - Đồi và Thung lũng"
description: "Chúng ta được cấp một dãy số nguyên và cần xác định \"đồi\" và \"thung lũng\" trong đó. Một ngọn đồi xuất hiện ở vị trí j nếu số ở đó lớn hơn cả hai hàng xóm, trong khi một thung lũng xuất hiện nếu nó nhỏ hơn cả hai hàng xóm."
date: "2026-06-11T01:38:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1467
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 695 (Div. 2)"
rating: 1700
weight: 1467
solve_time_s: 103
verified: true
draft: false
---

[CF 1467B - Đồi và Thung lũng](https://codeforces.com/problemset/problem/1467/B) 

**Đánh giá:** 1700 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và cần xác định những “ngọn đồi” và “thung lũng” trong đó. Một ngọn đồi xuất hiện ở vị trí`j`nếu con số ở đó lớn hơn cả hai láng giềng, trong khi đó một thung lũng xuất hiện nếu nó nhỏ hơn cả hai láng giềng. Tổng số đồi và thung lũng trong một chuỗi được gọi là giá trị đáng sợ. Chúng tôi được phép thay đổi chính xác một số trong chuỗi thành bất kỳ số nguyên nào hoặc giữ nguyên chuỗi và chúng tôi muốn giá trị đáng sợ tối thiểu có thể có sau thay đổi duy nhất này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài của chuỗi và chính chuỗi đó. Đầu ra là một số cho mỗi trường hợp thử nghiệm: giá trị đe dọa tối thiểu có thể đạt được. 

Các ràng buộc sao cho tổng độ dài của tất cả các chuỗi trong tất cả các trường hợp thử nghiệm không vượt quá 300.000. Vì mỗi trường hợp thử nghiệm có thể lớn tới 300.000 phần tử, nên$O(n^2)$cách tiếp cận sẽ quá chậm. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính theo kích thước của mỗi chuỗi. 

Các trường hợp cạnh phát sinh khi các chuỗi có rất ít phần tử, chẳng hạn như ba phần tử, trong đó chỉ phần tử ở giữa có thể là ngọn đồi hoặc thung lũng hoặc các chuỗi trong đó tất cả các số đều bằng nhau, không có ngọn đồi và thung lũng nào. Việc triển khai bất cẩn có thể cố gắng điều chỉnh các phần tử mà không kiểm tra ranh giới hoặc có thể tính toán các ngọn đồi và thung lũng không chính xác nếu những thay đổi liền kề bị bỏ qua. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên mọi vị trí có thể có trong chuỗi, cố gắng thay đổi nó thành mọi số nguyên có thể, tính toán lại toàn bộ giá trị đe dọa và giữ mức tối thiểu. Cách tiếp cận này đúng về nguyên tắc nhưng hoàn toàn không thực tế. Nếu chúng ta cố gắng kiểm tra tất cả các giá trị lên đến$10^9$đối với mỗi phần tử, số lượng thao tác bùng nổ, vượt xa giới hạn khả thi. 

Thông tin chuyên sâu quan trọng về tối ưu hóa xuất phát từ quan sát cho thấy việc thay đổi một số chỉ ảnh hưởng đến các ngọn đồi và thung lũng trong vùng lân cận của nó - cụ thể là bản thân chỉ số và hai chỉ số liền kề của nó. Vì vậy, chúng ta không cần phải tính toán lại toàn bộ chuỗi mỗi lần; chúng ta chỉ cần xem xét sự đóng góp cục bộ của các ngọn đồi và thung lũng xung quanh yếu tố đã thay đổi. 

Chúng ta có thể tính toán trước các ngọn đồi và thung lũng ban đầu cho toàn bộ chuỗi. Sau đó với mỗi vị trí ứng viên`i`(ngoại trừ phần tử đầu tiên và cuối cùng), chúng tôi xem xét ba tùy chọn: thay đổi nó để khớp với hàng xóm bên trái, khớp với hàng xóm bên phải hoặc giữ nguyên. Đối với mỗi lựa chọn, chúng tôi tính toán sự thay đổi ở các ngọn đồi và thung lũng địa phương (tại các chỉ số`i-1`,`i`, Và`i+1`). Chúng tôi chọn phương án làm giảm tổng giá trị đe dọa nhiều nhất. Lặp lại điều này cho từng vị trí và theo dõi các kết quả tối thiểu trong giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * value_range) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và mảng`a`. 
2. Xác định hàm trợ giúp`is_hill_or_valley(i)`điều đó trả về`1`nếu như`a[i]`là một ngọn đồi hoặc thung lũng và`0`nếu không thì. Điều này kiểm tra nếu`a[i] > a[i-1]`Và`a[i] > a[i+1]`hoặc`a[i] < a[i-1]`Và`a[i] < a[i+1]`. 
3. Tính toán trước tổng giá trị đe dọa ban đầu bằng cách tính tổng`is_hill_or_valley(i)`cho tất cả`2 ≤ i ≤ n-1`. 
4. Khởi tạo`best_reduction`về không. Điều này sẽ lưu trữ mức giảm tối đa về giá trị đe dọa mà chúng ta có thể đạt được chỉ với một thay đổi. 
5. Lặp lại các chỉ số`i`từ 1 đến`n-2`(lập chỉ mục dựa trên 0, bỏ qua phần tử đầu tiên và cuối cùng): 

1. Tính tổng số đồi và thung lũng hiện tại tại các vị trí`i-1`,`i`, Và`i+1`. 
2. Hãy thử thay đổi`a[i]`ĐẾN`a[i-1]`. Tính tổng mới tại các vị trí`i-1`,`i`, Và`i+1`. Tính mức giảm: tổng cũ trừ tổng mới. Cập nhật`best_reduction`nếu cái này lớn hơn 
3. Lặp lại bằng cách thay đổi`a[i]`ĐẾN`a[i+1]`. 
6. Giá trị đe dọa tối thiểu là`initial_total - best_reduction`. In nó cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Việc thay đổi một số chỉ ảnh hưởng đến các số lân cận. Bằng cách đánh giá tất cả các sửa đổi cục bộ có liên quan và chọn sửa đổi làm giảm tổng cục bộ nhiều nhất, chúng tôi đảm bảo rằng chúng tôi tìm thấy mức giảm thay đổi tối ưu. Không cần tính toán lại toàn cầu ngoài các vùng lân cận địa phương này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_hill_or_valley(a, i):
    return (a[i] > a[i-1] and a[i] > a[i+1]) or (a[i] < a[i-1] and a[i] < a[i+1])

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n < 3:
            print(0)
            continue
        hills_valleys = [0] * n
        total = 0
        for i in range(1, n-1):
            if is_hill_or_valley(a, i):
                hills_valleys[i] = 1
                total += 1
        best_reduction = 0
        for i in range(1, n-1):
            original = [hills_valleys[j] for j in range(max(0,i-1), min(n,i+2))]
            current_sum = sum(original)
            for new_val in [a[i-1], a[i+1]]:
                old = a[i]
                a[i] = new_val
                new = [1 if 1 <= j <= n-2 and is_hill_or_valley(a, j) else 0 for j in range(max(0,i-1), min(n,i+2))]
                reduction = current_sum - sum(new)
                best_reduction = max(best_reduction, reduction)
                a[i] = old
        print(total - best_reduction)

if __name__ == "__main__":
    solve()
```Mã này tính toán trước các ngọn đồi và thung lũng trong`hills_valleys`. Vòng lặp lồng nhau`i`kiểm tra thay đổi`a[i]`tới các giá trị của các láng giềng của nó. Việc sử dụng các lát đảm bảo chúng ta chỉ tính toán lại hiệu ứng cục bộ cho`i-1`,`i`,`i+1`. Đang khôi phục`a[i]`sau mỗi lần thử sẽ tránh được tác dụng phụ qua các lần lặp lại. Xử lý đặc biệt`n < 3`tránh lỗi chỉ mục. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`[1, 5, 3]`Đồi/thung lũng ban đầu: chỉ số 1 là đồi (5 > 1, 5 > 3), tổng = 1. 

Thay đổi`5`ĐẾN`1`hoặc`3`loại bỏ ngọn đồi. 

Giảm = 1, giá trị đe dọa tối thiểu cuối cùng = 0. 

| tôi | một[i] thay đổi | số tiền cũ của địa phương | tổng mới địa phương | giảm | tốt nhất_reduction | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 1 | 1 | 
| 1 | 3 | 1 | 0 | 1 | 1 | 

### Mẫu 2 

đầu vào:`[2, 2, 2, 2, 2]`Tất cả các phần tử đều bằng nhau, không có đồi hoặc thung lũng, tổng = 0. 

Bất kỳ thay đổi nào cũng không thể cải thiện tổng thể hơn nữa. Đầu ra = 0. 

| tôi | một[i] thay đổi | số tiền cũ của địa phương | tổng mới địa phương | giảm | tốt nhất_reduction | 
| --- | --- | --- | --- | --- | --- | 
| 1..3 | 2 (hàng xóm) | 0 | 0 | 0 | 0 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chỉ mục được kiểm tra nhiều nhất hai lần, với các phép tính lân cận theo thời gian không đổi | 
| Không gian | O(n) | Lưu trữ đồi và thung lũng cho từng chỉ số | 

Thời gian tuyến tính cho mỗi trường hợp thử nghiệm đảm bảo rằng với tổng`n`trên tất cả các trường hợp thử nghiệm 300.000, giải pháp chạy thoải mái trong vòng 1 giây. Việc sử dụng bộ nhớ là tuyến tính, nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n1 5 3\n5\n2 2 2 2 2\n6\n1 6 2 5 2 10\n5\n1 6 2 5 1\n") == "0\n0\n1\n0"

# Minimum size input
assert run("1\n1\n100\n") == "0", "single element"

# Two elements, no hills or valleys
assert run("1\n2\n3 3\n") == "0", "two elements"

# Maximum-size input, all equal
assert run(f"1\n300000\n{' '.join(['1']*300000)}\n") == "0", "all equal large"

# Peaks at boundaries
assert run("1\n5\n10 1 10 1 10\n") == "1", "alternating peaks"

# Random peaks
assert run("1
```
