---
title: "CF 1029A - Nhiều chuỗi con bằng nhau"
description: "Chúng ta được cung cấp một chuỗi mẫu t có độ dài n và số mục tiêu k. Chúng ta cần xây dựng một chuỗi s mới càng ngắn càng tốt sao cho khi chúng ta trượt một cửa sổ có độ dài n qua s, mẫu t sẽ xuất hiện chính xác k lần dưới dạng một chuỗi con."
date: "2026-06-16T21:08:48+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1029
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 506 (Div. 3)"
rating: 1300
weight: 1029
solve_time_s: 144
verified: true
draft: false
---

[CF 1029A - Nhiều chuỗi con bằng nhau](https://codeforces.com/problemset/problem/1029/A) 

**Đánh giá:** 1300 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi mẫu`t`chiều dài`n`và số mục tiêu`k`. Chúng ta cần xây dựng một chuỗi mới`s`càng ngắn càng tốt để khi chúng ta trượt một cửa sổ có độ dài`n`sang`s`, mẫu`t`xuất hiện chính xác`k`lần dưới dạng một chuỗi con. 

Mỗi lần xuất hiện hợp lệ được xác định bởi vị trí bắt đầu`i`, trong đó đoạn`s[i:i+n]`giống hệt với`t`. Cho phép xảy ra sự chồng chéo giữa các lần xuất hiện, vì vậy nhiều kết quả trùng khớp có thể chia sẻ các ký tự. 

Khó khăn chính là sự chồng chéo có thể làm giảm tổng chiều dài của`s`. Nếu chúng ta đặt bản sao của`t`quay lại với sự chồng chéo tối đa, chúng tôi giảm bớt các ký tự được thêm vào. Mục đích là sắp xếp`k`xảy ra theo cách giảm thiểu tổng thời lượng trong khi vẫn đảm bảo không xuất hiện thêm các sự cố ngoài ý muốn. 

Những hạn chế là nhỏ:`n, k ≤ 50`. Điều này ngay lập tức gợi ý rằng mọi suy luận bậc hai hoặc thậm chí bậc ba đều có thể chấp nhận được, vì không gian tìm kiếm rất nhỏ. Chúng tôi không tối ưu hóa các cấu trúc tổ hợp lớn mà đang xây dựng một chuỗi có các phần chồng chéo được kiểm soát một cách cẩn thận. 

Trường hợp cạnh tinh tế xuất hiện khi`t`có cấu trúc lặp lại, chẳng hạn như`"aaaa"`hoặc`"ababa"`. Trong những trường hợp như vậy, các bản sao chồng chéo có thể vô tình tạo ra sự xuất hiện bổ sung của`t`. Ví dụ, với`t = "aaa"`Và`s = "aaaa"`, có hai lần xuất hiện bắt đầu từ vị trí 0 và 1, nhưng các mẫu chồng chéo có thể dễ dàng tạo ra nhiều kết quả trùng khớp hơn dự định nếu không được kiểm soát cẩn thận. Bất kỳ công trình xây dựng nào cũng phải đảm bảo rằng sự chồng chéo chỉ tạo ra sự phù hợp như mong muốn. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là thử xây dựng chuỗi tăng dần và ở mỗi bước hãy quyết định vị trí nối các ký tự sao cho số lần xuất hiện của chuỗi`t`vẫn nằm trong tầm kiểm soát. Người ta có thể tưởng tượng việc brute buộc tất cả các phần mở rộng có thể có của chuỗi và kiểm tra bao nhiêu lần`t`xuất hiện sau mỗi phần mở rộng. Điều này nhanh chóng trở nên không khả thi bởi vì ngay cả với`n, k ≤ 50`, số lượng chuỗi ứng cử viên tăng theo cấp số nhân và mỗi lần kiểm tra tốn`O(nk)`. 

Quan sát quan trọng là việc xây dựng tối ưu phải sử dụng lại sự chồng chéo tối đa có thể có giữa các bản sao liên tiếp của`t`. Nếu chúng ta đặt hai bản sao của`t`với sự chồng chéo về chiều dài`x`, thì hậu tố có độ dài`x`của bản sao đầu tiên phải bằng tiền tố độ dài`x`của`t`. Trong số tất cả các sự trùng lặp có thể xảy ra, chúng tôi muốn có sự trùng lặp lớn nhất vì điều đó giúp giảm thiểu số lượng ký tự mới được thêm vào mỗi lần xuất hiện bổ sung. 

Điều này làm giảm vấn đề tính toán đường viền tối đa của`t`, nghĩa là tiền tố thích hợp dài nhất của`t`đó cũng là một hậu tố. Một khi chúng ta biết độ dài chồng chéo này`p`, mỗi bản sao bổ sung của`t`có thể được nối thêm bằng cách chỉ thêm`n - p`nhân vật. 

Để đảm bảo chính xác`k`xuất hiện, chúng tôi xây dựng bản sao đầu tiên của`t`đầy đủ rồi nối thêm`k-1`bản sao, mỗi bản chồng lên nhau bởi`p`. Điều này đảm bảo rằng mọi vị trí bắt đầu của một lần xuất hiện đều chính xác là sự dịch chuyển dự định và không có lần xuất hiện bổ sung nào xuất hiện vì bất kỳ kết quả khớp bổ sung nào sẽ yêu cầu cấu trúc đường viền khác, cấu trúc này đã được tính bằng cách chọn mức chồng chéo tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm xây dựng Brute Force | Hàm mũ | O(nk) | Quá chậm | 
| Xây dựng dựa trên biên giới | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên chúng ta cần tính toán xem chuỗi có thể chồng lên chính nó bao nhiêu. 

1. Tính tiền tố riêng lớn nhất của`t`đó cũng là một hậu tố. Điều này mang lại chiều dài chồng chéo`p`. Đây là sự thay đổi tối đa trong đó hai bản sao của`t`có thể căn chỉnh mà không phá vỡ sự bình đẳng. 
2. Khởi tạo chuỗi câu trả lời`s`BẰNG`t`. Tại thời điểm này, chúng tôi đã có một lần xuất hiện. 
3. Lặp lại`k - 1`lần: 

Lấy cái cuối cùng`p`nhân vật của`s`và nối thêm hậu tố còn lại của`t`bắt đầu từ chỉ mục`p`. Điều này mở rộng`s`do đó một sự xuất hiện mới của`t`bắt đầu chính xác ở vị trí dịch chuyển chính xác. 
4. Sau tất cả các lần lặp, hãy quay lại`s`. 

Lý do chúng tôi luôn sử dụng lại cùng một lớp phủ là vì việc sử dụng lớp phủ nhỏ hơn sẽ chỉ tăng độ dài mà không thay đổi độ chính xác và không thể sử dụng lớp phủ lớn hơn theo định nghĩa của`p`. 

### Tại sao nó hoạt động 

Việc xây dựng đảm bảo rằng mọi sự xuất hiện của`t`bắt đầu chính xác tại các vị trí khác nhau bởi`n - p`. Cấu trúc tiền tố-hậu tố đảm bảo rằng mỗi khối được thêm vào mới sẽ căn chỉnh hoàn hảo với khối trước đó, vì vậy mọi cửa sổ dự định đều khớp với nhau`t`. Bởi vì chúng tôi luôn sử dụng sự trùng lặp tối đa có thể, nên không có kết quả trùng khớp ngoài ý muốn bổ sung nào có thể xuất hiện giữa các lần xuất hiện được xây dựng, vì bất kỳ kết quả trùng khớp bổ sung nào sẽ ngụ ý đường viền lớn hơn`p`, mâu thuẫn với cực đại của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def prefix_function(s):
    n = len(s)
    pi = [0] * n
    for i in range(1, n):
        j = pi[i - 1]
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi

def solve():
    n, k = map(int, input().split())
    t = input().strip()

    pi = prefix_function(t)
    p = pi[-1]

    s = t
    for _ in range(k - 1):
        s += t[p:]

    print(s)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng hàm tiền tố (tiền xử lý KMP) để tính đường viền dài nhất của`t`. giá trị`pi[-1]`trực tiếp cung cấp độ dài của tiền tố dài nhất cũng là hậu tố. 

Sau đó chúng tôi xây dựng chuỗi cuối cùng lặp đi lặp lại. Mỗi thao tác chắp thêm sẽ sử dụng lại hậu tố bắt đầu từ`p`, đảm bảo sự chồng chéo tối đa. Vòng lặp chạy chính xác`k-1`lần, vì vậy chúng tôi tạo ra chính xác`k`sự cố bắt đầu tại các vị trí được kiểm soát. 

Một cạm bẫy triển khai phổ biến là tính toán không chính xác đường viền khi`t`không có sự lặp lại. Trong trường hợp đó`p = 0`và chúng ta phải nối thêm chuỗi đầy đủ mỗi lần để mã xử lý một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 4
t = "aba"
```Đường viền tính toán đầu tiên:`"aba"`có tiền tố hậu tố`"a"`, Vì thế`p = 1`. 

Chúng tôi xây dựng từng bước: 

| Bước | s | Hoạt động | 
| --- | --- | --- | 
| 0 | aba | ban đầu | 
| 1 | ba ba | nối "ba" | 
| 2 | baba | nối "ba" | 
| 3 | ababababa | nối "ba" | 

Mỗi lần xuất hiện mới bắt đầu ở mỗi 2 vị trí. Điều này đảm bảo chính xác 4 lần xuất hiện. 

Điều này xác nhận tính bất biến rằng các phần chồng lấp luôn được căn chỉnh ở đường viền tối đa. 

### Ví dụ 2 

đầu vào:```
n = 2, k = 3
t = "aa"
```Biên giới là`"a"`, Vì thế`p = 1`. 

| Bước | s | Hoạt động | 
| --- | --- | --- | 
| 0 | aa | ban đầu | 
| 1 | aaa | nối "a" | 
| 2 | aaa | nối "a" | 

Lần xuất hiện xuất hiện ở các vị trí 0, 1, 2, cho ra đúng 3 kết quả trùng khớp. 

Điều này cho thấy rằng ngay cả trong các trường hợp chồng chéo nhiều, việc xây dựng đường viền tối đa sẽ ngăn ngừa sự xuất hiện thiếu hoặc xảy ra thêm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k·n) | hàm tiền tố chạy trong O(n), mỗi lần thêm chi phí lên tới O(n), thực hiện k lần | 
| Không gian | O(n) | lưu trữ mảng tiền tố và chuỗi kết quả | 

Được cho`n, k ≤ 50`, tổng công việc không đáng kể, nằm trong giới hạn ngay cả khi sử dụng Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    import sys as _sys

    def solve():
        n, k = map(int, input().split())
        t = input().strip()

        pi = [0] * n
        for i in range(1, n):
            j = pi[i - 1]
            while j > 0 and t[i] != t[j]:
                j = pi[j - 1]
            if t[i] == t[j]:
                j += 1
            pi[i] = j

        p = pi[-1]
        s = t
        for _ in range(k - 1):
            s += t[p:]

        print(s)

    with redirect_stdout(out):
        solve()

    return out.getvalue().strip()

# provided sample
assert run("3 4\naba\n") == "ababababa"

# all same characters
assert run("1 5\na\n") == "aaaaa"

# no overlap case
assert run("3 3\nabc\n") == "abcabcabc"

# full overlap case
assert run("2 4\naa\n") == "aaaaa"

# mixed border
assert run("4 3\nabab\n") == "ababababab"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"3 4 aba"`|`ababababa`| trường hợp chồng chéo tiêu chuẩn | 
|`"1 5 a"`|`aaaaa`| cạnh ký tự đơn | 
|`"3 3 abc"`|`abcabcabc`| không chồng chéo | 
|`"2 4 aa"`|`aaaaa`| chồng chéo tối đa | 
|`"4 3 abab"`|`ababababab`| cấu trúc tuần hoàn | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`t`không có sự tự chồng chéo. Ví dụ,`t = "abc"`cho`p = 0`. Sau đó, thuật toán sẽ thêm các bản sao đầy đủ mỗi lần, tạo ra`abcabcabc...`. Việc thi công vẫn đảm bảo chính xác`k`xảy ra vì không có sự căn chỉnh nào được dịch chuyển có thể vô tình tạo ra các kết quả khớp một phần. 

Một trường hợp khác là sự lặp lại hoàn toàn, chẳng hạn như`t = "aaaa"`. Đây là biên giới`3`, vì vậy mỗi bản sao mới chỉ thêm một ký tự. Mặc dù sự chồng chéo là cực kỳ lớn nhưng hàm tiền tố vẫn nắm bắt chính xác đường viền tối đa, đảm bảo chuỗi tăng trưởng ở mức tối thiểu trong khi vẫn duy trì chính xác`k`sự cố được kiểm soát.
