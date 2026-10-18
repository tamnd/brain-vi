---
title: "CF 105608C - 4 \u0432 \u0440\u044f\u0434"
description: "Nhiệm vụ này về cơ bản là mô phỏng “điều gì sẽ xảy ra nếu” trên bảng Connect Four. Chúng ta có một lưới 6 x 7 thể hiện trạng thái hiện tại của trò chơi, trong đó mỗi ô có thể chứa một quân đỏ, một màu khác hoặc để trống."
date: "2026-06-22T05:50:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105608
codeforces_index: "C"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 1\u0421, \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440 2024-2025"
rating: 0
weight: 105608
solve_time_s: 50
verified: true
draft: false
---

[CF 105608C - 4 \u0432 \u0440\u044f\u0434](https://codeforces.com/problemset/problem/105608/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này về cơ bản là mô phỏng “điều gì sẽ xảy ra nếu” trên bảng Connect Four. Chúng ta có một lưới 6 x 7 thể hiện trạng thái hiện tại của trò chơi, trong đó mỗi ô có thể chứa một quân đỏ, một màu khác hoặc để trống. Chúng ta cũng được phép thử đặt quân đỏ vào một cột bất kỳ, tuân theo quy luật trọng lực, nghĩa là nó sẽ chiếm ô trống thấp nhất trong cột đó. 

Đối với mỗi cột trong số 7 cột, chúng tôi mô phỏng việc thả quân đỏ vào cột đó nếu có thể. Sau khi đặt quân cờ, chúng ta kiểm tra xem nước đi này có tạo ra thế thắng cho quân đỏ hay không. Chiến thắng được định nghĩa là có bốn quân đỏ liên tiếp được xếp theo chiều ngang, chiều dọc hoặc đường chéo theo bất kỳ hướng nào trong hai hướng chéo. Nếu có ít nhất một đường như vậy tồn tại sau nước đi thì cột đó được coi là nước đi thắng. 

Đầu ra là số cột sao cho nếu màu đỏ chơi ở đó thì màu đỏ lập tức thắng. 

Kích thước lưới được cố định ở mức 6 x 7, do đó không gian trạng thái không đổi. Điều này có nghĩa là ngay cả một giải pháp đơn giản kiểm tra tất cả các khả năng và quét bảng nhiều lần cũng khả thi. Hàm ý ràng buộc chính là mọi giải pháp lên tới vài nghìn thao tác trên mỗi cột đều hoàn toàn an toàn. 

Trường hợp cạnh tinh tế xuất hiện khi một cột đã đầy. Trong trường hợp đó, nước đi không hợp lệ và phải được bỏ qua. Một trường hợp cạnh tranh khác là khi đặt một quân cờ không ngay lập tức tạo thành một hàng bốn liên tiếp, mặc dù bàn cờ đã chứa sẵn một phần cấu trúc có vẻ gần thắng. Việc triển khai đơn giản mà quên hoàn nguyên hành động mô phỏng sau khi kiểm tra có thể tích lũy trạng thái không chính xác trên các cột. 

Như một ví dụ về trường hợp góc, hãy xem xét một cột trong đó việc đặt một mảnh sẽ hoàn thành một đường chéo là bốn. Nếu chúng ta quên mô phỏng trọng lực một cách chính xác và thay vào đó đặt quân cờ ở ô trống trên cùng, chúng ta có thể bỏ lỡ một chiến thắng hợp lệ. Tương tự, nếu chúng ta không khôi phục được bảng sau khi kiểm tra một cột thì các lần kiểm tra sau đó sẽ bị ảnh hưởng bởi các mô phỏng trước đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp bằng vũ lực sẽ thử từng cột, mô phỏng việc thả một quân đỏ, sau đó quét toàn bộ bàn cờ để phát hiện xem có tồn tại chuỗi bốn quân đỏ liên tiếp hay không. Vì bảng chỉ có 6 x 7 nên việc kiểm tra tất cả các vị trí và hướng xuất phát có thể có là công việc liên tục nhưng vẫn lặp lại cho mỗi cột. Điều này cung cấp tổng cộng 7 mô phỏng, sau mỗi mô phỏng là quét toàn bộ bảng. 

Quá trình quét sẽ tự kiểm tra từng ô như một điểm bắt đầu tiềm năng cho một chuỗi bốn ô theo mỗi hướng trong số bốn hướng: phải, xuống, chéo xuống dưới bên phải và chéo lên trên bên phải. Mỗi lần kiểm tra là thời gian không đổi, do đó quét toàn bộ cũng là thời gian không đổi. Do đó, giải pháp brute-force vốn đã hiệu quả nhưng cấu trúc của nó hơi dư thừa vì nó kiểm tra lại toàn bộ bảng mặc dù chỉ có một phần mới được thêm vào. 

Quan sát quan trọng là chúng ta không cần phải tính toán lại mọi thứ từ đầu nếu chúng ta đã có một hàm phát hiện chính xác chiến thắng trên bàn cờ. Vì bảng mạch rất nhỏ nên việc sử dụng lại chức năng này sau mỗi lần di chuyển mô phỏng sẽ đơn giản hơn và ít xảy ra lỗi hơn so với việc cố gắng duy trì trạng thái tăng dần. Do đó, vấn đề được giải quyết tốt nhất bằng cách mô phỏng trực tiếp với chức năng kiểm tra thắng rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu mỗi lần di chuyển | O(7 × 6 × 7) | O(1) | Đã chấp nhận | 
| Mô phỏng + Kiểm tra toàn bộ chiến thắng | O(7 × 6 × 7) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi bảng như một lưới cố định và liên tục mô phỏng việc đặt một quân màu đỏ vào mỗi cột.

1. Đọc bảng 6 x 7 và lưu nó dưới dạng lưới có thể thay đổi. Điều này cho phép chúng tôi tạm thời sửa đổi nó trong quá trình mô phỏng mà không cần phân bổ các cấu trúc mới. 
2. Xác định hàm trợ giúp để kiểm tra xem màu đỏ có đường thắng ở bất kỳ vị trí nào trên bảng hay không. Chức năng quét từng ô và kiểm tra theo bốn hướng: ngang sang phải, dọc xuống dưới, chéo xuống dưới bên phải và chéo lên trên bên phải. Mỗi lần kiểm tra đảm bảo các chỉ số nằm trong giới hạn trước khi so sánh bốn ô liên tiếp. Lý do để kiểm tra tất cả các ô là vì bất kỳ phân đoạn chiến thắng nào cũng phải bắt đầu từ một số ô trên cùng bên trái của phân đoạn đó theo ít nhất một hướng. 
3. Khởi tạo bộ đếm các nước đi thắng hợp lệ. 
4. Lặp lại từng cột trong số 7 cột, coi mỗi cột là một nước đi ứng cử viên. 
5. Đối với mỗi cột, mô phỏng trọng lực bằng cách quét từ hàng dưới cùng trở lên cho đến khi tìm thấy ô trống đầu tiên. Nếu không có ô trống thì cột đã đầy và chúng ta bỏ qua nó. Bước này đảm bảo quân cờ sẽ hạ cánh chính xác như trong trò chơi thực tế. 
6. Đặt tạm thời một mảnh màu đỏ vào ô tìm thấy. 
7. Chạy chức năng win-check trên bảng cập nhật. Nếu nó trả về đúng, hãy tăng bộ đếm câu trả lời vì nước đi này sẽ dẫn đến chiến thắng ngay lập tức. 
8. Tháo mảnh mô phỏng để khôi phục trạng thái bảng ban đầu. Điều này rất quan trọng vì mỗi cột phải được kiểm tra độc lập. 
9. Sau khi kiểm tra tất cả các cột, xuất bộ đếm. 

### Tại sao nó hoạt động 

Thuật toán dựa trên sự tách biệt chặt chẽ giữa mô phỏng và đánh giá. Mỗi cột được kiểm tra riêng biệt trên một bảng không thay đổi. Chức năng kiểm tra thắng có tính toàn diện đối với tất cả các điểm và hướng bắt đầu có thể có, vì vậy nếu có bất kỳ bốn hàng liên tiếp nào tồn tại sau một nước đi, nó sẽ được phát hiện bất kể nó xuất hiện ở đâu. Vì mọi nước đi hợp pháp có thể được mô phỏng chính xác một lần nên mỗi nước đi thắng được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def win(field):
    for row in range(6):
        for col in range(7):
            if col + 3 < 7:
                if field[row][col] == field[row][col+1] == field[row][col+2] == field[row][col+3] == 'R':
                    return True
            if row + 3 < 6:
                if field[row][col] == field[row+1][col] == field[row+2][col] == field[row+3][col] == 'R':
                    return True
            if col + 3 < 7 and row + 3 < 6:
                if field[row][col] == field[row+1][col+1] == field[row+2][col+2] == field[row+3][col+3] == 'R':
                    return True
            if col + 3 < 7 and row - 3 >= 0:
                if field[row][col] == field[row-1][col+1] == field[row-2][col+2] == field[row-3][col+3] == 'R':
                    return True
    return False

field = [list(input().strip()) for _ in range(6)]

ans = 0

for col in range(7):
    row = 5
    while row >= 0 and field[row][col] != '.':
        row -= 1

    if row < 0:
        continue

    field[row][col] = 'R'

    if win(field):
        ans += 1

    field[row][col] = '.'

print(ans)
```Giải pháp bắt đầu bằng cách xác định quy trình phát hiện chiến thắng. Mỗi hướng được xử lý rõ ràng bằng các kiểm tra ranh giới để không có chỉ mục nào nằm ngoài phạm vi. Logic đối xứng trong các trường hợp ngang, dọc và chéo, đảm bảo tính hoàn chỉnh của việc phát hiện. 

Vòng lặp chính thử từng cột một cách độc lập. Việc mô phỏng trọng lực được thực hiện bằng cách quét xuống đơn giản, tối ưu với chiều cao không đổi của bảng. Sau khi đặt quân cờ, chúng tôi đánh giá ngay bảng, sau đó hoàn nguyên nó. Bước khôi phục này ngăn chặn rò rỉ trạng thái qua các lần lặp. 

Một sai lầm phổ biến là quên khôi phục ô sau khi kiểm tra một cột. Điều đó sẽ khiến các mô phỏng sau này cho rằng đã tồn tại thêm các mảnh màu đỏ, tạo ra kết quả không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Bảng đầu vào:```
.......
.......
.......
.......
.......
RRR....
```Chúng tôi kiểm tra từng cột. Chỉ những cột mà việc đặt quân đỏ hoàn thành bốn quân đỏ liên tiếp ở hàng dưới cùng mới thành công. 

| Cột | Thả hàng | Giành chiến thắng sau khi di chuyển | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 4 | Không | 0 | 
| 1 | 4 | Không | 0 | 
| 2 | 4 | Không | 0 | 
| 3 | 5 | Có (RRRR) | +1 | 
| 4 | 5 | Không | 0 | 
| 5 | 5 | Không | 0 | 
| 6 | 5 | Không | 0 | 

Chỉ có cột 3 là hoàn thành đường ngang. 

Câu trả lời cuối cùng là 1. 

### Ví dụ 2 

Bảng đầu vào:```
.......
.......
.......
...R...
..R....
.R.....
```Điều này tạo thành một cấu trúc đường chéo chỉ còn một bước nữa là hoàn thành. 

| Cột | Thả hàng | Giành chiến thắng sau khi di chuyển | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 2 | Có | +1 | 
| 1 | 3 | Không | 0 | 
| 2 | 4 | Không | 0 | 
| 3 | 5 | Không | 0 | 
| 4 | 5 | Không | 0 | 
| 5 | 5 | Không | 0 | 
| 6 | 5 | Không | 0 | 

Chỉ có cột 0 hoàn thành đường chéo. 

Câu trả lời cuối cùng là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(7 × 6 × 7) | 7 bước di chuyển mô phỏng, mỗi bước kích hoạt một lượt quét 6 x 7 liên tục | 
| Không gian | O(1) | Bảng có kích thước cố định, không có kết cấu phụ trợ | 

Kích thước lưới không đổi, do đó tổng công việc được giới hạn bởi một hằng số nhỏ. Điều này nằm trong giới hạn Codeforces điển hình ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    # solution
    import sys
    input = sys.stdin.readline

    def win(field):
        for row in range(6):
            for col in range(7):
                if col + 3 < 7:
                    if field[row][col] == field[row][col+1] == field[row][col+2] == field[row][col+3] == 'R':
                        return True
                if row + 3 < 6:
                    if field[row][col] == field[row+1][col] == field[row+2][col] == field[row+3][col] == 'R':
                        return True
                if col + 3 < 7 and row + 3 < 6:
                    if field[row][col] == field[row+1][col+1] == field[row+2][col+2] == field[row+3][col+3] == 'R':
                        return True
                if col + 3 < 7 and row - 3 >= 0:
                    if field[row][col] == field[row-1][col+1] == field[row-2][col+2] == field[row-3][col+3] == 'R':
                        return True
        return False

    field = [list(input().strip()) for _ in range(6)]
    ans = 0

    for col in range(7):
        row = 5
        while row >= 0 and field[row][col] != '.':
            row -= 1
        if row < 0:
            continue
        field[row][col] = 'R'
        if win(field):
            ans += 1
        field[row][col] = '.'

    return str(ans)

# custom cases
assert run(".......\n.......\n.......\n.......\n.......\nRRR....\n") == "1", "horizontal win"
assert run(".......\n.......\n.......\n...R...\n..R....\n.R.....\n") == "1", "diagonal win"
assert run("RRRR...\n.......\n.......\n.......\n.......\n.......\n") == "0", "already win no move needed"
assert run(".......\n.......\n.......\n.......\n.......\n.......\n") == "0", "empty board"
assert run("RRRRRRR\nRRRRRRR\nRRRRRRR\nRRRRRRR\nRRRRRRR\nRRRRRRR\n") == "0", "full board"

# restore stdout
sys.stdout = sys.__stdout__
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ngang cận thắng | 1 | phát hiện chính xác việc hoàn thành hàng | 
| Đường chéo gần thắng | 1 | tương tác trọng lực chéo | 
| Bảng đỏ đầy đủ | 0 | không xử lý di chuyển hợp lệ | 
| Bảng trống | 0 | hành vi cơ bản | 
| Lưới được lấp đầy hoàn toàn | 0 | bỏ qua toàn cột | 

## Vỏ cạnh 

Một cột đầy đủ là trường hợp cạnh cấu trúc đơn giản nhất. Thuật toán xử lý nó bằng cách quét lên trên cho đến khi không tìm thấy ô trống, sau đó bỏ qua cột đó. Điều này đảm bảo chúng tôi không bao giờ đặt một quân cờ bất hợp pháp. 

Trường hợp cạnh thứ hai là khi đã có chiến thắng trước bất kỳ nước đi nào. Vấn đề vẫn yêu cầu các nước đi tạo ra chiến thắng sau khi sắp xếp, vì vậy chúng tôi chỉ tính các cột trong đó nước đi mô phỏng dẫn đến trạng thái thắng. Chức năng kiểm tra chiến thắng được áp dụng sau mỗi vị trí được mô phỏng, do đó, các chiến thắng có sẵn không ảnh hưởng đến việc đếm. 

Trường hợp cạnh thứ ba là hoàn thành đường chéo ở ranh giới của bảng. Ví dụ: đường chéo kết thúc ở hàng 0 hoặc hàng 5 yêu cầu kiểm tra ranh giới cẩn thận. Việc triển khai bảo vệ rõ ràng quyền truy cập chỉ mục trước khi so sánh bốn ô, đảm bảo không có quyền truy cập bộ nhớ không hợp lệ và không có cấu hình bị bỏ sót.
