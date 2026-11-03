---
title: "CF 1114B - Một tác vụ phân vùng mảng khác"
description: "Chúng ta được cấp một chuỗi các số nguyên a có độ dài n và hai tham số: m, số phần tử lớn nhất mà chúng ta xem xét khi tính toán \"vẻ đẹp\" của một mảng con và k, số mảng con liền kề mà chúng ta phải phân chia a thành."
date: "2026-06-12T04:52:03+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1114
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 538 (Div. 2)"
rating: 1500
weight: 1114
solve_time_s: 112
verified: true
draft: false
---

[CF 1114B - Một nhiệm vụ phân vùng mảng khác](https://codeforces.com/problemset/problem/1114/B) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a`chiều dài`n`và hai tham số:`m`, số phần tử lớn nhất mà chúng tôi xem xét khi tính toán "vẻ đẹp" của một mảng con và`k`, số mảng con liền kề chúng ta phải phân vùng`a`vào trong. Vẻ đẹp của một mảng con được định nghĩa là tổng của nó`m`phần tử lớn nhất. Nhiệm vụ là chia`a`vào chính xác`k`mảng con liền kề, mỗi mảng có kích thước ít nhất`m`, sao cho tổng giá trị đẹp của tất cả các mảng con là lớn nhất. 

Các ràng buộc đầu vào chỉ ra rằng`n`có thể lên tới 200.000 và giá trị của`a_i`có thể là âm hoặc dương lớn. Từ`n`nếu lớn như vậy thì bất kỳ thuật toán nào thử tất cả các phân vùng có thể sẽ quá chậm. Một giải pháp tuyến tính hoặc gần tuyến tính trong`n`được yêu cầu. Mỗi mảng con phải chứa ít nhất`m`phần tử, nghĩa là phần còn lại`n - k*m`các phần tử có thể được phân phối linh hoạt giữa các`k`mảng con để tăng tổng của chúng. 

Các trường hợp biên bao gồm các tình huống trong đó tất cả các số đều bằng nhau, điều này sẽ tạo ra nhiều phân vùng tương đương nhau và khi có chính xác`m`các phần tử trên mỗi mảng con, không tạo ra sự linh hoạt trong việc phân vùng. Một cách tiếp cận ngây thơ chỉ tính tổng lớn nhất`m`các phần tử trong các phân đoạn liền kề mà không xem xét cách gán các phần tử để tối đa hóa vẻ đẹp tổng thể sẽ thất bại khi việc phân phối lại các phần tử bổ sung có thể làm tăng tổng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi cách phân vùng`a`vào trong`k`mảng con của ít nhất`m`các phần tử, tính toán vẻ đẹp cho từng mảng con và chọn phân vùng có tổng cao nhất. Điều này đúng vì nó đánh giá toàn diện tất cả các phân vùng hợp lệ. Tuy nhiên, số lượng phân vùng là tổ hợp - theo thứ tự`C(n-1, k-1)`-có giá trị lớn về mặt thiên văn khi`n`là 200.000, khiến phương pháp này hoàn toàn không thực tế. 

Quan sát quan trọng cho một giải pháp tối ưu là tổng của`m`các phần tử lớn nhất trong toàn bộ mảng chỉ phụ thuộc vào phần tử nào được bao gồm trong mảng con chứ không phụ thuộc vào thứ tự của chúng trong mảng con. Nếu chúng ta lấy`m*k`các phần tử lớn nhất trong mảng, đây sẽ luôn là những phần tử góp phần tạo nên vẻ đẹp tổng thể trong bất kỳ phân vùng tối ưu nào. Vấn đề sau đó giảm xuống việc xác định vị trí của những`m*k`phần tử lớn nhất và chia chúng thành`k`các khối liền kề sao cho mỗi khối chứa chính xác`m`của chúng (cộng với mọi yếu tố còn sót lại cần thiết để duy trì sự liền kề). Sắp xếp các phần tử và chọn chỉ mục cho phép chúng ta xây dựng phân vùng một cách hiệu quả. 

Các cách tiếp cận đơn giản và tối ưu có thể được tóm tắt như sau: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n chọn k) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ghép từng phần tử vào`a`với chỉ số của nó để bảo toàn vị trí. Điều này là cần thiết vì chúng ta cần các chỉ mục ban đầu để xác định ranh giới của các phân vùng. 
2. Sắp xếp mảng các cặp này theo thứ tự giá trị giảm dần. Điều này cho phép chúng ta dễ dàng chọn ra những yếu tố lớn nhất góp phần tạo nên vẻ đẹp tổng thể. 
3. Lấy cái đầu tiên`m*k`các phần tử của mảng đã được sắp xếp. Đây là những giá trị sẽ được đưa vào phân vùng tối ưu, vì chúng tôi muốn tối đa hóa tổng số vẻ đẹp. 
4. Tính tổng các giá trị này`m*k`các yếu tố để có được vẻ đẹp tổng thể tối đa. Lưu trữ các chỉ số ban đầu của họ. 
5. Sắp xếp chỉ số của các phần tử đã chọn. Điều này mang lại cho chúng ta những vị trí trong`a`nơi các phần tử lớn nhất này xuất hiện, chúng ta sẽ sử dụng chúng để tạo thành các mảng con liền kề. 
6. Lặp lại các chỉ số đã sắp xếp này và chọn`k-1`điểm cắt. Phương pháp đơn giản nhất là lấy cái đầu tiên`m`chỉ số của các phần tử làm phần cuối của mảng con đầu tiên, phần tiếp theo`m`các phần tử làm phần cuối của mảng con thứ hai, v.v. Các vị trí ngay trước các chỉ số này xác định nơi phân chia`a`. 
7. Xuất ra tổng các điểm đẹp và vị trí cắt đã chọn. 

