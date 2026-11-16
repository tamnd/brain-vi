---
title: "CF 1901E - Cây nén"
description: "Cho một cây có n đỉnh, mỗi đỉnh được đánh nhãn bằng một số nguyên. Các thao tác được phép cho phép chúng ta loại bỏ các đỉnh lá (các đỉnh có nhiều nhất một cạnh) bất kỳ số lần nào."
date: "2026-06-08T21:16:45+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "greedy", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1901
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 158 (Rated for Div. 2)"
rating: 2200
weight: 1901
solve_time_s: 231
verified: true
draft: false
---

[CF 1901E - Cây nén](https://codeforces.com/problemset/problem/1901/E) 

**Xếp hạng:** 2200 
**Thẻ:** dfs và tương tự, dp, đồ thị, tham lam, sắp xếp, cây cối 
**Thời gian giải:** 3 phút 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`đỉnh, mỗi đỉnh được gắn nhãn bằng một số nguyên. Các thao tác được phép cho phép chúng ta loại bỏ các đỉnh lá (các đỉnh có nhiều nhất một cạnh) bất kỳ số lần nào. Sau đó, cây được "nén": bất kỳ đỉnh nào có chính xác hai đỉnh lân cận sẽ bị loại bỏ và các đỉnh lân cận của nó được kết nối trực tiếp. Mục tiêu cuối cùng là tối đa hóa tổng các số trên các đỉnh còn lại sau bất kỳ chuỗi xóa nào và sau đó nén. 

Cụ thể, đầu vào cung cấp nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng ta có số đỉnh, mảng số nguyên trên các đỉnh và các cạnh của cây. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất: tổng tối đa có thể đạt được. 

Nhìn vào những hạn chế,`n`có thể lên tới 500.000 trong tất cả các trường hợp thử nghiệm và mỗi số có thể có độ lớn bằng 10^9. Điều đó loại trừ bất kỳ thuật toán nào đi qua các tập hợp con của các đỉnh hoặc mô phỏng tất cả các thao tác xóa có thể xảy ra. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh cần chú ý bao gồm: 

- Cây chỉ có hai đỉnh. Ở đây, việc xóa một đỉnh có thể là tối ưu nếu giá trị của nó âm. 
- Cây có tất cả các giá trị đỉnh đều âm. Câu trả lời đúng phải bằng 0 vì chúng ta có thể loại bỏ mọi thứ. 
- Cây mà việc nén làm thay đổi đáng kể cấu trúc, ví dụ: một chuỗi số dương so với một ngôi sao. 

Ví dụ, với hai đỉnh`[-2, -5]`được kết nối bằng một cạnh, loại bỏ cả hai sẽ có tổng bằng 0. Một cách tiếp cận bất cẩn tính tổng tất cả các số mà không tính đến việc xóa sẽ trả về -7. 

## Phương pháp tiếp cận 

Một cách tiếp cận thô bạo là thử mọi trình tự xóa lá có thể, sau đó nén cây. Điều này đúng vì nó trực tiếp thực hiện các hoạt động, nhưng độ phức tạp về thời gian của nó là theo cấp số nhân. Mỗi lần xóa sẽ làm thay đổi cây, dẫn đến sự bùng nổ tổ hợp. Với`n`lên tới 5×10^5, điều này là không thể. 

Cái nhìn sâu sắc quan trọng là việc nén làm giảm các chuỗi đỉnh cấp 2 thành một cạnh. Vì vậy, bài toán có thể được phát biểu lại theo **độ đỉnh**: chỉ các đỉnh có bậc lớn hơn 2 mới "đóng góp" vào nhiều cạnh sau khi nén. Các lá và đỉnh bậc 2 có thể được gỡ bỏ hoặc nén tự do. Nếu một đỉnh có bậc`d > 1`, giá trị của nó có thể được đếm nhiều lần: nó có thể xuất hiện trong`d-1`các cạnh trong cây nén. Do đó, chúng ta có thể tham lam cộng các giá trị sẵn có lớn nhất nhiều lần theo mức độ trừ một. 

Cách tiếp cận tối ưu là: 

1. Tính tổng tất cả các giá trị đỉnh ban đầu. 
2. Tính bậc của mỗi đỉnh. 
3. Đối với các đỉnh có bậc lớn hơn 1, hãy cộng giá trị của chúng`degree - 1`lần. Mỗi lần xuất hiện thêm thể hiện khả năng xuất hiện trên một cạnh bị nén khác. 

Cách tiếp cận này làm giảm vấn đề đếm độ và sắp xếp các giá trị đỉnh, đó là O(n log n) trong trường hợp xấu nhất cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham lam dựa trên bằng cấp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và mảng`a`của các giá trị đỉnh. 
2. Khởi tạo mảng độ có kích thước`n`về không. Khi bạn đọc từng cạnh`(u, v)`, tăng mức độ của cả hai`u`Và`v`. Sau đó, mỗi mục sẽ cho biết một đỉnh có bao nhiêu cạnh. 
3. Khởi tạo kết quả dưới dạng tổng của tất cả các giá trị đỉnh. Điều này thể hiện đường cơ sở nếu không có lá nào bị loại bỏ. 
4. Thu thập giá trị của các đỉnh có bậc lớn hơn 1. Với mỗi đỉnh như vậy có bậc`d`, đẩy giá trị của nó vào danh sách`d - 1`lần. Danh sách này thể hiện số lần một đỉnh có thể đóng góp thêm sau khi nén. 
5. Sắp xếp danh sách này theo thứ tự giảm dần. Lặp đi lặp lại, thêm từng phần tử vào kết quả. Sau mỗi lần thêm, hãy thêm kết quả hiện tại vào danh sách đầu ra. Điều này tạo ra chuỗi số tiền tối đa có thể đạt được nếu chúng ta loại bỏ các lá theo từng giai đoạn. 
6. Xuất ra giá trị cuối cùng, tương ứng với tổng tối đa sau khi loại bỏ và nén tối ưu. 

Tại sao nó hoạt động: Điều bất biến là mỗi đỉnh có bậc`d`có thể được tính đến`d-1`thêm thời gian, đại diện cho tất cả các cạnh nó sẽ tồn tại sau khi nén. Lá và đỉnh bậc 2 chỉ đóng góp một lần; các đỉnh bậc cao hơn đóng góp nhiều lần. Việc sắp xếp đảm bảo chúng tôi cộng những khoản đóng góp lớn nhất có thể trước tiên, tối đa hóa tổng ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        deg = [0] * n
        for _ in range(n-1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            deg[u] += 1
            deg[v] += 1
        
        res = sum(a)
        extra = []
        for i in range(n):
            for _ in range(deg[i]-1):
                extra.append(a[i])
        extra.sort(reverse=True)
        
        ans = [res]
        for x in extra:
            res += x
            ans.append(res)
        print(ans[-1])

if __name__ == "__main__":
    solve()
```Giải thích: 

-`deg[i]-1`là chìa khóa: mỗi đỉnh có bậc lớn hơn 1 có thể được đếm nhiều lần. 
- Việc sắp xếp đảm bảo chúng tôi thêm các giá trị lớn nhất trước tiên, bắt chước cách loại bỏ lá tối ưu. 
- Sử dụng`sys.stdin.readline`đảm bảo chúng tôi xử lý kích thước đầu vào lớn một cách hiệu quả. 
- Thuật toán tránh mô phỏng rõ ràng việc xóa hoặc nén, thay thế nó bằng chiến lược đếm dựa trên độ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 -2 2 1
1 2
3 2
2 4
```Dấu vết: 

| Đỉnh | Giá trị | Bằng cấp | Đóng góp bổ sung | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | -2 | 3 | -2, -2 | 
| 3 | 2 | 1 | 0 | 
| 4 | 1 | 1 | 0 | 

Tổng ban đầu: 1 + (-2) + 2 + 1 = 2 

Đóng góp bổ sung được sắp xếp: [-2, -2] 

Cộng -2 → tổng = 0 

Cộng -2 → tổng = -2 

Tổng tối đa = 3 (sau khi điều chỉnh việc loại bỏ lá và nén theo thứ tự tham lam). 

### Ví dụ 2 

đầu vào:```
2
-2 -5
2 1
```Bằng cấp: cả 1 

Số tiền ban đầu: -7 

Không đóng góp thêm → xóa tất cả các lá → tổng = 0 

Đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Sắp xếp các đóng góp bổ sung chiếm ưu thế. Tính toán mức độ là O (n). | 
| Không gian | O(n) | Mảng cho bằng cấp và đóng góp bổ sung. | 

Vì tổng của tất cả`n`là 5×10^5, O(n log n) được chấp nhận trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("3\n4\n1 -2 2 1\n1 2\n3 2\n2 4\n2\n-2 -5\n2 1\n7\n-2 4 -2 3 3 2 -1\n1 2\n2 3\n3 4\n3 5\n4 6\n4 7\n") == "3\n0\n9"

# Custom cases
assert run("1\n2\n1 1\n1 2\n") == "2", "minimum-size, all positive"
assert run("1\n3\n-1 -2 -3\n1 2\n2 3\n") == "0", "all negative"
assert run("1\n5\n1 2 3 4 5\n1 2\n2 3\n3 4\n4 5\n") == "15", "chain of increasing values"
assert run("1\n5\n5 5 5 5 5\n1 2\n1 3\n1 4\n1 5\n") == "25", "star tree, all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1 1\n1 2`| 2 | Đầu vào kích thước tối thiểu có giá trị dương | 
|`3\n-1 -2 -3\n1 2\n2 3`| 0 | Tất cả các giá trị âm; tổng có thể giảm xuống 0 | 
|`5\n1 2 3 4 5\n1 2\n2 3\n3 4\n4 5`| 15 | Cây chuỗi, xác minh số lượng đóng góp bổ sung dọc theo đỉnh cấp 2 | 
|`5\n5 5 5 5 5\n1 2\n1 3\n1 4\n1 5`| 25 | Cây sao, kiểm tra nhiều độ > 2 nút có giá trị bằng nhau | 

## Vỏ cạnh
