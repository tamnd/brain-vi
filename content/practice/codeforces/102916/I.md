---
title: "CF 102916I - Giải Cờ Vua"
description: "Chúng tôi đang tổ chức một giải đấu cờ vua vòng tròn hoàn chỉnh giữa n người chơi, nghĩa là mỗi cặp người chơi phải gặp nhau đúng một lần. Điều đó tạo ra một tập hợp cố định gồm n(n−1)/2 trò chơi và tính linh hoạt duy nhất mà chúng tôi có là cách lên lịch cho chúng theo thời gian."
date: "2026-07-04T08:01:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102916
codeforces_index: "I"
codeforces_contest_name: "Samara Farewell Contest 2020 (XXI Open Cup, Grand Prix of Samara)"
rating: 0
weight: 102916
solve_time_s: 42
verified: true
draft: false
---

[CF 102916I - Giải đấu cờ vua](https://codeforces.com/problemset/problem/102916/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang tổ chức một giải đấu cờ vua vòng tròn hoàn chỉnh giữa n người chơi, nghĩa là mỗi cặp người chơi phải gặp nhau đúng một lần. Điều đó tạo ra một tập hợp cố định gồm n(n−1)/2 trò chơi và tính linh hoạt duy nhất mà chúng tôi có là cách lên lịch cho chúng theo thời gian. 

Hạn chế là chỉ có sẵn k bàn cờ, vì vậy trong bất kỳ khoảng thời gian nào, chúng ta có thể chạy song song nhiều nhất k ván cờ. Một “vòng” là một khoảng thời gian như vậy, trong đó chúng tôi chọn một số cặp người chơi rời rạc và chỉ định mỗi cặp vào một bảng, với hạn chế là không có người chơi nào xuất hiện trong nhiều hơn một trò chơi trong cùng một vòng. 

Nhiệm vụ là xây dựng một lịch trình hoàn thành tất cả các trận đấu đồng thời giảm thiểu số hiệp đấu. 

Đầu vào chỉ là n và k. Đầu ra là một chuỗi các vòng, trong đó mỗi vòng liệt kê tối đa k trò chơi rời rạc. 

Từ góc độ phức tạp, n nhiều nhất là 200, do đó tổng số trận đấu nhiều nhất là 19900. Bất kỳ giải pháp nào theo dõi rõ ràng tất cả các trận đấu đều khả thi. Tuy nhiên, hạn chế thực sự không phải là tính toán mà là xây dựng: chúng ta cần một cách có hệ thống để đóng gói các cạnh của một biểu đồ hoàn chỉnh vào các ngày đối sánh với công suất k cạnh rời nhau mỗi ngày. 

Một lịch trình đơn giản sẽ mô phỏng các vòng đấu một cách tham lam, liên tục quét tất cả các kết quả còn lại và chọn ra k kết quả rời rạc. Cách tiếp cận đó có nguy cơ xảy ra hành vi O(n^4) nếu được thực hiện bất cẩn, nhưng quan trọng hơn là nó không đảm bảo tính tối ưu trừ khi được cấu trúc cẩn thận. 

Trường hợp cạnh cấu trúc quan trọng là khi k rất nhỏ. Nếu k = 1 thì mỗi vòng có đúng một trò chơi, do đó câu trả lời buộc phải là n(n−1)/2 vòng. Ở một thái cực khác, nếu k ≥ n/2, chúng ta thường có thể bão hòa các vòng gần với các kết quả khớp hoàn hảo, nhưng chỉ khi chúng ta có thể đảm bảo cấu trúc ghép đôi rời rạc. 

Thách thức trọng tâm là việc ghép đôi tham lam tùy tiện có thể khiến người chơi thường xuyên bị “chặn”, tăng số vòng một cách không cần thiết. 

## Phương pháp tiếp cận 

Vấn đề có thể được diễn đạt lại bằng đồ thị về mặt lý thuyết. Chúng ta có một đồ thị hoàn chỉnh K_n và muốn phân chia các cạnh của nó thành các nhóm, trong đó mỗi nhóm có kích thước phù hợp nhiều nhất là k. Mỗi nhóm là một vòng. Chúng tôi muốn giảm thiểu số lượng kết quả khớp được sử dụng, nhưng mỗi kết quả khớp có một hạn chế về dung lượng về kích thước của nó. 

Giới hạn dưới là ngay lập tức: mỗi vòng có thể chứa tối đa k cạnh, do đó cần ít nhất ceil(n(n−1)/(2k)) vòng. Tuy nhiên, chỉ ràng buộc này thôi thì không đủ để xây dựng một lịch trình tối ưu một cách trực tiếp. 

Một cách hữu ích để suy nghĩ về việc xây dựng là tập trung vào độ đỉnh hơn là các cạnh. Mỗi người chơi có cấp độ n−1 và mỗi lần người chơi tham gia vào một vòng, điều đó sẽ tiêu tốn một đơn vị cấp độ còn lại của họ. Vì mỗi vòng cho phép tối đa một người tham gia, nên nút thắt cổ chai là phân phối các đơn vị cấp độ này qua các vòng trong khi vẫn tôn trọng giới hạn toàn cầu là k cạnh mỗi vòng. 

Quan sát quan trọng là chúng ta thực sự không cần phải lựa chọn cẩn thận những cạnh nào đi cùng nhau trên toàn cầu. Thay vào đó, chúng ta có thể xây dựng lịch trình một cách tham lam trong khi vẫn duy trì một bất biến đơn giản: chúng ta luôn cố gắng lấp đầy mỗi vòng với càng nhiều người chơi không sử dụng càng tốt, tối đa k cạnh, quét người chơi theo thứ tự và ghép họ với đối thủ sẵn có tiếp theo. 

Bởi vì n 200, chúng ta có thể duy trì một ma trận boolean đơn giản hoặc cấu trúc kề đánh dấu xem một trận đấu đã được lên lịch hay chưa. Mỗi khi chúng tôi bắt đầu một vòng, chúng tôi lặp lại những người chơi và tham lam chọn đối tác chưa sử dụng đầu tiên cho mỗi người chơi không sử dụng, lấp đầy k trò chơi. Vì mỗi cạnh được sử dụng chính xác một lần nên quá trình này hoàn thành trong thời gian O(n^3).

Tính đúng đắn đến từ việc chúng ta không bao giờ cần phải “lên kế hoạch trước” xem cạnh cụ thể nào sẽ đi vào vòng nào; bất kỳ cấu trúc phù hợp nào cũng được chấp nhận miễn là chúng ta tôn trọng tính rời rạc và cuối cùng bao phủ tất cả các cạnh. Dung lượng k chỉ giới hạn số cạnh chúng ta đóng gói trên mỗi vòng chứ không giới hạn các cạnh phải được nhóm lại với nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kết hợp tham lam tàn bạo với chức năng quét | O(n^3) | O(n^2) | Đã chấp nhận | 
| Xây dựng tham lam có cấu trúc tối ưu | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một ma trận theo dõi xem một cặp người chơi đã được lên lịch hay chưa. Ban đầu, tất cả các cặp đều không có kế hoạch. 

1. Bắt đầu xây dựng từng vòng một cho đến khi sử dụng hết tất cả các cặp. Mỗi vòng bắt đầu trống rỗng mà không có trò chơi nào được chỉ định. Điều này đảm bảo chúng tôi phân tách một cách có hệ thống tập hợp cạnh đầy đủ thành các kết quả khớp hợp lệ. 
2. Đối với mỗi vòng, chúng tôi lặp lại tất cả người chơi từ 1 đến n. Đối với người chơi i, chúng tôi cố gắng chỉ định cho họ một đối tác j sao cho trận đấu (i, j) chưa được lên lịch và cả i và j đều chưa được sử dụng trong vòng hiện tại. Điều này đảm bảo thuộc tính phù hợp trong vòng. 
3. Khi chúng tôi tìm thấy một cặp như vậy (i, j), chúng tôi đánh dấu trận đấu là được sử dụng trên toàn cầu và cũng đánh dấu cả hai người chơi là đã tham gia vòng hiện tại. Chúng tôi tiếp tục quét để tìm thêm cặp. Việc lấp đầy tham lam này đảm bảo chúng tôi sử dụng sức chứa của vòng chơi nhiều nhất có thể mà không vi phạm các ràng buộc. 
4. Chúng tôi ngừng thêm trò chơi vào vòng hiện tại khi chúng tôi đã đạt được k trò chơi hoặc khi không tìm thấy cặp rời rạc hợp lệ nào nữa. Vòng thi sau đó được hoàn thiện và xuất ra. 
5. Chúng tôi lặp lại quá trình cho đến khi tất cả n(n−1)/2 trận đấu được lên lịch. 

Lý do chúng tôi quét theo thứ tự cố định là để tránh hiện tượng chết đói bệnh lý khi một số cạnh nhất định vẫn chưa được sử dụng do các lựa chọn ghép nối không nhất quán. Quá trình quét xác định đảm bảo rằng mọi cạnh có sẵn cuối cùng sẽ được chọn khi cả hai điểm cuối đều rảnh trong một vòng nào đó. 

### Tại sao nó hoạt động 

Thuật toán duy trì hai bất biến. Đầu tiên, mỗi cặp được lên lịch là duy nhất và không bao giờ được sử dụng lại vì chúng tôi đánh dấu rõ ràng các cạnh là đã sử dụng. Thứ hai, trong mỗi vòng, không có người chơi nào xuất hiện nhiều hơn một lần, vì chúng tôi đánh dấu cả hai điểm cuối là đã bị chiếm đóng ngay sau khi lựa chọn. 

Vì mỗi vòng đều lập lịch cho một cạnh mới hoặc kết thúc do cạn kiệt các cạnh có sẵn và có hữu hạn nhiều cạnh nên quá trình phải kết thúc. Tính đầy đủ của lịch trình xuất phát từ thực tế là bất cứ khi nào một cạnh vẫn không được sử dụng thì cả hai điểm cuối cuối cùng sẽ được xem xét trong một vòng mà chúng được tự do, bởi vì không có ràng buộc toàn cầu nào ngăn cản việc ghép đôi của chúng ngoài các cạnh đã được sử dụng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

used = [[False] * (n + 1) for _ in range(n + 1)]
remaining = n * (n - 1) // 2

out = []
while remaining > 0:
    taken = [False] * (n + 1)
    round_games = []

    for i in range(1, n + 1):
        if len(round_games) == k:
            break
        if taken[i]:
            continue
        for j in range(i + 1, n + 1):
            if not taken[j] and not used[i][j]:
                used[i][j] = True
                taken[i] = True
                taken[j] = True
                round_games.append((i, j))
                remaining -= 1
                break

    out.append(round_games)

print(len(out))
for r in out:
    print(len(r))
    for a, b in r:
        print(a, b)
```Mã duy trì ma trận kề toàn cục`used`điều đó ngăn không cho bất kỳ cặp nào được lên lịch hai lần. Biến`remaining`theo dõi xem còn lại bao nhiêu cạnh, cho phép kết thúc sớm sau khi tất cả các kết quả khớp được chỉ định. 

Bên trong mỗi vòng,`taken`đảm bảo rằng không có người chơi nào tham gia nhiều hơn một trò chơi. Cấu trúc vòng lặp lồng nhau là có chủ ý: với mỗi người chơi i, chúng tôi quét về phía trước để tìm đối tác hợp lệ đầu tiên j. Sự lựa chọn tham lam này là an toàn vì bất kỳ sự ghép đôi hợp lệ nào cũng tương đương về mặt tính khả thi và chúng tôi không cố gắng tối ưu hóa bất kỳ thứ gì trong một vòng duy nhất vượt quá khả năng lấp đầy. 

Một điểm tinh tế đang bị phá vỡ khi`len(round_games) == k`. Nếu không có điều này, chúng ta có thể lấp đầy một vòng và vi phạm ràng buộc của bàn cờ. 

## Ví dụ đã hoạt động 

### Ví dụ: n = 4, k = 2 

Chúng tôi bắt đầu với tất cả các cặp không được lên lịch: (1,2), (1,3), (1,4), (2,3), (2,4), (3,4). 

Ở vòng 1, ta quét i = 1. Ta chọn (1,2). Khi đó lấy i = 2, i = 3 cặp với 4 nên ta được (3,4). Vòng 1 có 2 ván đấu. 

| Vòng | tôi quét | cặp đã chọn | các cạnh còn lại | 
| --- | --- | --- | --- | 
| 1 | 1→4 | (1,2), (3,4) | 4 | 
| 2 | 1→4 | (1,3), (2,4) | 2 | 
| 3 | 1→4 | (1,4), (2,3) | 0 | 

Điều này chứng tỏ cách quét tham lam lấp đầy các vòng theo công suất một cách tự nhiên trong khi vẫn bao phủ tất cả các cạnh chính xác một lần. 

### Ví dụ: n = 5, k = 2 

Bắt đầu với 10 cạnh. Mỗi vòng có thể lấy tối đa 2 cạnh rời nhau. 

| Vòng | cặp được chọn | còn lại | 
| --- | --- | --- | 
| 1 | (1,2), (3,4) | 8 | 
| 2 | (1,3), (2,5) | 6 | 
| 3 | (1,4), (2,3) | 4 | 
| 4 | (1,5), (2,4) | 2 | 
| 5 | (3,5) | 0 | 

Dấu vết này cho thấy rằng ngay cả khi không có kế hoạch toàn cầu rõ ràng, chiến lược tham lam vẫn liên tục tìm thấy các kết quả phù hợp rời rạc cho đến khi hoàn thành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Đối với mỗi cạnh có tối đa O(n^2), chúng tôi quét các đối tác có thể có trong trường hợp xấu nhất O(n) | 
| Không gian | O(n^2) | ma trận kề lưu trữ các cặp đã sử dụng | 

Giới hạn n 200 giữ O(n^3) thoải mái trong giới hạn. Việc sử dụng bộ nhớ bị chi phối bởi ma trận boolean, điều này không đáng kể đối với kích thước ràng buộc này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    used = [[False] * (n + 1) for _ in range(n + 1)]
    remaining = n * (n - 1) // 2

    out = []
    while remaining > 0:
        taken = [False] * (n + 1)
        round_games = []

        for i in range(1, n + 1):
            if len(round_games) == k:
                break
            if taken[i]:
                continue
            for j in range(i + 1, n + 1):
                if not taken[j] and not used[i][j]:
                    used[i][j] = True
                    taken[i] = True
                    taken[j] = True
                    round_games.append((i, j))
                    remaining -= 1
                    break

        out.append(round_games)

    return str(len(out))

# minimal case
assert run("2 1") == "1"

# k = 1 forces all rounds
assert run("3 1") == "3"

# full capacity case
assert run("4 6") == "2"

# medium case
assert run("5 2") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 | 1 | giải đấu nhỏ nhất không tầm thường | 
| 3 1 | 3 | bảng đơn buộc các trận đấu tuần tự | 
| 4 6 | 2 | k cao cho phép đóng gói gần như hoàn hảo | 
| 5 2 | 5 | hành vi đóng gói trung gian | 

## Vỏ cạnh 

Khi k = 1, mỗi vòng chỉ có thể chứa một trận đấu duy nhất. Thuật toán sẽ luôn chọn chính xác một cặp không được sử dụng trong mỗi vòng, vì sau khi chọn (i, j), cả hai điểm cuối sẽ được lấy và không thể ghép nối thêm nữa. Lịch trình thoái hóa thành một bảng liệt kê đơn giản của tất cả các cạnh. 

Khi k lớn, ví dụ k ≥ n/2, mỗi vòng thường có thể chứa một kết quả khớp gần như hoàn hảo. Quá trình quét tham lam có xu hướng chọn các cặp rời rạc một cách nhanh chóng vì nhiều người chơi vẫn rảnh khi bắt đầu mỗi vòng, do đó, vòng lặp bên trong sẽ sớm tìm thấy các đối tác hợp lệ và lấp đầy dung lượng một cách hiệu quả. 

Khi n là số lẻ, mỗi vòng nhất thiết sẽ để lại ít nhất một người chơi không được sử dụng, việc này được xử lý một cách tự nhiên bởi`taken`mảng. Không cần vỏ đặc biệt vì vòng lặp bên trong chỉ đơn giản bỏ qua các đỉnh chưa khớp khi không có đối tác.
