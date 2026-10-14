---
title: "CF 105174M - \u77f3\u5b50\u6e38\u620f"
description: "Chúng tôi được giao một trò chơi với một đống đá. Hai người chơi luân phiên nhau, Alice đi trước. Ở mỗi lượt, người chơi nhìn vào số lượng đá hiện tại, giả sử x, và loại bỏ các viên đá theo quy tắc phụ thuộc vào tính chẵn lẻ."
date: "2026-06-27T08:18:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105174
codeforces_index: "M"
codeforces_contest_name: "The 22nd Sichuan University Programming Contest"
rating: 0
weight: 105174
solve_time_s: 72
verified: true
draft: false
---

[CF 105174M - \u77f3\u5b50\u6e38\u620f](https://codeforces.com/problemset/problem/105174/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một trò chơi với một đống đá. Hai người chơi luân phiên nhau, Alice đi trước. Ở mỗi lượt, người chơi nhìn vào số lượng đá hiện tại, giả sử x, và loại bỏ các viên đá theo quy tắc phụ thuộc vào tính chẵn lẻ. 

Nếu x lẻ, người chơi không có lựa chọn nào khác và phải loại bỏ đúng một viên đá, giảm cọc xuống x−1. Nếu x chẵn, người chơi có hai lựa chọn: loại bỏ một viên đá hoặc loại bỏ chính xác một nửa số viên đá, chuyển trò chơi tương ứng sang x−1 hoặc x/2. Người chơi không đối mặt với quân nào khi bắt đầu lượt của mình sẽ thua ngay lập tức vì không thể di chuyển. 

Nhiệm vụ là xác định, với mỗi n ban đầu, liệu Alice có giành chiến thắng bắt buộc hay không khi giả sử cả hai người chơi đều chơi tối ưu. 

Các ràng buộc lên tới một triệu trường hợp thử nghiệm và số lượng đá lên tới một triệu. Điều này ngay lập tức loại trừ mọi mô phỏng trò chơi trong mỗi lần thử nghiệm. Ngay cả việc quét tuyến tính cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. Bất kỳ giải pháp nào được chấp nhận đều phải dựa vào tính toán trước lên tới 10^6 hoặc đặc tính dạng đóng của các trạng thái chiến thắng. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất phát từ việc giả định tính đối xứng giữa trạng thái chẵn và lẻ hoặc giả định “lấy một nửa” luôn mạnh. Ví dụ: tại n = 2, cả hai nước đi đều dẫn đến 1, người đi sau thắng, do đó 2 là thua. Với n = 4, lấy một nửa dẫn đến 2 (thua), do đó 4 trở thành thắng, mặc dù trừ một dẫn đến 3 (thắng). Điều này cho thấy ngay cả các vị trí cũng yêu cầu xem xét đồng thời cả hai phương án di chuyển chứ không chỉ mức giảm lớn hơn. 

Một cạm bẫy khác là cho rằng tất cả các vị trí lẻ đều thua vì chúng chỉ cho phép một nước đi duy nhất. Điều đó sai đối với các giá trị nhỏ như 1 và 3, trong đó chuỗi bắt buộc dẫn đến trạng thái thua cuộc đối với đối thủ. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải trò chơi là coi mọi số nguyên x là một trạng thái trò chơi và xác định xem nó thắng hay thua. Từ trạng thái x, chúng tôi xem xét tất cả các trạng thái có thể truy cập được trong một nước đi và đánh dấu x là thắng nếu ít nhất một trong các trạng thái có thể truy cập đó đang thua. 

Điều này tự nhiên dẫn đến một công thức lập trình động. Đặt dp[x] biểu thị liệu người chơi hiện tại có chiến lược chiến thắng với x viên đá còn lại hay không. Theo quy tắc, nếu x là số lẻ thì sự chuyển đổi duy nhất là sang x−1. Nếu x chẵn, các chuyển đổi sẽ chuyển sang x−1 và x/2. Điều này mang lại sự lặp lại trong đó dp[x] chỉ phụ thuộc vào các chỉ số nhỏ hơn, vì vậy chúng ta có thể tính dp từ 0 đến n. 

DP brute-force rất đơn giản và chính xác, nhưng chi phí của nó là O(n) cho mỗi trường hợp thử nghiệm nếu được tính toán lại mỗi lần, điều này là không thể đối với 10^6 truy vấn. Ngay cả khi được tính toán một lần, việc trả lời các truy vấn là O(1), vì vậy vấn đề thực sự là đảm bảo chúng tôi tính toán trước một lần cho đến n tối đa. 

Quan sát cấu trúc quan trọng là sau một tiền tố nhỏ, trò chơi sẽ ổn định thành một mô hình đơn giản. Các trạng thái lẻ vượt quá ngưỡng sẽ liên tục thua vì nước đi duy nhất của chúng dẫn trực tiếp đến trạng thái chẵn thắng. Ngay cả các trạng thái vượt quá ngưỡng cũng liên tục giành chiến thắng vì ít nhất một trong hai quá trình chuyển đổi của chúng luôn dẫn đến vị thế thua cuộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại đầy đủ cho mỗi truy vấn | O(n) mỗi truy vấn | O(1) | Quá chậm | 
| Tính toán trước DP lên tới tối đa n | Tổng số O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách tính toán các giá trị dp từ 0 đến 1e6 một lần.

1. Khởi tạo dp[0] là thua vì không có nước đi nào từ một đống trống. 
2. Với mỗi x từ 1 đến max_n, hãy xác định dp[x] từ các trạng thái được tính toán trước đó. 
3. Nếu x lẻ, bước di chuyển duy nhất là đến x−1, do đó dp[x] trở thành số đối của dp[x−1]. Điều này phản ánh trực tiếp rằng người chơi hiện tại sẽ thắng khi và chỉ khi nước đi bắt buộc khiến đối thủ rơi vào thế thua. 
4. Nếu x chẵn, hãy đánh giá cả hai phương án x−1 và x/2. Nếu một trong hai dẫn đến trạng thái thua, hãy đánh dấu dp[x] là thắng. Điều này mã hóa lối chơi tối ưu vì người chơi chọn nước đi tốt nhất hiện có. 
5. Sau khi điền dp, hãy trả lời từng truy vấn trong O(1) bằng cách in ra dp[n] có đúng hay không. 

Lý do điều này có tác dụng là vì dp[x] mã hóa chính xác định nghĩa về vị trí chiến thắng trong một trò chơi công bằng hữu hạn: một trạng thái sẽ thắng nếu có ít nhất một nước đi đến trạng thái thua và sẽ thua nếu tất cả các nước đi đều dẫn đến trạng thái chiến thắng. Vì mọi chuyển đổi đều làm giảm x nên DP có cơ sở vững chắc và bao gồm tất cả các phần phụ thuộc. 

Một sự đơn giản hóa cấu trúc sâu hơn xuất hiện từ việc kiểm tra sự truy hồi: các trạng thái lẻ truyền các giá trị từ x−1, trong khi các trạng thái chẵn bị ảnh hưởng bởi cả x−1 và x/2. Điều này nhanh chóng chuyển thành một mẫu ổn định sau các giá trị nhỏ, nhưng công thức DP là cách rõ ràng nhất để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 10**6

dp = [False] * (MAXN + 1)
dp[0] = False

for x in range(1, MAXN + 1):
    if x % 2 == 1:
        dp[x] = not dp[x - 1]
    else:
        lose = False
        if not dp[x - 1]:
            lose = True
        if not dp[x // 2]:
            lose = True
        dp[x] = lose

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append("Alice" if dp[n] else "Bob")
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng DP được tính toán một lần trên toàn cầu, vì vậy mỗi trường hợp kiểm thử đều được trả lời theo thời gian không đổi. Logic chuyển đổi tuân theo trực tiếp từ các quy tắc di chuyển: các trạng thái lẻ có một chuyển đổi bắt buộc duy nhất, trong khi các trạng thái chẵn kiểm tra cả hai nước đi có thể xảy ra và chọn xem có bất kỳ nước đi nào dẫn đến vị trí thua hay không. 

Một lỗi triển khai phổ biến là tính toán lại dp bên trong vòng lặp truy vấn, lỗi này sẽ ngay lập tức hết thời gian chờ dưới 10^6 đầu vào. Một vấn đề tế nhị khác là quên rằng dp[x] phải đúng nếu bất kỳ nước đi nào thua, chứ không phải nếu tất cả nước đi đều thắng. 

## Ví dụ đã hoạt động 

Xét n = 5, 6, 7. 

Đối với dấu vết này, chúng tôi sử dụng các quyết định DP dựa trên quy tắc. 

### Ví dụ 1: n = 5 

| x | chẵn lẻ | chuyển tiếp | dp[x−1] | quyết định | 
| --- | --- | --- | --- | --- | 
| 5 | lẻ | 4 | dp[4]=Đúng | dp[5]=Sai | 

Đến 5, nước đi duy nhất là 4, vốn đã là thế thắng nên người chơi hiện tại không có cách nào buộc phải thắng. 

### Ví dụ 2: n = 6 

| x | chẵn lẻ | x−1 | x/2 | kết quả | 
| --- | --- | --- | --- | --- | 
| 6 | thậm chí | 5 (F) | 3 (T) | dp[6]=Đúng | 

Ở quân 6, dù tiến lên 5 là xấu nhưng nước lên 3 lại mạnh vì khiến đối thủ rơi vào thế thua. 

Hai ví dụ này cho thấy các trạng thái chẵn có thể được cứu bằng cách di chuyển giảm một nửa, trong khi các trạng thái lẻ được xác định chặt chẽ bởi vị trí trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAXN + t) | DP được tính một lần lên tới 10^6, sau đó mỗi truy vấn là O(1) | 
| Không gian | O(MAXN) | mảng dp lưu trữ kết quả trò chơi cho tất cả các tiểu bang | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn vì chuyển đổi 10^6 là các kiểm tra thời gian liên tục đơn giản. Mỗi truy vấn được trả lời trong thời gian không đổi, giúp giải pháp trở nên hiệu quả ngay cả đối với kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    import io as sysio

    out = sysio.StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old
    return out.getvalue().strip()

# basic samples (illustrative)
assert run("1\n1\n") == "Alice"
assert run("1\n2\n") == "Bob"

# custom cases
assert run("1\n3\n") == "Alice", "small odd win case"
assert run("1\n4\n") == "Alice", "first even where half matters"
assert run("1\n5\n") == "Bob", "first stable losing odd"
assert run("1\n6\n") == "Alice", "even with winning half move"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Alice | vị trí chiến thắng nhỏ nhất | 
| 2 | Bob | thậm chí thua nhỏ nhất | 
| 3 | Alice | ngoại lệ kỳ lạ sớm | 
| 4 | Alice | lợi thế giảm một nửa đầu tiên | 
| 5 | Bob | ổn định lỗ lẻ | 
| 6 | Alice | trạng thái chẵn sử dụng x/2 | 

## Vỏ cạnh 

Với n = 1, trạng thái thắng ngay vì nước đi bắt buộc dẫn đến 0, khiến đối phương không thể di chuyển. 

Với n = 2, trạng thái thua vì cả hai nước đi có thể dẫn đến n = 1, tức là thắng, do đó không có lối thoát. 

Với n = 3, nước đi bắt buộc đi về 2 là thua, khiến 3 thắng dù là lẻ. 

Đối với các giá trị lẻ lớn hơn, chẳng hạn như n = 5, nước đi bắt buộc sẽ chuyển sang 4, tức là thắng, do đó vị trí trở thành thua. Điều này thể hiện sự chuyển đổi trong đó các trạng thái kỳ quặc không còn thuận lợi nữa khi đứa con duy nhất của họ trở nên ổn định và chiến thắng. 

Đối với các giá trị chẵn chẳng hạn như n = 4, bước giảm một nửa sẽ tiến tới 2, tức là thua, do đó 4 trở thành thắng mặc dù trừ đi một sẽ dẫn đến trạng thái thắng.
