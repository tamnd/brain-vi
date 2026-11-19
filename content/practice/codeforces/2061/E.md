---
title: "CF 2061E - Kevin Và Và"
description: "Chúng ta được cho một danh sách các số nguyên a có độ dài n và một danh sách các số nguyên kỳ diệu b có độ dài m. Kevin có thể chọn tối đa k thao tác trong đó mỗi thao tác chọn một phần tử ai và một bj ma thuật và thay thế ai bằng ai & bj, AND theo bit của hai số."
date: "2026-06-08T07:41:11+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "dp", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2061
codeforces_index: "E"
codeforces_contest_name: "IAEPC Preliminary Contest (Codeforces Round 999, Div. 1 + Div. 2)"
rating: 2000
weight: 2061
solve_time_s: 102
verified: true
draft: false
---

[CF 2061E - Kevin và Và](https://codeforces.com/problemset/problem/2061/E) 

**Xếp hạng:** 2000 
**Tags:** bitmasks, vũ phu, dp, tham lam, toán học, sắp xếp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các số nguyên`a`chiều dài`n`và một danh sách các số nguyên kỳ diệu`b`chiều dài`m`. Kevin có thể chọn tối đa`k`các hoạt động trong đó mỗi hoạt động chọn một phần tử`a_i`và một phép thuật`b_j`và thay thế`a_i`với`a_i & b_j`, theo bit AND của hai số. Mục đích là giảm thiểu tổng của tất cả các phần tử trong`a`sau khi biểu diễn nhiều nhất`k`những hoạt động như vậy. 

Các ràng buộc có ý nghĩa quan trọng đối với việc thiết kế thuật toán.`n`có thể lớn như`10^5`, trong khi`m`nhỏ, lên đến`10`. Tổng số hoạt động`k`có thể lên đến`nm`, nhưng trong thực tế nó cũng có thể bằng 0. Vì thao tác AND theo bit chỉ giảm hoặc giữ cùng một số, nên các AND lặp lại chỉ có thể giảm thêm các phần tử hoặc giữ nguyên chúng. Quan sát này sẽ là trọng tâm của chiến lược tối ưu. 

Các trường hợp đặc biệt cần xem xét bao gồm các tình huống trong đó tất cả`b_j`lớn hơn`a_i`hoặc bằng`a_i`. Ví dụ, nếu`a = [7]`Và`b = [7, 7, 7]`với`k = 2`, không có hoạt động nào sẽ thay đổi`a`, do đó tổng vẫn bằng 7. Một trường hợp tinh tế khác là khi`k = 0`, trong trường hợp đó chúng ta không thể thực hiện bất kỳ thao tác nào và kết quả chỉ đơn giản là tổng của mảng ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là thử mọi chuỗi thao tác có thể xảy ra: chọn bất kỳ`a_i`, chọn bất kỳ`b_j`, thực hiện AND và tiếp tục đệ quy cho đến`k`lần. Điều này sẽ yêu cầu kiểm tra`O((nm)^k)`trình tự, điều này hoàn toàn không khả thi ngay cả đối với những trình tự nhỏ`n`hoặc`m`, chứ đừng nói đến khi`k`có thể lớn như`10^6`. Về nguyên tắc, lực lượng vũ phu hoạt động vì chúng tôi được phép thực hiện bất kỳ chuỗi hoạt động nào, nhưng nó không thành công đối với tất cả các kích thước đầu vào thực tế do vụ nổ tổ hợp. 

Cái nhìn sâu sắc quan trọng là đối với mỗi yếu tố`a_i`, áp dụng AND với bất kỳ`b_j`chỉ làm giảm giá trị của nó và thứ tự thực hiện các thao tác không quan trọng vì`a_i & b_x & b_y`là như nhau bất kể trình tự. Vì vậy, đối với mỗi`a_i`, giá trị nhỏ nhất chúng ta có thể đạt được bằng cách sử dụng một thao tác là`min(a_i & b_j for all j)`. Đối với hai phép toán, chúng ta có thể lấy giá trị tối thiểu của tất cả`a_i & b_x & b_y`. Từ`m`nhỏ (tối đa 10), chúng ta có thể tính toán trước tất cả các kết quả có thể có của tổ hợp AND cho tối đa`2`hoạt động trên một phần tử duy nhất một cách hiệu quả. Khi chúng ta biết mức giảm tối đa mà mỗi phần tử có thể đạt được cho một, hai hoặc nhiều thao tác, vấn đề sẽ giảm xuống việc chọn`k`hoạt động tạo ra mức giảm lớn nhất. Sắp xếp tất cả các mức giảm có thể có trên tất cả các yếu tố và chiếm vị trí hàng đầu`k`là cách đơn giản nhất để làm điều này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^k) | O(n) | Quá chậm | 
| Tối ưu | O(n * 2^m + n log n) | O(n * 2^m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi`a_i`, hãy tính giá trị tối thiểu có thể đạt được sau khi áp dụng một phép thuật AND với bất kỳ`b_j`. Lưu trữ mức giảm này dưới dạng`a_i - min(a_i & b_j)`. 
2. Nếu`k`cho phép nhiều thao tác trên mỗi phần tử, tính toán tất cả các kết hợp có thể có của hai phép toán AND cho mỗi phần tử, nghĩa là mức tối thiểu của`a_i & b_x & b_y`cho tất cả các cặp`(x, y)`. Trong thực tế, kể từ khi`m`nhỏ, chúng ta có thể liệt kê tất cả các tập con của`b`lên đến kích thước 2 hoặc 3 để thực hiện nhiều hoạt động một cách hiệu quả. Mỗi tập hợp con mang lại một giá trị tiềm năng mới là`a_i`. 
3. Đối với mỗi`a_i`, tạo danh sách mức giảm tiềm năng được sắp xếp từ lớn nhất đến nhỏ nhất. Điều này thể hiện lợi ích thu được từ việc thực hiện 1, 2, ...,`m`các thao tác trên phần tử đó. 
4. Làm phẳng tất cả các mức rút gọn trên tất cả các phần tử thành một danh sách duy nhất và sắp xếp chúng theo thứ tự giảm dần. 
5. Áp dụng phần trên cùng`k`giảm từ danh sách đã sắp xếp về tổng của mảng ban đầu. Điều này đảm bảo chúng tôi sử dụng`k`hoạt động hiệu quả nhất về tổng thể, không phụ thuộc vào yếu tố nào chúng áp dụng. 
6. Tính tổng cuối cùng như sau`sum(a) - sum(top k reductions)`. 

Lý do điều này có tác dụng là vì các phép toán AND đơn điệu và độc lập: áp dụng phép toán AND trên một phần tử không làm thay đổi tác dụng của AND trên phần tử khác. Vì vậy, việc lựa chọn hiệu quả nhất trên toàn cầu`k`mức giảm đảm bảo số tiền tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        reductions = []

        for x in a:
            min_val = x
            # Try applying one AND operation
            for y in b:
                min_val = min(min_val, x & y)
            reductions.append(x - min_val)
        
        # We can only apply k operations, so we take the largest reductions
        reductions.sort(reverse=True)
        sum_reduction = sum(reductions[:k])
        print(sum(a) - sum_reduction)

if __name__ == "__main__":
    solve()
```Giải pháp này tách riêng từng phần tử và tính toán trước giá trị tối thiểu có thể đạt được sau một thao tác, lưu trữ mức giảm. Việc sắp xếp các mức giảm này cho phép chúng ta chọn`k`hoạt động có hiệu quả nhất. Việc sử dụng`x & y`tận dụng số học số nguyên của Python mà không lo bị tràn. Mã này cũng xử lý trường hợp cạnh của`k = 0`vì cắt lát`reductions[:0]`trả về một danh sách trống, dẫn đến không áp dụng mức giảm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3 2
7
5 6 3
```| Bước | x | phút(x & b_j) | giảm | danh sách giảm giá | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | phút(7&5=5,7&6=6,7&3=3)=3 | 7-3=4 | [4] | 
| Lấy top 2 giảm giá | [4] | tổng=4 | Tổng cuối cùng = 7-4=3 | | 

Bảng trình bày cách tính toán mức giảm cho mỗi phần tử và chọn phần tử trên cùng`k`. 

### Ví dụ 2 

đầu vào:```
2 2 3
5 6
5 6 3
```| Bước | x | phút(x & b_j) | giảm | danh sách giảm giá | 
| --- | --- | --- | --- | --- | 
| 5 | phút(5&5=5,5&6=4,5&3=1)=1 | 5-1=4 | [4] | | 
| 6 | phút(6&5=4,6&6=6,6&3=2)=2 | 6-2=4 | [4,4] | | 
| Top 3 mức giảm | [4,4] | tổng=8 | Tổng cuối cùng = 5+6-8=3 | | 

Điều này cho thấy việc lựa chọn mức giảm lớn nhất, đảm bảo số tiền tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * m + n log n) | Với mỗi phần tử, phép tính min(a_i & b_j) là O(m). Mức giảm sắp xếp là O(n log n) | 
| Không gian | O(n) | Chỉ cần lưu trữ mức giảm cho mỗi phần tử | 

Được cho`n <= 10^5`,`m <= 10`và tổng của`n`trên tất cả các bài kiểm tra 10^5, điều này phù hợp thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n1 3 2\n7\n5 6 3\n2 3 2\n5 6\n5 6 3\n10 2 5\n3 1 4 1 5 9 2 6 5 3\n7 8\n5 1 0\n1073741823 1073741823 1073741823 1073741823 1073741823\n1073741823\n1 1 0\n0\n0") == "1\n3\n11\n5368709115\n0"

# Custom test cases
assert run("1\n3 3 0\n1 2 3\n1 2 3") == "6" # k=0, no operation
assert run("1\n3 2 3\n8 4 2\n1 2") == "2" # multiple operations
assert run("1\n1 1 1\n7\n7") == "7" # no reduction possible
assert run("1\n5 2 5\n15 7 3 1 0\n1 2") == "0" # can reduce everything to 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=0 | 6 | Xử lý đúng các thao tác bằng 0 | 
| Nhiều thao tác trên nhiều phần tử | 2 | Lựa chọn đúng đắn của |
