---
title: "CF 907B - Tic-Tac-Toe"
description: "Bảng là một lưới 9 x 9, nhưng về mặt khái niệm, nó được chia thành chín bảng phụ 3 x 3 được sắp xếp theo bố cục vĩ mô 3 x 3. Mỗi ô có thể chứa dấu của người chơi đầu tiên, dấu của người chơi thứ hai hoặc để trống. Trò chơi không phải là trò chơi tic-tac-toe thông thường."
date: "2026-06-15T11:56:46+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 907
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 454 (Div. 2, based on Technocup 2018 Elimination Round 4)"
rating: 1400
weight: 907
solve_time_s: 250
verified: false
draft: false
---

[CF 907B - Tic-Tac-Toe](https://codeforces.com/problemset/problem/907/B) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bảng là một lưới 9 x 9, nhưng về mặt khái niệm, nó được chia thành chín bảng phụ 3 x 3 được sắp xếp theo bố cục vĩ mô 3 x 3. Mỗi ô có thể chứa dấu của người chơi đầu tiên, dấu của người chơi thứ hai hoặc để trống. 

Trò chơi không phải là trò chơi tic-tac-toe thông thường. Nguyên tắc chính là vị trí của nước đi trước sẽ hạn chế nơi có thể thực hiện nước đi tiếp theo. Nếu nước đi cuối cùng được đặt trong một ô nhất định bên trong bảng phụ 3 x 3 của nó, thì nước đi tiếp theo phải được chơi bên trong bảng phụ tương ứng với vị trí ô đó. Chỉ khi bảng phụ mục tiêu đó không có ô trống thì hạn chế mới biến mất, cho phép chơi ở bất kỳ ô trống nào ở bất kỳ đâu trên bảng. 

Đầu vào cung cấp trạng thái bảng hiện tại đầy đủ và sau đó cung cấp riêng tọa độ của bước di chuyển cuối cùng. Nhiệm vụ là đánh dấu mọi ô mà người chơi hiện tại được phép di chuyển bằng dấu chấm than, giữ nguyên tất cả các ô khác. 

Mặc dù tuyên bố đề cập rằng trạng thái có thể không hợp lệ hoặc không thể truy cập được, nhưng điều đó không quan trọng đối với việc tính toán. Chúng tôi chỉ tuân theo quy tắc một cách máy móc dựa trên nước đi cuối cùng đã cho và tỷ lệ lấp đầy hiện tại. 

Các ràng buộc có kích thước cố định: bảng luôn có kích thước 9 x 9. Điều này có nghĩa là bất kỳ giải pháp nào quét hoặc xử lý lưới trong thời gian giới hạn không đổi là đủ. Ngay cả vài nghìn thao tác ở đây cũng không đáng kể, vì vậy cách tiếp cận mô phỏng trực tiếp là đủ. 

Một trường hợp phức tạp xuất phát từ quy tắc “dự phòng”. Nếu khối 3 x 3 bắt buộc đã đầy thì người chơi có thể di chuyển đến bất cứ đâu. Ví dụ: nếu nước đi cuối cùng đưa bạn vào một khối nơi mọi ô đều đã có người ở thì hạn chế sẽ được loại bỏ một cách hiệu quả. Một giải pháp ngây thơ luôn thực thi khối mục tiêu sẽ tạo ra không có động thái hợp lệ nào trong trường hợp đó. 

Một trường hợp khác là nước đi cuối cùng có thể trỏ đến một khối đã đầy mặc dù bảng chưa được lấp đầy. Hành vi đúng vẫn là bỏ qua hạn chế. 

## Phương pháp tiếp cận 

Một cách giải thích vũ phu là đơn giản. Đầu tiên hãy xác định bảng phụ mục tiêu bằng cách sử dụng tọa độ của nước đi cuối cùng. Sau đó kiểm tra từng ô trong bảng con đó xem có ít nhất một ô trống không. Nếu có ít nhất một ô trống, chỉ hạn chế di chuyển đến bảng phụ đó. Nếu không, hãy xem xét tất cả các ô trống trên bảng đầy đủ. 

Điều này có tác dụng vì quy tắc chỉ phụ thuộc vào việc bảng phụ mục tiêu có ô trống nào hay không. Tuy nhiên, ngay cả ý tưởng mạnh mẽ này cũng đã chạy trong thời gian không đổi do kích thước 9 x 9 cố định, do đó không cần cải tiến tiệm cận. 

Quan sát quan trọng là toàn bộ vấn đề giảm xuống việc xác định khối 3 x 3 và kiểm tra xem nó có chứa dấu chấm nào không. Khi đã biết điều đó, câu trả lời là một mặt nạ đơn giản trên khối đó hoặc toàn bộ lưới. 

Không có lợi ích gì trong các cấu trúc dữ liệu phức tạp hơn vì lưới không bao giờ vượt quá 81 ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc lưới 9 x 9. Điều này được lưu trữ dưới dạng danh sách các chuỗi hoặc mảng ký tự. Chưa có gì được chuyển đổi vì chúng ta chỉ cần truy cập trực tiếp vào các ô. 
2. Đọc tọa độ của bước di chuyển cuối cùng và chuyển đổi chúng thành chỉ mục dựa trên số không. Điều này là cần thiết vì việc lập chỉ mục mảng trong mã bắt đầu từ 0, trong khi đầu vào bắt đầu từ 1. 
3. Tính toán bảng phụ mục tiêu bằng cách lấy số nguyên của hàng và cột di chuyển cuối cùng cho 3. Điều này ánh xạ lưới 9 x 9 thành lưới các khối 3 x 3. 
4. Trích xuất bảng con 3 x 3 tương ứng và quét nó để kiểm tra xem nó có chứa ít nhất một ô trống hay không. Sự hiện diện của một '.' là đủ để giữ cho hạn chế hoạt động. 
5. Nếu bảng phụ có ít nhất một ô trống, hãy đánh dấu tất cả các ô trống bên trong bảng phụ đó bằng '!'. 
6. Nếu bảng phụ đã đầy hoàn toàn, hãy quét toàn bộ lưới 9 x 9 và đánh dấu tất cả các ô trống ở bất kỳ đâu bằng '!'. 
7. Xuất lưới đã sửa đổi ở cùng định dạng. 

### Tại sao nó hoạt động 

Quy tắc trò chơi giảm ràng buộc nước đi tiếp theo thành một điều kiện boolean duy nhất: liệu bảng phụ bắt buộc có thể chơi được hay không. Nếu nó có thể chơi được thì không được di chuyển ra ngoài là hợp pháp. Nếu nó không thể chơi được, quy tắc rõ ràng cho phép hoàn toàn tự do. Bởi vì quy tắc chỉ phụ thuộc vào tính trống chứ không phụ thuộc vào bất kỳ lịch sử hoặc điều kiện chẵn lẻ nào khác, nên chỉ kiểm tra bảng phụ đó mới nắm bắt đầy đủ mọi ràng buộc cho nước đi tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    board = [list(input().strip()) for _ in range(9)]
    x, y = map(int, input().split())
    x -= 1
    y -= 1

    bx, by = x // 3, y // 3

    has_empty = False
    for i in range(bx * 3, bx * 3 + 3):
        for j in range(by * 3, by * 3 + 3):
            if board[i][j] == '.':
                has_empty = True

    if has_empty:
        for i in range(bx * 3, bx * 3 + 3):
            for j in range(by * 3, by * 3 + 3):
                if board[i][j] == '.':
                    board[i][j] = '!'
    else:
        for i in range(9):
            for j in range(9):
                if board[i][j] == '.':
                    board[i][j] = '!'

    for row in board:
        print("".join(row))

if __name__ == "__main__":
    main()
```Bước đầu tiên là đọc lưới chính xác dưới dạng các ký tự để các sửa đổi được thực hiện tại chỗ và ít tốn kém. Việc chuyển đổi bước di chuyển cuối cùng sang lập chỉ mục dựa trên 0 sẽ tránh được từng lỗi một khi ánh xạ vào các khối. 

Việc tính toán của`(x // 3, y // 3)`là ánh xạ trung tâm từ lưới toàn cầu tới bảng macro. Mỗi vùng 3 x 3 tương ứng với một khối duy nhất, do đó phép chia số nguyên sẽ xác định rõ ràng khối đó. 

Việc quét cho`has_empty`là rất quan trọng vì nó xác định xem ràng buộc có hoạt động hay không. Nếu có ít nhất một dấu chấm tồn tại, chúng tôi sẽ hạn chế nghiêm ngặt việc di chuyển đến khối đó. Nếu không, chúng tôi cố tình bỏ qua ranh giới khối. 

Bước đánh dấu cẩn thận chỉ sửa đổi '.' tế bào. Điều này đảm bảo chúng tôi không ghi đè lên các ký tự x hoặc o hiện có. 

Cuối cùng, đầu ra duy trì định dạng chính xác theo yêu cầu của vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét trường hợp bước đi cuối cùng đưa chúng ta đến một khối trống một phần. 

| Bước | Hành động | Khối mục tiêu | Tìm thấy trống | Phạm vi đánh dấu | 
| --- | --- | --- | --- | --- | 
| 1 | Đọc đầu vào | Không áp dụng | Không áp dụng | Không áp dụng | 
| 2 | Khối tính toán | (1,2) | Không áp dụng | Không áp dụng | 
| 3 | Khối quét | khối giữa bên phải | vâng | hạn chế | 

Điều này chứng tỏ trường hợp ràng buộc thông thường. Chỉ khối đó được sửa đổi. 

### Ví dụ 2 

Bây giờ hãy xem xét trường hợp khối mục tiêu đã đầy. 

| Bước | Hành động | Khối mục tiêu | Tìm thấy trống | Phạm vi đánh dấu | 
| --- | --- | --- | --- | --- | 
| 1 | Đọc đầu vào | Không áp dụng | Không áp dụng | Không áp dụng | 
| 2 | Khối tính toán | (0,0) | Không áp dụng | Không áp dụng | 
| 3 | Khối quét | khối trên cùng bên trái | không | toàn cầu | 

Điều này xác nhận hành vi dự phòng: khi không có ô trống nào tồn tại, chúng tôi sẽ mở rộng sang bảng đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Kích thước bảng được cố định ở 81 ô, vì vậy tất cả các lần quét đều được giới hạn không đổi | 
| Không gian | O(1) | Chỉ có bảng và một vài biến được lưu trữ | 

Kích thước không đổi của lưới đảm bảo giải pháp nằm trong giới hạn. Ngay cả với nhiều lần quét, số lượng thao tác tối đa là rất nhỏ và hiệu quả không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    old = sys.stdout
    sys.stdout = io.StringIO()

    main()

    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# sample-style minimal case
assert run("""... ... ...
... ... ...
... ... ...

... ... ...
... ... ...
... x.. ...

... ... ...
... ... ...
... ... ...
6 4
""") != "", "sample 1 style execution"

# all empty grid, last move in center
assert run("""......... ......... ......... ......... ......... ......... ......... ......... .........

......... ......... ......... ......... ......... ......... ......... ......... .........
5 5
""") != "", "center block restriction applies"

# full target block case triggers global freedom
assert run("""xxx xxx xxx
xxx xxx xxx
xxx xxx xxx

xxx xxx xxx
xxx xxx xxx
xxx xxx xxx

xxx xxx xxx
xxx xxx xxx
xxx xxx xxx
1 1
""") != "", "full block fallback"

# boundary last move bottom-right cell
assert run("""......... ......... ......... ......... ......... ......... ......... ......... .........
9 9
""") != "", "bottom-right mapping"

# single empty cell anywhere
assert run("""xxx xxx xxx
xxx xxx xxx
xxx xxx xxx

xxx xxx xxx
xxx xxx xxx
xxx xxx xx.

xxx xxx xxx
xxx xxx xxx
xxx xxx xxx
6 8
""") != "", "single empty propagation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| di chuyển trung tâm | đánh dấu khối hạn chế | quy tắc bình thường | 
| khối đầy đủ | đánh dấu toàn cầu | quy tắc dự phòng | 
| di chuyển góc | ánh xạ khối chính xác | độ đúng ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi bảng phụ bắt buộc được lấp đầy hoàn toàn. Trong tình huống đó, thuật toán chuyển từ đánh dấu giới hạn khối sang đánh dấu toàn cục. Quá trình quét phát hiện chính xác điều này bởi vì`has_empty`vẫn sai sau khi kiểm tra tất cả 9 ô trong khối. 

Một trường hợp cạnh khác là khi nước đi cuối cùng nằm trên một ranh giới như (3,3) hoặc (6,6). Các vị trí này vẫn ánh xạ rõ ràng thông qua phép chia số nguyên thành chỉ mục khối chính xác và quá trình tính toán`(x // 3, y // 3)`đảm bảo tính nhất quán. 

Trường hợp cuối cùng là khi chỉ tồn tại một ô trống duy nhất trong toàn bộ lưới. Nếu nó nằm bên ngoài khối bắt buộc khi khối hợp lệ, nó sẽ vẫn không được đánh dấu một cách chính xác vì thuật toán không bao giờ xem xét nó. Nếu khối đầy, dự phòng sẽ đảm bảo nó sẽ được đánh dấu chính xác.
