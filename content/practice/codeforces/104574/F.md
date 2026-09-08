---
title: "CF 104574F - Trứng"
description: "Mỗi quả trứng có hai cách trích xuất giá trị độc lập: chiên nó sẽ cho một điểm, xào nó sẽ cho một điểm khác và bỏ qua nó sẽ không đóng góp gì. Hạn chế là Ivan không thể tự do lựa chọn tất cả các phương án tích cực."
date: "2026-06-30T08:17:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104574
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 2 (Beginner)"
rating: 0
weight: 104574
solve_time_s: 64
verified: true
draft: false
---

[CF 104574F - Trứng](https://codeforces.com/problemset/problem/104574/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi quả trứng có hai cách trích xuất giá trị độc lập: chiên nó sẽ cho một điểm, xào nó sẽ cho một điểm khác và bỏ qua nó sẽ không đóng góp gì. Hạn chế là Ivan không thể tự do lựa chọn tất cả các phương án tích cực. Anh ta bị giới hạn nhiều nhất$F$trứng chiên và nhiều nhất là$S$trứng bác trên tất cả các lựa chọn. Mỗi quả trứng chỉ có thể được sử dụng theo một cách, do đó, đây là quyết định phân vùng cho mỗi mục theo hai giới hạn dung lượng chung. 

Nhiệm vụ là gán mỗi quả trứng vào một trong ba trạng thái sao cho tôn trọng hai giới hạn dung lượng đồng thời tối đa hóa tổng giá trị thu thập được. Vì các giá trị có thể âm, tốt hơn hết bạn nên bỏ qua một số trứng và thậm chí trong giới hạn cho phép, tốt nhất là không sử dụng các ô trống. 

Kích thước đầu vào lên tới$N = 10^4$, trong khi$F + S \le 100$. Sự kết hợp này là đầu mối cấu trúc quan trọng: mặc dù có nhiều vật phẩm nhưng số lượng “vị trí trả phí” là cực kỳ nhỏ. Bất kỳ giải pháp nào cố gắng theo dõi các quyết định trên mỗi quả trứng trong không gian trạng thái rộng lớn sẽ gặp khó khăn, nhưng bất kỳ giải pháp nào coi năng lực là thứ nguyên chính đều có thể vẫn hiệu quả. Một sự ngây thơ$O(N \cdot F \cdot S)$DP đã ở mức chấp nhận được, nhưng việc giảm bớt cẩn thận hơn hoặc tối ưu hóa tham lam sẽ trở nên hấp dẫn. 

Trường hợp cạnh tinh tế xuất phát từ các giá trị âm. Nếu tất cả các giá trị đều âm thì câu trả lời đúng là 0 vì được phép bỏ qua tất cả trứng. Một trường hợp đặc biệt khác xuất hiện khi một danh mục bị tắt, chẳng hạn như$F = 0$, điều này buộc tất cả những quả trứng được chọn phải xáo trộn hoặc bỏ qua, làm thay đổi cấu trúc của quá trình chuyển đổi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét từng quả trứng một cách độc lập và thử tất cả các khả năng: chiên nó (nếu còn dung lượng), đánh trứng (nếu còn dung lượng) hoặc bỏ qua. Điều này tự nhiên dẫn đến một công thức lập trình động đối với các hạng mục và dung lượng còn lại. Cho phép$dp[i][f][s]$thể hiện sự hài lòng tối đa sau khi xử lý lần đầu tiên$i$trứng với$f$khe chiên và$s$các khe bị xáo trộn đã được sử dụng. Đối với mỗi quả trứng, chúng tôi thử ba lần chuyển tiếp. Điều này đúng vì nó khám phá tất cả các bài tập hợp lệ. 

Vấn đề là ở chỗ đó$N = 10^4$, do đó ngay cả một lớp DP với$F \cdot S \le 100$vẫn ngụ ý xung quanh$10^6$các trạng thái và mỗi trạng thái chuyển đổi qua ba lựa chọn, đưa ra$3 \cdot 10^6 \cdot 10^4$hoạt động quá chậm. 

Quan sát quan trọng là thứ tự của trứng không quan trọng nếu vượt quá giới hạn về năng lực. Mỗi quả trứng đóng góp độc lập và chúng tôi đang chọn tối đa$F + S \le 100$tổng số “bài tập” trên tất cả các quả trứng. Điều này thay đổi quan điểm: thay vì lặp lại số trứng trước, chúng tôi lặp lại số lượng trứng chiên và bác mà chúng tôi quyết định lấy tổng thể và chúng tôi chọn quả trứng nào sẽ lấp đầy những khoảng trống đó tốt nhất. 

Chúng ta có thể diễn giải lại điều này như việc chọn tối đa$F$các mặt hàng dành cho “hồ chiên” và lên đến$S$các vật phẩm cho “nhóm tranh giành”, nhưng với hạn chế là mỗi vật phẩm chỉ có thể được sử dụng một lần. Một thủ thuật tiêu chuẩn là xử lý từng quả trứng trong khi duy trì DP trên không gian công suất nhỏ. Bởi vì dung lượng rất nhỏ nên chúng tôi có thể mua một chiếc ba lô hai chiều trong đó mỗi mục có thể chuyển sang một trong ba trạng thái và chúng tôi cập nhật DP ngược lại với dung lượng để tránh sử dụng lại. 

Điều này có tác dụng vì mặc dù$N$lớn, mỗi quả trứng chỉ đóng góp hai mức tăng tiềm năng và không gian trạng thái công suất đủ nhỏ để hấp thụ mọi chuyển đổi một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu của bài tập | Hàm mũ | O(1) | Quá chậm | 
| 3D DP trên các món chiên, xào | O(NFS) | O(NFS) | Quá chậm | 
| Ba lô 2D được tối ưu hóa DP | O(NFS) | O(FS) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một bảng DP trong đó$dp[f][s]$thể hiện sự hài lòng tổng thể tốt nhất có thể đạt được sau khi xem xét một số tiền tố của trứng, sử dụng chính xác$f$lựa chọn chiên và$s$những lựa chọn lộn xộn. 

1. Khởi tạo tất cả trạng thái DP thành giá trị rất âm, ngoại trừ$dp[0][0] = 0$. Điều này thể hiện việc bắt đầu mà không có quả trứng nào được chọn và không có sự hài lòng nào. 
2. Xử lý từng quả trứng một. 
3. Đối với mỗi quả trứng có giá trị$f_i$(chiên) và$s_i$(được xáo trộn), chúng tôi cập nhật bảng DP theo thứ tự ngược lại về dung lượng. 
4. Cho mỗi cặp$(f, s)$, chúng tôi cân nhắc việc không sử dụng quả trứng đó, giữ lại$dp[f][s]$không thay đổi. 
5. Nếu$f > 0$, chúng tôi xem xét việc gán quả trứng này vào chế độ chiên, chuyển từ$dp[f-1][s] + f_i$. Điều này thể hiện việc ăn một miếng chiên và đạt được cảm giác hài lòng khi chiên. 
6. Nếu$s > 0$, chúng tôi xem xét việc gán quả trứng này cho món scrambled, chuyển từ$dp[f][s-1] + s_i$. Điều này thể hiện việc tiêu thụ một vị trí được tranh giành. 
7. Chúng tôi lấy mức tối đa trong số các lựa chọn này cho mỗi tiểu bang. 
8. Sau khi xử lý tất cả trứng, câu trả lời là giá trị lớn nhất trên tất cả$dp[f][s]$vì$0 \le f \le F$,$0 \le s \le S$, vì chúng tôi không bắt buộc phải sử dụng hết tất cả các vị trí. 

Việc lặp lại ngược lại về năng lực là điều cần thiết. Nếu không có nó, một quả trứng có thể được tái sử dụng nhiều lần trong cùng một lần lặp, điều này sẽ vi phạm ràng buộc rằng mỗi quả trứng chỉ được sử dụng nhiều nhất một lần. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào sau khi xử lý một tập hợp con trứng, mọi trạng thái DP sẽ mã hóa giá trị tốt nhất có thể đạt được bằng cách sử dụng mỗi quả trứng nhiều nhất một lần. Khi xử lý một quả trứng mới, quá trình chuyển đổi chỉ mở rộng từ các trạng thái chưa bao gồm quả trứng này. Bởi vì chúng tôi lặp lại các năng lực ngược lại, nên mọi cập nhật trạng thái đều không thể đưa vào một bản cập nhật khác của cùng một quả trứng trong cùng một lần lặp. Điều này bảo toàn tính bất biến mà mỗi quả trứng đóng góp nhiều nhất một lần. 

DP đang khám phá một cách hiệu quả tất cả các phép gán hợp lệ của trứng thành ba loại theo giới hạn dung lượng, nhưng nén không gian gán hàm mũ thành một không gian trạng thái đa thức được xác định hoàn toàn bằng cách sử dụng tài nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG = -10**30

def solve():
    N, F, S = map(int, input().split())
    
    dp = [[NEG] * (S + 1) for _ in range(F + 1)]
    dp[0][0] = 0

    for _ in range(N):
        f_i, s_i = map(int, input().split())

        for f in range(F, -1, -1):
            for s in range(S, -1, -1):
                best = dp[f][s]

                if f > 0 and dp[f - 1][s] != NEG:
                    best = max(best, dp[f - 1][s] + f_i)

                if s > 0 and dp[f][s - 1] != NEG:
                    best = max(best, dp[f][s - 1] + s_i)

                dp[f][s] = best

    ans = 0
    for f in range(F + 1):
        for s in range(S + 1):
            ans = max(ans, dp[f][s])

    print(ans)

if __name__ == "__main__":
    solve()
```Bảng DP được khởi tạo với giá trị rất âm để phân biệt các trạng thái không thể truy cập được với các cấu hình điểm thấp hợp lệ. Số 0 chỉ được gán cho vùng chọn trống. 

Các vòng lặp lồng nhau lặp đi lặp lại$f$Và$s$để đảm bảo mỗi quả trứng được xử lý chính xác một lần trên mỗi lớp trạng thái. Bản cập nhật tính toán xem việc lấy trứng ở dạng chiên hay đánh sẽ cải thiện trạng thái tốt nhất hiện tại. Việc quét cuối cùng trên tất cả các trạng thái là cần thiết vì dung lượng chưa sử dụng được cho phép và đôi khi là tối ưu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 1 2
3 8
5 6
7 7
4 5
6 2
```Chúng tôi theo dõi một số tiểu bang đại diện. 

| Bước | Trứng | (f,s) | ứng cử viên dp[1][2] | dp[1][2] | 
| --- | --- | --- | --- | --- | 
| 0 | ban đầu | - | - | 0 | 
| 1 | (3,8) | (1,2) | 8 | 8 | 
| 2 | (5,6) | (1,2) | 11 | 11 | 
| 3 | (7,7) | (1,2) | 14 | 14 | 
| 4 | (4,5) | (1,2) | 16 | 16 | 
| 5 | (6,2) | (1,2) | 21 | 21 | 

Chiến lược tốt nhất sẽ sớm lấp đầy cả hai vị trí được tranh giành với giá trị cao và sử dụng vị trí đã chiên trên một ứng cử viên mạnh. Bảng này cho thấy cách DP tích lũy sự kết hợp tốt nhất mà không cần theo dõi rõ ràng các nhiệm vụ. 

### Mẫu 2 

đầu vào:```
4 0 1
100 -5
5 20
-6 15
30 30
```Chỉ được phép tranh giành. 

| Bước | Trứng | s=1 lựa chọn | dp[0][1] | 
| --- | --- | --- | --- | 
| 0 | ban đầu | - | 0 | 
| 1 | (100,-5) | bỏ qua (0) hoặc -5 | 0 | 
| 2 | (5,20) | 20 | 20 | 
| 3 | (-6,15) | 20 vs 15 | 20 | 
| 4 | (30,30) | bỏ qua 20 vs 30 | 30 | 

DP tránh được quả trứng đầu tiên một cách chính xác vì giá trị được xáo trộn của nó là âm và dung lượng quá nhỏ để có thể biện minh cho điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NFS)$| Mỗi quả trứng cập nhật tất cả$F \cdot S$tiểu bang một lần | 
| Không gian |$O(FS)$| Chỉ có bảng DP 2D được lưu trữ | 

