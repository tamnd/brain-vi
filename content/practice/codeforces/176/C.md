---
title: "CF 176C - Chơi đùa với keo siêu dính"
description: "Chúng ta có hai con chip trên lưới $n nhân m$. Trong mỗi lượt, người chơi đầu tiên di chuyển chính xác một con chip không được dán theo từng ô vuông trong lưới bốn người lân cận. Sau đó, người chơi thứ hai sẽ dán vĩnh viễn một ô vuông hiện đang trống. Một con chip được phép di chuyển lên một hình vuông được dán keo."
date: "2026-06-02T17:16:31+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 176
codeforces_index: "C"
codeforces_contest_name: "Croc Champ 2012 - Round 2"
rating: 2000
weight: 176
solve_time_s: 215
verified: false
draft: false
---

[CF 176C - Chơi với keo siêu dính](https://codeforces.com/problemset/problem/176/C) 

**Xếp hạng:** 2000 
**Tags:** tổ hợp, thuật toán xây dựng 
**Thời gian giải:** 3 phút 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai con chip trên một$n \times m$lưới. Trong mỗi lượt, người chơi đầu tiên di chuyển chính xác một con chip không được dán theo từng ô vuông trong lưới bốn người lân cận. Sau đó, người chơi thứ hai sẽ dán vĩnh viễn một ô vuông hiện đang trống. 

Một con chip được phép di chuyển lên một hình vuông được dán keo. Ngay khi làm như vậy, con chip đó sẽ bị kẹt vĩnh viễn. Người chơi đầu tiên thắng ngay khi hai quân bài chiếm cùng một ô. Người chơi thứ hai thắng nếu cả hai con chip không thể di chuyển trước khi chúng gặp nhau. 

Kích thước bảng có thể lớn bằng$100 \times 100$, nhưng điều đó hóa ra lại gây hiểu nhầm. Điều kiện thắng chỉ phụ thuộc vào độ dịch chuyển tương đối của hai con chip chứ không phụ thuộc vào tổng số ô. Không gian trạng thái của trò chơi đầy đủ là rất lớn vì vị trí dán keo tích lũy theo thời gian, do đó mọi nỗ lực nhằm lập mô hình toàn bộ cây trò chơi đều vô vọng. 

Việc triển khai đơn giản có thể cố gắng biểu diễn tất cả các ô được dán và chạy tìm kiếm tối đa. Ngay cả trên một$10 \times 10$bảng đã có rồi$100$những vị trí có thể dán keo ở lần di chuyển đầu tiên, sau đó$99$on the next move, and so on. The branching factor explodes immediately.

 Phần khó khăn là bản thân kích thước bảng hầu như không liên quan. Hai con chip cách nhau rất xa giúp người chơi thứ hai có đủ thời gian để xây dựng rào cản và buộc cả hai con chip bị mắc kẹt. Hai chip gần nhau thường có thể gặp nhau trước khi keo trở nên quyết định. 

Một sai lầm dễ mắc phải là chỉ nhìn vào khoảng cách Manhattan. Ví dụ:```
10 10 1 1 5 5
```Ở đây độ dịch chuyển là$(4,4)$. Khoảng cách Manhattan chỉ$8$, nhưng câu trả lời đúng là`Second`. Chiến lược chỉ dựa trên khoảng cách sẽ dự đoán sai chiến thắng cho người chơi đầu tiên. 

Một trường hợp tế nhị khác là:```
10 10 1 1 4 5
```Sự dịch chuyển là$(3,4)$. Đây cũng là cấu hình thua của người chơi đầu tiên dù nhiều vị trí lân cận đang giành chiến thắng. 

Một cái bẫy cuối cùng là giả định rằng mọi vị trí bên trong một$5 \times 5$khu phố đang chiến thắng. Các cặp$(3,4)$Và$(4,4)$là những trường hợp ngoại lệ đặc biệt. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đại diện cho toàn bộ bàn cờ, theo dõi ô nào được dán, tạo ra mọi nước đi hợp pháp cho người chơi đầu tiên và mọi vị trí dán keo cho người chơi thứ hai, sau đó chạy minimax. 

Điều này đúng vì nó khám phá toàn bộ cây trò chơi. Vấn đề là số lượng vị trí có thể tiếp cận tăng lên một cách chóng mặt. Chỉ sau một vài lượt quay đã có hàng triệu cấu hình keo khác nhau. Không có giải pháp dựa trên tìm kiếm nào có thể tồn tại trong giới hạn. 

Quan sát quan trọng là cách chơi tối ưu chỉ phụ thuộc vào vị trí tương đối của hai con chip. Sau khi phân tích trò chơi, người ta tìm thấy một bộ cấu hình bị mất khá nhỏ. 

Cho phép$$dx = |x_1 - x_2|,\qquad dy = |y_1 - y_2|$$và sắp xếp lại chúng sao cho$dx \le dy$. 

Giải pháp hoàn chỉnh là: 

Nếu$dy > 4$, người chơi thứ hai thắng. 

Nếu như$(dx,dy) = (4,4)$, người chơi thứ hai thắng. 

Nếu như$(dx,dy) = (3,4)$, người chơi thứ hai thắng. 

Mọi vị trí khác đều giành chiến thắng cho người chơi đầu tiên. Đặc tính này là giải pháp dự định của vấn đề. 

Khi đã biết được mẫu này, trò chơi sẽ giảm xuống còn một vài phép so sánh số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước bảng và tọa độ của hai con chip. 
2. Tính sai phân tọa độ tuyệt đối:$$dx = |x_1 - x_2|$$

$$dy = |y_1 - y_2|$$Chỉ có sự dịch chuyển tương đối mới quan trọng. 
3. Sắp xếp lại các giá trị sao cho$dx \le dy$. 

Điều này cho phép chúng ta xử lý các vị trí đối xứng một cách giống hệt nhau. 
4. Nếu$dy > 4$, đầu ra`"Second"`. 

Các vị trí cách nhau hơn bốn ô theo một hướng sẽ thua đối với người chơi đầu tiên. 
5. Nếu$(dx,dy) = (4,4)$, đầu ra`"Second"`. 
6. Nếu$(dx,dy) = (3,4)$, đầu ra`"Second"`. 
7. Trong mọi trường hợp còn lại, xuất ra`"First"`. 

### Tại sao nó hoạt động 

The game has a complete classification in terms of the relative displacement between the chips. Sau khi bình thường hóa để$dx \le dy$, mọi vị trí bên ngoài$5 \times 5$khu vực lân cận đang thua đối với người chơi đầu tiên vì người chơi thứ hai luôn có thể tạo đủ các ô được dán trước khi các con chip có thể gặp nhau. Bên trong vùng lân cận đó, phân tích trò chơi toàn diện chỉ để lại hai cấu hình thua cuộc,$(3,4)$Và$(4,4)$. Mọi sự dịch chuyển bình thường hóa khác đều thừa nhận một chiến lược bắt buộc đối với người chơi đầu tiên. 

Bởi vì thuật toán thực hiện chính xác đặc tính này nên câu trả lời của nó là đúng cho mọi đầu vào hợp pháp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, x1, y1, x2, y2 = map(int, input().split())

    dx = abs(x1 - x2)
    dy = abs(y1 - y2)

    if dx > dy:
        dx, dy = dy, dx

    if dy > 4:
        print("Second")
    elif (dx, dy) == (4, 4):
        print("Second")
    elif (dx, dy) == (3, 4):
        print("Second")
    else:
        print("First")

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán độ dịch chuyển tương đối giữa các chip. Tọa độ thực tế và thậm chí cả kích thước bảng không còn quan trọng sau bước này. 

Việc hoán đổi đảm bảo rằng các vị trí tương đương như$(1,4)$Và$(4,1)$được đại diện bởi cùng một cặp. Việc quên việc chuẩn hóa này sẽ phân loại sai các vị trí đối xứng. 

Logic còn lại trực tiếp thực hiện đặc tính vị trí chiến thắng. Thứ tự kiểm tra không quan trọng nhưng việc kiểm tra`dy > 4`đầu tiên làm cho cấu trúc rõ ràng hơn một chút. 

Không có vấn đề tràn tồn tại vì mọi giá trị đều tối đa$100$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 6 1 2 1 6
```| Biến | Giá trị | 
| --- | --- | 
| dx | 0 | 
| nhuộm | 4 | 
| Cặp chuẩn hóa | (0, 4) | 
| Mất hình mẫu? | Không | 
| Trả lời | Đầu tiên | 

Vị trí nằm trong vùng thắng và không phải là một trong hai trạng thái thua đặc biệt. 

### Ví dụ 2 

đầu vào:```
10 10 1 1 10 10
```| Biến | Giá trị | 
| --- | --- | 
| dx | 9 | 
| nhuộm | 9 | 
| Cặp chuẩn hóa | (9, 9) | 
| dy> 4? | Có | 
| Trả lời | Thứ hai | 

Các con chip bắt đầu cách nhau quá xa. Người chơi thứ hai có đủ thời gian để kiểm soát bàn cờ trước khi cuộc họp diễn ra. 

Ví dụ này chứng tỏ tính bất biến quan trọng nhất của nghiệm: một khi chênh lệch tọa độ lớn hơn vượt quá 4 thì hình học chính xác không còn quan trọng nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một vài phép tính số học và so sánh | 
| Không gian | O(1) | Bộ nhớ bổ sung liên tục | 

Bảng có thể chứa tới$10{,}000$các ô, nhưng thuật toán không bao giờ kiểm tra từng ô riêng lẻ. Nó thực hiện một lượng công việc cố định bất kể kích thước bảng, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    n, m, x1, y1, x2, y2 = map(int, input().split())

    dx = abs(x1 - x2)
    dy = abs(y1 - y2)

    if dx > dy:
        dx, dy = dy, dx

    if dy > 4 or (dx, dy) == (4, 4) or (dx, dy) == (3, 4):
        print("Second")
    else:
        print("First")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    global input
    input = sys.stdin.readline

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided samples
assert run("1 6 1 2 1 6\n") == "First", "sample 1"
assert run("6 5 4 3 2 1\n") == "First", "sample 2"
assert run("10 10 1 1 10 10\n") == "Second", "sample 3"

# custom cases
assert run("1 2 1 1 1 2\n") == "First", "minimum board"
assert run("10 10 1 1 5 5\n") == "Second", "(4,4) losing state"
assert run("10 10 1 1 4 5\n") == "Second", "(3,4) losing state"
assert run("100 100 1 1 1 5\n") == "First", "boundary dy=4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 1 1 1 2`|`First`| Hội đồng pháp lý nhỏ nhất | 
|`10 10 1 1 5 5`|`Second`| Trạng thái thua đặc biệt (4,4) | 
|`10 10 1 1 4 5`|`Second`| Trạng thái thua đặc biệt (3,4) | 
|`100 100 1 1 1 5`|`First`| Trường hợp biên trong đó dy bằng 4 | 

## Vỏ cạnh 

Hãy xem xét:```
10 10 1 1 5 5
```Chúng tôi có được$dx = 4$,$dy = 4$. Thuật toán đạt đến mức rõ ràng$(4,4)$kiểm tra và đầu ra`Second`. Giải pháp chỉ dựa trên quy tắc "tất cả các vị trí có cách biệt nhiều nhất là bốn sẽ thắng" sẽ thất bại ở đây. 

Bây giờ hãy xem xét:```
10 10 1 1 4 5
```Sự dịch chuyển chuẩn hóa là$(3,4)$. Đây là trạng thái thua lỗ đặc biệt khác. Kiểm tra chuyên dụng trả về chính xác`Second`. 

Cuối cùng:```
100 100 1 1 1 6
```Sự dịch chuyển chuẩn hóa là$(0,5)$. Từ$dy > 4$, thuật toán ngay lập tức xuất ra`Second`. Điều này xác nhận rằng quy tắc khoảng cách lớn được ưu tiên hơn tất cả các mẫu cục bộ nhỏ hơn. Câu trả lời được xác định mà không cần xem xét kích thước bảng thực tế.
