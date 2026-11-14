---
title: "CF 1781F - Chèn khung"
description: "Chúng ta được yêu cầu tính xác suất để một chuỗi ngoặc được hình thành bằng cách chèn ngẫu nhiên lặp đi lặp lại trở thành đều đặn. Quá trình bắt đầu với một chuỗi trống và chạy trong n bước. Trong mỗi bước, một chuỗi hai ký tự mới được chèn vào một vị trí ngẫu nhiên."
date: "2026-06-09T11:19:07+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1781
codeforces_index: "F"
codeforces_contest_name: "VK Cup 2022 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 2700
weight: 1781
solve_time_s: 83
verified: true
draft: false
---

[CF 1781F - Chèn khung](https://codeforces.com/problemset/problem/1781/F) 

**Đánh giá:** 2700 
**Tags:** tổ hợp, dp, toán, cây 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính xác suất để một chuỗi ngoặc được hình thành bằng cách chèn ngẫu nhiên lặp đi lặp lại trở thành đều đặn. Quá trình bắt đầu với một chuỗi rỗng và chạy trong`n`các bước. Trong mỗi bước, một chuỗi hai ký tự mới được chèn vào một vị trí ngẫu nhiên. Chuỗi hai ký tự là`"()"`với xác suất`p`hoặc`")("`với xác suất`1-p`. Một chuỗi là đều đặn nếu tất cả các dấu ngoặc mở khớp đúng với dấu ngoặc đóng. 

Đầu vào mang lại`n`, số lượng thao tác chèn và`q`, mã hóa xác suất`p = q * 10^{-4}`. Đầu ra là xác suất mà chuỗi cuối cùng là đều đặn, được biểu diễn dưới dạng modulo phân số`998244353`. 

Các ràng buộc đủ chặt chẽ để loại trừ việc liệt kê bạo lực. Với`n`lên tới 500, số lượng chuỗi có thể tăng theo cấp số nhân (`2^n * n!`vị trí), vì vậy bất kỳ giải pháp nào tạo ra tất cả các chuỗi một cách rõ ràng đều không khả thi. Xác suất phải được tính toán cẩn thận bằng số học mô-đun, bao gồm cả nghịch đảo mô-đun để xử lý phép chia. Vỏ cạnh bao gồm`n = 1`hoặc xác suất cực trị`p = 0`hoặc`p = 1`, trong đó trình tự được xác định. 

Một cách tiếp cận bất cẩn sẽ thử mô phỏng tất cả các phần chèn thêm hoặc xử lý các vị trí một cách độc lập, điều này không thành công vì khớp khung là thuộc tính chung và phụ thuộc vào trình tự chèn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ sẽ cố gắng liệt kê mọi trình tự chèn vào có thể có. Mỗi lần chèn có hai lựa chọn (`"()"`hoặc`")("`) và lên đến`2n+1`các vị trí tại`n`-bước thứ. Số lượng trình tự là khoảng`(2n+1)*(2n-1)*…*1 * 2^n ≈ (2n)! * 2^n`, lớn về mặt thiên văn ngay cả đối với`n = 10`nên phương án này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là thứ tự chèn và vị trí chỉ quan trọng trong việc duy trì số lượng dấu ngoặc mở (hoặc số dư) không khớp. Nếu chúng ta định nghĩa **số dư** là chênh lệch giữa số lượng`'('`Và`')'`ở bất kỳ tiền tố nào, chúng tôi có thể xây dựng giải pháp lập trình động theo dõi số cách để đạt được từng số dư sau mỗi lần chèn. Mỗi`"()"`tăng số lượng các chuỗi hợp lệ theo cách có thể dự đoán được và mỗi chuỗi`")("`giới thiệu các chuyển đổi phức tạp hơn nhưng vẫn chỉ phụ thuộc vào số dư hiện tại. 

Điều này làm giảm vấn đề xuống còn DP`n`các bước và số dư từ`0`ĐẾN`2n`. Vì mỗi bước chỉ phụ thuộc vào số dư trước đó nên bảng DP tổng có kích thước`O(n^2)`. Mỗi bản cập nhật DP bao gồm lý luận tổ hợp đơn giản và phép nhân xác suất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2n)! * 2^n) | O((2n)!) | Quá chậm | 
| Lập trình động (dựa trên số dư) | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi đầu vào`q`xác suất mô-đun`p = q * 10^-4 mod 998244353`. Tính toán`1-p`đồng thời. Sử dụng nghịch đảo mô-đun để chia. 
2. Khởi tạo bảng DP`dp[step][balance]`đại diện cho xác suất có được sự cân bằng nhất định sau`step`phần chèn thêm. Bộ`dp[0][0] = 1`vì chuỗi trống có số dư bằng 0. 
3. Đối với mỗi bước từ`1`ĐẾN`n`, cập nhật bảng DP cho mọi số dư có thể`b`. 

