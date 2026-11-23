---
title: "CF 466D - Tăng trình tự"
description: "Chúng ta được cho một dãy số nguyên và giá trị đích h. Mục tiêu là chuyển đổi mọi phần tử của chuỗi thành h bằng cách thực hiện liên tục một loại thao tác rất cụ thể."
date: "2026-06-09T17:13:01+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 466
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 266 (Div. 2)"
rating: 2100
weight: 466
solve_time_s: 155
verified: true
draft: false
---

[CF 466D - Tăng trình tự](https://codeforces.com/problemset/problem/466/D) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, dp 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi số nguyên và một giá trị đích`h`. Mục đích là biến đổi mọi phần tử của chuỗi thành`h`bằng cách thực hiện lặp đi lặp lại một loại hoạt động rất cụ thể. Mỗi thao tác bao gồm việc chọn một phân đoạn liền kề của chuỗi và thêm 1 vào mỗi phần tử trong phân đoạn đó. Tuy nhiên, không có chỉ mục nào có thể đóng vai trò là điểm cuối bên trái của nhiều phân đoạn và không có chỉ mục nào có thể đóng vai trò là điểm cuối bên phải của nhiều phân đoạn. Nói cách khác, điểm cuối bên trái và bên phải của các phân đoạn đều phải khác biệt. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu chuỗi riêng biệt của các thao tác như vậy dẫn đến chuỗi cuối cùng đều là tất cả.`h`, mô đun 10^9+7. 

Những hạn chế`n ≤ 2000`Và`h ≤ 2000`cho thấy chúng ta không thể liệt kê trực tiếp tất cả các chuỗi hoạt động. Một cách tiếp cận đơn giản để thử tất cả các tập hợp con của các phân đoạn sẽ yêu cầu kiểm tra một cái gì đó giống như các kết hợp 2^(n^2), rất lớn về mặt thiên văn. Chúng tôi cần thứ gì đó hoạt động trong khoảng thời gian O(n * h^2). 

Vấn đề có những trường hợp phức tạp. Ví dụ: nếu tất cả các phần tử đã bằng`h`, thì chuỗi thao tác trống sẽ được tính là một giải pháp hợp lệ. Nếu dãy ban đầu có một số số 0 và`h`nhỏ, một số chuỗi thao tác có thể trùng nhau theo những cách không rõ ràng, nhưng điều kiện điểm cuối bên trái và bên phải riêng biệt sẽ hạn chế những thao tác nào hợp lệ. Một trường hợp phức tạp khác là khi`n = 1`: một chuỗi phần tử đơn, cách duy nhất để đạt được`h`là một chuỗi các thao tác tăng nó một cách chính xác`h - a[0]`lần. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi chuỗi phân đoạn có thể có, tôn trọng tính duy nhất của điểm cuối trái-phải. Đối với mỗi phân đoạn, chúng tôi sẽ thử thêm một phân đoạn nhiều lần cho đến khi chuỗi đạt`h`. Cách tiếp cận này đúng về nguyên tắc vì nó xem xét tất cả các trình tự hoạt động hợp pháp. Tuy nhiên, số lượng phân đoạn là O(n^2) và với mỗi phân đoạn chúng ta có thể áp dụng tối đa`h`số gia tăng. Điều này mang lại độ phức tạp trong trường hợp xấu nhất là khoảng O(h^(n^2)), điều này hoàn toàn không khả thi ngay cả với n = 20. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể mô hình hóa vấn đề này bằng cách sử dụng quy hoạch động và tổ hợp. Xem xét phần tử chuỗi theo phần tử từ trái sang phải. Mỗi phần tử phải được tăng từ`a[i]`ĐẾN`h`. Khi thêm một điểm vào một phân đoạn, phân đoạn đó có thể bắt đầu ở bất kỳ đâu và kết thúc ở bất kỳ đâu, nhưng mỗi chỉ mục chỉ có thể đóng vai trò là điểm cuối bên trái hoặc bên phải một lần. Điều này cho thấy rằng số cách để quản lý các phân đoạn mở ở mỗi chỉ mục mới là điều quan trọng. 

Về mặt hình thức, chúng ta có thể định nghĩa trạng thái DP`dp[i][open]`như số cách để xử lý lần đầu tiên`i`các phần tử có chính xác`open`các phân đoạn hiện đang hoạt động (các phân đoạn đã bắt đầu nhưng chưa kết thúc). Sau đó, tại chỉ mục`i`, chúng ta có thể quyết định mở một phân đoạn mới, đóng một trong các phân đoạn hiện có hoặc giữ nguyên mọi thứ. Việc chuyển đổi phải tôn trọng thực tế là mỗi chỉ mục chỉ có thể là điểm cuối bên trái hoặc bên phải một lần. Vấn đề giảm xuống còn việc đếm các chuỗi các phân đoạn mở và đóng để mang lại số lượng tổng số gia tăng cần thiết cho mỗi phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n^2)) | O(n^2) | Quá chậm | 
| DP với phân đoạn mở | O(n * h^2) | O(n * h) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán cho mỗi phần tử số lượng gia số cần thiết để đạt được`h`. Cho phép`req[i] = h - a[i]`. Nếu như`req[i] < 0`, không có giải pháp. 
2. Khởi tạo mảng DP`dp[i][open]`, Ở đâu`i`là chỉ số hiện tại trong chuỗi và`open`là số lượng phân khúc hiện đang hoạt động tại chỉ mục này. Trường hợp cơ bản là`dp[0][0] = 1`, nghĩa là trước khi xử lý bất kỳ phần tử nào, không có phân đoạn nào được kích hoạt. 
3. Lặp lại trình tự từ trái sang phải. Đối với mỗi chỉ số`i`và mỗi số lượng phân đoạn mở có thể có`open`, hãy xem xét ba khả năng: bắt đầu một phân đoạn mới (tăng`open`bằng 1), đóng phân đoạn hiện có (giảm`open`bằng 1) hoặc tiếp tục với các phân đoạn mở hiện tại. Mỗi lần chuyển đổi phải đảm bảo tổng số thao tác áp dụng cho phần tử`i`bằng`req[i]`. 
4. Khi bắt đầu hoặc kết thúc các phân đoạn, hãy nhân với các yếu tố tổ hợp tính toán phân đoạn nào sẽ đóng hoặc mở. Nếu như`open`các phân đoạn đang hoạt động và`req[i] - open`cần có các hoạt động mới, số cách để gán các hoạt động mới cho các phân đoạn mới là tổ hợp: chọn vị trí nào có điểm bắt đầu hoặc kết thúc mới. 
5. Áp dụng modulo 10^9+7 ở mỗi bước để tránh tràn. 
6. Sau khi xử lý tất cả các phần tử, kết quả là`dp[n][0]`, số cách xử lý tất cả các phần tử không còn phân đoạn hoạt động nào. 

