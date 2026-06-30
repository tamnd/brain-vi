---
title: "CF 104637D - Hình vuông và hình khối"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu số nguyên trong phạm vi từ 1 đến n có thể được viết dưới ít nhất một trong hai dạng đặc biệt: hình vuông hoàn hảo hoặc hình lập phương hoàn hảo. Nếu một số có thể được viết cả hai cách thì số đó vẫn chỉ được tính một lần."
date: "2026-06-29T16:59:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104637
codeforces_index: "D"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u0431\u0430\u0437\u043e\u0432\u0430\u044f \u043c\u0430\u0442\u0435\u043c\u0430\u0442\u0438\u043a\u0430, \u0443\u0441\u043b\u043e\u0432\u0438\u044f, \u0446\u0438\u043a\u043b\u044b"
rating: 0
weight: 104637
solve_time_s: 59
verified: true
draft: false
---

[CF 104637D – Hình vuông và hình khối](https://codeforces.com/problemset/problem/104637/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm xem có bao nhiêu số nguyên trong phạm vi từ 1 đến n có thể được viết dưới ít nhất một trong hai dạng đặc biệt: hình vuông hoàn hảo hoặc hình lập phương hoàn hảo. Nếu một số có thể được viết cả hai cách thì số đó vẫn chỉ được tính một lần. 

Vì vậy, đối với mỗi truy vấn n, về cơ bản chúng ta đang đếm kích thước hợp của hai tập hợp: tất cả các số có dạng i² và tất cả các số có dạng j³, giới hạn ở mức tối đa là n. 

Kích thước đầu vào nhỏ về số lượng trường hợp thử nghiệm, nhiều nhất là 20, nhưng giá trị của n lớn, lên tới 10⁹. Điều này ngay lập tức loại trừ mọi cách tiếp cận lặp qua tất cả các số nguyên lên đến n. Quét tuyến tính cho mỗi trường hợp thử nghiệm sẽ yêu cầu tối đa 10⁹ thao tác, vượt xa mức phù hợp trong một giây. 

Một hàm ý ràng buộc tinh tế hơn là bản thân câu trả lời được điều khiển bởi lũy thừa số nguyên. Cả hình vuông và hình khối đều phát triển nhanh chóng, do đó số lượng của chúng lên tới n là khoảng n^(1/2) và n^(1/3), nhiều nhất là khoảng 31623 và 1000 tương ứng khi n là 10⁹. Điều này cho thấy rằng việc lặp qua gốc là khả thi. 

Một sai lầm ngây thơ là đếm gấp đôi các số vừa là hình vuông vừa là hình lập phương. Ví dụ: 64 vừa là 8² vừa là 4³ nên nó xuất hiện ở cả hai chuỗi. Bất kỳ cách tiếp cận nào chỉ đơn giản là thêm số lượng hình vuông và hình khối sẽ vượt quá số lượng đó. 

Trường hợp cạnh tinh tế thứ hai là giá trị nhỏ nhất. Tại n = 1, cả hai chuỗi hình vuông và hình lập phương đều bao gồm 1, nhưng nó vẫn chỉ đóng góp một lần. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là lặp lại mọi số nguyên x từ 1 đến n và kiểm tra xem đó là hình vuông hoàn hảo hay hình lập phương hoàn hảo. Việc kiểm tra xem x là hình vuông hay hình lập phương yêu cầu tính toán các căn nguyên nguyên và xác minh chúng, tức là O(1) cho mỗi lần kiểm tra. Điều này dẫn đến O(n) trên mỗi trường hợp thử nghiệm, với n = 10⁹ tương đương khoảng một tỷ lượt kiểm tra cho mỗi truy vấn, quá chậm ngay cả trước khi xem xét nhiều trường hợp thử nghiệm. 

Quan sát cấu trúc quan trọng là chúng ta không cần kiểm tra từng số nguyên, chỉ cần kiểm tra phần tạo của hình vuông và hình khối. Mọi hình vuông được tạo bởi một số nguyên i với i² ≤ n và mọi khối được tạo bởi một số nguyên j với j³ ≤ n. Điều đó làm giảm vấn đề đếm các chỉ số hợp lệ i và j, tức là về sqrt(n) và cbrt(n). 

Tuy nhiên, chỉ cần tính toán sqrt(n) + cbrt(n) sẽ vượt quá số lượng cả hình vuông và hình khối. Một số vừa là hình vuông vừa là hình lập phương khi nó là lũy thừa thứ sáu, vì lcm(2,3) = 6. Vì vậy, các số có dạng k⁶ xuất hiện trong cả hai danh sách và phải được trừ một lần. 

Điều này biến vấn đề thành một phép tính bao gồm-loại trừ cổ điển đối với các tập hợp lũy thừa: đếm bình phương, đếm lập phương, trừ lũy thừa thứ sáu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Tính toán tối ưu thông qua lũy thừa 

1. Với n cho trước, hãy tính xem có bao nhiêu số nguyên tôi thỏa mãn i² ≤ n. Điều này tương đương với sàn(sqrt(n)). Điều này đếm tất cả các ô vuông lên đến n. 
2. Tính xem có bao nhiêu số nguyên j thỏa mãn j³ ≤ n là sàn(cbrt(n)). Điều này đếm tất cả các hình khối lên đến n. 
3. Tính xem có bao nhiêu số nguyên k thỏa mãn k⁶ ≤ n, bằng sàn(n^(1/6)). Điều này đếm các số vừa là hình vuông vừa là hình khối, vì đó chính xác là lũy thừa thứ sáu. 
4. Cộng tổng số hình vuông và hình khối, sau đó trừ đi số lũy thừa sáu. Thao tác này sẽ loại bỏ các bản sao được tính trong cả bộ hình vuông và bộ hình khối. 
5. Xuất ra giá trị kết quả cho từng test case. 

Lý do mỗi bước hoạt động là vì chúng ta không lặp trực tiếp qua các số mà qua các số mũ tạo duy nhất của chúng và mỗi phép biến đổi đều đơn điệu trong biến trình tạo. 

### Tại sao nó hoạt động

Mọi số nguyên x ≤ n là hình vuông có một cách biểu diễn duy nhất x = i² cho một số nguyên i nào đó và tương tự, mọi khối lập phương đều có một cách biểu diễn duy nhất x = j³. Điều này tạo ra hai ánh xạ nội xạ từ các số nguyên i và j vào tập hợp các số hợp lệ. Sự trùng lặp duy nhất xảy ra khi một số có thể được viết đồng thời dưới dạng i² và j³, ngụ ý x là lũy thừa thứ sáu hoàn hảo. Vì lũy thừa thứ sáu được tính một lần trong mỗi bộ, nên việc trừ đi số đếm của chúng sẽ điều chỉnh số đếm thừa chính xác một lần cho mỗi phần tử chồng chéo. Không có sự trùng lặp nào khác tồn tại vì biểu diễn số mũ là duy nhất trong thuật ngữ phân tích thừa số nguyên tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def isqrt(n: int) -> int:
    lo, hi = 0, 10**9
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * mid <= n:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi

def icbrt(n: int) -> int:
    lo, hi = 0, 10**6
    while lo <= hi:
        mid = (lo + hi) // 2
        if mid * mid * mid <= n:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi

def i6th(n: int) -> int:
    lo, hi = 0, 10**3
    while lo <= hi:
        mid = (lo + hi) // 2
        p = mid * mid
        p = p * p * p
        if p <= n:
            lo = mid + 1
        else:
            hi = mid - 1
    return hi

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = isqrt(n)
        b = icbrt(n)
        c = i6th(n)
        print(a + b - c)

if __name__ == "__main__":
    solve()
```Việc triển khai tách ba nhiệm vụ đếm thành các phép tính gốc số nguyên. Mỗi hàm trợ giúp sử dụng tìm kiếm nhị phân thay vì các phép toán dấu phẩy động, tránh các vấn đề về độ chính xác ở các ranh giới lớn như 10⁹. 

Hàm căn bậc hai tìm kiếm tối đa 10⁹ vì sqrt(10⁹) là khoảng 31623, an toàn trong phạm vi. Không gian tìm kiếm căn bậc ba được giới hạn ở 10⁶, vốn đã cao hơn nhiều so với cbrt(10⁹) ≈ 1000. Căn thứ sáu được giới hạn ở 10³ vì (10³)⁶ = 10¹⁸, vượt xa tất cả các đầu vào có thể có. 

Công thức cuối cùng áp dụng trực tiếp loại trừ bao gồm mà không lưu trữ bất kỳ tập hợp trung gian nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 10 

Chúng tôi tính toán hình vuông, hình khối và lũy thừa thứ sáu. 

| Bước | Giá trị | 
| --- | --- | 
| tầng(sqrt(10)) | 3 (1, 4, 9) | 
| sàn(cbrt(10)) | 2 (1, 8) | 
| tầng(n^(1/6)) | 1 (1) | 
| kết quả | 3 + 2 - 1 = 4 | 

Kết quả xác nhận rằng 1, 4, 8 và 9 được tính một lần. 

### Ví dụ 2: n = 100 

| Bước | Giá trị | 
| --- | --- | 
| tầng(sqrt(100)) | 10 | 
| sàn(cbrt(100)) | 4 | 
| tầng(n^(1/6)) | 2 | 
| kết quả | 10 + 4 - 2 = 12 | 

Điều này bao gồm tất cả các hình vuông lên đến 100, tất cả các hình khối lên đến 100, với lũy thừa thứ sáu bị loại bỏ khỏi việc tính hai lần. 

Những dấu vết này cho thấy việc hiệu chỉnh chồng chéo luôn loại bỏ chính xác những con số xuất hiện trong cả hai chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) mỗi lần kiểm tra | Mỗi gốc được tính toán thông qua tìm kiếm nhị phân trên một phạm vi cố định | 
| Không gian | O(1) | Chỉ một vài biến được sử dụng cho mỗi bài kiểm tra | 

Các ràng buộc cho phép tối đa 20 trường hợp thử nghiệm, do đó tổng công việc là không đáng kể. Ngay cả với chi phí tìm kiếm nhị phân, giải pháp vẫn có hiệu quả về thời gian không đổi cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def isqrt(n: int) -> int:
        lo, hi = 0, 10**9
        while lo <= hi:
            mid = (lo + hi) // 2
            if mid * mid <= n:
                lo = mid + 1
            else:
                hi = mid - 1
        return hi

    def icbrt(n: int) -> int:
        lo, hi = 0, 10**6
        while lo <= hi:
            mid = (lo + hi) // 2
            if mid * mid * mid <= n:
                lo = mid + 1
            else:
                hi = mid - 1
        return hi

    def i6th(n: int) -> int:
        lo, hi = 0, 10**3
        while lo <= hi:
            mid = (lo + hi) // 2
            p = mid * mid
            p = p * p * p
            if p <= n:
                lo = mid + 1
            else:
                hi = mid - 1
        return hi

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = isqrt(n)
            b = icbrt(n)
            c = i6th(n)
            out.append(str(a + b - c))
        return "\n".join(out)

    return solve()

assert run("""6
10
1
25
1000000000
999999999
500000000
""") == """4
1
6
32591
32590
23125"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 1 | trường hợp chồng chéo nhỏ nhất | 
| n = 10⁹ | giá trị lớn | độ đúng giới hạn trên | 
| bài kiểm tra hỗn hợp | kết quả đầu ra khác nhau | tính nhất quán giữa các gốc | 

## Vỏ cạnh 

Với n = 1, cả số đếm hình vuông và số lập phương đều bằng 1, và số lũy thừa thứ sáu cũng bằng 1. Công thức tạo ra 1 + 1 − 1 = 1, tránh tính hai lần một cách chính xác. 

Với n = 10⁹, căn bậc hai là 31622, căn bậc ba là 1000 và căn bậc sáu là 10. Phép trừ đảm bảo 10 lũy thừa thứ sáu không được tính hai lần. Thuật toán xử lý việc này hoàn toàn thông qua tính toán gốc số nguyên mà không gặp vấn đề về tràn hoặc độ chính xác.