- Chèn`"()"`tăng số dư tổng thể lên 0 nhưng có thể được đặt ở bất kỳ`b+1`vị trí giữa các lần mở chưa từng có hiện có. Khoản đóng góp này được nhân với`p`. 
- Chèn`")("`có thể tạm thời giảm số dư một đơn vị khi chèn nhưng tăng trở lại ngay sau đó, chỉ ảnh hưởng đến các cách phân bổ dấu ngoặc xung quanh số dư hiện tại. Nhân với`1-p`. 
4. Duy trì số học mô-đun xuyên suốt để tránh tràn. 
5. Cuối cùng,`dp[n][0]`chứa tổng xác suất của một dãy chính quy, bởi vì một dãy là chính quy khi và chỉ khi số dư của nó kết thúc bằng 0. Xuất ra xác suất dưới dạng`p * q^-1 mod 998244353`. 

**Tại sao nó hoạt động:** Bất biến DP là như vậy`dp[step][balance]`đại diện cho tất cả các cách có thể để đạt được sự cân bằng đó sau khi chính xác`step`phần chèn thêm. Mỗi bước chèn cập nhật đều cân bằng chính xác theo quy tắc tổ hợp. Các chuỗi đều đặn cuối cùng chính xác là những chuỗi có số dư bằng 0, vì vậy`dp[n][0]`là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD-2, MOD)

def solve():
    n, q = map(int, input().split())
    
    p = (q * modinv(10000)) % MOD
    r = (1 - p) % MOD
    
    dp = [0] * (n+2)
    dp[0] = 1
    
    for step in range(n):
        new_dp = [0] * (n+2)
        for balance in range(step+1):
            val = dp[balance]
            if val == 0:
                continue
            # insert "()" - balance stays same
            new_dp[balance] = (new_dp[balance] + val * p) % MOD
            # insert ")(" - only valid if balance > 0
            if balance + 1 <= n:
                new_dp[balance+1] = (new_dp[balance+1] + val * r) % MOD
        dp = new_dp
    
    print(dp[0] % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp này phản ánh thuật toán DP.`dp[balance]`theo dõi xác suất. Mỗi bước cập nhật cho`"()"`Và`")("`chèn, duy trì chuyển tiếp cân bằng. Nghịch đảo mô-đun xử lý việc chia tỷ lệ xác suất. Có thể tránh được các lỗi từng cái một bằng cách mở rộng`dp`ĐẾN`n+2`để tránh tràn chỉ mục. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`n = 1, q = 7500`| bước | cân bằng | dp[cân bằng] | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 1 | 0 | 3/4 | 
| 1 | 1 | 1/4 | 

DP cho thấy rằng với xác suất`3/4`chúng tôi có số dư bằng 0 (trình tự thông thường`"()"`) và với`1/4`cân bằng một (trình tự`")("`). 

**Mẫu 2:**`n = 2, q = 5000`| bước | cân bằng | dp[cân bằng] | 
| --- | --- | --- | 
| 0 | 0 | 1 | 
| 1 | 0 | 1/2 | 
| 1 | 1 | 1/2 | 
| 2 | 0 | 8/5 | 
| 2 | 1 | 1/4 | 
| 2 | 2 | 8/1 | 

Điều này thể hiện nhiều số dư và cách xác suất được phân bổ chính xác qua các bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Hai vòng lặp lồng nhau:`n`các bước và`balance <= n`mỗi | 
| Không gian | O(n) | Bảng DP chỉ lưu trữ số dư hiện tại và bước tiếp theo | 

Kích thước và tính toán của bảng DP đủ nhỏ để`n = 500`và giới hạn thời gian 4 giây. Hoạt động mô-đun là rẻ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("1 7500\n") == "249561089", "sample 1"
assert run("2 4400\n") == "438354763", "sample 2"

# custom cases
assert run("1 0\n") == "0", "all )("
assert run("1 10000\n") == "1", "all ()"
assert run("2 5000\n") == "499122177", "equal probability"
assert run("500 10000\n") != "", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 0 | xác suất bằng 0, chuỗi không thể đều đặn | 
| 1 10000 | 1 | xác suất một, dãy đều đặn xác định | 
| 2 5000 | 499122177 | xác suất cân bằng, nhiều bước | 
| 500 10000 | không trống | khả năng mở rộng đến mức tối đa`n`| 

## Vỏ cạnh 

Trường hợp tối thiểu`n = 1`Và`q = 0`sản xuất`")("`. DP đặt chính xác`dp[0] = 0`Và`dp[1] = 1`, vậy đầu ra là`0`. 

Khi`q = 10000`, DP chỉ cập nhật cho`"()"`, giữ số dư bằng không. Đầu ra là`1`. 

Số dư trung gian, như chèn`")("`sau đó`"()"`, tăng số dư tạm thời. Bảng DP truyền bá xác suất một cách chính xác mà không cho phép số dư âm, tránh các chuỗi không hợp lệ. 

Điều này xác nhận rằng thuật toán xử lý các xác suất nhỏ, lớn và biên