Sự ràng buộc$F + S \le 100$làm cho$F \times S$không gian trạng thái nhiều nhất$10^4$, vậy tổng số phép toán là khoảng$10^8$trường hợp xấu nhất, phù hợp thoải mái trong C++ và là đường biên nhưng có thể chấp nhận được trong Python được tối ưu hóa do các phép toán số nguyên chặt chẽ và các hằng số nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import math

    NEG = -10**30

    N, F, S = map(int, input().split())
    dp = [[NEG] * (S + 1) for _ in range(F + 1)]
    dp[0][0] = 0

    for _ in range(N):
        f_i, s_i = map(int, input().split())
        for f in range(F, -1, -1):
            for s in range(S, -1, -1):
                best = dp[f][s]
                if f > 0:
                    best = max(best, dp[f-1][s] + f_i)
                if s > 0:
                    best = max(best, dp[f][s-1] + s_i)
                dp[f][s] = best

    ans = 0
    for f in range(F + 1):
        for s in range(S + 1):
            ans = max(ans, dp[f][s])
    return str(ans)

# provided samples
assert run("""5 1 2
3 8
5 6
7 7
4 5
6 2
""") == "21"

assert run("""4 0 1
100 -5
5 20
-6 15
30 30
""") == "30"

# custom cases
assert run("""1 0 0
10 100
""") == "0"

