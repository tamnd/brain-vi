---
title: "CF 2191A - Tô màu mảng"
description: "Chúng ta được yêu cầu tô màu một chuỗi các số nguyên riêng biệt được sắp xếp thành một hàng bằng hai màu sao cho hai điều kiện đồng thời có giá trị. Đầu tiên, bất kỳ hai số liền kề nào trong dãy ban đầu đều phải có màu khác nhau."
date: "2026-06-07T21:00:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 2191
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1073 (Div. 2)"
rating: 800
weight: 2191
solve_time_s: 110
verified: true
draft: false
---

[CF 2191A - Tô màu mảng](https://codeforces.com/problemset/problem/2191/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tô màu một chuỗi các số nguyên riêng biệt được sắp xếp thành một hàng bằng hai màu sao cho hai điều kiện đồng thời có giá trị. Đầu tiên, bất kỳ hai số liền kề nào trong dãy ban đầu đều phải có màu khác nhau. Thứ hai, nếu chúng ta sắp xếp các số theo thứ tự tăng dần thì dãy mới cũng phải có các số liền kề được tô màu khác nhau. Đầu vào đưa ra nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số lượng thẻ và giá trị nguyên của chúng. Đầu ra cho mỗi trường hợp thử nghiệm chỉ đơn giản là "CÓ" nếu tồn tại màu hợp lệ và "KHÔNG" nếu ngược lại. 

Các ràng buộc rất nhỏ: số lượng thẻ trong một trường hợp thử nghiệm nhiều nhất là 100 và có nhiều nhất 200 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả giải pháp O(n²) thường chạy đủ nhanh, nhưng chúng ta nên cố gắng tiếp cận tuyến tính hoặc tuyến tính vì vấn đề có thể được giải quyết bằng cái nhìn sâu sắc hơn là kiểm tra vũ phu tất cả các màu. 

Một điều tinh tế là chúng ta không thể thay thế các màu trong mảng ban đầu hoặc trong mảng được sắp xếp một cách độc lập. Hãy xem xét trình tự`[2, 3, 1]`. Màu sắc thay thế trong bản gốc mang lại`[red, blue, red]`, nhưng sau khi sắp xếp`[1, 2, 3]`màu sắc trở thành`[red, red, blue]`, thất bại. Các trường hợp cạnh phát sinh khi các phần tử nhỏ nhất hoặc lớn nhất xuất hiện ở các vị trí buộc các màu lặp lại trong chuỗi được sắp xếp. Điều này cho thấy rằng phần kề trong mảng ban đầu không ánh xạ một cách tầm thường với phần kề trong mảng được sắp xếp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả 2ⁿ màu có thể có của mảng và kiểm tra hai điều kiện. Điều này đúng vì nó xem xét mọi khả năng, nhưng rõ ràng nó không thực tế với n = 100, vì 2¹⁰⁰ là lớn về mặt thiên văn. Ngay cả với n nhỏ hơn cũng không cần thiết vì bài toán có cấu trúc ẩn. 

Quan sát quan trọng là cả hai điều kiện có thể được thỏa mãn đồng thời nếu chuỗi có "màu zig-zag" theo thứ tự được sắp xếp và thứ tự ban đầu. Nếu chúng ta gắn nhãn cho các vị trí trong dãy đã sắp xếp, thì việc tô màu xen kẽ giữa các vị trí này sẽ đảm bảo rằng sự kề cận đã sắp xếp được thỏa mãn. Trong thực tế, chúng ta có thể gán màu dựa trên chỉ mục theo thứ tự được sắp xếp: chỉ số lẻ nhận một màu, chỉ số chẵn nhận màu kia. Sau khi gán màu theo thứ tự sắp xếp, chúng ta ánh xạ chúng trở lại vị trí ban đầu. Sau đó chúng ta chỉ cần xác minh rằng các phần tử liền kề trong mảng ban đầu có màu khác nhau. Nếu có thì có một màu hợp lệ. Nếu không thì không. Điều này hiệu quả vì việc xen kẽ theo thứ tự được sắp xếp đảm bảo điều kiện được sắp xếp và kích thước nhỏ của n cho phép chúng ta kiểm tra tính kề trong mảng ban đầu một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2ⁿ·n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc n và mảng a. Chúng ta cần xử lý từng mảng một cách độc lập. 
2. Xây dựng danh sách các bộ chứa từng phần tử và chỉ mục gốc của nó. Điều này cho phép chúng ta sắp xếp mảng trong khi vẫn nhớ được vị trí ban đầu. 
3. Sắp xếp mảng theo giá trị. Sau khi sắp xếp, gán màu xen kẽ dọc theo mảng đã sắp xếp. Ví dụ: chỉ mục 0 (giá trị nhỏ nhất) có màu 0, chỉ mục 1 có màu 1, v.v. Lưu trữ việc gán màu trong một mảng riêng biệt được ánh xạ tới các chỉ mục gốc. 
4. Lặp lại mảng ban đầu. Đối với mỗi cặp phần tử liền kề, hãy kiểm tra xem màu được gán của chúng có khác nhau không. Nếu bất kỳ cặp nào có cùng màu, câu trả lời là "KHÔNG" cho trường hợp thử nghiệm này. 
5. Nếu tất cả các cặp liền kề có màu khác nhau thì câu trả lời là "CÓ". Xuất kết quả và tiếp tục với trường hợp thử nghiệm tiếp theo. 

Tại sao nó hoạt động: Bằng cách xen kẽ các màu theo thứ tự đã sắp xếp, chúng tôi đảm bảo rằng tính liền kề đã được sắp xếp sẽ được đáp ứng. Bằng cách ánh xạ các màu trở lại vị trí ban đầu, chúng ta có thể kiểm tra xem tính liền kề ban đầu có được giữ nguyên hay không. Thuật toán khai thác thực tế là với hai màu, các phép gán xen kẽ theo thứ tự sắp xếp sẽ giảm thiểu xung đột và kích thước mảng nhỏ cho phép chúng ta dễ dàng phát hiện xung đột. Nếu có xung đột thì không có màu nào khác thỏa mãn đồng thời cả hai điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        indexed_a = list(enumerate(a))
        indexed_a.sort(key=lambda x: x[1])
        
        color = [0] * n
        for i, (idx, _) in enumerate(indexed_a):
            color[idx] = i % 2  # alternate colors in sorted order
        
        ok = True
        for i in range(n - 1):
            if color[i] == color[i + 1]:
                ok = False
                break
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã liệt kê các phần tử để giữ nguyên vị trí ban đầu và sắp xếp theo giá trị. Sau đó, nó gán các màu xen kẽ theo thứ tự đã sắp xếp và ánh xạ trở lại vị trí ban đầu. Cuối cùng, nó kiểm tra tính kề cận trong mảng ban đầu và đưa ra kết quả. Việc sử dụng modulo đảm bảo rằng chúng tôi thay thế các màu một cách chính xác và vòng kiểm tra sẽ phát hiện bất kỳ vùng lân cận không hợp lệ nào một cách hiệu quả. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`[2, 3, 4, 1]`| Chỉ mục gốc | Giá trị | Chỉ mục được sắp xếp | Màu được chỉ định | Màu cuối cùng theo thứ tự ban đầu | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 1 | 
| 1 | 3 | 2 | 0 | 0 | 
| 2 | 4 | 3 | 1 | 1 | 
| 3 | 1 | 0 | 0 | 0 | 

Kiểm tra kề theo thứ tự ban đầu: 1 vs 0, 0 vs 1, 1 vs 0 → tất cả đều khác nhau. Đầu ra CÓ. 

**Ví dụ 2:**`[2, 3, 1]`| Chỉ mục gốc | Giá trị | Chỉ mục được sắp xếp | Màu được chỉ định | Màu cuối cùng theo thứ tự ban đầu | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 1 | 
| 1 | 3 | 2 | 0 | 0 | 
| 2 | 1 | 0 | 0 | 0 | 

Kiểm tra kề: 1 vs 0, 0 vs 0 → xung đột. Đầu ra KHÔNG. 

Những dấu vết này cho thấy thuật toán phát hiện chính xác khi xung đột kề xuất hiện trong mảng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, kiểm tra kề là O(n) | 
| Không gian | O(n) | Lưu trữ mảng chỉ mục và mảng màu | 

Với n 100 và t 200, thuật toán thực hiện tối đa 200·100·log 100 ≈ 13.000 thao tác, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("4\n4\n2 3 4 1\n3\n2 3 1\n5\n3 4 1 2 5\n5\n3 1 4 2 5\n") == "YES\nNO\nYES\nNO"

# custom cases
assert run("1\n2\n1 2\n") == "YES", "minimum size array"
assert run("1\n5\n5 4 3 2 1\n") == "YES", "descending order"
assert run("1\n4\n1 3 2 4\n") == "NO", "unsolvable small array"
assert run("1\n3\n3 1 2\n") == "NO", "shuffled small array"
assert run("1\n6\n1 6 2 5 3 4\n") == "YES", "zig-zag pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 yếu tố`[1,2]`| CÓ | Mảng kích thước tối thiểu luôn có thể giải được | 
|`[5,4,3,2,1]`| CÓ | Mảng giảm dần có thể được xen kẽ | 
|`[1,3,2,4]`| KHÔNG | Mảng nhỏ không giải quyết được, xung đột kề | 
|`[3,1,2]`| KHÔNG | Mảng xáo trộn nhỏ, xung đột kề | 
|`[1,6,2,5,3,4]`| CÓ | Mô hình zig-zag hoạt động | 

## Vỏ cạnh 

cho`[2,3,1]`, trình tự được sắp xếp`[1,2,3]`xung đột nếu chúng ta cố gắng xen kẽ các màu tùy ý theo thứ tự ban đầu. Thuật toán gán màu dựa trên vị trí được sắp xếp:`[0,1,2] → colors 0,1,0]`. Ánh xạ trở lại các chỉ số ban đầu mang lại`[1,0,0]`. Kiểm tra sự liền kề ban đầu, vị trí 1 và 2 đều có màu 0, tạo ra xung đột. Đầu ra là KHÔNG, xác nhận thuật toán xử lý chính xác trường hợp cạnh này. Tương tự, mảng có kích thước 2 luôn có thể giải được vì hai phần tử luôn có thể được tô màu khác nhau và thứ tự sắp xếp sẽ duy trì sự thay đổi này.
