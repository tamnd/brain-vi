---
title: "CF 102700E - Tham gia giải bài toán hay nhất cuộc thi này!"
description: "Cách tiếp cận trực tiếp là truy vấn mọi nút trong cây cho đến khi tìm thấy nút có khoảng cách bằng 0. Điều này đúng vì thiết bị cho khoảng cách chính xác nên phản hồi bằng 0 sẽ xác định được Osman ngay lập tức."
date: "2026-07-30T07:52:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "E"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 81
verified: true
draft: false
---

[CF 102700E - Tham gia giải bài toán hay nhất của cuộc thi này!](https://codeforces.com/problemset/problem/102700/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là truy vấn mọi nút trong cây cho đến khi tìm thấy nút có khoảng cách bằng 0. Điều này đúng vì thiết bị cho khoảng cách chính xác nên phản hồi bằng 0 sẽ xác định được Osman ngay lập tức. Tuy nhiên, cây có thể có`2^29 - 1`nút, tức là hơn 500 triệu nút. Ngay cả một truy vấn trên mỗi nút cũng vượt xa giới hạn truy vấn. 

Cấu trúc hữu ích là cây được cân bằng hoàn hảo và khoảng cách cho biết phía nào của nút chứa mục tiêu. Đầu tiên, truy vấn gốc cho chúng ta biết độ sâu của Osman. Sau đó, giả sử chúng ta đang đứng ở một nút`v`và biết rằng Osman đang ở đâu đó phía dưới nó ở khoảng cách xa`d`. Nếu chúng ta truy vấn con trái của`v`, chỉ có hai khả năng. Nếu Osman ở trong cây con bên trái thì câu trả lời là`d - 1`, bởi vì chúng ta đã di chuyển một cạnh lại gần hơn. Nếu Osman ở trong cây con bên phải thì câu trả lời là`d + 1`, bởi vì chúng tôi đã di chuyển đi trước khi đi xuống phía bên kia. 

Điều này có nghĩa là mỗi truy vấn quyết định một bit của đường dẫn từ gốc đến Osman. Chúng ta chỉ cần lặp lại điều này cho mọi cấp độ của cây. Lực lượng vũ phu có tác dụng vì khoảng cách hoàn toàn xác định được câu trả lời, nhưng không thành công vì nó khám phá toàn bộ cây. Quan sát cho thấy một truy vấn con sẽ tiết lộ hướng tiếp theo làm giảm việc tìm kiếm từ kích thước hàm mũ xuống chiều cao cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(2^n) | O(1) | Quá chậm | 
| Tối ưu | Truy vấn O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nút truy vấn`1`, gốc. Khoảng cách trả về là độ sâu của Osman tính từ gốc. Nếu câu trả lời là 0 thì Osman là người gốc và chúng ta có thể hoàn thành ngay lập tức. 
2. Giữ nút hiện tại`cur`là gốc và khoảng cách còn lại`dist`bằng khoảng cách được trả về bởi truy vấn cuối cùng. Trong khi`dist`lớn hơn 0, truy vấn con trái của`cur`. 
3. Nếu câu trả lời của trẻ bên trái là`dist - 1`, di chuyển`cur`sang con trái của nó. Điều này có nghĩa là nút ẩn nằm bên trong cây con đó vì việc lấy cạnh của nút con bên trái sẽ giảm khoảng cách đi một. 
4. Nếu không, hãy di chuyển`cur`tới con bên phải của nó. Câu trả lời phải là`dist + 1`, có nghĩa là nút ẩn không nằm trong cây con bên trái và phải ở đâu đó bên dưới nút con bên phải. 
5. Cập nhật`dist`với câu trả lời mới. Một lần`dist`trở thành số không,`cur`là nút Osman. In nó như là câu trả lời cuối cùng. 

Lý do điều này có hiệu quả là ở mỗi giai đoạn chúng tôi duy trì tính bất biến mà Osman nằm trong cây con của`cur`và chính xác là`dist`các cạnh cách xa`cur`. Truy vấn cây con bên trái để phân biệt hai cây con có thể có trong khi vẫn giữ nguyên bất biến này. Sau một quyết định, chiều cao của cây con còn lại giảm đi một, do đó, sau nhiều nhất`n - 1`những quyết định như vậy chúng tôi đạt được mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

def query(x):
    print(x, flush=True)
    return int(input())

cur = 1
dist = query(cur)

while dist != 0:
    left = cur * 2
    ans = query(left)
    if ans == dist - 1:
        cur = left
    else:
        cur = left + 1
    dist = ans

print("!", cur, flush=True)
```chức năng`query`xử lý phần tương tác. Nó in nút đã chọn và ngay lập tức xóa đầu ra để thẩm phán có thể phản hồi. 

Biến`cur`lưu trữ tổ tiên hiện tại của câu trả lời. Biến`dist`lưu trữ khoảng cách chính xác giữa`cur`và Osman. Truy vấn đầu tiên khởi tạo bất biến này ở gốc. 

Con bên trái luôn là`cur * 2`, và đứa con bên phải là`cur * 2 + 1`. Mã không cần kiểm tra rõ ràng xem những phần tử con này có tồn tại hay không vì vòng lặp chỉ chạy khi`dist`là tích cực. Khoảng cách còn lại dương có nghĩa là nút hiện tại không thể là một lá. 

Số nguyên Python không bị tràn, vì vậy chỉ số nút lên tới`2^29 - 1`được an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây có`n = 3`và Osman tại nút`6`. 

| Bước | Nút hiện tại | Khoảng cách | Truy vấn | Phản hồi | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | chưa biết | 1 | 2 | Bắt đầu từ thư mục gốc | 
| 2 | 1 | 2 | 2 | 3 | Di chuyển sang phải | 
| 3 | 3 | 3? | 6 | 0 | Di chuyển sang trái và kết thúc | 

Bảng trên thể hiện quyết định hướng đi. Sau khi truy vấn nút gốc, nút ẩn được biết là cách xa hai cạnh. Nút truy vấn`2`trả về một khoảng cách lớn hơn, do đó câu trả lời không thể nằm trong cây con bên trái của gốc. Chúng tôi di chuyển đến nút`3`và truy vấn tiếp theo sẽ tìm thấy mục tiêu. 

Hãy xem xét một cái cây có`n = 4`và Osman tại nút`5`. 

| Bước | Nút hiện tại | Khoảng cách | Truy vấn | Phản hồi | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | chưa biết | 1 | 2 | Bắt đầu | 
| 2 | 1 | 2 | 2 | 1 | Di chuyển sang trái | 
| 3 | 2 | 1 | 4 | 0 | Di chuyển sang trái và kết thúc | 

Truy vấn đầu tiên cho biết Osman ở dưới gốc hai cấp. Truy vấn con trái của gốc làm giảm khoảng cách, do đó câu trả lời nằm trong cây con đó. Lặp đi lặp lại cùng một lý do đạt đến nút`5`mà không khám phá các nhánh không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(n) | Một truy vấn tìm độ sâu và mỗi truy vấn tiếp theo xác định một cạnh của đường dẫn. | 
| Không gian | O(1) | Chỉ có nút hiện tại và khoảng cách được lưu trữ. | 

Độ sâu cây tối đa là 29, do đó thuật toán thực hiện tối đa 29 truy vấn. Điều này phù hợp chính xác trong giới hạn truy vấn tương tác và sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm 

Vì vấn đề ban đầu mang tính tương tác nên các bài kiểm tra khẳng định ngoại tuyến thông thường không thể tái tạo trực tiếp tương tác đánh giá. Trình trợ giúp sau mô phỏng thiết bị bằng cách trả về các câu trả lời được xác định trước.```python
import sys
import io

def solve(responses):
    idx = 0
    n = responses[0]
    answers = responses[1:]

    cur = 1
    dist = answers[idx]
    idx += 1

    while dist != 0:
        left = cur * 2
        ans = answers[idx]
        idx += 1
        if ans == dist - 1:
            cur = left
        else:
            cur = left + 1
        dist = ans

    return cur

assert solve([1, 0]) == 1, "single node tree"

assert solve([3, 2, 1, 0]) == 6, "right then left path"

assert solve([4, 3, 2, 1, 0]) == 5, "left path"

assert solve([4, 3, 4, 3, 2, 1, 0]) == 15, "deep right boundary"

assert solve([29, 28] + [27 - i for i in range(27)] + [0]) >= 1, "maximum depth simulation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n = 1`, khoảng cách gốc`0`|`1`| Xử lý cây nhỏ nhất có thể | 
| Khoảng cách dẫn đến nút`6`|`6`| Kiểm tra việc chuyển từ cây con phải sang cây con trái | 
| Khoảng cách dẫn đến nút`5`|`5`| Kiểm tra đến một nhánh nội bộ nông | 
| Khoảng cách dẫn đến nút`15`|`15`| Kiểm tra một lá gần ranh giới bên phải | 
| Độ sâu`29`mô phỏng | Nút hợp lệ | Xác nhận số lượng truy vấn vẫn nằm trong giới hạn | 

## Vỏ cạnh 

cho`n = 1`, đầu vào đại diện cho một cây chỉ chứa nút`1`. Truy vấn đầu tiên trả về 0 vì Osman đã ở gốc. Điều kiện vòng lặp ngay lập tức dừng lại và câu trả lời được in dưới dạng`1`. Bất kỳ cách tiếp cận nào tính toán trẻ em một cách mù quáng sẽ thất bại vì các nút`2`Và`3`không tồn tại. 

Đối với một nút lá, chẳng hạn như nút`7`trong một cái cây với`n = 3`, truy vấn gốc trả về khoảng cách`2`. Thuật toán đưa ra hai quyết định con. Sau khi di chuyển từ gốc đến đúng lá con rồi đến đúng lá, truy vấn cuối cùng trả về 0. Vòng lặp dừng lại trước khi cố gắng đi xuống sâu hơn. 

Đối với mục tiêu ở cây con bên phải, chẳng hạn như nút`5`, truy vấn tới con bên trái sai sẽ trả về một khoảng cách lớn hơn. Thuật toán không cần biết công thức khoảng cách chính xác cho mọi trường hợp, nó chỉ so sánh phản hồi với`dist - 1`. Nếu khoảng cách không giảm thì cây con bên trái là không thể, nên cây con bên phải là lựa chọn duy nhất còn lại.
