---
title: "CF 103577C - Corona"
description: "Mỗi trường hợp thử nghiệm đưa ra một chuỗi bộ gen và chúng ta phải chỉ định một điểm bằng số đến từ tất cả các chuỗi con liền kề của nó. Đối với bất kỳ chuỗi con nào, chúng tôi xem xét mức độ lặp lại của mẫu tiền tố ở cuối chuỗi đó."
date: "2026-07-03T03:31:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103577
codeforces_index: "C"
codeforces_contest_name: "2021 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 103577
solve_time_s: 148
verified: true
draft: false
---

[CF 103577C - Corona](https://codeforces.com/problemset/problem/103577/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra một chuỗi bộ gen và chúng ta phải chỉ định một điểm bằng số đến từ tất cả các chuỗi con liền kề của nó. Đối với bất kỳ chuỗi con nào, chúng tôi xem xét mức độ lặp lại của mẫu tiền tố ở cuối chuỗi đó. Chính xác hơn, đối với một chuỗi con, chúng tôi lấy tiền tố thích hợp dài nhất cũng xuất hiện dưới dạng hậu tố của cùng một chuỗi con và chúng tôi đo độ dài của nó. Độ dài đó được gọi là cấp độ của chuỗi con. Nhiệm vụ là tính tổng mức này trên mọi chuỗi con có thể có của chuỗi đã cho. 

Đầu vào bao gồm tối đa 50 chuỗi, mỗi chuỗi có độ dài tối đa 5000. Điều này ngay lập tức loại trừ mọi cách tiếp cận xử lý từng chuỗi con một cách độc lập bằng quét tuyến tính. Có các chuỗi con O(n^2) trên mỗi chuỗi, do đó, ngay cả O(n) hoạt động trên mỗi chuỗi con cũng dẫn đến khoảng 1,25 × 10^11 thao tác trong trường hợp xấu nhất, vượt xa tính khả thi. 

Một vấn đề tế nhị là cấp độ được xác định bằng cách sử dụng tiền tố và hậu tố thích hợp, do đó, toàn bộ chuỗi con không được phép ngay cả khi nó khớp một cách tầm thường. Ví dụ: đối với "aaaa", chuỗi con "aa" có cấp 1, nhưng "a" có cấp 0. Một điểm khác thường gây ra lỗi là cấu trúc chồng chéo: các chuỗi con khác nhau chia sẻ phần lớn tính toán, do đó, việc tính toán lại cấu trúc tiền tố-hậu tố từ đầu cho mỗi chuỗi con là lãng phí. 

Các trường hợp cạnh phát sinh khi chuỗi không có sự lặp lại, chẳng hạn như "abcde", trong đó mọi chuỗi con đều có cấp 0 và khi chuỗi có tính lặp lại cao, chẳng hạn như "aaaaa", trong đó mọi chuỗi con đều có cấu trúc viền lớn. Một giải pháp ngây thơ quên loại trừ toàn bộ chuỗi con vì đường viền hợp lệ sẽ bị tính quá mức trong các trường hợp như "aaa", trong đó "aaa" sẽ đóng góp 2 chứ không phải 3. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực kiểm tra mọi chuỗi con và tính toán cấp độ của nó một cách độc lập. Đối với chuỗi con s[l..r], chúng ta có thể chạy tính toán tiền tố-hậu tố trên chuỗi con đó, chẳng hạn như hàm tiền tố KMP, để tìm đường viền dài nhất của nó. Việc tính toán này tốn O(r-l+1) và vì có các chuỗi con O(n^2), nên tổng độ phức tạp sẽ trở thành O(n^3). Với n lên tới 5000, điều này đã quá chậm và việc lặp lại tới 50 trường hợp thử nghiệm sẽ trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là mức độ của chuỗi con chỉ phụ thuộc vào hành vi của hàm tiền tố bên trong chuỗi con đó và hàm tiền tố có thể được tính tăng dần theo thời gian tuyến tính cho bất kỳ vị trí bắt đầu cố định nào. Nếu chúng ta sửa điểm cuối bên trái i thì với hậu tố s[i:], chúng ta có thể tính hàm tiền tố của nó một lần trong O(n). Mỗi tiền tố của hậu tố này tương ứng chính xác với một chuỗi con bắt đầu từ i. Giá trị tiền tố-hàm tại vị trí j cho biết mức của chuỗi con s[i..i+j]. Điều này loại bỏ việc tính toán lại lặp đi lặp lại và giảm tổng công việc xuống còn O(n^2) trên mỗi chuỗi. 

Quá trình chuyển đổi từ lực lượng vũ phu sang tối ưu về cơ bản là chuyển từ tính toán lại cấu trúc trên mỗi chuỗi con sang sử dụng lại các chuyển đổi giống như máy tự động của KMP trên tất cả các chuỗi con có cùng điểm khởi đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Chức năng tiền tố mỗi lần bắt đầu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng chuỗi một cách độc lập. 

1. Cố định chỉ số bắt đầu i từ 0 thành n − 1. Chúng ta sẽ coi s[i:] là một chuỗi mới. 
2. Xây dựng mảng tiền tố pi cho hậu tố này bằng logic KMP tiêu chuẩn. 

Tại vị trí j trong hậu tố này, pi[j] biểu thị độ dài của tiền tố thích hợp dài nhất của s[i..i+j] cũng là hậu tố của nó. 
3. Với mỗi j, hãy thêm pi[j] vào câu trả lời. Điều này trực tiếp tích lũy cấp độ của chuỗi con s[i..i+j]. 
4. Lặp lại cho tất cả các vị trí bắt đầu i, tính tổng các đóng góp. 

Chi tiết quan trọng là pi được tính toán lại cho từng hậu tố chứ không phải cho toàn bộ chuỗi một lần. Điều này là cần thiết vì mối quan hệ tiền tố có liên quan đến phần đầu của chuỗi con. 

### Tại sao nó hoạt động

Đối với i cố định, phép tính hàm tiền tố mô phỏng các tiền tố phù hợp của s[i:] với các hậu tố của chính nó. Tại mỗi vị trí j, pi[j] được tính toán chính xác là đường viền dài nhất của chuỗi con s[i..i+j] vì hàm tiền tố được xác định để theo dõi tiền tố thích hợp dài nhất cũng là hậu tố của tiền tố kết thúc tại j. Vì mỗi chuỗi con được biểu diễn duy nhất dưới dạng tiền tố của một hậu tố nào đó, nên việc tính tổng tất cả pi[j] trên tất cả i,j sẽ bao gồm mọi chuỗi con chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_one(s: str) -> int:
    n = len(s)
    total = 0

    for i in range(n):
        pi = [0] * (n - i)
        for j in range(1, n - i):
            k = pi[j - 1]
            while k > 0 and s[i + j] != s[i + k]:
                k = pi[k - 1]
            if s[i + j] == s[i + k]:
                k += 1
            pi[j] = k
            total += k

    return total

def main():
    data = sys.stdin.read().strip().split()
    out = []
    for s in data:
        out.append(str(solve_one(s)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Vòng lặp bên ngoài sửa lỗi bắt đầu chuỗi con và vòng lặp bên trong xây dựng hàm tiền tố tăng dần. Biến k đóng vai trò là ứng cử viên có độ dài đường viền hiện tại, chính xác như trong KMP. Mỗi dự phòng không khớp tuân theo các liên kết lỗi được mã hóa bằng pi. 

Một cạm bẫy triển khai phổ biến là quên bù các chỉ số bằng i khi so sánh các ký tự. Một trường hợp khác là vô tình sử dụng một hàm tiền tố toàn cục duy nhất, hàm này sẽ trộn các chuỗi con không liên quan và tạo ra các đường viền không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi "aaa". 

Với tôi = 0: 

| j | chuỗi con | pi[j] | 
| --- | --- | --- | 
| 0 | một | 0 | 
| 1 | aa | 1 | 
| 2 | aaa | 2 | 

Đóng góp là 3. 

Với tôi = 1: 

| j | chuỗi con | pi[j] | 
| --- | --- | --- | 
| 0 | một | 0 | 
| 1 | aa | 1 | 

Đóng góp là 1. 

Với tôi = 2: 

| j | chuỗi con | pi[j] | 
| --- | --- | --- | 
| 0 | một | 0 | 

Đóng góp là 0. 

Tổng cộng là 4. 

Bây giờ hãy xem xét "ababa". 

Với tôi = 0: 

| j | chuỗi con | pi[j] | 
| --- | --- | --- | 
| 0 | một | 0 | 
| 1 | ab | 0 | 
| 2 | aba | 1 | 
| 3 | abab | 2 | 
| 4 | ba ba | 3 | 

Tổng là 6. 

Với i = 1 ("baba"), số tiền đóng góp là 0,0,1,2 cho 3. Với i = 2 ("aba"), chúng tôi nhận được 0,0,1 cho 1. Số lần bắt đầu còn lại đóng góp 0. Tổng cộng là 10. 

Những dấu vết này cho thấy rằng mỗi chuỗi con được xử lý chính xác một lần dưới dạng tiền tố của hậu tố bắt đầu từ điểm cuối bên trái của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) mỗi chuỗi | Mỗi hậu tố xây dựng một hàm tiền tố tuyến tính và có n hậu tố | 
| Không gian | O(n) | Mỗi lần chỉ có một mảng hàm tiền tố được lưu trữ | 

Với n 5000 và nhiều nhất là 50 chuỗi, điều này phù hợp thoải mái trong giới hạn thời gian, vì tổng số thao tác ở mức 1,25 × 10^8 trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve_one(s: str) -> int:
        n = len(s)
        total = 0
        for i in range(n):
            pi = [0] * (n - i)
            for j in range(1, n - i):
                k = pi[j - 1]
                while k > 0 and s[i + j] != s[i + k]:
                    k = pi[k - 1]
                if s[i + j] == s[i + k]:
                    k += 1
                pi[j] = k
                total += k
        return total

    out = []
    for s in sys.stdin.read().split():
        out.append(str(solve_one(s)))
    return "\n".join(out)

# provided-like samples
assert run("a") == "0"
assert run("aa") == "1"
assert run("aaa") == "4"

# custom cases
assert run("abc") == "0", "no repetition"
assert run("aaaa") == str(10), "high repetition"
assert run("ababa") == "10", "overlapping borders"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "abc" | 0 | không có cấu trúc biên giới ở bất cứ đâu | 
| "aaa" | 10 | sự lặp lại mạnh mẽ và biên giới tích lũy | 
| "ababa" | 10 | chuỗi tiền tố-hậu tố chồng chéo | 

## Vỏ cạnh 

Đối với một chuỗi như "abcde", mọi sự không khớp xảy ra ngay lập tức trong hàm tiền tố, do đó mọi pi[j] vẫn bằng 0. Thuật toán tích lũy chính xác số 0 cho tất cả các chuỗi con vì không có hậu tố nào có chung cấu trúc tiền tố không tầm thường. 

Đối với một chuỗi thống nhất như "aaaaa", hàm tiền tố tăng tuyến tính cho mọi hậu tố. Đối với mỗi vị trí bắt đầu i, phần đóng góp có dạng hình tam giác và thuật toán tích lũy các phần đóng góp này mà không cần tính toán lại. Việc xây dựng hàm tiền tố đảm bảo rằng tại vị trí j chúng ta luôn tính đường viền hợp lệ dài nhất hoàn toàn ngắn hơn chuỗi con, không bao giờ là chuỗi con đầy đủ. 

Đối với các mẫu có mức độ chồng chéo cao như "ababa", dự phòng lặp lại thông qua pi[k-1] sẽ nắm bắt chính xác các đường viền lồng nhau như "a", "aba" và "ababa". Mỗi bước dự phòng duy trì bất biến rằng k luôn là độ dài đường viền hợp lệ của tiền tố hiện tại, do đó không có độ dài đường viền không hợp lệ nào được thêm vào.