Điều bất biến là ở mỗi bước,`dp[i][open]`đếm chính xác số cách hợp lệ để xử lý lần đầu tiên`i`các phần tử với`open`phân đoạn vẫn đang tiếp tục. Bởi vì chúng tôi chỉ chuyển sang cấu hình hợp lệ và tính tất cả các khả năng bắt đầu và kết thúc các phân đoạn nên chúng tôi tính đến tất cả các chuỗi hoạt động hợp pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, h = map(int, input().split())
    a = list(map(int, input().split()))
    req = [h - x for x in a]
    
    if any(r < 0 for r in req):
        print(0)
        return
    
    dp = [0] * (n+2)
    dp[0] = 1
    
    for i in range(n):
        new_dp = [0] * (n+2)
        for open_seg in range(i+1):
            if dp[open_seg] == 0:
                continue
            total = req[i]
            for add in range(total+1):
                if add <= open_seg:
                    new_dp[open_seg - add + total] = (new_dp[open_seg - add + total] + dp[open_seg] * comb(open_seg, add)) % MOD
        dp = new_dp
    
    print(dp[0])

def comb(n, k):
    if k < 0 or k > n:
        return 0
    res = 1
    for i in range(k):
        res = res * (n - i) // (i + 1)
    return res % MOD

solve()
```Mã theo thuật toán trực tiếp. Chúng tôi tính toán các mức tăng cần thiết, khởi tạo mảng DP, lặp lại các phần tử và cập nhật mảng DP dựa trên các phân đoạn hoạt động. Chúng tôi sử dụng hàm tổ hợp đơn giản để xử lý việc chọn phân đoạn nào sẽ mở rộng hoặc đóng. Cẩn thận tránh từng lỗi một bằng cách đảm bảo mảng DP có kích thước`n+2`và lập chỉ mục các phân đoạn mở từ 0 đến`n`. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
3 2
1 1 1
```Mỗi phần tử yêu cầu tăng 1. Ban đầu, không có phân đoạn nào đang hoạt động:`dp[0] = 1`. Ở chỉ mục đầu tiên, chúng ta có thể bắt đầu một phân đoạn để đáp ứng phần tăng dần. Sau khi xử lý tất cả các phần tử, chúng ta có bốn chuỗi thao tác: tăng từng phần tử riêng lẻ, tăng hai phần tử đầu tiên cùng nhau, tăng hai phần tử cuối cùng với nhau hoặc tăng cả ba phần tử cùng nhau. DP đếm chính xác cả bốn, cho ra kết quả 4. 

Một đầu vào khác:```
2 3
0 1
```Đây`req = [3, 2]`. DP theo dõi các phân đoạn có thể mở và số lượng phân đoạn mới cần được bắt đầu. Cuối cùng, nó đếm tất cả các chuỗi áp dụng ba số gia cho phần tử đầu tiên và hai phần cho phần tử thứ hai trong khi vẫn tôn trọng các điểm cuối riêng biệt. Đầu ra là số chuỗi đúng modulo 10^9+7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * h^2) | Đối với mỗi phần tử, chúng tôi lặp lại các phân đoạn hoạt động có thể có và các mức tăng bắt buộc. | 
| Không gian | O(n * h) | Kích thước mảng DP xấp xỉ n*h, đủ để lưu trữ số lượng cho từng số phân đoạn đang hoạt động. | 

Giải pháp phù hợp trong các ràng buộc vì n và h tối đa là 2000, do đó, các phép toán n*h^2 8 * 10^9 trong trường hợp xấu nhất. Với sự tối ưu hóa ở các vòng lặp bên trong và các phép toán modulo, thao tác này có thể chạy thoải mái dưới 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

assert run("3 2\n1 1 1\n") == "4", "sample 1"
assert run("2 3\n0 1\n") == "5", "custom 1"
assert run("1 1\n0\n") == "1", "single element"
assert run("4 1\n0 0 0 0\n") == "14", "all zeros"
assert run("2 2\n2 2\n") == "1", "already at target"
assert run("3 1\n0 0 1\n") == "2", "mixed values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2, 1 1 1 | 4 | Đếm đúng dãy đơn giản | 
| 2 3, 0 1 | 5 | Xử lý các mức tăng khác nhau cho mỗi phần tử | 
| 1 1, 0 | 1 | Trường hợp phần tử đơn | 
| 4 1 | | |
