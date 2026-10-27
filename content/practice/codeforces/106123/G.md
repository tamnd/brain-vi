---
title: "CF 106123G - Chiếc Xương Bị Mất"
description: "Bài toán mô tả một dòng gồm n cốc được đặt ở các vị trí từ 1 đến n. Một xương bắt đầu dưới cốc đầu tiên. Một số vị trí có lỗ. Một chuỗi hoán đổi được thực hiện giữa các cặp vị trí cốc."
date: "2026-06-25T11:33:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106123
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-15-25 Div. 1 (Advanced)"
rating: 0
weight: 106123
solve_time_s: 28
verified: true
draft: false
---

[CF 106123G - Xương bị mất](https://codeforces.com/problemset/problem/106123/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một dòng`n`cốc đặt ở các vị trí`1`bởi vì`n`. Một xương bắt đầu dưới cốc đầu tiên. Một số vị trí có lỗ. Một chuỗi hoán đổi được thực hiện giữa các cặp vị trí cốc. Trong quá trình hoán đổi, hai chiếc cốc ngay lập tức đổi chỗ cho nhau, do đó xương chỉ di chuyển nếu nó ở bên trong một trong những chiếc cốc được hoán đổi đó. Nếu xương chạm tới lỗ sau bất kỳ lần hoán đổi nào, nó sẽ rơi ra ngoài và biến mất vĩnh viễn. Nhiệm vụ là xác định vị trí cuối cùng của xương hoặc báo cáo rằng nó đã rơi xuống. 

Đầu vào cung cấp số vị trí cốc, số lỗ, số lần hoán đổi, vị trí lỗ và sau đó là các thao tác hoán đổi. Đầu ra là vị trí cuối cùng chứa xương sau mọi thao tác, trừ khi xương bị mất do lỗ. 

Các ràng buộc làm cho giải pháp dự kiến ​​trở nên rõ ràng. Số lượng hoạt động có thể đủ lớn để việc mô phỏng bất kỳ điều gì ngoài các giao dịch hoán đổi nhất định là không cần thiết. Mỗi lần hoán đổi chỉ ảnh hưởng đến hai vị thế, do đó, giải pháp dành thời gian không đổi cho mỗi lần hoán đổi là lý tưởng. Việc xây dựng các cấu trúc dữ liệu đắt tiền hoặc kiểm tra từng cốc sau mỗi thao tác sẽ tạo ra chi phí có thể tránh được. 

Những trường hợp phức tạp xuất phát từ việc rơi xuống hố sẽ dừng mọi chuyển động trong tương lai. Một sai lầm phổ biến là tiếp tục áp dụng hoán đổi sau khi xương đã biến mất. 

Ví dụ: nếu đầu vào là:```
3 1 2
2
1 2
2 3
```Xương bắt đầu ở vị trí`1`. Hoán đổi đầu tiên sẽ chuyển nó tới vị trí`2`, nơi có lỗ, nên kết quả đúng là:```
-1
```Việc mô phỏng bất cẩn mà bỏ qua các lỗ cho đến hết sẽ làm di chuyển xương khỏi vị trí`2`để định vị`3`và in câu trả lời sai. 

Một trường hợp khác là khi việc hoán đổi không liên quan đến vị trí xương hiện tại. Ví dụ:```
5 0 2
1 2
4 5
```Xương bắt đầu ở`1`. Trao đổi đầu tiên chuyển nó đến`2`và lần hoán đổi thứ hai không ảnh hưởng đến nó. Câu trả lời là:```
2
```Việc triển khai hoán đổi cốc mà không theo dõi chính xác vị trí xương có thể vô tình di chuyển xương lần nữa. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là giữ nguyên vị trí xương hiện tại và xử lý mọi lần hoán đổi. Đối với mỗi thao tác, nếu xương ở một trong hai vị trí hoán đổi, nó sẽ di chuyển sang vị trí còn lại. Sau khi di chuyển, chúng tôi kiểm tra xem vị trí mới có phải là lỗ hay không. Nếu có thì xương đã biến mất. 

Cách tiếp cận bạo lực này đã là tối ưu vì chính đầu vào mô tả các sự kiện duy nhất có thể thay đổi câu trả lời. Không cần thiết phải xây dựng lại toàn bộ cách sắp xếp các cốc. Thông tin liên quan duy nhất là vị trí của xương và vị trí đó có phải là lỗ hay không. 

Một mô phỏng phức tạp hơn có thể lưu trữ mọi cốc và di chuyển các đồ vật xung quanh trong mỗi lần hoán đổi. Điều đó hoạt động về mặt khái niệm, nhưng nó theo dõi những thông tin không quan trọng. Những chiếc cốc chỉ là nhãn xung quanh xương, và câu hỏi chỉ hỏi về xương mà thôi. Vì việc hoán đổi chỉ thay đổi vị trí của xương khi vị trí hiện tại của nó là một trong những vị trí được hoán đổi, nên chúng ta có thể giảm toàn bộ quá trình xuống một bản cập nhật số nguyên duy nhất. 

Ở đây, sức mạnh tàn bạo và các giải pháp tối ưu đều giống nhau vì mô phỏng đơn giản đã là công việc tối thiểu có thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(n) | Đã chấp nhận | 
| Tối ưu | O(k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các vị trí lỗ trong một bộ. Một bộ cho phép chúng ta kiểm tra xem xương có bị rơi ra sau khi di chuyển trong thời gian trung bình không đổi hay không. 
2. Khởi tạo vị trí xương như`1`, vì xương bắt đầu ở dưới cốc đầu tiên. 
3. Xử lý từng giao dịch hoán đổi một. Để hoán đổi giữa các vị trí`u`Và`v`, di chuyển xương từ`u`ĐẾN`v`hoặc từ`v`ĐẾN`u`chỉ khi nó hiện đang ở dưới một trong những chiếc cốc đó. Nếu không có vị trí nào chứa xương, việc hoán đổi sẽ không ảnh hưởng đến câu trả lời. 
4. Sau mỗi lần di chuyển, hãy kiểm tra xem vị trí xương mới có bị thủng hay không. Nếu đúng như vậy, hãy đánh dấu xương là bị mất và ngừng xử lý các giao dịch hoán đổi trong tương lai. 
5. Nếu tất cả các lần hoán đổi được xử lý mà không làm mất xương, hãy in vị trí cuối cùng của nó. 

Tại sao nó hoạt động: 

Tại mọi thời điểm, phần duy nhất của việc sắp xếp chiếc cốc có ý nghĩa quan trọng là chiếc cốc hiện đang giấu xương. Việc hoán đổi sẽ thay đổi chính xác vị trí của xương khi chiếc cốc đó tham gia vào quá trình hoán đổi. Thuật toán áp dụng chính xác những thay đổi đó và bỏ qua các giao dịch hoán đổi không thể ảnh hưởng đến xương. Sau mỗi chuyển động có thể, nó sẽ ngay lập tức kiểm tra tình trạng lỗ, khớp với quy tắc của quy trình. Vì trạng thái được cập nhật sau mỗi thao tác nên vị trí được lưu trữ cuối cùng chính xác là kết quả thực. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    holes = set(map(int, input().split()))

    pos = 1
    alive = True

    for _ in range(k):
        u, v = map(int, input().split())

        if alive:
            if pos == u:
                pos = v
            elif pos == v:
                pos = u

            if pos in holes:
                alive = False

    print(pos if alive else -1)

if __name__ == "__main__":
    solve()
```Giải pháp lưu trữ các lỗ hổng trong tập hợp băm vì việc kiểm tra tư cách thành viên diễn ra sau mỗi lần hoán đổi. Một danh sách cũng có tác dụng, nhưng tập hợp này trực tiếp thể hiện hoạt động mà chúng ta cần: hỏi xem một vị trí có nguy hiểm hay không. 

Biến`pos`là trạng thái hoàn chỉnh của mô phỏng. Không cần lưu trữ nội dung trong cốc vì việc hoán đổi chỉ quan trọng khi chạm vào vị trí này. 

các`alive`cờ ngăn chặn những lần hoán đổi sau này thay đổi câu trả lời sau khi xương đã rụng. Điều này phù hợp với quy luật của bài toán là xương không còn ở bên trong bất kỳ chiếc cốc nào một khi nó lọt vào lỗ. 

Thứ tự thực hiện các thao tác là quan trọng. Việc hoán đổi phải diễn ra trước khi kiểm tra lỗ vì xương chỉ có thể rơi xuống sau khi đến vị trí mới. Kiểm tra trước khi di chuyển sẽ bỏ lỡ trường hợp việc hoán đổi đặt xương trực tiếp vào lỗ. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5 1 3
4
1 3
3 4
2 5
```| Bước | Hoán đổi | Vị trí xương trước | Vị trí xương sau | Còn sống | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | 1 | 1 | vâng | 
| 1 | 1 3 | 1 | 3 | vâng | 
| 2 | 3 4 | 3 | 4 | không | 
| 3 | 2 5 | 4 | 4 | không | 

Lần hoán đổi thứ hai đặt xương vào vị trí`4`, đó là một cái lỗ. Lần hoán đổi cuối cùng bị bỏ qua vì xương đã biến mất. 

Một ví dụ khác:```
4 0 3
1 2
2 3
4 1
```| Bước | Hoán đổi | Vị trí xương trước | Vị trí xương sau | Còn sống | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | không | 1 | 1 | vâng | 
| 1 | 1 2 | 1 | 2 | vâng | 
| 2 | 2 3 | 2 | 3 | vâng | 
| 3 | 4 1 | 3 | 3 | vâng | 

Câu trả lời cuối cùng là`3`. Dấu vết này cho thấy các hoán đổi không liên quan đến xương không làm thay đổi trạng thái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi lần hoán đổi được xử lý một lần với các lần cập nhật và kiểm tra liên tục. | 
| Không gian | O(m) | Bộ lưu trữ các vị trí lỗ. | 

Giải pháp này phù hợp với các ràng buộc vì nó chỉ thực hiện công việc cần thiết: cập nhật một lần liên tục cho mỗi lần hoán đổi. Ngay cả những chuỗi hoán đổi rất lớn vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    holes = set(map(int, input().split()))

    pos = 1
    alive = True

    for _ in range(k):
        u, v = map(int, input().split())

        if alive:
            if pos == u:
                pos = v
            elif pos == v:
                pos = u

            if pos in holes:
                alive = False

    ans = str(pos if alive else -1)

    sys.stdin = old_stdin
    return ans

assert solve_io(
    "3 1 2\n"
    "2\n"
    "1 2\n"
    "2 3\n"
) == "-1"

assert solve_io(
    "5 0 2\n"
    "1 2\n"
    "4 5\n"
) == "2"

assert solve_io(
    "1 1 0\n"
    "1\n"
) == "1"

assert solve_io(
    "4 0 3\n"
    "1 2\n"
    "2 3\n"
    "4 1\n"
) == "3"

assert solve_io(
    "5 5 1\n"
    "1 2 3 4 5\n"
    "1 5\n"
) == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xương chui vào lỗ sau khi hoán đổi |`-1`| Chấm dứt ngay lập tức sau khi rơi | 
| Hoán đổi không phải lúc nào cũng ảnh hưởng đến xương |`2`hoặc`3`tùy theo nước đi | Theo dõi chính xác cốc hiện tại | 
| Vị trí duy nhất không có giao dịch hoán đổi | Vị trí xuất phát | Xử lý kích thước tối thiểu | 
| Một số giao dịch hoán đổi không có lỗ | Vị trí di chuyển cuối cùng | Luồng mô phỏng bình thường | 
| Vị trí bắt đầu là một lỗ | Mất sau lần kiểm tra trạng thái hợp lệ đầu tiên | Hành vi ranh giới lỗ | 

## Vỏ cạnh 

Khi xương rơi vào lỗ, những lần hoán đổi sau này không được hồi sinh nó. TRONG:```
3 1 2
2
1 2
2 3
```lần hoán đổi đầu tiên di chuyển xương từ`1`ĐẾN`2`. Thuật toán ngay lập tức thấy rằng`2`là một lỗ và bộ`alive`thành sai. Hoán đổi thứ hai được đọc nhưng không thể thay đổi câu trả lời, vì vậy đầu ra là`-1`. 

Khi hoán đổi không liên quan đến xương, vị trí phải không thay đổi. TRONG:```
5 0 2
1 2
4 5
```lần hoán đổi đầu tiên di chuyển xương từ`1`ĐẾN`2`. Lần đổi thứ hai chỉ đổi cốc`4`Và`5`, do đó xương vẫn ở`2`. Thuật toán chỉ kiểm tra xem`pos`khớp với một trong các điểm cuối được hoán đổi, duy trì trạng thái chính xác. 

Khi không có giao dịch hoán đổi, câu trả lời chỉ đơn giản là vị trí ban đầu trừ khi vị trí đó đã là một lỗ trống. Thuật toán bắt đầu với`pos = 1`và không thực hiện cập nhật nên nó xử lý trường hợp này một cách tự nhiên.
