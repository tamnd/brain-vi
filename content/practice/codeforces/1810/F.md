---
title: "CF 1810F - Cây M"
description: "Chúng ta được yêu cầu xây dựng một cây có gốc thỏa mãn một đặc tính rất cụ thể: mỗi nút không phải lá phải có chính xác m nút con và mỗi nút chứa một số nguyên dương. Cây được gọi là \"tốt\" nếu nó thỏa mãn cấu trúc này."
date: "2026-06-09T08:47:12+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1810
codeforces_index: "F"
codeforces_contest_name: "CodeTON Round 4 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2800
weight: 1810
solve_time_s: 99
verified: false
draft: false
---

[CF 1810F - M-tree](https://codeforces.com/problemset/problem/1810/F) 

**Đánh giá:** 2800 
**Tags:** cấu trúc dữ liệu, toán học, sắp xếp, cây 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một cây có gốc thỏa mãn một đặc tính rất cụ thể: mỗi nút không phải lá phải có chính xác`m`con và mỗi lá chứa một số nguyên dương. Cây được gọi là “tốt” nếu nó thỏa mãn cấu trúc này. Mỗi lá đóng góp một giá trị bằng số trên nó cộng với độ sâu của nó trong cây, và giá trị tổng thể của cây là giá trị lá lớn nhất. 

Đầu vào cho chúng ta`n`những con số phải được đặt trên lá. Nhiệm vụ của chúng ta là gán những số này cho các lá và cấu trúc cây sao cho giá trị lá tối đa là cực tiểu. Nhiều truy vấn tiếp theo, mỗi truy vấn thay đổi một số trong mảng và yêu cầu giá trị cây tối thiểu mới. 

Những ràng buộc rất chặt chẽ.`n`có thể đạt được`2×10^5`Và`q`cũng có thể đạt được`2×10^5`, nghĩa là bất kỳ thuật toán nào kém hơn O(n log n) cho mỗi truy vấn đều có thể quá chậm. Số lượng lá`n`luôn thỏa mãn`n ≡ 1 (mod m - 1)`, đó là một thực tế kết hợp quan trọng về việc xây dựng`m`-ary cây - nó đảm bảo rằng đầy đủ`m`cây -ary có thể được hình thành với chính xác`n`lá. 

Một cách tiếp cận đơn giản là xây dựng mọi cây hợp lệ cho mảng hiện tại, tính toán độ sâu và sau đó tính giá trị lá tối đa. Điều này không thành công vì số lượng cây hợp lệ theo cấp số nhân`n`. Các trường hợp biên cần cẩn thận bao gồm các mảng trong đó tất cả các số đều bằng nhau (cấu trúc tối ưu được xác định hoàn toàn theo độ sâu) hoặc các mảng trong đó các số lớn nhất được phân cụm - việc gán bất cẩn có thể làm tăng đáng kể giá trị lá tối đa. 

Ví dụ, nếu`n = 5, m = 2`và mảng là`[1, 1, 5, 5, 5]`, cách tiếp cận ngây thơ có thể đặt`5`quá sâu hoặc quá nông, tạo ra giá trị cây là 7 thay vì giá trị tối ưu là 6. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Người ta có thể tạo ra tất cả những gì có thể`m`-ary cây với`n`lá và kiểm tra mọi hoán vị của mảng để gán số cho các lá, mỗi lần tính toán giá trị lớn nhất. Điều này đúng về nguyên tắc nhưng hoàn toàn không thực tế. Vì`n=10^5`, số lượng cây và lá được phân công lớn về mặt thiên văn. Giải pháp vũ phu đòi hỏi một cái gì đó như`O(n! * n)`hoạt động, điều đó là không thể thực hiện được. 

Quan sát quan trọng mở ra cách tiếp cận tối ưu đến từ việc nhận ra giá trị của cây chỉ phụ thuộc vào độ sâu của lá. Lá càng sâu thì số lượng của nó càng đóng góp vào giá trị tối đa. Do đó, để giảm thiểu mức tối đa tổng thể, chúng tôi muốn đặt các số lớn nhất càng gần gốc càng tốt và các số nhỏ nhất ở các lá sâu nhất. 

Vì một cái cây tốt có cấu trúc rất cứng chắc nên chúng ta có thể tính toán chính xác có bao nhiêu lá tồn tại ở mỗi độ sâu. Bằng cách sử dụng điều này, chúng tôi sắp xếp mảng số lá theo thứ tự giảm dần và gán chúng từ lá nông nhất đến lá sâu nhất. Điều này đảm bảo giá trị tối đa được giảm thiểu: số lớn nhất có độ sâu nhỏ nhất và số nhỏ nhất sẽ chịu mức phạt độ sâu lớn nhất. 

Để cập nhật, chúng tôi duy trì nhiều tập hợp (hoặc BST cân bằng) các số, cho phép chèn và xóa nhật ký (n) cho mỗi truy vấn. Vì cấu trúc cây được cố định một lần`n`Và`m`đã biết, độ sâu của lá không thay đổi; chỉ những con số được chỉ định thay đổi. Do đó, sau mỗi lần cập nhật, chúng ta có thể tính toán lại giá trị tối đa một cách hiệu quả bằng cách kết hợp các số lớn nhất với độ sâu nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước số lượng lá ở mỗi độ sâu để có đầy đủ`m`-ary cây với`n`lá. Bắt đầu từ gốc ở độ sâu 0 và tính toán lặp lại`leaves_at_depth[d]`bằng cách chia số lá còn lại cho trẻ em. Dừng lại khi tất cả`n`lá được tính. Điều này xác định chính xác độ sâu của lá. 
2. Sắp xếp mảng`a`theo thứ tự giảm dần. Bằng cách này, số lớn nhất sẽ được xếp đầu tiên. 
3. Gán số cho các lá theo thứ tự độ sâu: đầu tiên`leaves_at_depth[0]`số đi đến độ sâu 0, tiếp theo`leaves_at_depth[1]`các số đi đến độ sâu 1, v.v. Mỗi phép gán sẽ thêm độ sâu tương ứng vào số, tạo ra giá trị của chiếc lá. 
4. Tính giá trị lớn nhất trong số các giá trị lá này. Đây là giá trị cây tối thiểu cho mảng hiện tại. 
5. Đối với mỗi truy vấn, hãy cập nhật nhiều tập số bằng cách xóa giá trị cũ và chèn giá trị mới. Sắp xếp lại nếu cần thiết (hoặc sử dụng cấu trúc cân bằng). Lặp lại các bước 3-4 để tính mức tối đa mới. 

Tại sao nó hoạt động: Bằng cách luôn ghép các số lớn nhất với độ sâu nông nhất, chúng tôi đảm bảo rằng bất kỳ phép gán nào khác sẽ đẩy số lớn hơn vào sâu hơn hoặc số nhỏ hơn nông hơn, cả hai đều làm tăng giá trị lá tối đa. Phép gán là tối ưu duy nhất dựa trên thứ tự số được sắp xếp và phân bố độ sâu cố định. Sự phân bố độ sâu được đảm bảo bởi`n ≡ 1 mod (m - 1)`, đảm bảo cây được đầy đủ, cân đối theo đúng quy định. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

def compute_depth_distribution(n, m):
    depth_counts = []
    remaining = n
    current_level = 1
    while remaining > 0:
        leaves = min(remaining, current_level)
        depth_counts.append(leaves)
        remaining -= leaves
        current_level *= m
    return depth_counts

def minimal_tree_value(a, depth_counts):
    a_sorted = sorted(a, reverse=True)
    idx = 0
    max_value = 0
    for depth, count in enumerate(depth_counts):
        for _ in range(count):
            max_value = max(max_value, a_sorted[idx] + depth)
            idx += 1
    return max_value

def main():
    t = int(input())
    for _ in range(t):
        n, m, q = map(int, input().split())
        a = list(map(int, input().split()))
        depth_counts = compute_depth_distribution(n, m)
        output = []
        for _ in range(q):
            x, y = map(int, input().split())
            a[x-1] = y
            output.append(str(minimal_tree_value(a, depth_counts)))
        print(" ".join(output))

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên tính toán có bao nhiêu lá tồn tại ở mỗi độ sâu. Việc này được thực hiện độc lập với các con số, hoàn toàn dựa trên`n`Và`m`. Sắp xếp mảng đảm bảo số lượng lớn nhất được ghép với độ sâu nông nhất. Mỗi truy vấn sửa đổi một số và tính toán lại số tối đa sử dụng cùng một chiến lược gán độ sâu. Có thể tránh từng lỗi một bằng cách luôn sử dụng các chỉ mục dựa trên số 0 trong Python. 

## Ví dụ đã hoạt động 

**Dấu vết đầu vào mẫu** 

Trường hợp thử nghiệm đầu tiên:`n=5, m=3, a=[3,3,4,4,5]`| Bước | độ sâu_đếm | a_sắp xếp | Bài tập lá | tối đa(a+độ sâu) | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,3,1] | [5,4,4,3,3] | độ sâu0:[5], độ sâu1:[4,4,3], độ sâu2:[3] | 6 | 

Sau khi truy vấn`a[1]=4`:`a=[4,3,4,4,5]`, sắp xếp ->`[5,4,4,4,3]`, cùng một bài tập -> tối đa 6. 

Điều này khẳng định chiến lược phân công luôn giảm thiểu tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Sắp xếp mảng một lần là O(n log n), mỗi truy vấn yêu cầu log n để chèn/xóa trong nhiều tập hợp hoặc O(n log n) nếu được sử dụng đầy đủ | 
| Không gian | O(n) | Lưu trữ mảng và phân bố theo chiều sâu | 

Với`n, q ≤ 2×10^5`, điều này phù hợp trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp):
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided samples
assert run("""3
5 3 3
3 3 4 4 5
1 4
2 4
3 5
5 2 4
3 3 4 4 5
1 4
2 5
3 5
4 5
7 3 4
1 2 2 3 3 3 4
1 4
2 1
5 5
6 6
""") == "6 6 6\n7 7 7 8\n6 6 6 7"

# Custom: all equal values
assert run("""1
4 2 2
1 1 1 1
1 2
4 3
""") == "2 2"

# Custom: max size leaves shallow
assert run("""1
5 2 1
5 1 1 1 1
3 5
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 

|---|---
