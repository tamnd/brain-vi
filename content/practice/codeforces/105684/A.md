---
title: "CF 105684A - \u0421\u043a\u043e\u0440\u043e \u043d\u043e\u0432\u044b\u0439 \u0433\u043e\u0434"
description: "Chúng tôi được cấp một số lượng kẹo cố định và một số lượng nhân viên cố định. Mỗi nhân viên phải nhận đúng 2 viên kẹo hoặc đúng 3 viên kẹo. Nếu nhân viên nhận được 3 viên kẹo thì coi như họ vui vẻ; nếu không, họ nhận được 2 viên kẹo và không vui."
date: "2026-06-22T05:01:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105684
codeforces_index: "A"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u041d\u0415\u0419\u041c\u0410\u0420\u041a 2024-25, \u0412\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 105684
solve_time_s: 50
verified: true
draft: false
---

[CF 105684A - \u0421\u043a\u043e\u0440\u043e \u043d\u043e\u0432\u044b\u0439 \u0433\u043e\u0434](https://codeforces.com/problemset/problem/105684/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số lượng kẹo cố định và một số lượng nhân viên cố định. Mỗi nhân viên phải nhận đúng 2 viên kẹo hoặc đúng 3 viên kẹo. Nếu nhân viên nhận được 3 viên kẹo thì coi như họ vui vẻ; nếu không, họ nhận được 2 viên kẹo và không vui. Nhiệm vụ là tối đa hóa số lượng nhân viên hài lòng trong khi phân phát tất cả kẹo để mỗi nhân viên nhận được một gói hợp lệ. 

Đầu vào chứa hai số nguyên lớn, tổng số kẹo và số lượng nhân viên. Các ràng buộc cho phép các giá trị lên đến$10^9$cho kẹo và lên đến$5 \cdot 10^8$dành cho nhân viên, vì vậy mọi giải pháp đều phải chạy trong thời gian không đổi hoặc logarit cho mỗi trường hợp thử nghiệm. Việc quét tuyến tính đối với nhân viên hoặc kẹo sẽ chỉ được chấp nhận nếu có nhiều trường hợp thử nghiệm có giới hạn nhỏ, nhưng ở đây thang đo đã gợi ý một cách xây dựng số học trực tiếp. 

Một điểm tinh tế là tổng số kẹo ít nhất phải gấp đôi số nhân viên. Điều này đảm bảo rằng việc giao 2 viên kẹo cho mọi người luôn khả thi, vì vậy chúng tôi không bao giờ gặp phải tình huống bất khả thi khi một số nhân viên không thể nhận được số lượng yêu cầu tối thiểu. Vấn đề hoàn toàn là việc nâng cấp một số nhân viên từ 2 lên 3 viên kẹo bằng cách phân phối lại số kẹo dư thừa. 

Các trường hợp cạnh xuất phát từ cấu trúc chẵn lẻ và dư thừa. 

Nếu như$n = 2m$, thì mỗi nhân viên phải nhận đúng 2 viên kẹo. Không có chỗ để thăng cho ai 3 viên kẹo nên đáp án là 0. Ví dụ:$n = 10, m = 5$, sự phân phối tổng thể là bắt buộc và không ai có thể hạnh phúc. 

Nếu như$n = 3m$, khi đó mỗi nhân viên được nhận 3 viên kẹo nên tất cả đều vui vẻ và câu trả lời là$m$. 

Giữa những thái cực này, chúng ta phải suy luận cẩn thận về việc có thể nâng cấp bao nhiêu viên kẹo mà không vi phạm tính khả thi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng chỉ định từng nhân viên kẹo, chọn 2 hoặc 3 viên kẹo cho mỗi nhân viên và kiểm tra tất cả các kết hợp. Với$m$nhân viên, điều này dẫn đến$2^m$những nhiệm vụ có thể thực hiện được, điều này hoàn toàn không khả thi ngay cả đối với$m = 40$, chứ đừng nói đến$5 \cdot 10^8$. 

Một chế độ xem có cấu trúc hơn là bắt đầu từ đường cơ sở, nơi mọi người đều nhận được 2 viên kẹo. Điều này tiêu thụ$2m$kẹo. Phần còn lại$n - 2m$kẹo đại diện cho các đơn vị bổ sung có thể được sử dụng để nâng cấp một số nhân viên từ 2 lên 3 kẹo. Mỗi lần nâng cấp như vậy sẽ tốn thêm đúng 1 viên kẹo và tăng số lượng nhân viên hài lòng lên thêm một viên. 

Tuy nhiên, việc nâng cấp tất cả số kẹo còn lại không phải lúc nào cũng tương đương trực tiếp với câu trả lời vì chúng tôi phải đảm bảo không vượt quá số lượng nhân viên. Dù còn nhiều kẹo nhưng chúng ta cũng không thể có nhiều hơn$m$nhân viên hạnh phúc. Do đó, câu trả lời bị giới hạn bởi cả thặng dư và số lượng nhân viên. 

Điều này dẫn đến một cách giải thích trực tiếp: mỗi nhân viên hạnh phúc sẽ tiêu thụ đúng 3 chiếc kẹo, vì vậy nếu chúng ta chọn$x$nhân viên hạnh phúc và$m - x$nhân viên không hài lòng, tổng số kẹo cần thiết là$$3x + 2(m - x) = 2m + x.$$Điều này phải nhiều nhất$n$, Vì thế$x \le n - 2m$. Cũng$x \le m$. Khả thi tối đa$x$do đó là mức tối thiểu của hai giới hạn này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

###Các bước suy luận tối ưu 

1. Bắt đầu bằng cách tưởng tượng rằng mỗi nhân viên được phân bổ tối thiểu 2 chiếc kẹo. Điều này tiêu thụ chính xác$2m$kẹo và cấu hình này luôn hợp lệ do ràng buộc$n \ge 2m$. 
2. Tính số kẹo còn lại sau phép gán cơ bản này. Hãy để điều này được$r = n - 2m$. Đây là những viên kẹo duy nhất có sẵn để tăng cường hạnh phúc. 
3. Quan sát thấy rằng việc tăng một nhân viên từ 2 viên kẹo lên 3 viên kẹo sẽ tiêu tốn thêm đúng 1 viên kẹo và biến nhân viên đó trở thành một người hạnh phúc. Vì vậy, mỗi nhân viên hạnh phúc tương ứng với việc chi tiêu 1 đơn vị từ$r$. 
4. Kết luận rằng nhiều nhất$r$nhân viên có thể cảm thấy vui vẻ khi sử dụng số kẹo dư thừa. 
5. Cũng lưu ý rằng số lượng nhân viên hạnh phúc không thể vượt quá tổng số nhân viên$m$, vì mỗi nhân viên chỉ được tính tối đa một lần. 
6. Lấy mức tối thiểu của hai ràng buộc này,$r$Và$m$, là câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi nhiệm vụ hợp lệ có thể được chuyển thành nhiệm vụ cơ bản của tất cả nhân viên có 2 viên kẹo bằng cách trừ đi 1 viên kẹo cho mỗi nhân viên hài lòng. Điều này cho thấy sự tương ứng một-một giữa một giải pháp với$x$nhân viên vui vẻ và cấu hình chính xác$x$kẹo dư thừa được phân phối trên đường cơ sở. Vì đường cơ sở là cố định và tiêu thụ lượng kẹo tối thiểu có thể nên mọi cấu hình hợp lệ đều phải đáp ứng$x \le n - 2m$. Không có sự sắp xếp nào có thể vượt qua giới hạn này vì mỗi đơn vị hạnh phúc có chi phí cố định là đúng một viên kẹo vượt quá yêu cầu cơ bản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    base = 2 * m
    surplus = n - base
    if surplus < 0:
        surplus = 0
    print(min(m, surplus))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp mã hóa lý do. Biến`base`đại diện cho sự phân bổ bắt buộc trong đó mọi người đều nhận được 2 viên kẹo. các`surplus`biến ghi lại số lượng tăng thêm 1 viên kẹo mà chúng tôi có thể phân phối. Câu trả lời cuối cùng được giới hạn bởi cả số dư thừa này và số lượng nhân viên. Việc bảo vệ chống lại thặng dư âm là không cần thiết về mặt kỹ thuật trong các ràng buộc, nhưng nó vẫn đảm bảo tính đúng đắn nếu hạn chế được nới lỏng hoặc hiểu sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 4
```Chúng tôi tính toán: 

| Bước | Đế (2m) | Thặng dư (n - 2m) | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 8 | - | - | 
| Tính toán | 8 | 2 | - | 
| Cuối cùng | 8 | 2 | 2 | 

Điều này có nghĩa là chúng ta có thể nâng cấp tối đa 2 nhân viên để nhận 3 viên kẹo, 2 người còn lại nhận được 2 viên kẹo. 

Điều này phù hợp với cấu trúc: hai nhân viên lấy 3 viên kẹo (tổng cộng 6 viên) và hai nhân viên lấy 2 viên kẹo (tổng cộng 4 viên), tổng cộng là 10. 

### Ví dụ 2 

đầu vào:```
10 5
```| Bước | Đế (2m) | Thặng dư (n - 2m) | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 10 | - | - | 
| Tính toán | 10 | 0 | - | 
| Cuối cùng | 10 | 0 | 0 | 

Không còn kẹo thừa sau khi đưa cho mọi người 2 viên kẹo, vì vậy không ai có thể được nâng cấp. 

Điều này thể hiện trường hợp ranh giới chặt chẽ trong đó đường cơ sở khớp chính xác với tổng tài nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó không thực hiện lặp lại$m$hoặc$n$, có thể cực kỳ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n, m = map(int, input().split())
    base = 2 * m
    surplus = n - base
    if surplus < 0:
        surplus = 0
    return str(min(m, surplus))

# provided samples
assert run("10 4\n") == "2"
assert run("10 5\n") == "0"

# minimum edge (smallest valid m)
assert run("4 2\n") == "0"

# exact all-happy case
assert run("6 2\n") == "2"

# large surplus
assert run("100 10\n") == "10"

# tight base equality
assert run("20 10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2 | 0 | trường hợp khả thi tối thiểu, không có dư thừa | 
| 6 2 | 2 | hạnh phúc trọn vẹn nhất có thể | 
| 100 10 | 10 | thặng dư vượt quá m, áp dụng giới hạn | 
| 20 10 | 0 | trường hợp bão hòa cơ sở chính xác | 

## Vỏ cạnh 

Đối với trường hợp$n = 2m$, mỗi nhân viên nhận được chính xác 2 viên kẹo và không thể nâng cấp. Ví dụ, đầu vào`10 5`sản xuất cơ sở$2m = 10$và thặng dư 0. Bộ thuật toán`surplus = 0`và trả về`min(5, 0) = 0`, khớp chính xác với phân phối bắt buộc. 

Khi$n$đủ lớn để hỗ trợ tất cả nhân viên nhận được 3 viên kẹo, chẳng hạn như`6 2`, phép tính có cơ số 4 và thặng dư 2. Kết quả là`min(2, 2) = 2`, nghĩa là tất cả nhân viên đều có thể được hạnh phúc. Thuật toán bão hòa tự nhiên tại$m$, ngăn chặn việc đếm quá mức ngay cả khi số dư lớn.