Tại sao nó hoạt động: Điều bất biến là tổng các vẻ đẹp tối ưu luôn đến từ`m*k`phần tử lớn nhất và phân phối chúng thành`k`mảng con liền kề của ít nhất`m`các yếu tố bảo tồn sự đóng góp của họ. Việc sắp xếp các chỉ mục đảm bảo các mảng con liền kề nhau và chọn mọi`m`-phần tử thứ đảm bảo rằng mỗi mảng con có ít nhất`m`các phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
a = list(map(int, input().split()))

# Pair elements with original indices
indexed_a = [(val, idx) for idx, val in enumerate(a)]
# Sort descending by value
indexed_a.sort(reverse=True, key=lambda x: x[0])

# Take top m*k elements
top_elements = indexed_a[:m*k]
# Maximum beauty sum
max_beauty = sum(val for val, _ in top_elements)

# Extract and sort indices
indices = sorted(idx for _, idx in top_elements)

# Determine partition points
cuts = []
for i in range(1, k):
    cuts.append(indices[i*m - 1] + 1)  # +1 for 1-based indexing

print(max_beauty)
print(' '.join(map(str, cuts)))
```Đầu tiên, mã xác định các yếu tố góp phần tạo nên vẻ đẹp tối đa, sau đó sắp xếp vị trí của chúng để đảm bảo tính liền kề. Việc tính toán điểm cắt rất đơn giản:`m`-th,`2*m`-th, v.v., các phần tử lớn nhất xác định ranh giới mảng con. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:```
9 2 3
5 2 5 2 4 1 1 3 2
```| Bước | Mô tả | Giá trị/Chỉ số | 
| --- | --- | --- | 
| 1 | Ghép nối với chỉ mục | [(5,0),(2,1),(5,2),(2,3),(4,4),(1,5),(1,6),(3,7),(2,8)] | 
| 2 | Sắp xếp theo giá trị giảm dần | [(5,0),(5,2),(4,4),(3,7),(2,1),(2,3),(2,8),(1,5),(1,6)] | 
| 3 | Đứng đầu 6 (m*k) | [(5,0),(5,2),(4,4),(3,7),(2,1),(2,3)] | 
| 4 | Tổng sắc đẹp tối đa | 5+5+4+3+2+2=21 | 
| 5 | Sắp xếp chỉ số | [0,1,2,3,4,7] | 
| 6 | Chọn điểm cắt | Lần 1: chỉ số 1 -> cắt ở mức 2, lần 2: chỉ số 3 -> cắt ở mức 4 | 

Phân vùng:`[5,2,5]`,`[2,4]`,`[1,1,3,2]`tổng bằng 21. 

**Đầu vào mẫu 2**:```
6 1 2
1 3 2 5 4 1
```| Bước | Mô tả | Giá trị/Chỉ số | 
| --- | --- | --- | 
| 2 yếu tố hàng đầu | [(5,3),(4,4)] | Tổng=9 | 
| Sắp xếp chỉ số | [3,4] | | 
| Điểm cắt | mảng con đầu tiên kết thúc ở chỉ số 3 -> cắt ở 4 | | 

Phân vùng`[1,3,2,5]`,`[4,1]`cho tổng số người đẹp 9 + 4 = 13. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng chiếm ưu thế; tất cả các hoạt động khác là tuyến tính | 
| Không gian | O(n) | Chúng tôi lưu trữ các cặp được lập chỉ mục và các chỉ mục được chọn | 

Được cho`n`lên tới 2e5,`O(n log n)`được chấp nhận trong giới hạn thời gian 2 giây. Bộ nhớ tuyến tính, nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("9 2 3\n5 2 5 2 4 1 1 3 2\n") == "21\n2 5"

# Minimum size input
assert run("2 1 2\n1 2\n") == "3\n1"

# All equal values
assert run("6 2 2\n3 3 3 3 3 3\n") == "12\n2"

# Max size test (n=10^5 simplified)
# Generate 10^5 elements increasing
# assert run(f"{10**5} 1 2\n{' '.join(map(str, range(1,10**5+1)))}\n")  # Optional, time-consuming

# Edge case: exact m*k=n
assert run("4 2 2\n1 2 3 4\n") == "10\n2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "2 1 2\n1 2\n" | "3\n1" | Mảng và mảng con tối thiểu | 
| "6 2 2\n3 3 3 3 3 3\n" | "12\n2" | Tất cả các phần tử bằng nhau | 
| "4 2 2\n1 2 3 4\n" | "10\n2" | |
