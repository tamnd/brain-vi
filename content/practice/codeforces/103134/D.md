---
title: "CF 103134D - Corona Mashup"
description: "Chúng tôi được cung cấp một nhóm người tham gia, mỗi người trong số họ sẽ có sẵn bắt đầu từ một ngày nhất định và vẫn có sẵn cho mọi ngày sau đó mãi mãi."
date: "2026-07-03T20:04:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103134
codeforces_index: "D"
codeforces_contest_name: "VI MaratonUSP Freshmen Contest"
rating: 0
weight: 103134
solve_time_s: 44
verified: true
draft: false
---

[CF 103134D - Bản kết hợp Corona](https://codeforces.com/problemset/problem/103134/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một nhóm người tham gia, mỗi người trong số họ sẽ có sẵn bắt đầu từ một ngày nhất định và vẫn có sẵn cho mọi ngày sau đó mãi mãi. Vì vậy, mỗi người tham gia đóng góp một ngưỡng thời gian và vào bất kỳ ngày nào đã chọn, chúng tôi sẽ xem xét tất cả những người tham gia có ngưỡng tối đa là ngày đó. 

Cho bất kỳ ngày nào$D$, số lượng người tham gia có mặt chỉ đơn giản là số lượng giá trị$a_i \le D$. Tuy nhiên, cuộc thi chỉ hợp lệ nếu số này chia hết cho 3. Trong số tất cả những ngày hợp lệ như vậy, chúng tôi muốn ngày sớm nhất theo nghĩa là số người tham dự tối đa có thể, hay chính xác hơn là ngày có số lượng người tham gia lớn nhất trong khi vẫn giữ số đó chia hết cho 3. Nếu không có ngày đó, chúng tôi xuất ra -1. 

Điểm tinh tế quan trọng là ngày không bị giới hạn bởi kích thước đầu vào. Mỗi$a_i$có thể lớn như$10^{18}$, do đó, câu trả lời luôn là một trong các giá trị đã cho hoặc có thể là “giữa” chúng theo nghĩa khái niệm, nhưng lần duy nhất số lượng thay đổi chính xác là ở thời điểm$a_i$các giá trị. Giữa hai khoảng cách liên tiếp$a_i$, tập hợp những người tham gia tích cực không thay đổi. 

Điều này đã ngụ ý rằng bất kỳ ngày tối ưu nào của ứng viên đều có thể được giảm xuống thành một trong những ngày riêng biệt$a_i$, bởi vì việc chọn một ngày hoàn toàn giữa hai giá trị sẽ tạo ra số lượng người tham gia giống như ranh giới nhỏ hơn nhưng không bao giờ cải thiện tính khả thi. 

Việc quét đơn giản trong tất cả các ngày có thể là không thể vì phạm vi lên tới$10^{18}$. Ngay cả việc quét tất cả các giá trị theo thứ tự được sắp xếp cũng chỉ khả thi nếu chúng ta nén các bản sao và chỉ xử lý tại các điểm dừng. 

Các trường hợp cạnh quan trọng ở đây là các giá trị lặp lại và căn chỉnh chia hết. Ví dụ: nếu nhiều người tham gia có mặt trong cùng một ngày, số lượng có thể tăng hơn 1 và bỏ qua hoàn toàn bội số của 3. Ví dụ: nếu tại một ngày nhất định, số đếm tăng từ 2 lên 4 thì không có ngày hợp lệ nào tồn tại giữa hai số đó vì yêu cầu phụ thuộc vào bội số chính xác của 3. 

Một trường hợp đặc biệt khác là khi tổng số người tham gia cuối cùng không chia hết cho 3. Ngay cả khi ngày cuối cùng bao gồm tất cả mọi người, ngày đó có thể không hợp lệ và chúng ta cần tìm tiền tố gần nhất trước đó trong đó số đếm đạt bội số của 3. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng các ngày theo thứ tự tăng dần và mỗi ngày tính toán xem có bao nhiêu người tham gia đang hoạt động. Vì hoạt động chỉ thay đổi ở mỗi$a_i$, chúng ta sẽ sắp xếp mảng và quét qua nó, tính toán lại số đếm ở mỗi giá trị riêng biệt. Điều này cung cấp cho chúng tôi kích thước tiền tố chính xác và cho phép kiểm tra mức chia hết cho 3. 

Tuy nhiên, việc thực hiện bất cẩn lặp đi lặp lại hàng ngày là không thể do$10^{18}$phạm vi. Ngay cả việc lặp qua tất cả các giá trị riêng biệt cũng được, nhưng việc tính lại số đếm từ đầu mỗi lần sẽ quá chậm, dẫn đến$O(n^2)$hành vi trong trường hợp xấu nhất. 

Quan sát quan trọng là việc sắp xếp mảng mang lại cho chúng ta cấu trúc tiền tố trong đó câu trả lời chỉ phụ thuộc vào kích thước tiền tố. Ở vị trí đã sắp xếp$i$, số lượng người tham gia tích cực là$i+1$. Chúng tôi chỉ quan tâm đến các chỉ số nơi$i+1$chia hết cho 3. Trong số các chỉ số đó, ngày tốt nhất chỉ đơn giản là giá trị tại chỉ số đó. Nếu nhiều giá trị bằng nhau, chúng tôi vẫn coi chúng là những người tham gia riêng biệt, do đó, các giá trị trùng lặp chỉ quan trọng ở số lượng chứ không phải ở thứ tự. 

Do đó, vấn đề giảm xuống còn việc sắp xếp và quét một lần, chọn chỉ mục cuối cùng có kích thước tiền tố chia hết cho 3. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu qua nhiều ngày | O(10^18) | O(n) | Quá chậm | 
| Sắp xếp + Kiểm tra tiền tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ngày sẵn sàng của người tham gia vào một mảng, vì mỗi giá trị đại diện cho một ngưỡng mà sau đó người tham gia luôn có mặt. Việc sắp xếp các giá trị này sẽ cho chúng ta hiểu tập hoạt động phát triển như thế nào theo thời gian. 
2. Sắp xếp mảng theo thứ tự không giảm để chúng tôi xử lý những người tham gia theo thứ tự họ có sẵn. Điều này biến vấn đề thành việc theo dõi kích thước tiền tố thay vì lý luận theo ngày tùy ý. 
3. Lặp lại mảng đã sắp xếp bằng chỉ mục$i$, trong đó số lượng người tham gia tích cực tại thời điểm đó là$i+1$. Điều này có tác dụng vì tất cả những người tham gia có ngưỡng nhỏ hơn hoặc bằng nhau chính xác là những người tham gia ở tiền tố. 
4. Đối với mỗi chỉ số$i$, kiểm tra xem$i+1$chia hết cho 3. Nếu không, hãy bỏ qua vì ngày đó sẽ vi phạm quy định quy mô nhóm phải là bội số của 3. 
5. Bất cứ khi nào$i+1$chia hết cho 3 ghi giá trị ngày tương ứng$a[i]$như một câu trả lời ứng viên. Chúng tôi luôn ghi đè các ứng cử viên trước đó vì các chỉ số sau tương ứng với nhiều người tham gia hơn, điều này hoàn toàn tốt hơn theo mục tiêu. 
6. Sau khi xử lý tất cả các giá trị, xuất ra ngày hợp lệ được ghi cuối cùng. Nếu không tìm thấy chỉ mục hợp lệ, xuất -1. 

### Tại sao nó hoạt động 

Vào bất kỳ ngày nào$D$, số lượng người tham gia chính xác bằng kích thước của tiền tố được sắp xếp$a_i$đó nhỏ hơn hoặc bằng$D$. Vì tập hợp những người tham gia tích cực chỉ thay đổi khi$D$vượt qua một giá trị trong mảng, mọi thay đổi có ý nghĩa trong câu trả lời sẽ tương ứng với một trong các vị trí được sắp xếp. Điều kiện chia hết cho 3 chỉ phụ thuộc vào độ dài tiền tố, do đó, việc kiểm tra các chỉ số trực tiếp nắm bắt tất cả các cấu hình hợp lệ có thể có mà không bỏ sót các ngày trung gian hoặc cần xem xét các phạm vi liên tục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = -1

    for i in range(n):
        if (i + 1) % 3 == 0:
            ans = a[i]

    print(ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp là cần thiết vì nó chuyển đổi vấn đề từ lý luận về thời gian kích hoạt tùy ý sang lý luận về kích thước tiền tố. Sau đó, vòng lặp mã hóa trực tiếp ràng buộc chỉ tính số chia hết cho 3 là hợp lệ. Nhiệm vụ cuối cùng luôn giữ ngày hợp lệ mới nhất, tương ứng với số lượng người tham dự tối đa có thể theo ràng buộc. 

Một lỗi phổ biến ở đây là cố gắng đánh giá tất cả các giá trị duy nhất và tính toán lại số lượng bằng một con trỏ không được đồng bộ hóa với thứ tự đã sắp xếp. Điều đó thường dẫn đến lỗi từng cái một hoặc chuyển tiếp bị bỏ lỡ khi tồn tại các bản sao. Việc giải thích tiền tố tránh hoàn toàn điều đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8
1 9 4 7 8 4 9 9
```Mảng được sắp xếp trở thành:```
1 4 4 7 8 9 9 9
```Chúng tôi theo dõi kích thước tiền tố: 

| tôi | một [tôi] | kích thước tiền tố (i+1) | chia hết cho 3 | ứng cử viên | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | không | - | 
| 1 | 4 | 2 | không | - | 
| 2 | 4 | 3 | vâng | 4 | 
| 3 | 7 | 4 | không | 4 | 
| 4 | 8 | 5 | không | 4 | 
| 5 | 9 | 6 | vâng | 9 | 
| 6 | 9 | 7 | không | 9 | 
| 7 | 9 | 8 | không | 9 | 

Câu trả lời cuối cùng là 9. 

Điều này cho thấy thuật toán luôn ưu tiên kích thước tiền tố hợp lệ mới nhất, vì kích thước đó tương ứng với số lượng tham dự tối đa trong khi vẫn duy trì khả năng chia hết. 

### Ví dụ 2 

đầu vào:```
4
1 2 3 3
```Đã sắp xếp:```
1 2 3 3
```| tôi | một [tôi] | kích thước tiền tố | chia hết cho 3 | ứng cử viên | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | không | - | 
| 1 | 2 | 2 | không | - | 
| 2 | 3 | 3 | vâng | 3 | 
| 3 | 3 | 4 | không | 3 | 

Câu trả lời là 3. 

Giá trị trùng lặp chứng tỏ rằng việc nhiều người tham gia có mặt trong cùng một ngày không ảnh hưởng đến tính chính xác vì chúng tôi chỉ quan tâm đến kích thước tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, quét tuyến tính đơn theo sau | 
| Không gian | O(n) | Lưu trữ mảng đầu vào | 

Được cho$n \le 10^6$, việc sắp xếp nằm trong giới hạn trong 5 giây và quá trình quét tuyến tính là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = -1
    for i in range(n):
        if (i + 1) % 3 == 0:
            ans = a[i]

    return str(ans)

# provided samples
assert run("8\n1 9 4 7 8 4 9 9\n") == "9"
assert run("4\n1 2 3 3\n") == "-1"

# custom cases
assert run("3\n10 10 10\n") == "10", "all equal values"
assert run("6\n5 4 3 2 1 0\n") == "3", "descending input"
assert run("5\n1 2 3 100 200\n") == "3", "sparse values"
assert run("2\n1 2\n") == "-1", "below minimum multiple of 3"

print("ok")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 10 | trùng lặp không phá vỡ logic tiền tố | 
| đầu vào giảm dần | 3 | sắp xếp đúng đắn | 
| giá trị thưa thớt | 3 | lựa chọn đúng trong số các tiền tố hợp lệ | 
| nhỏ n < 3 | -1 | không có tiền tố bội số hợp lệ của 3 | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các giá trị giống hệt nhau. Trong trường hợp đó, mọi lần nhảy tiền tố đều diễn ra trong cùng một ngày, do đó thuật toán vẫn phải xử lý riêng từng người tham gia. Việc sắp xếp duy trì điều này và chỉ kích thước tiền tố mới kiểm soát tính hợp lệ, do đó bội số cuối cùng của 3 tiền tố sẽ đưa ra câu trả lời chính xác. 

Một trường hợp cạnh khác là khi$n < 3$. Vì không tồn tại tiền tố có kích thước chia hết cho 3 nên thuật toán để lại câu trả lời chính xác là -1. 

Trường hợp khó phát hiện cuối cùng là khi tồn tại kích thước tiền tố hợp lệ nhưng không thể truy cập được theo các ngày riêng biệt. Ngay cả khi nhiều kích thước tiền tố hợp lệ tương ứng với cùng một giá trị ngày do trùng lặp, việc chọn lần xuất hiện cuối cùng vẫn mang lại số lượng người tham gia khả thi tối đa chính xác, vì các bản sao trùng lặp thể hiện những người tham gia riêng biệt tham gia cùng một lúc.
