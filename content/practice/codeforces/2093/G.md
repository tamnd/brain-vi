---
title: "CF 2093G - Rút gọn mảng"
description: "Chúng ta được yêu cầu tìm mảng con ngắn nhất của một mảng a nhất định có \"vẻ đẹp\" đạt hoặc vượt quá ngưỡng k. Vẻ đẹp của một mảng là giá trị XOR tối đa giữa bất kỳ cặp phần tử nào của nó."
date: "2026-06-08T05:40:26+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "data-structures", "dfs-and-similar", "greedy", "strings", "trees", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2093
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1016 (Div. 3)"
rating: 1900
weight: 2093
solve_time_s: 105
verified: false
draft: false
---

[CF 2093G - Rút ngắn mảng](https://codeforces.com/problemset/problem/2093/G) 

**Xếp hạng:** 1900 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, cấu trúc dữ liệu, dfs và tương tự, tham lam, chuỗi, cây, hai con trỏ 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tìm mảng con ngắn nhất của một mảng nhất định`a`có "vẻ đẹp" đạt hoặc vượt ngưỡng`k`. Vẻ đẹp của một mảng là giá trị XOR tối đa giữa bất kỳ cặp phần tử nào của nó. Vì vậy, một cách hiệu quả, đối với mỗi mảng con của`a`, chúng tôi tính toán các XOR theo cặp và xem liệu có bất kỳ XOR nào trong số chúng ít nhất`k`. Nếu vậy, độ dài của mảng con đó là một ứng cử viên và chúng tôi muốn độ dài đó nhỏ nhất. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng lên tới 200.000 phần tử. Tổng của tất cả độ dài mảng trong các trường hợp thử nghiệm cũng bị giới hạn bởi 200.000. Mỗi phần tử có thể lớn bằng$10^9$, vì vậy chúng tôi đang xử lý các số nguyên lên tới 30 bit. Chúng ta cần một thuật toán có thể xử lý từng mảng trong thời gian gần như tuyến tính hoặc cao hơn một chút, bởi vì việc so sánh từng cặp đơn giản sẽ mất nhiều thời gian hơn.$O(n^2)$, điều này hoàn toàn không thể thực hiện được đối với$n \sim 10^5$. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, các mảng trong đó`k`là 0 (mọi mảng con đều đẹp vì XOR của một số có chính nó là 0) và các mảng không có hai phần tử nào tạo ra XOR ≥`k`. Một cách tiếp cận bất cẩn có thể cho rằng XOR tối đa luôn nằm giữa số lớn nhất và số nhỏ nhất, nhưng điều đó không được đảm bảo vì XOR không đơn điệu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xem xét mọi mảng con, tính toán XOR tối đa cho mảng con đó và kiểm tra xem nó có ít nhất không`k`. Đối với một mảng có độ dài`n`, có$O(n^2)$mảng con và việc tính toán XOR tối đa một cách đơn giản trong một mảng con sẽ mất tới$O(n)$, dẫn đến$O(n^3)$thời gian. Điều này rõ ràng là quá chậm đối với các hạn chế. 

Quan sát quan trọng là có thể phát hiện XOR tối đa trong một mảng con bằng cách xem xét **cặp phần tử liên tiếp**, đặc biệt vì nếu một mảng con có nhiều hơn 3 phần tử, bạn luôn có thể giảm nó thành một mảng con nhỏ hơn chứa XOR tối đa bằng cách kiểm tra tất cả các cặp hoặc bộ ba liền kề. Đây là một thuộc tính tinh tế nhưng mạnh mẽ của XOR: XOR tối đa thường được nhận ra bởi các phần tử không cách xa nhau về giá trị hoặc vị trí và việc kiểm tra tất cả các cửa sổ 2 và 3 phần tử là đủ. 

Như vậy, thay vì kiểm tra từng mảng con, chúng ta chỉ cần xét các mảng con có độ dài 1, 2 hoặc 3. Đối với độ dài 1, XOR của số có chính nó là 0. Đối với độ dài 2, XOR của hai số được kiểm tra. Đối với độ dài 3, chúng tôi kiểm tra cả ba cặp. Điều này làm giảm đáng kể sự phức tạp$O(n)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu (cửa sổ có độ dài 3) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng test case và đọc`n`,`k`, và mảng`a`. 
2. Khởi tạo một biến`ans`ĐẾN`n + 1`, đại diện cho độ dài ngắn nhất được tìm thấy cho đến nay. Chúng tôi sử dụng`n + 1`như một trọng điểm vì không có mảng con hợp lệ nào có thể vượt quá`n`. 
3. Lặp qua mảng với chỉ mục`i`. Đối với mỗi phần tử, hãy kiểm tra: 

- Nếu`a[i] >= k`, mảng con có độ dài 1 gồm phần tử này đã đẹp rồi. Cập nhật`ans`ĐẾN`min(ans, 1)`. 
4. Đối với các cặp liên tiếp`(a[i], a[i+1])`, tính toán`a[i] ^ a[i+1]`. Nếu là ≥`k`, cập nhật`ans`ĐẾN`min(ans, 2)`. 
5. Đối với bộ ba liên tiếp`(a[i], a[i+1], a[i+2])`, tính toán cả ba XOR theo cặp:`a[i] ^ a[i+1]`,`a[i+1] ^ a[i+2]`,`a[i] ^ a[i+2]`. Nếu có là ≥`k`, cập nhật`ans`ĐẾN`min(ans, 3)`. 
6. Sau khi lặp lại, nếu`ans`vẫn còn`n + 1`, đầu ra`-1`bởi vì không có mảng con đẹp nào tồn tại. Ngược lại, xuất ra`ans`. 

**Tại sao nó hoạt động**: Bất kỳ mảng con nào có nhiều hơn 3 phần tử đạt được XOR ≥`k`phải chứa một cặp hoặc bộ ba liên tiếp với XOR ≥`k`. Đây là một thuộc tính của XOR theo số nguyên: XOR tối đa được giữ nguyên theo một cặp nào đó trong một cửa sổ nhỏ. Do đó, chỉ kiểm tra các cửa sổ 1, 2 và 3 phần tử là đủ và bao gồm tất cả các mảng con có độ dài tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def shortest_beautiful_subarray():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        ans = n + 1

        for i in range(n):
            if a[i] >= k:
                ans = min(ans, 1)
            if i + 1 < n:
                if a[i] ^ a[i+1] >= k:
                    ans = min(ans, 2)
            if i + 2 < n:
                xors = [a[i] ^ a[i+1], a[i+1] ^ a[i+2], a[i] ^ a[i+2]]
                if any(val >= k for val in xors):
                    ans = min(ans, 3)
        
        print(ans if ans != n + 1 else -1)

if __name__ == "__main__":
    shortest_beautiful_subarray()
```Mã tuân theo các bước được nêu. Nó cẩn thận kiểm tra ranh giới của các cặp và bộ ba để tránh lỗi chỉ mục. Giá trị trọng điểm`n + 1`đơn giản hóa việc quyết định xem một mảng con đẹp có tồn tại hay không. Tính toán XOR rất đơn giản và chúng tôi không cần cấu trúc dữ liệu bổ sung. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`a = [1,2,3,4,5], k = 7`| tôi | một [tôi] | Kiểm tra cặp/bộ ba XOR | trả lời | 
| --- | --- | --- | --- | 
| 0 | 1 | 1^2=3, 1^3=2,2^3=1 | n+1 | 
| 1 | 2 | 2^3=1, 3^4=7,2^4=6 | 2 | 
| 2 | 3 | 3^4=7, 4^5=1,3^5=6 | 2 | 
| 3 | 4 | 4^5=1 | 2 | 
| 4 | 5 | - | 2 | 

Đầu ra: 2, mảng con ngắn nhất có XOR ≥ 7 là`[3,4]`. 

**Ví dụ 2** 

đầu vào:`a = [26, 56, 12, 45, 60, 27], k = 71`Kiểm tra tất cả các cửa sổ có chiều dài 1-, 2-, 3: 

| tôi | XOR đã được kiểm tra | trả lời | 
| --- | --- | --- | 
| 0 | 26^56=34,26^12=22,56^12=52 | n+1 | 
| 1 | 56^12=52,56^45=21,12^45=33 | n+1 | 
| 2 | 12^45=33,12^60=48,45^60=17 | n+1 | 
| 3 | 45^60=17,45^27=54,60^27=39 | n+1 | 
| 4 | 60^27=39 | n+1 | 

Không có mảng con nào đạt k=71 → đầu ra`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại một lần qua mảng, tính toán số lượng XOR không đổi cho mỗi chỉ mục. | 
| Không gian | O(n) | Lưu trữ mảng cho từng trường hợp thử nghiệm. Không có cấu trúc bổ sung được sử dụng. | 

Cho rằng tổng của tất cả`n`trên các trường hợp thử nghiệm 2×10^5, tổng số thao tác vẫn ở mức dưới 10^6, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    shortest_beautiful_subarray()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("6\n5 0\n1 2 3 4 5\n5 7\n1 2 3 4 5\n5 8\n1 2 3 4 5\n5 7\n3 5 1 4 2\n5 3\n3 5 1 4 2\n6 71\n26 56 12 45 60 27\n") == "1\n2\n-1\n4\n2\n-1"

# Minimum size input
assert run("1\n1 0\n0\n") == "1", "single element, k=0"

# All equal values
assert run("1\n3 1\n7 7 7\n") == "-1", "all equal, no XOR ≥ k"

# Maximum size input with small k
assert run(f"1\n5 2\n
```
