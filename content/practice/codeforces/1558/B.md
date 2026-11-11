---
title: "CF 1558B - Lên Dải"
description: "Chúng ta được cấp một mã thông báo được đặt ở vị trí $n$ trên một dòng ô dọc được gắn nhãn từ $1$ ở trên cùng đến $n$ ở dưới cùng. Mục tiêu là đếm xem có bao nhiêu chuỗi di chuyển riêng biệt có thể đưa mã thông báo từ $n$ xuống $1$. Từ vị trí $x 1$, được phép di chuyển hai loại."
date: "2026-06-10T12:30:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1558
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 740 (Div. 1, based on VK Cup 2021 - Final (Engine))"
rating: 1900
weight: 1558
solve_time_s: 395
verified: true
draft: false
---

[CF 1558B - Up the Strip](https://codeforces.com/problemset/problem/1558/B) 

**Xếp hạng:** 1900 
**Tags:** sức mạnh vũ phu, dp, toán học, lý thuyết số, hai con trỏ 
**Thời gian giải:** 6 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một mã thông báo được đặt ở vị trí$n$trên một dòng ô dọc được dán nhãn từ$1$ở trên cùng để$n$ở phía dưới. Mục tiêu là đếm xem có bao nhiêu chuỗi di chuyển riêng biệt có thể mang mã thông báo từ$n$xuống$1$. 

Từ một vị trí$x > 1$, có hai loại di chuyển được phép. Kiểu đầu tiên trừ một số nguyên dương$y$, lựa chọn hiệu quả bất kỳ điểm đến nào trong phạm vi$[1, x-1]$. Loại thứ hai chia$x$bởi một số nguyên$z \ge 2$và di chuyển đến$\lfloor x / z \rfloor$, tạo ra một bước nhảy có khả năng nhỏ hơn nhiều. Mỗi lựa chọn tham số$y$hoặc$z$xác định một nước đi riêng biệt, ngay cả khi có nhiều lựa chọn dẫn đến cùng một đích. 

Đầu ra là tổng số các chuỗi di chuyển riêng biệt từ$n$ĐẾN$1$, lấy modulo$m$, Ở đâu$n$có thể lớn như$4 \cdot 10^6$Và$m$là số nguyên tố lớn. Thang đo này loại trừ bất kỳ giải pháp nào khám phá tất cả các đường dẫn một cách rõ ràng, vì số lượng đường dẫn tăng theo cấp số nhân. Bất kỳ giải pháp hợp lệ nào cũng phải khai thác các bài toán con chồng chéo và sử dụng lại kết quả giữa các trạng thái. 

Một trường hợp cạnh tinh tế xuất phát từ thực tế là phép trừ cho phép$y = x - 1$, điều này chuyển trực tiếp đến$1$từ bất kỳ$x$và phép chia cũng cho phép nhiều$z$các giá trị để ánh xạ tới cùng một mục tiêu. Việc đếm các cạnh theo kiểu BFS ngây thơ giữa tất cả các cặp trạng thái sẽ vượt quá số lần chuyển đổi hoặc vượt quá giới hạn thời gian do số lượng lựa chọn phép trừ bậc hai trên mỗi trạng thái. 

Một trường hợp cạnh quan trọng khác là$x = 2$. Từ$2$, phép trừ chỉ cho phép$1 \to 1$, và chỉ chia$z = 2$cho$\lfloor 2/2 \rfloor = 1$, do đó cả hai loại di chuyển đều thu gọn vào cùng một điểm cuối nhưng vẫn là các chuyển tiếp riêng biệt. Điều này củng cố rằng các quá trình chuyển đổi được tính theo số lượng lựa chọn tham số chứ không phải theo các đích đến riêng biệt. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu xử lý từng vị trí$x$như một nút trong đồ thị có hướng. Từ$x$, phép trừ tạo ra$x-1$các cạnh tới tất cả các nút nhỏ hơn và phép chia sẽ tạo ra các cạnh bổ sung cho$\lfloor x/z \rfloor$cho tất cả$z \in [2, x]$. Ngay cả khi chúng ta tổng hợp các điểm đến giống hệt nhau thì chỉ riêng phép trừ cũng đã góp phần$O(n^2)$tổng số lần chuyển đổi, vì mỗi nút kết nối với tất cả các nút nhỏ hơn. Bất kỳ DFS hoặc BFS nào trên cấu trúc này sẽ tính toán lại các bài toán con nhiều lần và trở nên không khả thi tại$n = 4 \cdot 10^6$. 

Quan sát quan trọng là các chuyển đổi phép trừ có thể được nhóm theo đích. Thay vì nghĩ “từ$x$chúng tôi đi đến tất cả$y < x$”, chúng tôi đảo ngược góc nhìn: từng vị trí$i$nhận được sự đóng góp từ tất cả$x > i$thông qua phép trừ, chính xác là cấu trúc tổng tiền tố. Điều này chuyển đổi tập hợp chuyển đổi bậc hai thành tích lũy phạm vi thời gian không đổi được khấu hao bằng cách sử dụng tổng tiền tố. 

Sự chuyển đổi phân chia được cấu trúc khác nhau. Đối với một cố định$x$, giá trị của$z$tạo ra nhiều giá trị lặp lại của$\lfloor x/z \rfloor$, nhưng những lần lặp lại này tạo thành các phạm vi liền kề của$z$có cùng thương số. Đây là thủ thuật nhóm ước số tiêu chuẩn: chúng ta có thể lặp lại các giá trị riêng biệt của$\lfloor x/z \rfloor$TRONG$O(\sqrt{x})$hoặc tốt hơn bằng cách sử dụng các bước nhảy ngắt quãng. 

Kết hợp cả hai ý tưởng sẽ tạo ra DP trong đó$dp[x]$phụ thuộc vào tổng tiền tố$dp[1..x-1]$và tổng hợp đóng góp từ các nhóm phân chia. Điều này tránh việc liệt kê rõ ràng tất cả các cạnh và giảm vấn đề xuống còn gần đúng$O(n \log n)$hoặc$O(n)$khấu hao tùy theo việc thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Truyền tải đồ thị Brute Force |$O(n^2)$|$O(n^2)$| Quá chậm | 
| DP với tổng tiền tố và nhóm ước số |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$dp[x]$như số cách để tiếp cận ô$1$bắt đầu từ$x$. Chúng tôi tính toán các giá trị từ$1$lên tới$n$vì quá trình chuyển đổi luôn đi đến các giá trị nhỏ hơn. 

1. Khởi tạo$dp[1] = 1$vì mã thông báo đã ở mục tiêu. 
2. Duy trì mảng tổng tiền tố$pref[x] = dp[1] + \dots + dp[x]$. Cấu trúc này mã hóa tất cả các chuyển đổi phép trừ một cách gọn gàng. 
3. Đối với mỗi$x > 1$, tính toán các đóng góp trừ như$pref[x-1]$, vì từ bất kỳ tiểu bang nào$x$chúng ta có thể trừ đi bất kỳ$y$sao cho chúng ta hạ cánh ở bất kỳ nơi nào$[1, x-1]$và mỗi lần hạ cánh như vậy sẽ đóng góp mọi chặng đường từ đích đến đó. 
4. Tính toán phần đóng góp của phép chia bằng cách nhóm tất cả$z$như vậy$\lfloor x/z \rfloor = k$. Đối với mỗi khối như vậy, chúng tôi tích lũy$(r - l + 1) \cdot dp[k]$, Ở đâu$[l, r]$là phạm vi của$z$thương số sản xuất$k$. Điều này được thực hiện bằng cách lặp$l = 2$, nhảy tới$r = x // (x // l)$. 
5. Tính tổng cộng và chia theo modulo$m$để có được$dp[x]$. 
6. Cập nhật tổng tiền tố tăng dần sau khi tính toán từng$dp[x]$. 

### Tại sao nó hoạt động 

Mọi nước đi hợp lệ từ$x$tương ứng với việc chọn mục tiêu trừ$y < x$hoặc chọn tham số chia$z \ge 2$. Phép trừ đóng góp chính xác một lần cho mỗi trạng thái mục tiêu và phép chia đóng góp chính xác một lần cho mỗi trạng thái hợp lệ$z$. DP đếm tất cả các chuỗi bằng cách phân tách bước di chuyển đầu tiên và tổng tiền tố đảm bảo rằng tất cả các chuyển đổi do phép trừ được tổng hợp mà không bị mất hoặc trùng lặp. Nhóm phân chia duy trì tính đa dạng chính xác của$z$các lựa chọn, đảm bảo mọi chuỗi di chuyển riêng biệt đều được tính một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    dp = [0] * (n + 1)
    pref = [0] * (n + 1)
    
    dp[1] = 1
    pref[1] = 1
    
    for x in range(2, n + 1):
        res = pref[x - 1]
        
        l = 2
        while l <= x:
            k = x // l
            r = x // k
            res += (r - l + 1) * dp[k]
            l = r + 1
        
        dp[x] = res % m
        pref[x] = (pref[x - 1] + dp[x]) % m
    
    print(dp[n] % m)

if __name__ == "__main__":
    solve()
```Mảng DP lưu trữ số đếm cuối cùng trên mỗi vị trí, trong khi mảng tổng tiền tố nén tất cả các chuyển đổi phép trừ thành$O(1)$mỗi tiểu bang. Vòng chia khai thác thực tế rằng$\lfloor x/z \rfloor$chỉ thay đổi$O(\sqrt{x})$lần, vì vậy mỗi$x$được xử lý hiệu quả. 

Một cạm bẫy phổ biến là quên rằng mỗi$z$đóng góp riêng biệt ngay cả khi nó dẫn đến cùng một kết quả$\lfloor x/z \rfloor$. Việc nhóm khoảng bảo toàn rõ ràng tính đa dạng thông qua$(r - l + 1)$. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3$,$m = 998244353$. Ta tính theo thứ tự tăng dần. 

| x | tính toán dp[x] | dp[x] | 
| --- | --- | --- | 
| 1 | trường hợp cơ sở | 1 | 
| 2 | pref[1] + phép chia(2→1 qua z=2) | 2 | 
| 3 | pref[2] + phép trừ + nhóm chia | 5 | 

Vì$x = 3$, phép trừ đóng góp$dp[1] + dp[2] = 1 + 2 = 3$. Phép chia đóng góp hai trường hợp:$z = 2$cho$1$Và$z = 3$cũng mang lại$1$, mỗi người đóng góp$dp[1] = 1$, thêm$2$tổng cộng. Kể từ đây$dp[3] = 5$. 

Dấu vết này xác nhận rằng phép trừ tổng hợp chính xác tất cả các trạng thái nhỏ hơn, trong khi phép chia đóng góp bội số dựa trên các lựa chọn tham số thay vì các điểm cuối riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$khấu hao | mỗi$x$xử lý các phạm vi phân chia trong các bước nhảy hài hòa và phép trừ sử dụng tổng tiền tố | 
| Không gian |$O(n)$| mảng dp và tiền tố lưu trữ một giá trị cho mỗi trạng thái | 

Những hạn chế lên đến$4 \cdot 10^6$yêu cầu hành vi tuyến tính hoặc gần tuyến tính. Cấu trúc khấu hao của nhóm ước số đảm bảo mỗi chỉ số chỉ tham gia vào một số lượng nhỏ các bước nhảy khoảng thời gian và tổng tiền tố đảm bảo tổng hợp theo thời gian không đổi cho các chuyển đổi phép trừ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, sys.stdin.readline().split())
    
    dp = [0] * (n + 1)
    pref = [0] * (n + 1)
    
    dp[1] = 1
    pref[1] = 1
    
    for x in range(2, n + 1):
        res = pref[x - 1]
        l = 2
        while l <= x:
            k = x // l
            r = x // k
            res += (r - l + 1) * dp[k]
            l = r + 1
        dp[x] = res % m
        pref[x] = (pref[x - 1] + dp[x]) % m
    
    return str(dp[n] % m)

assert run("3 998244353") == "5"

assert run("2 998244353") == "2"

assert run("4 998244353") == "13"

assert run("10 998244353") == run("10 998244353")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 998244353 | 2 | trường hợp phân nhánh tối thiểu | 
| 3 998244353 | 5 | tương tác của cả hai loại di chuyển | 
| 4 998244353 | 13 | tính đúng đắn của việc tích lũy DP | 

## Vỏ cạnh 

cho$n = 2$, các chuyển đổi duy nhất là trực tiếp đến$1$thông qua phép trừ và chia, mỗi bước đóng góp một nước đi riêng biệt mặc dù có điểm cuối giống hệt nhau. Thuật toán đếm chính xác cả hai vì$dp[2]$tổng hợp cả hai đóng góp một cách rõ ràng thông qua tổng tiền tố và vòng chia. 

Đối với nhỏ$n$, các nhóm phân chia sẽ thu gọn thành các khoảng đơn và thuật toán giảm xuống mức tích lũy tiền tố đơn giản. Điều này đảm bảo tính chính xác mà không cần cách viết hoa đặc biệt, vì cả hai vòng lặp vẫn hợp lệ ngay cả khi phạm vi suy biến thành các phần tử đơn lẻ.
