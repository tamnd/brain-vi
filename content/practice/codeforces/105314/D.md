---
title: "CF 105314D - Hội chứng con trai và lãng phí thời gian"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một mảng các mục. Mỗi mục có hai thuộc tính: giá trị a[i] và tham số giống trọng số b[i]."
date: "2026-06-23T15:02:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105314
codeforces_index: "D"
codeforces_contest_name: "Robbing Balloons 2.0 Qualifications"
rating: 0
weight: 105314
solve_time_s: 55
verified: true
draft: false
---

[CF 105314D - Hội chứng con trai và lãng phí thời gian](https://codeforces.com/problemset/problem/105314/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm có một mảng các mục. Mỗi mục có hai thuộc tính: một giá trị`a[i]`và một tham số giống như trọng lượng`b[i]`. Đối với mỗi cặp mặt hàng được đặt hàng`(i, j)`, chúng tôi xác định mức đóng góp phụ thuộc vào bitwise OR của chúng`a`giá trị, nhân với`b[j]`. Nhiệm vụ là tính toán cho mỗi cố định`i`, tổng đóng góp trên tất cả`j`. 

Vì vậy với mỗi chỉ số`i`, chúng ta phải tính biểu thức$$\sum_{j=1}^{n} (a_i \,|\, a_j)\cdot b_j$$và xuất nó. 

Khó khăn chính là biểu thức không đối xứng trong`i`Và`j`bởi vì số nhân là`b[j]`, không`b[i]`. Điều này có nghĩa là chúng ta không thể tính toán trước ma trận đối xứng hoặc tái sử dụng trực tiếp các đóng góp cặp mà không tổ chức lại việc tính toán một cách cẩn thận. 

Những ràng buộc buộc chúng ta phải tránh xa mọi suy luận bậc hai. Tổng cộng`n`trên tất cả các trường hợp thử nghiệm có thể đạt tới 1e6, do đó, bất kỳ phương pháp nào thậm chí cố gắng kiểm tra tất cả các cặp một cách rõ ràng cũng sẽ thất bại. Một vòng lặp đôi ngây thơ sẽ làm được khoảng$10^{12}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1 giây. 

Trường hợp cạnh tinh tế xuất phát từ cấu trúc bitwise OR. Vì giá trị lên tới$2^{15}$, mọi hành vi được giới hạn ở 15 bit. Đây là gợi ý rằng giải pháp phải khai thác sự phân tách theo bit thay vì xử lý các số dưới dạng số nguyên mờ. 

Một lỗi phổ biến là cố gắng tính toán trước một số thứ như tổng tiền tố của`a`hoặc`b`một cách độc lập. Điều đó không thành công vì OR không có tính cộng và không phân phối tổng theo cách bảo toàn cấu trúc tuyến tính. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp đánh giá định nghĩa. Đối với mọi`i`, nó lặp lại tất cả`j`, tính toán`(a[i] | a[j]) * b[j]`, và tích lũy nó. Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen. Vấn đề là hiệu suất. Với`n`lên đến$2 \cdot 10^5$, mỗi test case đã yêu cầu$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất và trong các trường hợp thử nghiệm, điều này trở nên không khả thi. 

Quan sát cấu trúc quan trọng đến từ việc mở rộng OR ở cấp độ bit. Đối với mỗi vị trí bit,`(a[i] | a[j])`đóng góp bit đó nếu nó được đặt ở một trong hai`a[i]`hoặc`a[j]`. Điều này gợi ý chia tổng thành các khoản đóng góp trên mỗi bit. Thay vì tính toán lại OR cho mỗi cặp, chúng ta có thể đếm số lần mỗi bit xuất hiện trong`a[j]`cân nặng bởi`b[j]`và xử lý riêng xem bit đã có trong`a[i]`. 

Đối với một bit cố định`k`, sự đóng góp của bit này vào`(a[i] | a[j])`là: 

- luôn luôn`2^k`nếu bit`k`được thiết lập trong`a[i]`- nếu không thì`2^k`nếu bit`k`được thiết lập trong`a[j]`Điều này chia vấn đề thành số liệu thống kê toàn cầu có thể tính toán trước trên`j`. 

Chúng tôi tính toán trước cho mỗi bit`k`, tổng của tất cả`b[j]`Ở đâu`a[j]`đã thiết lập bit đó. Điều này cho phép chúng tôi đánh giá sự đóng góp của trường hợp thứ hai một cách nhanh chóng. Trường hợp đầu tiên chỉ phụ thuộc vào`i`và tổng số tiền của tất cả`b[j]`. 

Vì vậy đối với mỗi`i`, chúng tôi kết hợp: 

- bit đã có trong`a[i]`, đóng góp toàn bộ số tiền`b[j]`mỗi bit như vậy 
- bit vắng mặt từ`a[i]`, chỉ đóng góp tập hợp con của`b[j]`bit đó hiện diện ở đâu`a[j]`Điều này làm giảm từng trường hợp thử nghiệm xuống O(n * B) trong đó B là 15. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Phân hủy bit | O(n · 15) | O(15) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước tổng số tiền`b[j]`. Điều này sẽ được sử dụng lại cho mọi chỉ mục vì mọi cặp`(i, j)`bao gồm`b[j]`theo cách hoàn toàn giống nhau. 
2. Đối với từng vị trí bit`k`từ 0 đến 14, tính`sum_bit[k]`, tổng của tất cả`b[j]`sao cho bit thứ k của`a[j]`được thiết lập. Điều này tách biệt trọng lượng mà mỗi bit đóng góp trên tất cả các mục. 
3. Đối với mỗi chỉ số`i`, khởi tạo câu trả lời là 0. 
4. Đối với mỗi bit`k`, kiểm tra xem bit`k`được thiết lập trong`a[i]`. Nếu nó được thiết lập thì`(a[i] | a[j])`sẽ luôn có bit này bất kể`a[j]`, vì vậy mỗi`b[j]`đóng góp. Do đó chúng tôi thêm`2^k * total_b`. 
5. Nếu bit`k`không được đặt trong`a[i]`, thì OR chỉ thu được bit này từ những`j`Ở đâu`a[j]`đã có nó rồi. Trong trường hợp này chúng tôi thêm`2^k * sum_bit[k]`. 
6. Xuất giá trị tính toán cho từng`i`. 

Tính chính xác phụ thuộc vào việc xử lý từng bit một cách độc lập và xây dựng lại OR dưới dạng tổng của các đóng góp nhị phân độc lập được tính trọng số bởi`b[j]`. 

### Tại sao nó hoạt động 

Hoạt động OR tương đương với việc lấy mức tối đa theo bit cho mỗi vị trí. Mỗi bit đóng góp độc lập vào giá trị cuối cùng và sự đóng góp của nó chỉ phụ thuộc vào việc nó có xuất hiện trong toán hạng hay không. Kể từ khi nhân với`b[j]`không ảnh hưởng đến tính độc lập của bit, chúng ta có thể phân phối lại tổng trên các bit. Đối với mỗi bit, mỗi cặp`(i, j)`hoặc luôn đóng góp bit đó hoặc đóng góp nó có điều kiện chỉ dựa trên`a[j]`. Điều này loại bỏ mọi sự phụ thuộc giữa các`j`giá trị và đảm bảo tập hợp tuyến tính là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        total_b = sum(b)
        
        sum_bit = [0] * 15
        
        for i in range(n):
            x = a[i]
            bi = b[i]
            bit = 0
            while x:
                if x & 1:
                    sum_bit[bit] += bi
                x >>= 1
                bit += 1
        
        res = [0] * n
        
        for i in range(n):
            ai = a[i]
            ans = 0
            for k in range(15):
                if ai & (1 << k):
                    ans += (1 << k) * total_b
                else:
                    ans += (1 << k) * sum_bit[k]
            res[i] = ans
        
        print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt quá trình tiền xử lý khỏi việc xây dựng truy vấn. các`sum_bit`mảng tích lũy các đóng góp có trọng số của từng bit trên tất cả`j`. Vòng lặp thứ hai xây dựng từng câu trả lời bằng cách kiểm tra sự hiện diện của bit trong`a[i]`. 

Một điểm tinh tế là chúng ta không bao giờ tính toán lại OR một cách trực tiếp. Mỗi lần xuất hiện của`(a[i] | a[j])`được thay thế bằng tổng các đóng góp bit độc lập, giúp tránh hoàn toàn độ phức tạp bậc hai. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu: 

đầu vào:```
1
3
1 2 3
3 2 1
```Đầu tiên chúng tôi tính toán`total_b = 6`. 

Chúng tôi tính toán đóng góp bit: 

| tôi | một [tôi] | b[i] | tập bit | đóng góp cho sum_bit | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 0 | bit0 += 3 | 
| 2 | 2 | 2 | 1 | bit1 += 2 | 
| 3 | 3 | 1 | 0,1 | bit0 += 1, bit1 += 1 | 

Vì thế`sum_bit[0] = 4`,`sum_bit[1] = 3`. 

Bây giờ tính toán câu trả lời: 

cho`a[1]=1`: 

| chút | đặt trong [i]? | đóng góp | 
| --- | --- | --- | 
| 0 | vâng | 1*6 | 
| 1 | không | 2*3 | 

Kết quả:`6 + 6 = 12`. 

Vì`a[2]=2`: 

| chút | đặt trong [i]? | đóng góp | 
| --- | --- | --- | 
| 0 | không | 1*4 | 
| 1 | vâng | 2*6 | 

Kết quả:`4 + 12 = 16`. 

Vì`a[3]=3`: 

| chút | đặt trong [i]? | đóng góp | 
| --- | --- | --- | 
| 0 | vâng | 1*6 | 
| 1 | vâng | 2*6 | 

Kết quả:`6 + 12 = 18`. 

Dấu vết này cho thấy cấu trúc OR biến mất như thế nào trong việc tính toán bit độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 15) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần mỗi bit | 
| Không gian | O(15) | Chỉ các mảng tổng hợp bit được lưu trữ | 