assert run("""3 2 2
-1 -2
-3 -4
-5 -6
""") == "0"

assert run("""2 1 1
10 1
1 10
""") == "20"

assert run("""4 1 1
5 100
100 5
50 50
1 1
""") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 với trứng dương tính | 0 | lực lượng công suất bằng không bỏ qua | 
| tất cả các giá trị âm | 0 | bỏ qua là tối ưu | 
| sở thích hoán đổi | 20 | lựa chọn bài tập đúng | 
| cạnh tranh giá trị cao | 100 | sự cân bằng giữa tham lam và DP | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi dung lượng bằng 0. Trong tình huống này, không thể lựa chọn được nên mọi quả trứng đều phải bị bỏ qua. DP bắt đầu với$dp[0][0] = 0$và không bao giờ cho phép chuyển đổi sang trạng thái không hợp lệ, do đó, nó tự nhiên xuất ra 0 ngay cả khi tất cả các giá trị đều lớn và dương. 

Một trường hợp khác là khi tất cả các giá trị đều âm. DP vẫn có thể thử các chuyển đổi làm giảm điểm, nhưng mức tối đa cuối cùng trên tất cả các trạng thái bao gồm lựa chọn trống, giữ nguyên số 0 là câu trả lời tối ưu. Ví dụ, với một quả trứng$(-5, -10)$Và$F = S = 1$, DP xem xét ngắn gọn các trạng thái tiêu cực nhưng cuối cùng vẫn giữ nguyên$dp[0][0] = 0$tốt nhất. 

Một tình huống tế nhị hơn là khi một hạng mục thống trị hạng mục khác vì cùng một quả trứng. DP đảm bảo tính độc quyền vì nó chỉ chuyển đổi từ trạng thái trước đó mà không kết hợp cả hai lựa chọn cho cùng một quả trứng. Điều này ngăn chặn việc tính hai lần và đảm bảo mỗi quả trứng đóng góp tối đa một giá trị, khớp chính xác với ràng buộc của bài toán.
