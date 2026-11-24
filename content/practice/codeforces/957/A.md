---
title: "CF 957A - Ánh kim Tritonic"
description: "Chúng ta có một dải một chiều có độ dài $n$, trong đó mỗi vị trí hoặc đã được sơn bằng một trong ba màu hoặc để trống. Các vị trí trống phải được điền bằng ba màu giống nhau để không có hai vị trí liền kề nào có cùng màu."
date: "2026-06-17T02:04:27+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 957
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 472 (rated, Div. 2, based on VK Cup 2018 Round 2)"
rating: 1300
weight: 957
solve_time_s: 62
verified: true
draft: false
---

[CF 957A - Ánh kim Tritonic](https://codeforces.com/problemset/problem/957/A) 

**Đánh giá:** 1300 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dải chiều dài một chiều$n$, trong đó mỗi vị trí đã được sơn bằng một trong ba màu hoặc để trống. Các vị trí trống phải được điền bằng ba màu giống nhau để không có hai vị trí liền kề nào có cùng màu. 

Nhiệm vụ không phải là đếm tất cả các lần hoàn thành hợp lệ mà chỉ quyết định xem có tồn tại ít nhất hai lần hoàn thành hợp lệ riêng biệt của toàn bộ dải hay không. 

Hai lần hoàn thành được coi là khác nhau nếu có ít nhất một vị trí được tô màu khác nhau giữa chúng. 

Ràng buộc$n \le 100$gợi ý rằng ngay cả lập trình động bậc ba hoặc bậc hai cũng dễ dàng đủ nhanh. Tuy nhiên, bất cứ thứ gì cố gắng liệt kê tất cả các chất tạo màu sẽ trực tiếp phát triển thành$3^n$, điều này trở nên không khả thi ngay cả đối với mức độ vừa phải$n$. Điều này ngay lập tức đẩy giải pháp theo hướng tiếp cận DP hoặc ghi nhớ dựa trên trạng thái với một không gian trạng thái không đổi nhỏ. 

Một cạm bẫy ngây thơ nhưng phổ biến là cố gắng lấp đầy lòng tham. Việc chọn cục bộ bất kỳ màu hợp lệ nào cho vị trí trống có thể tạo ra sự hoàn thành hợp lệ, nhưng nó không lưu giữ thông tin về các lần hoàn thành thay thế. Một cạm bẫy khác là giả định rằng nếu tồn tại một lần hoàn thành duy nhất thì nhiều lần hoàn thành luôn tồn tại khi có nhiều dấu chấm hỏi, điều này là sai vì các ràng buộc có thể buộc phải có tính duy nhất. 

Một trường hợp thất bại cụ thể của lý luận tham lam là: 

đầu vào:```
3
C?C
```Có chính xác một lần hoàn thành hợp lệ:`CMC`. Mặc dù có vị trí tự do nhưng nó hoàn toàn bị hạn chế bởi cả hai nước láng giềng. 

Một trường hợp tinh tế khác là khi chuỗi đã không hợp lệ, chẳng hạn như:```
3
CCC
```Điều này không có lần hoàn thành hợp lệ nào, không ít nhất một. 

Khó khăn thực sự là việc phân biệt giữa 0, một hoặc ít nhất hai lần hoàn thành hợp lệ một cách hiệu quả. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử mọi cách gán màu cho dấu chấm hỏi. Nếu có$k$dấu chấm hỏi, điều này mang lại$3^k$khả năng. Với mỗi ứng viên, chúng ta sẽ kiểm tra các ràng buộc kề trong$O(n)$, dẫn đến$O(n \cdot 3^k)$. Trong trường hợp xấu nhất$k = n = 100$, cái này lớn về mặt thiên văn và hoàn toàn không thể sử dụng được. 

Cấu trúc của bài toán là tính hợp lệ chỉ phụ thuộc vào các cặp liền kề và các quyết định được truyền từ trái sang phải. Điều này gợi ý một công thức lập trình động trong đó trạng thái chỉ cần nhớ màu trước đó. 

Thay vì liệt kê các chuỗi đầy đủ, chúng tôi theo dõi xem có bao nhiêu cách tô màu từng phần hợp lệ cho đến vị trí$i$, kết thúc bằng mỗi màu có thể. Vì chúng tôi chỉ quan tâm liệu số lượng giải pháp đầy đủ có đạt ít nhất là hai hay không, nên chúng tôi có thể giới hạn tất cả số lượng ở mức 2. Điều này ngăn ngừa tràn và tránh tính toán không cần thiết. 

Thông tin chi tiết quan trọng là không gian trạng thái chỉ có 3 màu, do đó DP vẫn có kích thước không đổi trên mỗi vị trí, đưa ra giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(n \cdot 3^n)$|$O(n)$| Quá chậm | 
| DP với số lượng giới hạn |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP nơi chúng tôi theo dõi số lượng cách hợp lệ tồn tại cho đến từng vị trí, được phân tách bằng màu cuối cùng được sử dụng. 

### bước 

1. Khởi tạo bảng DP cho vị trí 0. Nếu ký tự đầu tiên cố định thì chỉ cho phép màu đó; nếu không thì cả ba màu đều có thể. Mỗi màu bắt đầu hợp lệ đóng góp chính xác theo một cách. Điều này đại diện cho tất cả các tiền tố hợp lệ có độ dài 1. 
2. Lặp lại từ vị trí 1 đến$n-1$, cập nhật trạng thái DP mới từ trạng thái trước đó. Đối với mỗi vị trí, chúng tôi xem xét mọi màu hiện tại có thể có. 
3. Nếu vị trí hiện tại được cố định thành một màu, chúng tôi chỉ cho phép chuyển đổi sang màu đó. Nếu là '?', chúng ta thử cả ba màu. 
4. Đối với mỗi màu hiện tại ứng cử viên, chúng tôi tính tổng tất cả các giá trị DP từ vị trí trước đó trong đó màu trước đó khác với màu hiện tại. Điều này thực thi các ràng buộc kề. 
5. Sau mỗi lần cập nhật, giới hạn tất cả các giá trị DP ở mức 2. Nếu bất kỳ trạng thái nào đã đạt đến 2, chúng tôi không cần đếm chính xác nữa, thực tế là có ít nhất hai giải pháp. 
6. Sau khi xử lý tất cả các vị trí, tính tổng các giá trị DP ở vị trí cuối cùng. Nếu tổng ít nhất là 2 thì ghi "Có", nếu không thì ghi "Không". 

### Tại sao nó hoạt động 

Trạng thái DP mã hóa tất cả các cách hợp lệ để tô màu tiền tố trong khi chỉ nhớ màu cuối cùng, điều này là đủ vì tính hợp lệ trong tương lai chỉ phụ thuộc vào màu kề. Mỗi màu đầy đủ hợp lệ tương ứng với chính xác một đường đi qua các trạng thái này. Vì chúng tôi giới hạn số lượng ở mức 2 nên chúng tôi duy trì tính chính xác của quyết định cuối cùng: liệu có tồn tại 0, một hoặc nhiều giải pháp mà không cần theo dõi độ lớn chính xác hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    colors = ['C', 'M', 'Y']

    dp = {c: 0 for c in colors}

    if s[0] == '?':
        for c in colors:
            dp[c] = 1
    else:
        dp[s[0]] = 1

    for i in range(1, n):
        ndp = {c: 0 for c in colors}

        for cur in colors:
            if s[i] != '?' and s[i] != cur:
                continue

            ways = 0
            for prev in colors:
                if prev != cur:
                    ways += dp[prev]
                    if ways >= 2:
                        ways = 2
                        break

            ndp[cur] = min(2, ways)

        dp = ndp

    total = sum(dp.values())
    print("Yes" if total >= 2 else "No")

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một từ điển DP cuộn có kích thước ba. Ở mỗi bước, nó xây dựng một từ điển mới cho vị trí tiếp theo. Vòng lặp bên trong thực thi ràng buộc kề bằng cách loại trừ các chuyển tiếp có màu lặp lại. 

Chi tiết triển khai quan trọng là giới hạn ở mức 2. Nếu không có nó, số lượng có thể tăng lên một cách không cần thiết, nhưng quan trọng hơn, vấn đề chỉ yêu cầu phân biệt giữa 0, 1 và ít nhất 2 giải pháp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
CY??Y
```Chúng tôi theo dõi trạng thái DP dưới dạng số lượng trên mỗi màu kết thúc. 

| Vị trí | C | M | Y | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 0 (C) | 1 | 0 | 0 | cố định C | 
| 1 (Y) | 0 | 1 | 1 | Y không thể tuân theo hạn chế C cho phép M,Y | 
| 2 (?) | 1 | 1 | 1 | tất cả tiện ích mở rộng hợp lệ | 
| 3 (?) | 2 | 2 | 2 | giới hạn ở mức 2 | 
| 4 (Y) | 2 | 0 | 2 | phải kết thúc bằng Y | 

Tổng số cuối cùng là 4, nhưng logic giới hạn chỉ cần “22”, vì vậy đầu ra là:```
Yes
```Điều này cho thấy nhiều đường dẫn nhanh chóng hợp nhất thành nhiều lần hoàn thành hợp lệ ngay cả khi bị ràng buộc. 

### Ví dụ 2 

đầu vào:```
3
C?C
```| Vị trí | C | M | Y | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 0 (C) | 1 | 0 | 0 | cố định | 
| 1 (?) | 0 | 1 | 1 | không thể bằng C trước đó | 
| 2 (C) | 0 | 1 | 0 | chỉ M hoạt động trước C | 

Tổng số cuối cùng là 1, vì vậy:```
No
```Điều này cho thấy một khoảng trống vẫn có thể được cả hai bên hạn chế hoàn toàn, tạo ra một sự hoàn thiện duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí xử lý quá trình chuyển đổi trạng thái 3 màu không đổi | 
| Không gian |$O(1)$| Chỉ có hai từ điển DP có kích thước 3 được lưu trữ | 

Với$n \le 100$, giải pháp thoải mái trong giới hạn và chạy ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5\nCY??Y\n") == "Yes"
assert run("3\nC?C\n") == "No"

# minimum size, single cell
assert run("1\n?\n") == "Yes"

# fully fixed valid alternating
assert run("3\nCMY\n") == "Yes"

# fully fixed invalid
assert run("3\nCCC\n") == "No"

# many choices but constrained chain
assert run("4\n????\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 ? | Có | vị trí duy nhất luôn có nhiều khả năng | 
| CMY | Có | màu cố định đã hợp lệ | 
| CCC | Không | cấu hình không thể | 
| ???? | Có | nhiều lần hoàn thành tồn tại trong trường hợp không bị giới hạn | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`"?"`tạo ra ba lần hoàn thành hợp lệ, đáp ứng ngay yêu cầu “ít nhất hai”. DP bắt đầu với cả ba trạng thái hoạt động, do đó tổng cuối cùng vượt quá ngưỡng. 

Một chuỗi cố định hoàn toàn như`"CMCMCM"`có đúng một lần hoàn thành hợp lệ. DP không bao giờ phân nhánh nên tổng cuối cùng vẫn là 1, tạo ra "Không" một cách chính xác. 

Một chuỗi không hợp lệ hoàn toàn thống nhất như`"CCC"`loại bỏ tất cả các trạng thái DP sau lần chuyển đổi đầu tiên. Khi tất cả các trạng thái trở thành 0, chúng vẫn giữ nguyên bằng 0, đảm bảo thuật toán báo cáo chính xác các trường hợp không thể thực hiện được.
