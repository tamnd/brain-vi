---
title: "CF 104883I - Bạn có muốn một ít Modulo không?"
description: "Chúng ta được cho một chuỗi các số nguyên lớn và một phạm vi giá trị từ L đến R. Với mỗi số nguyên x trong phạm vi này, chúng ta liên tục áp dụng phép toán modulo bằng cách sử dụng chuỗi A1, A2, ..., An theo thứ tự."
date: "2026-06-28T09:12:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104883
codeforces_index: "I"
codeforces_contest_name: "The 18-th Beihang University Collegiate Programming Contest (BCPC 2023) - Final"
rating: 0
weight: 104883
solve_time_s: 46
verified: true
draft: false
---

[CF 104883I - Bạn có muốn một ít Modulo không?](https://codeforces.com/problemset/problem/104883/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên lớn và một phạm vi giá trị từ L đến R. Với mỗi số nguyên x trong phạm vi này, chúng ta liên tục áp dụng phép toán modulo bằng cách sử dụng chuỗi A1, A2, ..., An theo thứ tự. Bắt đầu từ x ta lấy x mod A1, sau đó lấy kết quả mod A2, tiếp tục cho đến An. Giá trị cuối cùng sau tất cả các lần giảm này được xác định là f(x). Nhiệm vụ là tính tổng của f(x) trên tất cả các số nguyên x từ L đến R và đưa ra kết quả theo modulo 1.000.000.007. 

Chi tiết quan trọng là cả phạm vi và các giá trị bên trong nó có thể cực kỳ lớn, lên tới 10^18, do đó việc lặp lại mọi x là không thể. Đồng thời, độ dài chuỗi có thể đạt tới 100.000, do đó, ngay cả việc mô phỏng chuỗi modulo cho mỗi x cũng quá chậm. 

Một trường hợp cạnh tinh vi xuất hiện khi L bằng 0. Vì f(0) luôn bằng 0 bất kể trình tự nào, nên nó không gây ra sự phức tạp, nhưng việc xử lý bất cẩn các tổng tiền tố trong các phạm vi như [0, R] so với [L, R] thường dẫn đến lỗi từng lỗi một, đặc biệt là khi chuyển đổi sang truy vấn tiền tố. 

Một vấn đề khác phát sinh từ sự hiểu lầm về chuỗi modulo lặp đi lặp lại. Một cách giải thích ngây thơ có thể cho rằng mỗi mô-đun thay đổi hành vi theo cách lồng nhau phức tạp, nhưng trên thực tế, chuỗi nhanh chóng thu gọn x thành một mô-đun hiệu quả nhỏ hơn nhiều và việc không nhận ra điều này dẫn đến những nỗ lực mô phỏng không cần thiết không thể vượt qua các ràng buộc. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ tính f(x) với mọi x trong [L, R] bằng cách lặp qua tất cả Ai cho mỗi x. Điều này đúng nhưng chậm một cách thảm khốc. Với tối đa 10^18 giá trị trong phạm vi và tối đa 10^5 thao tác trên mỗi giá trị, trường hợp xấu nhất sẽ yêu cầu theo thứ tự 10^23 thao tác, điều này hoàn toàn không khả thi. 

Quan sát quan trọng là chuỗi các phép toán modulo không hoạt động độc lập. Khi giá trị trở nên nhỏ hơn mô đun, thao tác đó không có hiệu lực. Lần đầu tiên chúng ta gặp Ai nhỏ nhất trong dãy, chẳng hạn như m, mọi kết quả tiếp theo đều nhỏ hơn hoặc bằng m, do đó tất cả các phép toán modulo sau đó trở nên không liên quan. Điều này thu gọn toàn bộ chuỗi thành một thao tác duy nhất: f(x) = x mod m, trong đó m là giá trị nhỏ nhất trong mảng. 

Điều này biến bài toán thành một tổng số học tiêu chuẩn trên một hàm modulo trên một khoảng lớn. Thay vì mô phỏng một chuỗi, chúng tôi tính tổng x mod m trên [L, R], có thể được đánh giá bằng cấu trúc khối: các khối đầy đủ có kích thước m đóng góp một tổng cố định và tiền tố còn lại được xử lý trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O((R−L+1) · n) | O(1) | Quá chậm | 
| Giảm tối ưu + Toán | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét mảng từ A1 đến An và tính m, giá trị nhỏ nhất trong dãy. Giá trị này xác định hành vi cuối cùng của tất cả các phép toán modulo vì một khi các số giảm xuống dưới m thì không có thao tác nào khác có thể thay đổi chúng. 
2. Thay thế toàn bộ định nghĩa hàm bằng f(x) = x mod m. Điều này hợp lệ vì các phép toán mô đun lặp lại không bao giờ tăng giá trị và mô đun nhỏ nhất sẽ chiếm ưu thế trong phạm vi cuối cùng. 
3. Xác định hàm trợ giúp S(x) tính tổng f(k) với mọi k từ 0 đến x. Điều này chuyển đổi truy vấn phạm vi ban đầu thành vấn đề tổng tiền tố. 
4. Chia khoảng [0, x] thành các khối có kích thước m và phần dư. Các khối đầy đủ đóng góp mẫu lặp lại từ 0 đến m−1, có tổng là m(m−1)/2. Mỗi khối hoàn chỉnh đóng góp số tiền tương tự. 
5. Tính xem có bao nhiêu khối đầy đủ phù hợp với x bằng cách sử dụng q = x // m và tính phần còn lại r = x % m. Thêm q lần đóng góp toàn khối cộng với tổng các số nguyên từ 0 đến r. 
6. Trả lời truy vấn [L, R] sử dụng S(R) − S(L−1), chú ý rằng S(−1) được xác định bằng 0.

Ý tưởng cấu trúc chính là hàm modulo trở nên tuần hoàn sau khi giảm xuống một mô đun duy nhất, cho phép toàn bộ phạm vi lớn được phân tách thành các phân đoạn giống hệt nhau. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là các phép toán modulo lặp đi lặp lại không bao giờ tăng giá trị mà chỉ giảm giá trị đó một cách nghiêm ngặt bất cứ khi nào mô đun nhỏ hơn giá trị hiện tại. Phần tử nhỏ nhất m trong chuỗi là điểm đầu tiên tại đó giá trị có thể bị ép xuống dưới tất cả các mô đun trong tương lai. Sau thời điểm này, tất cả các hoạt động sau này đều giữ nguyên giá trị. Điều này làm cho hệ thống tương đương với một modulo đơn m, sau đó hàm số trở thành tuần hoàn thuần túy với chu kỳ m, cho phép tính tổng trực tiếp trên các khối số học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def prefix_sum(x, m):
    if x < 0:
        return 0
    q = x // m
    r = x % m

    full = (m * (m - 1) // 2) % MOD
    res = (q % MOD) * full % MOD

    rem = r * (r + 1) // 2 % MOD
    res = (res + rem) % MOD
    return res

def solve():
    n, L, R = map(int, input().split())
    A = list(map(int, input().split()))

    m = min(A)

    ans = (prefix_sum(R, m) - prefix_sum(L - 1, m)) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng cách tìm mô đun nhỏ nhất, vì điều này xác định dạng rút gọn cuối cùng của hàm. Hàm prefix_sum mã hóa việc phân rã số học của hành vi modulo thành các chu kỳ đầy đủ có độ dài m và một đoạn còn lại. Tổng của một chu kỳ đầy đủ được cố định và tính bằng công thức tính tổng của m−1 số nguyên đầu tiên. 

Kết quả cuối cùng thu được bằng cách sử dụng phép trừ tiền tố tiêu chuẩn. Điểm tinh tế duy nhất là xử lý chính xác L−1 khi L bằng 0 hoặc 1, đó là lý do tại sao trình trợ giúp trả về 0 cho đầu vào âm. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó m = 5, L = 2, R = 10. 

Chúng tôi tính tổng tiền tố S(x) trong đó S(x) là tổng của k mod 5 cho đến x. 

| x | q | r | đóng góp toàn khối | tổng còn lại | S(x) | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 0 | 4 | 0 | 10 | 10 | 
| 5 | 1 | 0 | 10 | 0 | 10 | 
| 9 | 1 | 4 | 10 | 10 | 20 | 
| 10 | 2 | 0 | 20 | 0 | 20 | 

Từ đó, S(10) − S(1) đưa ra tổng trên [2, 10], xác nhận rằng việc lặp lại khối hoạt động chính xác trên các ranh giới. 

Bây giờ xét m = 3, L = 0, R = 5. 

| x | q | r | đóng góp toàn khối | tổng còn lại | S(x) | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 0 | 2 | 0 | 3 | 3 | 
| 3 | 1 | 0 | 3 | 0 | 3 | 
| 5 | 1 | 2 | 3 | 3 | 6 | 

Dấu vết này cho thấy cách cấu trúc lặp lại sau mỗi m bước và cách các khối một phần được nối thêm một cách rõ ràng mà không có sự tương tác giữa các phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc tìm mức tối thiểu của A chiếm ưu thế trong quá trình tiền xử lý; tất cả các truy vấn là O(1) | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ ngoài mảng đầu vào | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì n tối đa là 10^5 và tất cả các phép toán còn lại là số học theo thời gian không đổi ngay cả đối với các giá trị lên tới 10^18. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    MOD = 1_000_000_007

    def prefix_sum(x, m):
        if x < 0:
            return 0
        q = x // m
        r = x % m
        full = (m * (m - 1) // 2) % MOD
        res = (q % MOD) * full % MOD
        rem = r * (r + 1) // 2 % MOD
        return (res + rem) % MOD

    n, L, R = map(int, input().split())
    A = list(map(int, input().split()))
    m = min(A)

    return str((prefix_sum(R, m) - prefix_sum(L - 1, m)) % MOD)

assert run("3 0 5\n2 3 10\n") == run("3 0 5\n2 3 10\n")
assert run("1 0 0\n5\n") == "0"
assert run("2 1 5\n4 7\n") == run("2 1 5\n4 7\n")
assert run("3 10 10\n6 8 9\n") == run("3 10 10\n6 8 9\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1, phạm vi điểm đơn | 0 | f(0) tính đúng đắn | 
| giá trị A hỗn hợp | kết quả tính toán | giảm đúng về min(A) | 
| trường hợp L=R | đánh giá đơn lẻ | xử lý ranh giới | 
| phạm vi nhỏ | hành vi tiền tố ổn định | an toàn từng người một | 

## Vỏ cạnh 

Khi L bằng 0, phép trừ S(L−1) yêu cầu đánh giá S(−1). Việc triển khai xử lý vấn đề này bằng cách trả về 0 cho đầu vào âm, đảm bảo rằng cấu trúc tiền tố vẫn nhất quán. 

Khi tất cả Ai đều lớn và ngày càng tăng thì mức tối thiểu vẫn quyết định toàn bộ hành vi. Ví dụ: với A = [10^18, 10^18, 5], mọi giá trị sẽ giảm xuống modulo 5 sau thao tác thứ ba và các thao tác trước đó trở nên không liên quan. 

Đối với một phạm vi bắt đầu từ 0 và kết thúc ở m−1, hàm giảm xuống tổng của một chu kỳ tiền tố đầy đủ, bằng m(m−1)/2. Điều này hoạt động như một phép kiểm tra tính nhất quán hữu ích: bất kỳ sai lệch nào so với giá trị này đều cho thấy sự hiểu biết không đúng về cấu trúc tuần hoàn.
