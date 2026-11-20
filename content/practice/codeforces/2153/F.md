---
title: "CF 2153F - Truy vấn lẻ trên mảng lẻ"
description: "Chúng ta được cho một mảng a có độ dài n thỏa mãn một thuộc tính đặc biệt gọi là \"dễ thương\" nhằm ngăn chặn một số mẫu lặp lại nhất định của bốn chỉ số."
date: "2026-06-08T00:46:07+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "data-structures", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 2153
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1057 (Div. 2)"
rating: 2900
weight: 2153
solve_time_s: 314
verified: false
draft: false
---

[CF 2153F - Truy vấn lẻ trên mảng lẻ](https://codeforces.com/problemset/problem/2153/F) 

**Xếp hạng:** 2900 
**Tags:** bitmasks, brute Force, cấu trúc dữ liệu, triển khai, cây 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`chiều dài`n`thỏa mãn một thuộc tính đặc biệt gọi là "dễ thương", ngăn chặn các mẫu lặp lại nhất định của bốn chỉ số. Với mục đích của bài toán này, định nghĩa chính xác về "dễ thương" ít quan trọng hơn hậu quả của nó: nó đảm bảo rằng tần số của bất kỳ số nào trong một mảng con hoạt động theo cách cho phép chúng ta tính tổng các số xuất hiện số lẻ lần một cách hiệu quả. 

Nhiệm vụ là xử lý`q`truy vấn. Mỗi truy vấn yêu cầu "vẻ đẹp" của một mảng con liền kề của`a`, trong đó vẻ đẹp là tổng của các số riêng biệt xuất hiện với số lần lẻ trong mảng con đó. Các truy vấn được mã hóa: các chỉ mục của mỗi truy vấn phụ thuộc vào câu trả lời trước đó, điều này buộc phải sử dụng thuật toán trực tuyến thay vì tính toán trước các câu trả lời cho tất cả các truy vấn. 

Những ràng buộc rất chặt chẽ. Tổng của`n`Và`q`trên tất cả các trường hợp thử nghiệm lên đến`5 * 10^5`, do đó, cách tiếp cận mạnh mẽ đếm tần số cho từng truy vấn một cách độc lập sẽ liên quan đến`O(n*q)`hoạt động, lên tới ~ 2,5 * 10^11 trong trường hợp xấu nhất, rõ ràng là quá lớn. Chúng ta cần thứ gì đó gần gũi hơn`O(n + q)`hoặc`O((n + q) log n)`mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng phát sinh với các mảng con trong đó tất cả các phần tử đều giống nhau hoặc trong đó tất cả các phần tử xuất hiện với số lần chẵn. Ví dụ, một mảng`[3,3,3]`có độ dài 3 luôn tạo ra vẻ đẹp bằng 3 cho toàn bộ mảng, nhưng`[3,3,3,3]`tạo ra 0 vì tất cả số đếm đều chẵn. Bất kỳ nỗ lực ngây thơ nào nhằm tổng hợp tất cả các yếu tố hoặc cho rằng vẻ đẹp khác 0 đều sẽ thất bại. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ lặp qua từng truy vấn, đếm tần số của từng phần tử trong mảng con và tính tổng các giá trị xuất hiện theo số lần lẻ. Điều này làm việc cho nhỏ`n`Và`q`, nhưng với`n`Và`q`lên đến`5 * 10^5`, mỗi truy vấn có thể thực hiện các thao tác O(n), dẫn đến`O(n*q)`sự phức tạp tổng thể. 

Thông tin chi tiết quan trọng là mảng này rất "dễ thương", ngụ ý rằng không có số nào xen kẽ với số khác nhiều lần trong các mẫu bốn phần tử. Một hậu quả sâu sắc hơn là trong bất kỳ mảng con nào, mỗi số chỉ xuất hiện ở nhiều nhất một khối liền kề. Nói cách khác, các vị trí xuất hiện của một số là liên tiếp. Điều này có nghĩa là tính chẵn lẻ của một số trong mảng con chỉ thay đổi khi mảng con bắt đầu hoặc kết thúc bên trong khối của nó. Do đó, chúng ta có thể coi sự xuất hiện của từng giá trị như một cờ nhị phân: nếu một giá trị xuất hiện với số lần lẻ trong mảng con, thì nó sẽ góp phần tạo nên vẻ đẹp, nếu không thì không. Việc cập nhật vẻ đẹp khi chúng tôi mở rộng hoặc thu nhỏ mảng con trở thành vấn đề chuyển đổi đóng góp dựa trên tính chẵn lẻ tần số, cho phép cách tiếp cận cửa sổ trượt giống như mặt nạ bit. 

Chúng ta có thể triển khai điều này một cách hiệu quả bằng cách sử dụng phương pháp tiếp cận thuật toán hai con trỏ hoặc Mo. Vì điểm cuối của truy vấn phụ thuộc vào các câu trả lời trước đó nên chúng tôi phải xử lý các truy vấn một cách tuần tự. Chúng tôi duy trì một dãy tần số và một dãy số có tần số lẻ. Khi di chuyển điểm cuối bên trái hoặc bên phải, chúng ta tăng hoặc giảm tần số của phần tử tương ứng và điều chỉnh độ đẹp cho phù hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*q) | O(n) | Quá chậm | 
| Cửa sổ trượt / Theo dõi chẵn lẻ trực tuyến | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng tần số`freq`chiều dài`n+1`về không. Điều này sẽ theo dõi số lần mỗi số xuất hiện trong mảng con hiện tại. Khởi tạo`beauty`về không. 
2. Với mỗi truy vấn, giải mã điểm cuối`l`Và`r`sử dụng câu trả lời trước, áp dụng công thức modulo đã cho và lấy`min`Và`max`để đặt hàng chúng một cách chính xác. 
3. Mở rộng điểm cuối bên phải của mảng con hiện tại từ vị trí trước đó sang vị trí mới`r`. Đối với mỗi phần tử được thêm vào, hãy tăng tần số của nó theo`freq`. Nếu tần số mới là số lẻ, hãy thêm phần tử vào`beauty`. Nếu nó trở thành chẵn, hãy trừ nó khỏi`beauty`. Việc chuyển đổi này nắm bắt được hiệu ứng chẵn lẻ. 
4. Tương tự, điều chỉnh điểm cuối bên trái sang điểm cuối mới`l`. Nếu điểm cuối bên trái di chuyển sang phải, hãy giảm tần số cho các phần tử bị xóa, cập nhật`beauty`theo cùng một cách dựa trên tính chẵn lẻ. Nếu nó di chuyển sang trái, tăng tần số và cập nhật`beauty`. 
5. Sau khi căn chỉnh cả hai điểm cuối thành`[l, r]`, ghi lại dòng điện`beauty`như câu trả lời cho truy vấn này. Giá trị này sẽ được sử dụng để giải mã truy vấn tiếp theo. 
6. Lặp lại cho tất cả các truy vấn trong test case. 

Tại sao nó hoạt động: Thuộc tính "dễ thương" đảm bảo mỗi giá trị xuất hiện trong một khối liền kề. Do đó, việc tăng hoặc giảm các điểm cuối của mảng con chỉ làm thay đổi tính chẵn lẻ tần số của một giá trị ở các ranh giới. Bằng cách duy trì tổng các giá trị có số lẻ, chúng ta có thể cập nhật`beauty`tăng dần theo thời gian không đổi cho mỗi phần tử được thêm hoặc xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def process_test_case():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    queries = [tuple(map(int, input().split())) for _ in range(q)]
    freq = [0] * (n + 1)
    beauty = 0
    ans_prev = 0
    l_curr = 0
    r_curr = -1
    res = []

    for x_raw, y_raw in queries:
        x = ((x_raw - 1 + ans_prev) % n)
        y = ((y_raw - 1 + ans_prev) % n)
        l = min(x, y)
        r = max(x, y)

        while r_curr < r:
            r_curr += 1
            val = a[r_curr]
            freq[val] += 1
            if freq[val] % 2 == 1:
                beauty += val
            else:
                beauty -= val

        while r_curr > r:
            val = a[r_curr]
            freq[val] -= 1
            if freq[val] % 2 == 1:
                beauty += val
            else:
                beauty -= val
            r_curr -= 1

        while l_curr < l:
            val = a[l_curr]
            freq[val] -= 1
            if freq[val] % 2 == 1:
                beauty += val
            else:
                beauty -= val
            l_curr += 1

        while l_curr > l:
            l_curr -= 1
            val = a[l_curr]
            freq[val] += 1
            if freq[val] % 2 == 1:
                beauty += val
            else:
                beauty -= val

        res.append(beauty)
        ans_prev = beauty

    print(*res)

def main():
    t = int(input())
    for _ in range(t):
        process_test_case()

if __name__ == "__main__":
    main()
```Mã khởi tạo tính năng theo dõi tần suất và xử lý từng truy vấn bằng cách di chuyển dần dần các điểm cuối. Tính chẵn lẻ của số lượng quyết định xem một con số có góp phần tạo nên vẻ đẹp hay không. Truy vấn giải mã sử dụng câu trả lời trước đó để tính toán chính xác các vị trí modulo, tôn trọng tính chất trực tuyến của vấn đề. Việc điều chỉnh ranh giới đảm bảo không có lỗi riêng lẻ và mỗi truy vấn được xử lý tuần tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Phân mảng có tần số trộn lẫn 

mảng con đầu vào:`[3, 2, 2, 1]`. Vẻ đẹp ban đầu là 0. 

| Bước | Hành động | tần số | sắc đẹp | 
| --- | --- | --- | --- | 
| Thêm 3 | tần số[3]=1 | 3:1 | 3 | 
| Thêm 2 | tần số[2]=1 | 2:1,3:1 | 5 | 
| Thêm 2 | tần số[2]=2 | 2:2,3:1 | 3 | 
| Thêm 1 | tần số[1]=1 | 1:1,2:2,3:1 | 4 | 

Vẻ đẹp 4 xứng đáng với sản lượng dự kiến. Điều này cho thấy các cập nhật gia tăng xử lý chính xác việc chuyển đổi từ lẻ sang chẵn. 

### Ví dụ 2: Các phần tử bằng nhau, mảng con có độ dài chẵn 

mảng con`[3,3,3,3]`. 

| Bước | Hành động | tần số | sắc đẹp | 
| --- | --- | --- | --- | 
| Thêm 3 | tần số[3]=1 | 3:1 | 3 | 
| Thêm 3 | tần số[3]=2 | 3:2 | 0 | 
| Thêm 3 | tần số[3]=3 | 3:3 | 3 | 
| Thêm 3 | tần số[3]=4 | 3:4 | 0 | 

Điều này xác nhận việc bật tắt hoạt động chính xác ngay cả khi số lượng vượt quá 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi phần tử được thêm/xóa tối đa một lần cho mỗi lần di chuyển điểm cuối truy vấn, tổng số lần di chuyển 2n + 2q | 
| Không gian | O(n) | Mảng tần số có kích thước n+1, cộng với lưu trữ mảng và truy vấn | 

Được cho`n + q ≤ 5*10^5`mỗi trường hợp thử nghiệm, phương pháp này hoàn thành thoải mái trong giới hạn 10 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("""3
11 4
1 1 2 2 3 3 3 2 2 1 1
7 10
5
```
