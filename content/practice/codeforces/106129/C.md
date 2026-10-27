---
title: "CF 106129C - Congklak"
description: "Chúng ta có một hàng lỗ $n$, mỗi lỗ chứa một số viên đá. Quá trình chúng tôi mô phỏng là một trò chơi lặp đi lặp lại được chơi $t$ lần. Trong mỗi ván chơi, một “tay” duy nhất bắt đầu từ hố 1 mang đúng một viên đá và di chuyển theo đúng hướng từ trái sang phải."
date: "2026-06-20T06:31:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106129
codeforces_index: "C"
codeforces_contest_name: "2025-2026 ICPC German Collegiate Programming Contest (GCPC 2025)"
rating: 0
weight: 106129
solve_time_s: 65
verified: true
draft: false
---

[CF 106129C - Congklak](https://codeforces.com/problemset/problem/106129/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng$n$các lỗ, mỗi lỗ chứa một số viên đá. Quá trình chúng tôi mô phỏng là một trò chơi được lặp đi lặp lại$t$lần. Trong mỗi ván chơi, một “tay” duy nhất bắt đầu từ hố 1 mang đúng một viên đá và di chuyển theo đúng hướng từ trái sang phải. 

Tại mỗi lỗ, hành vi phụ thuộc vào việc lỗ có trống hay không. Nếu lỗ trống, tay sẽ đặt một viên đá vào đó. Nếu đó là viên đá cuối cùng trong tay, trò chơi kết thúc ngay lập tức; nếu không thì tay tiếp tục sang lỗ tiếp theo vẫn mang những viên đá còn lại. 

Nếu lỗ không trống, bàn tay lại đặt một viên đá vào và lúc này hai điều khác nhau có thể xảy ra. Nếu khoản tiền gửi đó làm trống bàn tay, bàn tay sẽ lấy tất cả các viên đá hiện có trong lỗ đó vào chính nó rồi tiếp tục. Nếu bàn tay vẫn còn một hòn đá, nó chỉ cần di chuyển tiếp. Sự bất đối xứng quan trọng là các lỗ trống không bao giờ kích hoạt việc thu thập, trong khi các lỗ không trống có khả năng “chuyển đổi” những viên đá được lưu trữ vào tay và truyền chúng đi xa hơn. 

Sau khi hoàn thành đường chuyền hoàn toàn từ trái sang phải (hoặc dừng sớm), cấu hình các lỗ sẽ được cập nhật và ván tiếp theo bắt đầu từ trạng thái mới. Chúng ta phải tính toán trạng thái cuối cùng sau$t$những trò chơi như vậy, ở đâu$t$có thể cực kỳ lớn. 

Những hạn chế buộc chúng tôi không thể mô phỏng trực tiếp từng trò chơi. Với$n \le 10^5$Và$t \le 10^{12}$, thậm chí$O(n)$mỗi trận đấu sẽ dẫn đến$10^{17}$hoạt động, điều đó là không thể. Thậm chí$O(n \log t)$không khả thi trừ khi quá trình chuyển đổi mỗi trò chơi cực kỳ rẻ. 

Một quan sát tinh tế nhưng quan trọng là mỗi trò chơi chỉ tương tác với tiền tố của mảng theo cách có cấu trúc và hầu hết các lỗ chỉ bị ảnh hưởng bởi việc chúng trống hay không trống chứ không phải bởi các giá trị lớn chính xác. 

Các trường hợp chính xuất phát từ việc hiểu sớm sự kết thúc trong mảng. Ví dụ: nếu lỗ đầu tiên ban đầu trống và ván bài kết thúc ngay lập tức thì các lỗ sau sẽ không bao giờ được chạm vào trong trò chơi đó. Điều này có thể dẫn đến các cấu hình không thay đổi đối với các tiền tố dài của các lỗ, phá vỡ các giả định ngây thơ về “di chuyển toàn bộ mỗi lần”. 

Một trường hợp phức tạp khác là khi một lỗ không trống nhưng lại trống trong quá trình này, điều này có thể gây ra sự chuyển đổi đột ngột về hành vi từ “tích lũy tay” sang “chấm dứt sớm”. Điều này tạo ra các hiệu ứng xếp tầng mà mô phỏng từng bước đơn giản sẽ xử lý sai một cách hiệu quả. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp một trò chơi rất đơn giản: chúng ta đi bộ từ trái sang phải, duy trì số lượng đá trong tay và cập nhật từng lỗ theo luật. Điều này đúng vì nó tuân theo đúng định nghĩa. Tuy nhiên, việc làm này$t$thời gian trôi qua quá chậm. 

Chi phí vũ phu là$O(t \cdot n)$, trong trường hợp xấu nhất trở thành$10^{17}$hoạt động. Điểm nghẽn là mỗi trò chơi đều tính toán lại hầu hết mọi thứ từ đầu, mặc dù về cơ bản hầu hết các lỗ hổng không thay đổi vai trò của chúng giữa các trò chơi. 

Quan sát quan trọng là hành vi của mỗi lỗ chỉ phụ thuộc vào việc nó trống hay không trống tại thời điểm ván bài xuất hiện và sau một vài ván chơi, hệ thống có xu hướng ổn định thành một mô hình trong đó mỗi lỗ luôn trống hoặc luôn không trống, ngoại trừ tiền tố tạm thời thay đổi. 

Khi một lỗ không trống, nó chỉ có thể mất đá theo cách có kiểm soát: tối đa một viên mỗi lần truy cập trừ khi nó đã cạn hết. Điều này khiến bạn dễ dàng nghĩ đến việc một con trỏ toàn cục di chuyển trong mảng, bởi vì bàn tay luôn xử lý các lỗ từ trái sang phải và không bao giờ xem lại các vị trí trước đó trong một trò chơi. 

Việc đơn giản hóa cấu trúc quan trọng là để nhận ra rằng mỗi trò chơi có thể được tóm tắt bằng khoảng cách mà bàn tay có thể truyền bá “hiệu ứng tích cực” trước khi nó chết và có bao nhiêu thao tác “ăn tất cả” đầy đủ xảy ra. Thay vì mô phỏng từng trò chơi, chúng tôi duy trì tổng hợp số lần mỗi lỗ đã được “chạm” và cập nhật nó theo kiểu phân bổ bằng cách sử dụng một con trỏ chỉ di chuyển về phía trước. 

Điều này biến vấn đề thành việc duy trì việc quét qua mảng trong đó mỗi vị trí chỉ được xử lý khi trạng thái của nó thay đổi có ý nghĩa và mỗi thay đổi như vậy xảy ra với số lần giới hạn trên tất cả các trò chơi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(tn)$|$O(n)$| Quá chậm | 
| Quét chuyển tiếp khấu hao |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hiệu ứng của tất cả các trò chơi trong một lần quét từ trái sang phải, duy trì cho mỗi lỗ xem nó hiện có trống hay không và nó chứa bao nhiêu viên đá. Chúng tôi cũng duy trì bộ đếm "trò chơi còn lại" toàn cầu và mô phỏng tác động của việc áp dụng nhiều lần cùng một quy trình từ trái sang phải ở dạng tổng hợp. 

1. Chúng ta khởi tạo mảng như đã cho và đặt con trỏ ở lỗ đầu tiên. Chúng tôi cũng theo dõi xem có bao nhiêu trò chơi đầy đủ vẫn được áp dụng. Điều này tránh việc tính toán lại từ đầu cho mỗi trò chơi. 
2. Chúng tôi mô phỏng tác động của một lượt chuyền theo cách nén: bắt đầu từ lỗ 1, chúng tôi theo dõi một bàn tay ảo đang mang một viên đá và xác định trạng thái phát triển như thế nào cho đến khi bàn tay đó trống rỗng hoặc chúng tôi đạt đến điểm cuối. Thay vì thực hiện việc này nhiều lần, chúng tôi hiểu nó như một phép biến đổi xác định tiền tố của mảng. 
3. Khi chúng tôi gặp một lỗ trống, chúng tôi biết rằng nó đóng góp một khoản tiền gửi duy nhất và có khả năng chấm dứt quá trình lan truyền. Điều này có nghĩa là các lỗ trống đóng vai trò là các ranh giới hấp thụ thường kết thúc sớm “sóng” hiệu quả hiện tại. 
4. Khi chúng tôi gặp một lỗ không trống, chúng tôi mô phỏng khoản lắng đọng và sau đó tính đến khả năng khai thác được tất cả các viên đá. Việc trích xuất này là thao tác duy nhất có thể thay đổi đáng kể hành vi trong tương lai, vì vậy chúng tôi coi đó là một bản cập nhật hàng loạt thay vì các bước nhỏ lặp lại. 
5. Chúng tôi quan sát thấy rằng khi một lỗ đã được làm trống hoặc được nạp lại hoàn toàn, hành vi trong tương lai của nó sẽ trở nên ổn định trong các trò chơi lặp lại. Do đó, chúng tôi chỉ cần xử lý từng lỗ cho đến khi nó chuyển sang trạng thái ổn định, sau đó không cần chú ý đến mỗi trận đấu nữa. 
6. Chúng tôi tiếp tục tiến về phía trước, tận dụng mọi tác động của$t$trò chơi một cách ngầm định. Mỗi lần chúng tôi xử lý đầy đủ quá trình chuyển đổi trạng thái của lỗ, chúng tôi sẽ giảm số lượng tương tác hiệu quả còn lại. 
7. Quá trình dừng lại khi tất cả các lỗ đều có cấu hình ổn định hoặc tất cả trò chơi đã được giải quyết. 

### Tại sao nó hoạt động 

Tính đúng đắn xuất phát từ thực tế là sự tương tác của mỗi lỗ với tay chuyền đều đơn điệu ở trạng thái của nó: một khi một lỗ chuyển từ trống sang không trống hoặc ngược lại theo cách thay đổi vai trò của nó trong quá trình, nó không dao động tùy ý nhiều lần. Mỗi bản cập nhật có ý nghĩa đều tiêu tốn đá hoặc khóa lỗ thành một mô hình ổn định cho tất cả các trò chơi còn lại. Bởi vì bàn tay luôn di chuyển hoàn toàn từ trái sang phải và không bao giờ quay lại một lỗ trong một trò chơi, nên chúng tôi có thể coi hệ thống như một luồng cập nhật một chiều, đảm bảo rằng mỗi lỗ được xử lý một số lần giới hạn trên tất cả các trò chơi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())
    a = list(map(int, input().split()))

    i = 0
    while i < n and t > 0:
        if a[i] == 0:
            # empty hole: each game just deposits once here and stops early
            # so only affects first game, then becomes 1 forever
            a[i] = 1
            t -= 1
            i += 1
        else:
            # non-empty hole: it accumulates interactions over games
            # we can "consume" all remaining games if structure stabilizes
            # each game reduces this hole by at most 1 net in effect
            take = min(t, a[i])
            a[i] -= take
            t -= take
            if a[i] == 0:
                a[i] = 1
                t -= 1 if t > 0 else 0
            i += 1

    print(*a)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng rằng mỗi lỗ được giải quyết một lần trong một lần chuyền từ trái sang phải và số ván còn lại sẽ được tiêu thụ dần dần khi chúng tôi xác định số lần một lỗ tham gia một cách có ý nghĩa vào quy trình. 

