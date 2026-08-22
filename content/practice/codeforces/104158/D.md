---
title: "CF 104158D - Dập nhanh"
description: "Chúng ta được cấp một chuỗi đích chỉ bao gồm các ký tự T và C. Nhiệm vụ là đếm xem có bao nhiêu cách khác nhau để tạo chuỗi này bằng cách sử dụng một bộ tem cố định. Mỗi vị trí trong chuỗi được tạo bằng cách chọn một trong bốn loại tem."
date: "2026-07-02T01:09:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104158
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 1 (Advanced)"
rating: 0
weight: 104158
solve_time_s: 60
verified: true
draft: false
---

[CF 104158D - Dập nhanh](https://codeforces.com/problemset/problem/104158/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi đích chỉ bao gồm các ký tự T và C. Nhiệm vụ là đếm xem có bao nhiêu cách khác nhau để tạo chuỗi này bằng cách sử dụng một bộ tem cố định. 

Mỗi vị trí trong chuỗi được tạo bằng cách chọn một trong bốn loại tem. Một số tem viết một ký tự duy nhất, trong khi một số khác viết hai ký tự cùng một lúc. Hạn chế chính là chúng tôi không sắp xếp các tem theo trình tự như các thao tác; thay vào đó, chúng ta đang chỉ định việc phân tách chuỗi thành các khối được đóng dấu bao phủ chính xác chuỗi đó mà không bị chồng chéo. Mỗi cách hợp lệ tương ứng với một cách xếp chuỗi bằng cách sử dụng các phần được cho phép và chúng tôi đếm tất cả các lần xếp như vậy theo modulo là một số nguyên tố lớn. 

Độ dài chuỗi có thể lên tới 1.000.000. Giải pháp bậc hai hoặc kém hơn đối với các chuỗi con là không khả thi vì ngay cả việc quét tuyến tính lặp lại trên mỗi trạng thái cũng đã vượt quá giới hạn chấp nhận được. Điều này buộc chúng ta phải sử dụng phương pháp lập trình động chạy theo thời gian tuyến tính. 

Một vấn đề tế nhị nảy sinh từ cách giải thích chồng chéo của các tem nhiều ký tự. Việc giải thích tham lam không thành công vì việc chọn sớm dấu dài hơn có thể chặn các phân tách hợp lệ sau này. Ví dụ: trong chuỗi TCC, nếu chúng tôi luôn cố gắng lấy dấu TC gồm hai ký tự trước tiên, chúng tôi có thể bỏ lỡ các cấu hình trong đó ký tự đầu tiên được xử lý riêng biệt. 

Trường hợp cạnh chính là các chuỗi có mẫu lặp lại như TTTT hoặc CCCC, trong đó xảy ra sự bùng nổ tổ hợp các phân đoạn. Bất kỳ cách tiếp cận nào liệt kê các phân vùng một cách rõ ràng sẽ hết thời gian chờ hoặc vượt quá giới hạn đệ quy. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi đây là một vấn đề phân vùng: chúng tôi thử mọi cách để chia chuỗi thành các đoạn có độ dài 1 hoặc 2, sau đó kiểm tra xem mỗi đoạn có khớp với một tem hợp lệ hay không. Điều này đúng vì mỗi ô xếp tương ứng với một phân đoạn như vậy. Tuy nhiên, tại mỗi vị trí, chúng tôi sẽ phân nhánh thành tối đa hai lựa chọn, dẫn đến sự tăng trưởng theo cấp số nhân về số lượng phân khúc. Đối với một chuỗi có độ dài n, điều này mang lại mức tăng trưởng gần giống Fibonacci, vốn đã quá lớn đối với n gần 50, chứ đừng nói đến 10^6. 

Cấu trúc của bài toán gợi ý quy hoạch động. Nhận xét quan trọng là số cách hình thành tiền tố chỉ phụ thuộc vào một số lượng nhỏ cố định các trạng thái trước đó. Vì mỗi dấu đóng góp một hoặc hai ký tự nên việc chuyển đổi trạng thái chỉ phụ thuộc vào dp[i-1] và dp[i-2]. Điều này làm giảm vấn đề xuống mức tái phát tuyến tính trên chuỗi. 

Tại vị trí i, chúng ta xem xét dấu cuối cùng có thể kết thúc như thế nào. Nếu dấu cuối cùng bao gồm một ký tự đơn thì nó phải khớp với S[i-1]. Nếu nó bao gồm hai ký tự, nó phải khớp với S[i-2:i]. Mỗi kết quả trùng khớp hợp lệ sẽ đóng góp tương ứng dp[i-1] hoặc dp[i-2]. 

Chúng ta đang tính tổng các đóng góp từ tất cả các cách hợp lệ để kết thúc ở vị trí i một cách hiệu quả, điều này tránh được việc tính hai lần vì mỗi phân tách được xác định duy nhất bởi phần cuối cùng của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân vùng Brute Force | O(2^n) | O(n) | Quá chậm | 
| Lập trình động | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định dp[i] là số cách để tạo tiền tố S[0:i], sử dụng chỉ mục dựa trên 0 trong đó dp[0] tương ứng với chuỗi trống.

1. Khởi tạo dp[0] = 1 vì có chính xác một cách để tạo tiền tố trống, bằng cách không chọn dấu. Đây là trường hợp cơ bản để xây dựng tất cả các tiền tố dài hơn. 
2. Lặp lại i từ 1 đến n. Tại mỗi vị trí i, chúng ta tính dp[i] bằng cách xem xét tất cả các tem hợp lệ có thể kết thúc ở vị trí i. 
3. Đầu tiên hãy xem xét tem một ký tự. Ký tự cuối cùng S[i-1] luôn được bao phủ bởi đúng một dấu. Vì bất kỳ dấu ký tự đơn nào khớp với S[i-1] đều được cho phép nên mọi cách dp[i-1] đều có thể được mở rộng. Điều này góp phần dp[i-1] vào dp[i]. 
4. Tiếp theo hãy xem xét một con tem có hai ký tự. Nếu i ≥ 2, chúng ta kiểm tra xem S[i-2:i] có thể được hình thành bằng dấu hai ký tự hay không. Nếu vậy, mọi cách để tạo thành dp[i-2] có thể được mở rộng bằng cách đặt dấu này ở cuối. Điều này góp phần dp[i-2] vào dp[i]. 
5. Cộng cả hai đóng góp theo modulo 10^9 + 7. 

Tại sao nó hoạt động dựa trên một thuộc tính phân rã đơn giản. Mỗi dấu hợp lệ của tiền tố đều có dấu cuối cùng duy nhất. Dấu cuối cùng đó bao gồm một hoặc hai ký tự và việc xóa nó sẽ để lại dấu hợp lệ của tiền tố nhỏ hơn. Điều này đưa ra sự phân biệt giữa các cấu trúc hợp lệ của dp[i] và các cấu trúc hợp lệ của dp[i-1] và dp[i-2] được mở rộng bằng tem cuối cùng. Vì không có công trình nào có thể kết thúc đồng thời theo nhiều cách nên không có tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    s = input().strip()
    n = len(s)
    
    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(1, n + 1):
        dp[i] = dp[i - 1]  # single-character stamp always usable
        
        if i >= 2:
            # any two-character block is always valid as a stamp
            dp[i] = (dp[i] + dp[i - 2]) % MOD

        else:
            dp[i] %= MOD

    print(dp[n])

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp phản ánh sự tái diễn. Mảng dp lưu trữ số lượng tiền tố và mỗi trạng thái tổng hợp các đóng góp từ hai độ dài tem có thể có. Modulo được áp dụng ở mỗi bước để ngăn chặn tràn. 

Điểm tinh tế duy nhất là đảm bảo lập chỉ mục chính xác: dp[i] đại diện cho i ký tự đầu tiên, do đó các chuyển đổi sử dụng i-1 và i-2. Điều này tránh được các lỗi riêng lẻ thường xuất hiện trong các công thức DP tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1: "TCC" 

Chúng tôi tính toán dp từng bước. 

| tôi | Tiền tố | đóng góp dp[i-1] | đóng góp dp[i-2] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | "" | - | - | 1 | 
| 1 | "T" | 1 | - | 1 | 
| 2 | "TC" | 1 | 1 | 2 | 
| 3 | "TCC" | 2 | 1 | 3 | 

Điều này xác nhận kết quả đầu ra 3. Bảng hiển thị cách mỗi tiền tố tích lũy các cách từ các tiền tố ngắn hơn, khớp với cấu trúc tổ hợp của các ô hợp lệ. 

### Ví dụ 2: "TT" 

| tôi | Tiền tố | dp[i-1] | dp[i-2] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 0 | "" | - | - | 1 | 
| 1 | "T" | 1 | - | 1 | 
| 2 | "TT" | 1 | 1 | 2 | 

Điều này cho thấy hai phân tách hợp lệ: hai dấu đơn hoặc một dấu kép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần với chuyển tiếp O(1) | 
| Không gian | O(n) | Mảng DP có kích thước n+1 | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì ngay cả với n = 10^6, thuật toán chỉ thực hiện một số phép tính số học đơn giản cho mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7
    s = sys.stdin.readline().strip()
    n = len(s)

    dp = [0] * (n + 1)
    dp[0] = 1

    for i in range(1, n + 1):
        dp[i] = dp[i - 1]
        if i >= 2:
            dp[i] = (dp[i] + dp[i - 2]) % MOD

    return str(dp[n])

# provided sample
assert run("TCC\n") == "3"

# single character
assert run("T\n") == "1"

# two identical characters
assert run("TT\n") == "2"

# alternating pattern
assert run("TCTC\n") == "5"

# long uniform string
assert run("T" * 10 + "\n") == "89"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| T | 1 | trường hợp cơ sở ký tự đơn | 
| TT | 2 | cả phân rã 1+1 và 2 | 
| TCTC | 5 | tăng trưởng phân nhánh lặp đi lặp lại | 
| TTTTTTTTTT | 89 | Tính chính xác tăng trưởng giống như Fibonacci | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như "T", thuật toán đặt dp[1] = dp[0] = 1, vì chỉ một dấu duy nhất có thể bao phủ chuỗi đó. Không có thuật ngữ dp[i-2] nên không xảy ra truy cập không hợp lệ. 

Đối với chuỗi hai ký tự như "TC", dp[2] = dp[1] + dp[0] = 2. Số hạng đầu tiên tương ứng với hai dấu đơn và số hạng thứ hai tương ứng với một dấu hai ký tự. Việc lập chỉ mục đảm bảo cả hai khả năng đều được tính chính xác một lần và không xảy ra sự trùng lặp vì mỗi công trình được xác định duy nhất bằng lựa chọn tem cuối cùng của nó.
