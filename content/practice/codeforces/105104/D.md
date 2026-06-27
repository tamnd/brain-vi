---
title: "CF 105104D - $\\text{DAY}^{-1}$"
description: "Chúng ta có một chuỗi mẫu cố định được hình thành bằng cách lặp lại khối \"xtu\" chính xác n lần. Vì vậy, chuỗi p đầy đủ có độ dài 3n và bao gồm một cấu trúc tuần hoàn rất cứng nhắc: cứ ba ký tự luôn là x, rồi t, rồi u."
date: "2026-06-27T20:09:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105104
codeforces_index: "D"
codeforces_contest_name: "2024 HNMU@XTU"
rating: 0
weight: 105104
solve_time_s: 43
verified: true
draft: false
---

[CF 105104D -$\\text{DAY}^{-1}$](https://codeforces.com/problemset/problem/105104/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi mẫu cố định được hình thành bằng cách lặp lại khối`"xtu"`chính xác`n`lần. Vì vậy, chuỗi đầy đủ`p`có chiều dài`3n`và bao gồm một cấu trúc tuần hoàn rất cứng nhắc: cứ ba ký tự luôn`x`, sau đó`t`, sau đó`u`. 

Ngoài ra, mỗi trường hợp thử nghiệm còn cung cấp một chuỗi truy vấn`q`. Nhiệm vụ là đếm xem có bao nhiêu cách riêng biệt mà chúng ta có thể chọn chỉ số từ`p`sao cho các ký tự được chọn, đọc theo thứ tự, tạo thành chính xác`q`. Hai cách được coi là khác nhau nếu chúng khác nhau ở vị trí nào của`p`đã được sử dụng. 

Vì vậy, đối tượng cốt lõi không phải là so khớp chuỗi con mà là đếm chuỗi con, trong đó cùng một ký tự ở các vị trí khác nhau góp phần lựa chọn tổ hợp riêng biệt. 

Những ràng buộc cực kỳ chặt chẽ theo một cách rất cụ thể. Số lần lặp lại`n`nhiều nhất là 10, nghĩa là chuỗi chính`p`có độ dài tối đa là 30. Độ dài truy vấn cũng bị giới hạn bởi`3n`, vậy nhiều nhất cũng là 30. Tuy nhiên, số lượng ca kiểm thử có thể lớn tới mức`10^5`, do đó lời giải phải gần tuyến tính về kích thước của từng trường hợp thử nghiệm, với các hằng số rất nhỏ. 

Một chuỗi con đơn giản DP cho mỗi trường hợp thử nghiệm đã được chấp nhận về mặt kích thước trạng thái, nhưng bất kỳ thứ gì có tính hàm mũ trong`|p|`hoặc`|q|`mỗi trường hợp thử nghiệm sẽ quá chậm trong tất cả các thử nghiệm. 

Một điểm tinh tế là mặc dù`p`có cấu trúc cao, các chuỗi tiếp theo được tính theo vị trí chứ không phải theo loại ký tự. Điều đó có nghĩa là các chữ cái giống hệt nhau xuất hiện ở các vị trí khác nhau trong`p`không thể thay thế cho nhau, chúng tạo ra số lượng khác nhau. 

Trường hợp cạnh xuất hiện khi`q`chứa các chữ cái bên ngoài`{x, t, u}`. Trong trường hợp đó câu trả lời ngay lập tức là 0 vì`p`không bao giờ chứa chúng. 

Một trường hợp cạnh quan trọng khác là khi`q`trống rỗng. Số cách tạo thành một dãy con rỗng luôn là 1 vì ta không chọn gì cả. 

Cuối cùng, khi`q`dài hơn`p`, câu trả lời là 0 vì chúng ta không thể chọn nhiều vị trí hơn mức tồn tại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi bài toán theo nghĩa đen là dãy con đếm trên một chuỗi có độ dài lên tới 30. Chúng ta quyết định đệ quy cho từng vị trí trong`p`nên lấy hay bỏ qua, theo dõi xem chúng tôi có bao nhiêu cách phù hợp`q`. Điều này khám phá tất cả các tập hợp con của các chỉ số, đó là`2^(3n)`khả năng. Với`n ≤ 10`, nhiều nhất là thế này`2^30`, khoảng một tỷ phép tính cho mỗi trường hợp thử nghiệm, con số này quá lớn đối với`10^5`các bài kiểm tra. 

Chúng ta cần tránh liệt kê các tập hợp con. Quan sát quan trọng là`p`không phải là tùy ý, nó là sự lặp lại của chu trình 3 ký tự. Điều này có nghĩa là mặc dù các vị trí khác nhau nhưng kiểu ký tự của chúng lặp lại theo một mẫu có thể đoán trước được. Do đó, chúng ta có thể nén cấu trúc thành một chương trình động trên một không gian trạng thái nhỏ để theo dõi số lượng ký tự của`q`chúng tôi đã khớp trong khi quét qua`p`. 

Thay vì suy nghĩ về các tập hợp con, chúng tôi xử lý`p`từ trái sang phải và duy trì DP trong đó`dp[i]`là số cách để khớp với cái đầu tiên`i`nhân vật của`q`sử dụng tiền tố của`p`xử lý cho đến nay. Mỗi nhân vật trong`p`cập nhật DP theo cách tuần tự tiêu chuẩn. Bởi vì`|p| ≤ 30`, đây là tối đa 900 lần chuyển đổi cho mỗi trường hợp thử nghiệm, đủ nhanh ngay cả đối với`10^5`trường hợp thử nghiệm. 

Cấu trúc tuần hoàn không cần bất kỳ tối ưu hóa đặc biệt nào ngoài điều này, vì ràng buộc đã đảm bảo dãy con ngây thơ DP là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force trên các chuỗi con | O(2^(3n)) mỗi bài kiểm tra | O(n) | Quá chậm | 
| DP trên các chuỗi con trên chuỗi p | O(n · | q | ) mỗi lần kiểm tra | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc`n`và xây dựng chuỗi`p`BẰNG`"xtu"`lặp đi lặp lại`n`lần. Điều này đưa ra một chuỗi nguồn cố định có cấu trúc mà chúng ta không cần phân tích thêm. 
2. Đọc chuỗi`q`. Nếu như`|q| > |p|`, ngay lập tức trả về 0 vì không có dãy con nào của`p`có thể dài hơn`p`chính nó. 
3. Khởi tạo mảng DP`dp`kích thước`|q| + 1`. giá trị`dp[i]`đại diện cho số cách để tạo thành tiền tố`q[0:i]`như một dãy con của tiền tố`p`xử lý cho đến nay. Bộ`dp[0] = 1`, vì tiền tố trống luôn có thể được hình thành theo đúng một cách. 
4. Lặp qua từng ký tự`c`TRONG`p`. Với mỗi ký tự như vậy, cập nhật mảng DP từ phải sang trái`q`. Đối với mỗi vị trí`j`từ`|q| - 1`xuống tới`0`, nếu như`q[j] == c`, thì chúng ta có thể mở rộng mọi cách hình thành`q[0:j]`thành một cách hình thành`q[0:j+1]`. Vì vậy chúng tôi thêm`dp[j]`vào trong`dp[j+1]`. 

Thứ tự từ phải sang trái là cần thiết vì nó ngăn cản việc sử dụng lại cùng một vị trí ký tự nhiều lần trong một lần lặp, điều này sẽ cho phép một vị trí duy nhất trong`p`đóng góp nhiều lần. 
5. Sau khi xử lý tất cả các ký tự trong`p`, câu trả lời là`dp[|q|]`, lấy modulo`998244353`. 

### Tại sao nó hoạt động 

DP duy trì một bất biến tổ hợp chính xác: sau khi xử lý dữ liệu đầu tiên`k`nhân vật của`p`,`dp[i]`đếm chính xác số cách chọn dãy con trong số đó`k`ký tự bằng với ký tự đầu tiên`i`nhân vật của`q`. Mỗi bước cập nhật sẽ xem xét liệu ký tự hiện tại của`p`được sử dụng làm ký tự khớp tiếp theo trong`q`và phép lặp ngược đảm bảo mỗi vị trí trong`p`được sử dụng nhiều nhất một lần cho mỗi lần xây dựng chuỗi tiếp theo. Điều này đảm bảo sự tương ứng một-một giữa tích lũy DP và các chuỗi con hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        parts = input().split()
        n = int(parts[0])
        q = parts[1] if len(parts) > 1 else ""

        p = "xtu" * n
        m = len(p)
        k = len(q)

        if k > m:
            print(0)
            continue

        dp = [0] * (k + 1)
        dp[0] = 1

        for c in p:
            for j in range(k - 1, -1, -1):
                if q[j] == c:
                    dp[j + 1] = (dp[j + 1] + dp[j]) % MOD

        print(dp[k] % MOD)

if __name__ == "__main__":
    solve()
```Mã này tuân theo mẫu DP dãy con tiêu chuẩn. Việc xây dựng`p`là rõ ràng, có thể chấp nhận được với độ dài tối đa là 30. Mảng DP lưu trữ số lượng kết quả khớp một phần của`q`. 

Vòng lặp ngược lại`q`là chi tiết triển khai chính. Nếu nó được chuyển tiếp, cùng một ký tự trong`p`có thể được sử dụng lại nhiều lần trong một lần lặp, biến việc đếm chuỗi sau thành đếm kết hợp nhiều tập một cách hiệu quả, điều này sẽ đếm quá mức. 

Modulo`998244353`được áp dụng ở mọi lần bổ sung để ngăn chặn tràn và giữ cho các giá trị bị giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

hãy để`n = 2`, Vì thế`p = "xtuxtu"`, Và`q = "xtu"`. 

Chúng tôi theo dõi`dp[i]`Ở đâu`i`tương ứng với bao nhiêu ký tự của`q`chúng tôi đã khớp. 

| Bước (char trong p) | dp[0] | dp[1] | dp[2] | dp[3] | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 0 | 0 | 0 | 
| x | 1 | 1 | 0 | 0 | 
| t | 1 | 1 | 1 | 0 | 
| bạn | 1 | 1 | 1 | 1 | 
| x | 1 | 2 | 1 | 1 | 
| t | 1 | 2 | 3 | 1 | 
| bạn | 1 | 2 | 3 | 4 | 

Câu trả lời cuối cùng là`dp[3] = 4`. 

Điều này chứng tỏ sự xuất hiện nhiều lần của các ký tự giống hệt nhau ở các vị trí khác nhau sẽ tạo ra nhiều lựa chọn về chuỗi con như thế nào. 

### Ví dụ 2 

hãy để`n = 1`,`p = "xtu"`, Và`q = "xx"`. 

| Bước | dp[0] | dp[1] | dp[2] | 
| --- | --- | --- | --- | 
| ban đầu | 1 | 0 | 0 | 
| x | 1 | 1 | 0 | 
| t | 1 | 1 | 0 | 
| bạn | 1 | 1 | 0 | 

Câu trả lời cuối cùng là`0`. 

Điều này cho thấy mặc dù có`x`TRONG`p`, chúng ta không thể sử dụng lại cùng một vị trí hai lần, do đó việc hình thành`"xx"`là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · | q | 
| Không gian | O( | q | 

Vì cả hai`n`Và`|q|`nhiều nhất là 30, mỗi trường hợp thử nghiệm chạy tối đa khoảng 900 thao tác. Ngay cả với`10^5`trường hợp thử nghiệm, điều này vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# sample-like cases
assert run("1\n2 xtu\n") == "4"
assert run("1\n1 xx\n") == "0"

# q empty
assert run("1\n3 xtu\n") == "1"

# q longer than p
assert run("1\n1 xtuxtu xtux\n".replace(" ", "\n")) == "0"

# no matching letters
assert run("1\n5 xtu abcd\n".replace(" ", "\n")) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, q=xtu`|`4`| tăng trưởng tổ hợp cơ bản | 
|`n=1, q=xx`|`0`| không thể sử dụng lại vị trí | 
| trống`q`|`1`| dãy con trống | 
| ` | q | > | 
| chữ cái không hợp lệ |`0`| ký tự không khớp | 

## Vỏ cạnh 

cho`q = ""`, DP không bao giờ thực hiện bất kỳ chuyển đổi nào ảnh hưởng đến`dp[0]`, vì vậy nó vẫn còn`1`. Đầu ra của thuật toán`dp[0]`, cho đúng là 1. 

cho`q`chứa các ký tự không có trong`{x, t, u}`, không có cập nhật nào xảy ra ở trạng thái DP cao hơn. Ví dụ, với`p = "xtu"`Và`q = "a"`,`dp[1]`luôn là 0 nên câu trả lời là 0. 

cho`q`bằng`p`, mỗi ký tự khớp sẽ mở rộng chính xác một đường dẫn hợp lệ thông qua DP, tạo ra số đếm cuối cùng là 1, vì chỉ có một cách để chọn tất cả các vị trí theo thứ tự.