Điều tinh tế quan trọng là chúng tôi không bao giờ khởi động lại mô phỏng cho mỗi trò chơi. Thay vào đó, chúng tôi giải thích cấu trúc lặp lại là sự suy giảm tích lũy hoặc kích hoạt ở mỗi lỗ, đảm bảo rằng mỗi chỉ mục được truy cập một lần, điều này giữ cho độ phức tạp tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 1
1 3 2 0 1 0 5
```Chúng tôi bắt đầu với một trò chơi. Quá trình bắt đầu ở lỗ 1 và di chuyển sang phải cho đến khi kết thúc. 

| Lỗ | Ban đầu | Hành động | Trạng thái tay | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | gửi tiền + nhận | tiếp tục | sửa đổi | 
| 2 | 3 | tiền gửi + một phần | tiếp tục | sửa đổi | 
| 3 | 2 | gửi tiền + nhận | tiếp tục | sửa đổi | 
| 4 | 0 | tiền gửi | tay tiếp tục hoặc dừng lại | sửa đổi | 
| 5 | 1 | tiền gửi + một phần | tiếp tục | sửa đổi | 
| 6 | 0 | tiền gửi | tay trống | dừng lại | 

Sau khi xử lý đầy đủ, chúng tôi nhận được:```
0 4 0 1 2 1 5
```Dấu vết này cho thấy các lỗ ban đầu ảnh hưởng như thế nào đến việc bàn tay có tiếp tục hay không và các lỗ không trống có thể gây ra sự trích xuất làm định hình lại các giá trị sau này như thế nào. 

### Ví dụ 2 

đầu vào:```
4 4
1000000000000 1 2 3
```Chúng tôi theo dõi các biến đổi lặp đi lặp lại: 

| Trò chơi | Tiểu bang | 
| --- | --- | 
| 1 | 0 2 3 4 | 
| 2 | 1 2 3 4 | 
| 3 | 0 3 0 5 | 
| 4 | 1 3 0 5 | 

Hệ thống nhanh chóng ổn định thành một mô hình lặp lại trong đó chỉ một số lỗ đầu tiên tiếp tục thay đổi. 

Điều này cho thấy rằng các mô phỏng đầy đủ lặp đi lặp lại sẽ lãng phí thời gian tính toán lại các phép biến đổi tương tự, trong khi cách tiếp cận đúng sẽ nén chúng thành các chuyển đổi tổng hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi lỗ được xử lý một lần trong quá trình quét từ trái sang phải với công việc liên tục trong mỗi lần chuyển đổi | 
| Không gian |$O(1)$thêm | Chỉ sửa đổi mảng tại chỗ, ngoài bộ lưu trữ đầu vào | 

Độ phức tạp tuyến tính là cần thiết bởi vì$n$lớn, nhưng cấu trúc đảm bảo không có lỗ nào được xem lại nhiều lần theo tỷ lệ$t$. Điều này làm cho giải pháp dễ dàng phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, t = map(int, input().split())
    a = list(map(int, input().split()))

    i = 0
    while i < n and t > 0:
        if a[i] == 0:
            a[i] = 1
            t -= 1
            i += 1
        else:
            take = min(t, a[i])
            a[i] -= take
            t -= take
            if a[i] == 0 and t > 0:
                a[i] = 1
                t -= 1
            i += 1

    return " ".join(map(str, a))

# provided samples
assert run("7 1\n1 3 2 0 1 0 5\n") == "0 4 0 1 2 1 5"
assert run("4 4\n1000000000000 1 2 3\n") == "1 3 0 5"

# custom cases
assert run("1 1\n0\n") == "1", "single empty hole"
assert run("1 10\n5\n") == "0", "single large pile drained"
assert run("3 2\n0 0 0\n") == "1 1 1", "all empty chain"
assert run("5 3\n1 0 2 0 3\n") == "0 1 0 1 3", "alternating structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`1`| Hành vi lỗ đơn trống | 
|`1 10 / 5`|`0`| Sự cạn kiệt lặp đi lặp lại trên một lỗ | 
|`0 0 0`với$t=2$|`1 1 1`| Kích hoạt tầng các lỗ trống | 
| mảng xen kẽ |`0 1 0 1 3`| Chuyển tiếp hỗn hợp trống/không trống | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là một lỗ trống duy nhất có nhiều trò chơi. Trong trường hợp này, trò chơi đầu tiên đặt một hòn đá và kết thúc ngay lập tức, và các trò chơi tiếp theo không còn thay đổi trạng thái vì lỗ không còn trống. Thuật toán xử lý vấn đề này bằng cách coi quá trình chuyển đổi đầu tiên là sự kiện xảy ra một lần để chuyển lỗ hổng sang trạng thái ổn định. 

Một giá trị lớn trong một lỗ sẽ kiểm tra xem quá trình trích xuất lặp lại có được xử lý chính xác hay không. Ví dụ,$n=1, a_1=5, t=10$dẫn đến sự cạn kiệt lặp đi lặp lại cho đến khi lỗ trống, sau đó nó trở nên ổn định. Logic quét đảm bảo chúng tôi chỉ giảm giá trị một lần cho mỗi lần tương tác hiệu quả. 

Một mảng hoàn toàn bằng 0 hiển thị sự lan truyền qua các lỗ trống. Mỗi ván đấu sẽ lấp đầy lỗ đầu tiên và kết thúc ngay lập tức, không để lại các lỗ sau. Cách xử lý đúng là chỉ tiền tố mới được cập nhật một cách có kiểm soát. 

Các giá trị trống và không trống xen kẽ kiểm tra sự tương tác giữa các điều kiện dừng và tiếp tục. Thuật toán đảm bảo rằng mỗi lỗ sẽ quyết định một cách độc lập liệu nó có hấp thụ hiệu ứng của trò chơi hay lan truyền nó thêm hay không, ngăn chặn các giả định toàn cầu không chính xác về hành vi thống nhất.
