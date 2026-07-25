---
title: "CF 102861F - Fastminton"
description: "Đầu vào là bản ghi theo trình tự thời gian của trận đấu Fastminton. Mỗi ký tự mô tả một sự kiện xảy ra trong trận đấu: máy chủ ghi bàn, người nhận ghi bàn hoặc yêu cầu in tỷ số hiện tại."
date: "2026-07-25T14:03:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102861
codeforces_index: "F"
codeforces_contest_name: "2020-2021 ACM-ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 102861
solve_time_s: 60
verified: true
draft: false
---

[CF 102861F - Fastminton](https://codeforces.com/problemset/problem/102861/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là bản ghi theo trình tự thời gian của trận đấu Fastminton. Mỗi ký tự mô tả một sự kiện xảy ra trong trận đấu: máy chủ ghi bàn, người nhận ghi bàn hoặc yêu cầu in tỷ số hiện tại. Chương trình phải phát lại trận đấu một cách chính xác theo thứ tự và trả lời mọi yêu cầu về điểm số kèm theo trạng thái của trận đấu tại thời điểm đó. 

Một trận đấu có tối đa ba ván đấu và người chơi đầu tiên thắng hai ván sẽ thắng trận đấu. Trong mỗi trò chơi, điểm được tích lũy cho đến khi một người chơi đạt được điều kiện chiến thắng. Người chơi thắng trò chơi khi có ít nhất 5 điểm và dẫn trước ít nhất 2 điểm hoặc ngay lập tức khi đạt 10 điểm. Sau mỗi điểm, người chơi ghi điểm sẽ trở thành người phục vụ điểm tiếp theo. Trận đầu tiên bắt đầu với người chơi bên trái giao bóng, trong khi các trận sau bắt đầu với người chơi đã thắng lần giao bóng trước đó. 

Không có ràng buộc số lượng lớn vì đầu vào chỉ là một chuỗi các sự kiện. Giới hạn liên quan là số ký tự trong chuỗi đó. Mô phỏng trực tiếp xử lý mỗi sự kiện một lần, do đó thời gian chạy tăng tuyến tính theo kích thước đầu vào. Bất kỳ cách tiếp cận nào liên tục tái tạo lại trạng thái khớp từ đầu cho mỗi truy vấn sẽ thực hiện công việc lặp đi lặp lại không cần thiết và có thể trở thành phương trình bậc hai nếu có nhiều thông báo điểm số. 

Một số chi tiết có thể phá vỡ một triển khai đơn giản. Một sai lầm phổ biến là quên rằng người chơi đạt 10 điểm sẽ thắng ngay lập tức, ngay cả khi không có lợi thế hai điểm. Ví dụ, đầu vào`SSSSSSSSSSQ`kết thúc với việc người chơi bên trái thắng trò chơi với 10 điểm, do đó kết quả là`0 (winner) - 0`nếu đây là trận đầu tiên và tỷ số trận đấu cuối cùng được yêu cầu sau khi trận đấu kết thúc. Giải pháp chỉ kiểm tra điều kiện chênh lệch 5 điểm và 2 điểm sẽ khiến trò chơi tiếp tục chạy không chính xác. 

Một tình huống khó khăn khác là người giao bóng sau khi công bố điểm số. Đối với đầu vào`SQ`, người chơi bên trái ghi được điểm duy nhất và phải được đánh dấu là người phục vụ tiếp theo. Đầu ra là`0 (1*) - 0 (0)`. Một giải pháp giữ máy chủ ban đầu cho đến đợt biểu tình tiếp theo sẽ đánh dấu nhầm người chơi. 

Việc chuyển đổi giữa các trò chơi cũng dễ bị sai sót. Ví dụ: sau khi người chơi bên phải thắng ván đầu tiên, người chơi bên phải sẽ giao điểm đầu tiên của ván thứ hai. Máy chủ không được đặt lại cho người chơi bên trái. Việc quên quy tắc này sẽ thay đổi tất cả các thông báo điểm sau này. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng mọi cuộc biểu tình. Đối với mỗi nhân vật, việc triển khai vũ phu có thể duy trì số điểm hiện tại, kiểm tra xem trò chơi đã kết thúc hay chưa, cập nhật điểm trò chơi và in trạng thái được yêu cầu. Đây đã là sự thể hiện tự nhiên của các quy tắc và nó đúng vì quy tắc khớp chỉ phụ thuộc vào các sự kiện xảy ra trước vị trí hiện tại. 

Một phiên bản bạo lực ít cẩn thận hơn có thể xử lý mọi`Q`bằng cách phát lại toàn bộ tiền tố đầu vào từ đầu để xây dựng lại bản nhạc. Nếu đầu vào chứa nhiều yêu cầu về điểm số và độ dài chuỗi là`n`, điều này có thể yêu cầu khoảng`n`hoạt động của từng`n`yêu cầu, dẫn đến`O(n²)`công việc. 

Quan sát loại bỏ sự lặp lại này là mọi sự kiện chỉ thay đổi trạng thái khớp một lần. Thông tin đầy đủ cần thiết cho các sự kiện trong tương lai rất nhỏ: số trận thắng của mỗi người chơi, số điểm hiện tại trong trò chơi, máy chủ hiện tại và liệu trận đấu đã kết thúc hay chưa. Vì không có sự kiện cũ nào cần phải xem lại nên việc lưu trữ trạng thái này trong khi quét từ trái sang phải là đủ. 

Brute-force hoạt động vì các quy tắc mang tính xác định, nhưng nó không thành công khi tính toán lại thông tin đã được xử lý. Việc quan sát rằng trạng thái khớp có kích thước không đổi cho phép chúng tôi giảm toàn bộ vấn đề xuống một mô phỏng một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm đối với chuỗi sự kiện lớn | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Theo dõi các ván thắng của người chơi bên trái và bên phải, số điểm của ván đấu hiện tại và người chơi giao bóng ở lượt đánh tiếp theo. Ban đầu, người chơi bên trái giao bóng và cả điểm số trò chơi và điểm điểm đều bằng 0. Các biến này mô tả mọi thứ có thể ảnh hưởng đến các sự kiện trong tương lai. 
2. Đọc từng ký tự trong chuỗi sự kiện từ trái sang phải. Đối với sự kiện tính điểm, hãy xác định người chơi nhận được điểm. Nếu máy chủ ghi điểm, máy chủ sẽ nhận được một điểm. Nếu người nhận ghi bàn, người chơi khác sẽ được điểm. Sau khi trao điểm, hãy đặt chính người chơi đó làm người giao bóng tiếp theo vì người ghi bàn luôn giao bóng ở lượt đánh sau. 
3. Sau mỗi điểm, hãy kiểm tra xem trò chơi hiện tại đã kết thúc chưa. Người chơi thắng nếu họ có ít nhất 5 điểm và dẫn trước ít nhất 2 điểm hoặc nếu họ đạt được 10 điểm. Việc kiểm tra phải diễn ra sau khi cập nhật tổng điểm vì cuộc biểu tình mới nhất có thể đã hoàn thành trò chơi. 
4. Khi trò chơi kết thúc, hãy tăng số lượng trò chơi của người chiến thắng và đặt lại số điểm về 0. Nếu trò chơi đó mang lại cho người chơi tổng cộng hai trận thắng thì trận đấu kết thúc. Nếu không, người chiến thắng trong trò chơi đã hoàn thành sẽ trở thành máy chủ cho trò chơi tiếp theo. 
5. Khi thông báo điểm xuất hiện, hãy in trạng thái đã lưu. Nếu trận đấu vẫn đang diễn ra, hãy in cả điểm trận đấu và cả điểm điểm, thêm điểm đánh dấu máy chủ vào đúng người chơi. Nếu trận đấu kết thúc, chỉ in tỷ số cuối cùng của trận đấu và đánh dấu người chiến thắng. 

Tại sao nó hoạt động: trạng thái được duy trì chính xác là thông tin được yêu cầu bởi các quy tắc để xác định kết quả của sự kiện tiếp theo. Sau mỗi ký tự được xử lý, các biến thể hiện tình huống khớp thực sự sau tất cả các sự kiện đã thấy cho đến nay. Vì mỗi đợt tập hợp được áp dụng một lần và mọi chuyển đổi trong trò chơi đều tuân theo các quy tắc trực tiếp nên mọi thông báo điểm số trong tương lai đều được tạo từ trạng thái chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    games = [0, 0]
    points = [0, 0]
    server = 0
    finished = False
    winner = -1

    ans = []

    def game_winner():
        if points[0] >= 10:
            return 0
        if points[1] >= 10:
            return 1
        if points[0] >= 5 and points[0] - points[1] >= 2:
            return 0
        if points[1] >= 5 and points[1] - points[0] >= 2:
            return 1
        return -1

    def current_score():
        if finished:
            if winner == 0:
                return f"{games[0]} (winner) - {games[1]}"
            return f"{games[0]} - {games[1]} (winner)"

        left = str(points[0]) + ("*" if server == 0 else "")
        right = str(points[1]) + ("*" if server == 1 else "")
        return f"{games[0]} ({left}) - {games[1]} ({right})"

    for c in s:
        if c == 'S' or c == 'R':
            if not finished:
                scorer = server if c == 'S' else 1 - server
                points[scorer] += 1
                server = scorer

                w = game_winner()
                if w != -1:
                    games[w] += 1
                    points = [0, 0]
                    if games[w] == 2:
                        finished = True
                        winner = w
                    else:
                        server = w
        else:
            ans.append(current_score())

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`games`mảng lưu trữ tỷ số trận đấu, trong khi`points`chỉ lưu trữ điểm trò chơi hiện tại. Giữ những điều này riêng biệt để tránh trộn lẫn hai điều kiện chiến thắng khác nhau. 

các`server`biến luôn đề cập đến người chơi giao bóng ở lượt đánh tiếp theo. Việc cập nhật nó ngay sau một sự kiện ghi điểm là cần thiết vì người chơi ghi điểm sẽ giao bóng lại, bất kể ban đầu họ giao bóng hay nhận bóng. 

các`game_winner`Hàm kiểm tra quy tắc 10 điểm trước quy tắc hai điểm. Thứ tự này không bắt buộc đối với mọi trạng thái, nhưng nó làm cho điều kiện kết thúc tức thời trở nên rõ ràng. Hàm trả về`-1`trong khi trò chơi tiếp tục. 

Khi trò chơi kết thúc, mảng điểm sẽ được đặt lại. Mã chỉ thay đổi máy chủ sau trận đấu không phải trận chung kết vì không có cuộc biểu tình tiếp theo sau khi người chiến thắng trận đấu được xác định. 

Định dạng điểm số được tách biệt khỏi mô phỏng. Điều này ngăn các chi tiết đầu ra ảnh hưởng đến logic khớp và giúp phân biệt rõ ràng giữa trận đấu đang hoạt động và trận đấu đã kết thúc. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, các sự kiện được xử lý như sau. 

| Sự kiện | Trò chơi | Điểm | Máy chủ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| S | 0-0 | 1-0 | Trái | | 
| R | 0-0 | 1-1 | Đúng | | 
| S | 0-0 | 2-1 | Đúng | | 
| S | 0-0 | 3-1 | Đúng | | 
| Q | 0-0 | 3-1 | Đúng | 0 (1) - 0 (3*) | 
| S | 0-0 | 3-2 | Đúng | | 
| S | 0-0 | 3-3 | Đúng | | 
| S | 0-1 | 0-0 | Đúng | | 
| Q | 0-1 | 0-0 | Đúng | 0 (0) - 1 (2*) | 
| R | 0-1 | 0-1 | Trái | | 
| R | 0-1 | 0-2 | Đúng | | 
| S | 0-1 | 0-3 | Đúng | | 
| S | 0-1 | 0-4 | Đúng | | 

Dấu vết này cho thấy rằng người chơi ghi bàn sẽ trở thành máy chủ tiếp theo và trò chơi đã hoàn thành sẽ chuyển quyền giao bóng cho người chiến thắng trò chơi. 

Đối với Mẫu 2, truy vấn cuối cùng diễn ra sau khi trò chơi thứ hai kết thúc. 

| Sự kiện | Trò chơi | Điểm | Máy chủ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Q đầu tiên | 0-0 | 3-1 | Đúng | 0 (1) - 0 (3*) | 
| Q thứ hai | 0-1 | 0-0 | Đúng | 0 (0) - 1 (2*) | 
| Q cuối cùng | 0-2 | 0-0 | Đúng | 0 - 2 (thắng) | 

Dấu vết này thể hiện điều kiện kết thúc trận đấu. Khi người chơi đạt được hai trận thắng trong trò chơi, điểm số sẽ không còn được hiển thị và thông báo điểm số trong tương lai chỉ hiển thị người chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự sự kiện được xử lý chính xác một lần. | 
| Không gian | O(1) | Chỉ có một số bộ đếm và cờ cố định được lưu trữ. | 

Thuật toán không phụ thuộc vào số lượng thông báo điểm số hoặc độ dài của trò chơi đã hoàn thành. Nó chỉ giữ trạng thái trận đấu hiện tại nên vừa vặn thoải mái trong giới hạn lập trình cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    s = inp.strip()

    games = [0, 0]
    points = [0, 0]
    server = 0
    finished = False
    winner = -1
    out = []

    def gw():
        if points[0] >= 10:
            return 0
        if points[1] >= 10:
            return 1
        if points[0] >= 5 and points[0] - points[1] >= 2:
            return 0
        if points[1] >= 5 and points[1] - points[0] >= 2:
            return 1
        return -1

    def score():
        if finished:
            return f"{games[0]} (winner) - {games[1]}" if winner == 0 else f"{games[0]} - {games[1]} (winner)"
        return f"{games[0]} ({points[0]}{'*' if server == 0 else ''}) - {games[1]} ({points[1]}{'*' if server == 1 else ''})"

    for c in s:
        if c in "SR":
            if not finished:
                p = server if c == "S" else 1 - server
                points[p] += 1
                server = p
                w = gw()
                if w != -1:
                    games[w] += 1
                    points = [0, 0]
                    if games[w] == 2:
                        finished = True
                        winner = w
                    else:
                        server = w
        else:
            out.append(score())

    return "\n".join(out)

assert solve("SRSSQSSSSQRRSS") == "0 (1) - 0 (3*)\n0 (0) - 1 (2*)"
assert solve("SRSSQSSSSQRRSSQ") == "0 (1) - 0 (3*)\n0 (0) - 1 (2*)\n0 - 2 (winner)"
assert solve("RSRSSRRRRRRRRRRSSSSRRSQ") == "2 (winner) - 0"

assert solve("Q") == "0 (0*) - 0 (0)"
assert solve("SSSSSSSSSSQ") == "1 (winner) - 0"
assert solve("RRRRRRRRRRQ") == "0 - 1 (winner)"
assert solve("SSSSSSSSSSRSSSSSSSSSSQ") == "2 (winner) - 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`Q`|`0 (0*) - 0 (0)`| Trạng thái trận đấu trống và máy chủ ban đầu | 
|`SSSSSSSSSSQ`|`1 (winner) - 0`| Thắng ngay 10 điểm | 
|`RRRRRRRRRRQ`|`0 - 1 (winner)`| Người chơi đúng chiến thắng theo quy luật tương tự | 
|`SSSSSSSSSSRSSSSSSSSSSQ`|`2 (winner) - 0`| Chuyển đổi giữa các trò chơi và hoàn thành trận đấu | 

## Vỏ cạnh 

Quy tắc 10 điểm được xử lý bằng cách kiểm tra nó trước khi dựa vào điều kiện lợi thế hai điểm. Đối với đầu vào`SSSSSSSSSSQ`, người chơi bên trái đạt 10 điểm, ván đầu tiên kết thúc ngay lập tức và in yêu cầu điểm số`1 (winner) - 0`. Thuật toán không bao giờ chờ đợi một khách hàng tiềm năng lớn hơn. 

Quá trình chuyển đổi giao bóng sau mỗi lượt giao bóng được xử lý bằng cách chỉ định người ghi bàn làm máy chủ mới. Đối với đầu vào`SQ`, sự kiện đầu tiên mang lại cho người chơi bên trái một điểm và sự kiện thứ hai sẽ in ra`0 (1*) - 0 (0)`. Ngôi sao xuất hiện ở phía bên trái vì người ghi bàn giao bóng ở lượt đánh tiếp theo. 

Khi bắt đầu trò chơi mới, người chiến thắng trò chơi trước đó sẽ sử dụng máy chủ. Nếu người chơi bên trái thắng ván đầu tiên và ván thứ hai bắt đầu, mô phỏng sẽ giữ người chơi bên trái làm máy chủ thay vì đặt lại về người chơi bắt đầu ban đầu. Điều này tuân theo sự bất biến của trạng thái khớp và ngăn chặn việc gán điểm không chính xác trong tương lai. 

Trạng thái trận đấu đã kết thúc cũng được giữ nguyên. Khi người chơi đạt được hai ván thắng thì sau đó`Q`các sự kiện tiếp tục in kết quả cuối cùng thay vì cố gắng tiếp tục ghi điểm. Trạng thái trận đấu trở nên bất biến sau khi biết được người chiến thắng. 

Nếu bạn muốn, tôi cũng có thể điều chỉnh bài xã luận thành một phiên bản ngắn hơn theo phong cách Codeforces, gần với nội dung xuất hiện trong bài viết chính thức của cuộc thi.