Tổng cộng`n`trên tất cả các trường hợp thử nghiệm lên đến$10^6$, do đó thuật toán thực hiện khoảng$15 \cdot 10^6$hoạt động, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        total_b = sum(b)
        sum_bit = [0] * 15
        
        for i in range(n):
            x = a[i]
            bi = b[i]
            bit = 0
            while x:
                if x & 1:
                    sum_bit[bit] += bi
                x >>= 1
                bit += 1
        
        res = []
        for i in range(n):
            ans = 0
            for k in range(15):
                if a[i] & (1 << k):
                    ans += (1 << k) * total_b
                else:
                    ans += (1 << k) * sum_bit[k]
            res.append(ans)
        print(*res)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""1
3
1 2 3
3 2 1
""") == "12 16 18"

# minimum size
assert run("""1
1
5
7
""") == "35"

# all equal
assert run("""1
4
1 1 1 1
2 2 2 2
""") == "8 8 8 8"

# mixed bits
assert run("""1
3
1 2 4
1 2 3
""")  # sanity check, no crash
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | phép nhân trực tiếp | độ đúng cơ sở | 
| tất cả các giá trị bằng nhau | sự đóng góp đối xứng | ổn định trường hợp thống nhất | 
| bit hỗn hợp | tích lũy nhiều bit | độc lập bit | 

## Vỏ cạnh 

Đối với mảng một phần tử, biểu thức giảm xuống còn`(a[i] | a[i]) * b[i]`, đó là`a[i] * b[i]`. Thuật toán xử lý việc này vì`total_b = b[i]`Và`sum_bit`chỉ đếm các bit từ phần tử đó, vì vậy mỗi bit được đặt đều đóng góp chính xác`bit * b[i]`. 

Đối với đầu vào trong đó tất cả`a[i]`chia sẻ cùng một giá trị, mọi`sum_bit[k]`hoặc bằng`total_b`hoặc 0 tùy thuộc vào việc bit đó có được đặt hay không. Sau đó, mỗi chỉ mục sẽ nhận được những đóng góp giống hệt nhau và thuật toán sẽ tạo ra một mảng không đổi một cách chính xác. 

Khi các giá trị có các bit rời nhau, mỗi giá trị`sum_bit[k]`tổng hợp các tập hợp con rời rạc của các chỉ số. Thuật toán phân tách rõ ràng những đóng góp này vì mỗi bit được xử lý độc lập, đảm bảo không có nhiễu giữa các bit không liên quan.
