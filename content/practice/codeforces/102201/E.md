---
title: "CF 102201E - Ăn tiết kiệm"
description: "Có chính xác (2N) menu riêng biệt. Thực đơn (i) có giá bữa trưa (li) và giá bữa tối (di). Đối với một chuyến đi kéo dài (k) ngày, chúng ta phải chọn (k) thực đơn khác nhau cho bữa trưa và (k) thực đơn khác cho bữa tối, không được phép xuất hiện thực đơn nào ở cả hai nhóm."
date: "2026-08-18T20:43:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102201
codeforces_index: "E"
codeforces_contest_name: "Moscow Pre-Finals Workshop 2019. KAIST Contest"
rating: 0
weight: 102201
solve_time_s: 272
verified: true
draft: false
---

[CF 102201E - Ăn uống tiết kiệm](https://codeforces.com/problemset/problem/102201/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 32 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có chính xác (2N) menu riêng biệt. Thực đơn (i) có giá bữa trưa (l_i) và giá bữa tối (d_i). Đối với một chuyến đi kéo dài (k) ngày, chúng ta phải chọn (k) thực đơn khác nhau cho bữa trưa và (k) thực đơn khác cho bữa tối, không được phép xuất hiện thực đơn nào ở cả hai nhóm. Mục tiêu là giảm thiểu tổng của tất cả (2k) giá. Chúng ta cần mức tối thiểu này cho mọi (k=1,\dots,N). 

Một cách hữu ích để xem giải pháp từng phần là chia mọi menu thành ba trạng thái. Hiện tại nó có thể không được sử dụng, được giao cho bữa trưa hoặc được giao cho bữa tối. Chuyển thực đơn từ chưa sử dụng sang chi phí bữa trưa (l_i), đồng thời chuyển thực đơn từ chưa sử dụng sang chi phí bữa tối (d_i). Việc chuyển thực đơn bữa tối đã chọn sang bữa trưa sẽ thay đổi chi phí theo (l_i-d_i) và sự thay đổi tương tự từ chi phí bữa trưa sang bữa tối (d_i-l_i). 

Giới hạn (N\le 250000) nghĩa là có thể có (500000) menu. Thuật toán (O(N^2)) sẽ yêu cầu khoảng (6,25\times10^{10}) lần lặp cơ bản ở giới hạn trên, vượt xa giới hạn 3 giây cho phép. Về cơ bản chúng ta cần công việc (O(N\log N)) hoặc (O(N)). Giá có thể lớn tới (10^9) và có thể có (500000) bữa ăn được chọn trong câu trả lời cuối cùng, do đó tổng số có thể lên tới (5\times10^{14}). Số nguyên Python tự động xử lý việc này, trong khi việc triển khai C++ cần số nguyên 64 bit. 

Trường hợp khó khăn đầu tiên là khi bữa trưa rẻ nhất và bữa tối rẻ nhất có cùng một thực đơn. Ví dụ,```
1
1 100
2 2
```Câu trả lời đúng là`3`, bởi vì thực đơn đầu tiên có thể là bữa trưa cho 1 người và thực đơn thứ hai phải là bữa tối cho 2 người. Đơn giản chỉ cần lấy bữa trưa rẻ nhất và bữa tối rẻ nhất một cách độc lập (1+100=101), vi phạm điều kiện không tái sử dụng. 

Một trường hợp tinh tế khác là khi thay thế một menu đã được chọn sẽ rẻ hơn so với việc lấy menu rẻ nhất hiện chưa được sử dụng. Ví dụ,```
2
1 100
100 1
2 1000
3 1000
```Câu trả lời đầu tiên là (2), sử dụng thực đơn đầu tiên cho bữa trưa và thực đơn thứ hai cho bữa tối. Đối với ngày thứ hai, giá bữa tối còn lại đều là 1000, nhưng chúng ta có thể chuyển thực đơn đầu tiên từ bữa trưa sang bữa tối, tăng phần đóng góp của nó lên (100-1=99) và sử dụng thực đơn có giá bữa trưa 3 cho bữa trưa. Do đó, câu trả lời thứ hai là (2+99+3=104) nếu sự sắp xếp trực tiếp đó được xem xét từ trạng thái đầu tiên, nhưng trạng thái tốt nhất thực sự đạt được bằng cách lần đầu tiên dùng thực đơn 3 làm bữa trưa, cho tổng cộng (2+2+102=106). Điều này minh họa tại sao mỗi lần tăng cường phải so sánh một lựa chọn trực tiếp với một lựa chọn hoán đổi thay vì luôn lấy menu chưa sử dụng rẻ nhất. 

Đầu vào tối thiểu cũng cần xử lý đặc biệt vì không thể hoán đổi trước khi bất kỳ thứ gì được chọn. Vì```
1
4 9
5 3
```cặp hợp lệ duy nhất sử dụng các menu khác nhau, vì vậy câu trả lời là`7`. 

Cuối cùng, giá bằng nhau không phải là một trường hợp toán học đặc biệt, nhưng chúng rất hữu ích trong việc phát hiện các lỗi cập nhật trạng thái. Với```
2
5 5
5 5
5 5
5 5
```câu trả lời là`10`Và`20`. Bất kỳ triển khai nào vô tình cho phép thực đơn vẫn có sẵn trong cả đống bữa trưa và bữa tối đều có thể tạo ra giá trị không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là liệt kê các nhiệm vụ bữa trưa và bữa tối có thể có cho mỗi (k), từ chối các nhiệm vụ sử dụng lại thực đơn và tính toán chi phí của chúng. Đối với (k=N), việc chọn suất ăn trưa đã mang lại khả năng (\binom{2N}{N}), vì khi đó suất ăn tối buộc phải là phần bổ sung của nó. Việc đánh giá từng khả năng cần có công việc (\Theta(N)), do đó, giá trị duy nhất của (k) này yêu cầu các thao tác (\Theta(N\binom{2N}{N})). Ngay cả ở (N=20), (\binom{40}{20}=137846528820), cũng đã quá lớn. Tại (N=250000), việc liệt kê là không khả thi. 

Tối ưu hóa tự nhiên là ngừng suy nghĩ về mỗi (k) như một vấn đề hoàn toàn riêng biệt. Giả sử chúng ta đã có giải pháp tối ưu với (k-1) bữa trưa và (k-1) bữa tối. Chúng tôi muốn tăng cả hai số lượng lên một. Đây chính xác là một vấn đề về dòng chi phí tối thiểu gia tăng. Hướng dẫn cuộc thi mô tả công thức tương tự thông qua các đường tăng cường ngắn nhất liên tiếp. 

Hãy tưởng tượng một mạng luồng có nguồn được kết nối với mọi menu, mọi menu được kết nối với các nút bữa trưa và bữa tối và hai nút đó được kết nối với bồn rửa. Một thực đơn chỉ có thể mang một đơn vị dòng chảy, vì vậy nó không thể đồng thời là bữa trưa và bữa tối. Chi phí cho bữa trưa (l_i) và chi phí cho bữa tối (d_i). 

Giả sử chúng ta hiện đang tăng số lượng bữa trưa lên một. Chỉ có hai hình dạng có thể có cho đường dẫn tăng cường hữu ích. Đầu tiên là lấy trực tiếp thực đơn chưa sử dụng cho bữa trưa, trả tiền (l_i). Thứ hai là lấy thực đơn chưa sử dụng cho bữa tối và chuyển thực đơn bữa tối đã chọn hiện tại sang bữa trưa. Nếu thực đơn (x) được chọn cho bữa tối, hãy thay đổi thành chi phí bữa trưa (l_x-d_x). Do đó, chi phí của con đường thứ hai 

[ 
d_y+(l_x-d_x), 
] 

trong đó (y) là một menu không được sử dụng. 

Không có hình dạng đường dẫn hữu ích thứ ba. Sau khi vào nút ăn tối, cách duy nhất để kết thúc bữa trưa là sử dụng cạnh ngược của thực đơn bữa tối đã chọn. Tuyến đường dài hơn sẽ truy cập lại một trong hai nút danh mục và sẽ chứa một chu trình có thể tháo rời. 

Lập luận tương tự cũng được áp dụng khi tăng số lượng bữa tối. Chúng tôi hoặc lấy trực tiếp thực đơn chưa sử dụng rẻ nhất cho bữa tối hoặc chuyển một thực đơn bữa trưa đã chọn sang bữa tối và đồng thời lấy thực đơn chưa sử dụng cho bữa trưa. 

Quan sát này làm giảm toàn bộ vấn đề để duy trì bốn hàng đợi ưu tiên. Phần đầu tiên chứa các thực đơn chưa sử dụng được sắp xếp theo giá bữa trưa. Phần thứ hai chứa các thực đơn chưa sử dụng được sắp xếp theo giá bữa tối. Phần thứ ba chứa các thực đơn bữa tối được chọn lọc theo thứ tự (l_i-d_i), vì đó là chi phí để chuyển một thực đơn từ bữa tối sang bữa trưa. Phần thứ tư chứa các thực đơn bữa trưa được chọn lọc theo thứ tự (d_i-l_i), vì đó là chi phí để chuyển một thực đơn từ bữa trưa sang bữa tối. 

Mỗi lần tăng cường chọn đường tăng cường rẻ hơn trong số hai đường tăng cường có thể có của nó, thực hiện các thay đổi trạng thái tương ứng và tiếp tục. Vì mỗi bước là một đường tăng cường ngắn nhất từ ​​một luồng đã tối ưu nên trạng thái kết quả vẫn là tối ưu. Bốn đống cho phép chúng ta tìm từng ứng cử viên trong thời gian (O(\log N)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(N\binom{2N}{N})) chỉ dành cho (k=N) | Hàm mũ | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ giá bữa trưa, giá bữa tối và trạng thái hiện tại của mỗi thực đơn. Trạng thái sử dụng`0`vì chưa sử dụng,`1`cho bữa trưa, và`2`cho bữa tối. Mảng trạng thái cũng là thứ khiến cho việc xóa lười biếng khỏi đống có thể xảy ra. 
2. Đặt mọi thực đơn vào một đống bữa trưa chưa sử dụng được khóa bởi (l_i) và một đống bữa tối chưa sử dụng được khóa bởi (d_i). Ban đầu cả hai đống đều chứa mọi menu vì không có gì được chọn. 
3. Duy trì một đống các thực đơn bữa tối đã chọn được khóa bởi (l_i-d_i). Nếu thực đơn như vậy được thay đổi từ bữa tối sang bữa trưa thì đây chính xác là sự thay đổi chi phí của nó. Duy trì một đống khác cho các thực đơn bữa trưa đã chọn được khóa bởi (d_i-l_i) cho hoạt động đối xứng. 
4. Với mỗi (k) từ (1) đến (N), trước tiên hãy tăng số lượng thực đơn bữa trưa cần thiết từ (k-1) lên (k). Hãy để (u) là thực đơn bữa trưa rẻ nhất hiện chưa được sử dụng. Đặt (v) là thực đơn bữa tối được chọn với mức tối thiểu (l_v-d_v) và gọi (w) là thực đơn bữa tối rẻ nhất hiện chưa được sử dụng. Hai chi phí có thể xảy ra là (l_u) cho lựa chọn trực tiếp và ((l_v-d_v)+d_w) cho hoán đổi. Chọn cái nhỏ hơn. 
5. Nếu đường ăn trưa trực tiếp thắng, hãy chuyển (u) từ không sử dụng sang bữa trưa và thêm (l_u) vào tổng số. Nếu con đường hoán đổi thắng, hãy chuyển (v) từ bữa tối sang bữa trưa, chuyển (w) từ không sử dụng sang bữa tối và thêm ((l_v-d_v)+d_w) vào tổng số. 
6. Bây giờ hãy tăng số lượng thực đơn bữa tối cần thiết từ (k-1) lên (k). Hãy để (u) là thực đơn bữa tối rẻ nhất chưa sử dụng. Gọi (v) là thực đơn bữa trưa được chọn với giá trị tối thiểu (d_v-l_v) và gọi (w) là thực đơn bữa trưa rẻ nhất chưa sử dụng. Chi phí ứng viên trực tiếp (d_u), trong khi chi phí ứng viên hoán đổi ((d_v-l_v)+l_w). Một lần nữa chọn con đường nhỏ hơn. 
7. Cập nhật các trạng thái và đống hoán đổi thích hợp tùy theo mức tăng bữa tối đã chọn. Sau cả hai lần tăng cường, chính xác (k) menu ở trạng thái bữa trưa và chính xác (k) ở trạng thái bữa tối. Ghi lại chi phí tích lũy làm câu trả lời cho (k). 
8. Sử dụng tính năng xóa lười trong tất cả các đống. Khi một menu thay đổi trạng thái, các mục heap cũ sẽ không bị xóa về mặt vật lý. Trước khi đọc giá trị tối thiểu của vùng nhớ heap, hãy liên tục loại bỏ mục nhập trên cùng của nó nếu menu của nó không còn ở trạng thái được biểu thị bởi vùng nhớ đó nữa. Mỗi mục nhập cũ sẽ bị xóa nhiều nhất một lần, do đó tổng công việc của đống vẫn giữ nguyên tuyến tính theo số lượng mục nhập được chèn. 

### Tại sao nó hoạt động 

Điều bất biến là sau mỗi lần tăng thêm, nhiệm vụ hiện tại là nhiệm vụ có chi phí tối thiểu cho số bữa trưa và bữa tối hiện tại của nó. Trong mạng luồng dư, bất kỳ sự gia tăng nào làm tăng số lượng bữa trưa lên một đều có chính xác một trong hai hình thức được thuật toán xem xét: chọn thực đơn chưa sử dụng cho bữa trưa hoặc lấy thực đơn chưa sử dụng cho bữa tối trong khi đảo ngược thực đơn bữa tối đã chọn thành bữa trưa. Thuật toán chọn đường tăng cường ngắn nhất rẻ hơn. Tuyên bố tương tự cũng có tính đối xứng đối với việc tăng cường bữa tối. Bắt đầu từ luồng tối ưu trống, các lần tăng ngắn nhất liên tiếp sẽ duy trì tính tối ưu, do đó, sau lần tăng bữa trưa và bữa tối cho (k), trạng thái tối ưu cho chính xác (k) bữa trưa và (k) bữa tối. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def compute(n, menus):
    m = 2 * n

    lunch = [0] * m
    dinner = [0] * m

    for i, (l, d) in enumerate(menus):
        lunch[i] = l
        dinner[i] = d

    # 0 = unused, 1 = lunch, 2 = dinner
    state = [0] * m

    # Unused menus.
    unused_lunch = [(lunch[i], i) for i in range(m)]
    unused_dinner = [(dinner[i], i) for i in range(m)]
    heapq.heapify(unused_lunch)
    heapq.heapify(unused_dinner)

    # Selected dinner, ordered by cost of changing dinner -> lunch.
    dinner_to_lunch = []

    # Selected lunch, ordered by cost of changing lunch -> dinner.
    lunch_to_dinner = []

    def clean(heap, wanted_state):
        while heap and state[heap[0][1]] != wanted_state:
            heapq.heappop(heap)
        return heap[0] if heap else None

    def move_unused_to_lunch(i):
        state[i] = 1
        heapq.heappush(lunch_to_dinner, (dinner[i] - lunch[i], i))

    def move_unused_to_dinner(i):
        state[i] = 2
        heapq.heappush(dinner_to_lunch, (lunch[i] - dinner[i], i))

    def move_dinner_to_lunch(i):
        state[i] = 1
        heapq.heappush(lunch_to_dinner, (dinner[i] - lunch[i], i))

    def move_lunch_to_dinner(i):
        state[i] = 2
        heapq.heappush(dinner_to_lunch, (lunch[i] - dinner[i], i))

    total = 0
    answer = []

    for _ in range(n):
        # Increase the lunch count by one.
        direct = clean(unused_lunch, 0)
        swap_menu = clean(dinner_to_lunch, 2)
        replacement = clean(unused_dinner, 0)

        direct_cost = direct[0] if direct is not None else 10**30

        if swap_menu is not None and replacement is not None:
            swap_cost = swap_menu[0] + replacement[0]
        else:
            swap_cost = 10**30

        if direct_cost <= swap_cost:
            i = direct[1]
            total += direct_cost
            move_unused_to_lunch(i)
        else:
            old_dinner = swap_menu[1]
            new_dinner = replacement[1]

            total += swap_cost
            move_dinner_to_lunch(old_dinner)
            move_unused_to_dinner(new_dinner)

        # Increase the dinner count by one.
        direct = clean(unused_dinner, 0)
        swap_menu = clean(lunch_to_dinner, 1)
        replacement = clean(unused_lunch, 0)

        direct_cost = direct[0] if direct is not None else 10**30

        if swap_menu is not None and replacement is not None:
            swap_cost = swap_menu[0] + replacement[0]
        else:
            swap_cost = 10**30

        if direct_cost <= swap_cost:
            i = direct[1]
            total += direct_cost
            move_unused_to_dinner(i)
        else:
            old_lunch = swap_menu[1]
            new_lunch = replacement[1]

            total += swap_cost
            move_lunch_to_dinner(old_lunch)
            move_unused_to_lunch(new_lunch)

        answer.append(total)

    return answer

def solve():
    n = int(input())
    menus = [tuple(map(int, input().split())) for _ in range(2 * n)]
    print("\n".join(map(str, compute(n, menus))))

if __name__ == "__main__":
    solve()
```Hai vùng chưa sử dụng được khởi tạo với tất cả các menu. Chúng không bao giờ bị xóa rõ ràng khi một menu được chọn. Thay vào đó, mảng trạng thái cho biết`clean`liệu mục trên cùng vẫn có thể sử dụng được hay không. Điều này tránh việc xóa tùy ý tốn kém khỏi Python`heapq`. 

Khi thực đơn trở thành bữa trưa, giá trị (d_i-l_i) của nó sẽ được chèn vào`lunch_to_dinner`. Khi nó trở thành bữa tối, giá trị (l_i-d_i) của nó được chèn vào`dinner_to_lunch`. Một menu thay đổi danh mục có thể để lại các mục cũ trong đống cũ của nó. Những mục đó sẽ bị loại bỏ khi chúng đạt đến đỉnh. 

Việc tăng cường bữa trưa được thực hiện trước khi tăng cường bữa tối vì hai công suất được tăng lần lượt. Sau lần tăng cường đầu tiên, trạng thái biểu thị một giải pháp tối ưu với (k) bữa trưa và (k-1) bữa tối. Lần tăng thêm thứ hai sau đó tạo ra một giải pháp tối ưu với (k) của mỗi giải pháp. 

Việc so sánh sử dụng`<=`, vì vậy các mối quan hệ luôn chọn con đường trực tiếp. Bất kỳ lựa chọn ràng buộc nào cũng là tối ưu, vì vậy điều này không ảnh hưởng gì đến câu trả lời. 

Tổng chi phí có thể vào khoảng (5\times10^{14}), do đó, nó không thể được lưu trữ an toàn trong số nguyên 32 bit. Các số nguyên chính xác tùy ý của Python giúp việc tích lũy trở nên an toàn. 

Việc thực hiện sử dụng`10**30`như một ứng cử viên không thể truy cập thay vì dựa vào trạng thái heap đặc biệt. Với các ràng buộc đã cho, mọi câu trả lời thực đều nhỏ hơn rất nhiều so với giá trị này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
1
4 9
5 3
```Có hai thực đơn. Ban đầu cả hai đều không được sử dụng. Chúng ta cần một bữa trưa và một bữa tối. 

| Bước | Thay đổi trạng thái | Ứng viên trực tiếp | Trao đổi ứng viên | Giá đã chọn | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ăn trưa | thực đơn 1: chưa sử dụng -> bữa trưa | 4 | không có sẵn | 4 | 4 | 
| Bữa tối | thực đơn 2: chưa sử dụng -> bữa tối | 3 | không có sẵn | 3 | 7 | 

Thực đơn đầu tiên rẻ hơn cho bữa trưa, trong khi thực đơn thứ hai rẻ hơn cho bữa tối sau khi đã dùng hết thực đơn đầu tiên. Câu trả lời là`7`. 

Ví dụ này xác nhận tính bất biến cơ bản và cũng kiểm tra điều kiện ban đầu trong đó không có vùng trao đổi nào chứa bất kỳ thứ gì. 

### Mẫu 2 

Đầu vào là```
2
1 6
2 4
5 3
3 1
```Gọi các menu từ 1 đến 4 theo thứ tự đầu vào. 

| (k) | Tăng cường | Ứng viên trực tiếp | Trao đổi ứng viên | Hành động được chọn | Tổng số chạy | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Ăn trưa | thực đơn 1 giá 1 | không có sẵn | thực đơn 1 -> bữa trưa | 1 | 
| 1 | Bữa tối | thực đơn 4 giá 1 | không có sẵn | thực đơn 4 -> bữa tối | 2 | 
| 2 | Ăn trưa | thực đơn 2, giá 2 | thực đơn 4 -> chi phí ăn trưa (3-1+3=5) | thực đơn 2 -> bữa trưa | 4 | 
| 2 | Bữa tối | thực đơn 3 giá 3 | thực đơn 1 -> chi phí bữa tối (6-1+5=10) | thực đơn 3 -> bữa tối | 7 | 

Sau cặp bổ sung đầu tiên, menu 1 và 4 được chọn. Đối với bữa trưa thứ 2, thực đơn 2 rẻ hơn so với việc đổi thực đơn 4 từ bữa tối sang bữa trưa. Đối với bữa tối thứ 2, thực đơn 3 rẻ hơn so với việc đổi thực đơn 1 từ bữa trưa sang bữa tối. 

Các câu trả lời kết quả là`2`Và`7`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Có (2N) phần mở rộng, mỗi phần thực hiện một số lượng thao tác heap không đổi. Mỗi mục nhập cũ sẽ bị xóa nhiều nhất một lần. | 
| Không gian | (O(N)) | Có (2N) menu và một số mục nhập heap tuyến tính được tạo trong suốt thuật toán. | 

Đối với (N=250000), thuật toán chỉ thực hiện một số lượng thao tác heap không đổi trên mỗi menu, mỗi lần tính giá (O(\log N)). Điều này phù hợp với kích thước đầu vào của menu (500000), trong khi các phương pháp tiếp cận bậc hai hoặc hàm mũ bị loại trừ bởi các ràng buộc. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp trên đã được lưu dưới dạng`solution.py`. Nó kêu giống nhau`compute`được chương trình chính sử dụng, do đó các xác nhận sẽ kiểm tra thuật toán thực tế thay vì triển khai riêng biệt.```python
import io
import sys

from solution import compute

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    menus = []
    p = 1

    for _ in range(2 * n):
        menus.append((data[p], data[p + 1]))
        p += 2

    return "\n".join(map(str, compute(n, menus))) + "\n"

# Sample 1
assert run("""\
1
4 9
5 3
""") == """\
7
""", "sample 1"

# Sample 2
assert run("""\
2
1 6
2 4
5 3
3 1
""") == """\
2
7
""", "sample 2"

# Sample 3
assert run("""\
4
7 5
5 7
7 4
4 2
2 5
6 4
3 2
1 9
""") == """\
3
7
16
26
""", "sample 3"

# Minimum size, and the cheapest lunch and dinner belong to the same menu.
assert run("""\
1
1 100
2 2
""") == """\
3
""", "must not reuse one menu"

# All prices equal.
assert run("""\
2
5 5
5 5
5 5
5 5
""") == """\
10
20
""", "all equal"

# Forces a lunch -> dinner swap on the second dinner augmentation.
assert run("""\
2
1 100
100 1
2 1000
3 1000
""") == """\
2
106
""", "swap is cheaper than direct dinner selection"

# Maximum-size structural test.
# Every menu costs 1 in both roles, so the kth answer is exactly 2*k.
n = 250000
inp = str(n) + "\n" + ("1 1\n" * (2 * n))
expected = "".join(f"{2 * k}\n" for k in range(1, n + 1))
assert run(inp) == expected, "maximum-size all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 100 / 2 2`|`3`| Bữa trưa và bữa tối rẻ nhất không thể sử dụng cùng một thực đơn. | 
|`2 / four menus all 5 5`|`10, 20`| Giá bằng nhau, khóa heap lặp lại và số lượng trạng thái chính xác. | 
|`2 / 1 100, 100 1, 2 1000, 3 1000`|`2, 106`| Việc hoán đổi danh mục có thể rẻ hơn đáng kể so với việc sử dụng menu rẻ nhất chưa sử dụng cho danh mục được yêu cầu. | 
|`250000 / 500000 menus all 1 1`|`2, 4, ..., 500000`| Kích thước đầu vào tối đa, các khóa bằng nhau lặp lại, hiệu suất vùng heap và hành vi biên tại (k=N). | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là đầu vào tối thiểu và không có bất kỳ menu nào được chọn ban đầu. Vì```
1
4 9
5 3
```phần bổ sung bữa trưa không thấy thực đơn bữa tối nào được chọn, vì vậy chỉ tồn tại ứng cử viên trực tiếp có giá 4. Sau khi thực đơn 1 trở thành bữa trưa, phần bổ sung bữa tối sẽ nhìn thấy thực đơn 2 với giá bữa tối 3 và chọn thực đơn đó. Tổng số trở thành 7, đó là đầu ra cần thiết. 

Trường hợp thứ hai là sự va chạm giữa ứng viên ăn bữa trưa rẻ nhất và bữa tối rẻ nhất. Vì```
1
1 100
2 2
```phần bổ sung bữa trưa chọn thực đơn 1 cho giá 1. Sau đó, nhóm bữa tối sẽ bỏ qua thực đơn 1 vì trạng thái của nó là bữa trưa nên thực đơn 2 với giá bữa tối 2 được chọn. Câu trả lời là 3. Một cặp tìm kiếm tối thiểu độc lập sẽ sử dụng sai menu 1 hai lần. 

Trường hợp cạnh thứ ba là một sự hoán đổi đánh bại sự lựa chọn trực tiếp. Vì```
2
1 100
100 1
2 1000
3 1000
```ngày đầu tiên chọn thực đơn 1 cho bữa trưa và thực đơn 2 cho bữa tối, tổng cộng là 2. Vào bữa trưa thứ hai, thực đơn 3 được chọn trực tiếp với chi phí 2. Đối với bữa tối thứ hai, bữa tối rẻ nhất chưa sử dụng có giá 1000. Thay vào đó, thực đơn 1 có thể chuyển từ bữa trưa sang bữa tối với mức tăng (100-1=99), trong khi thực đơn 4 trở thành bữa trưa mới cho 3. Do đó, lần tăng thứ hai có giá (99+3=102), cho tổng số 106. Thuật toán tìm thấy chính xác là cải tiến đường dư này. 

Trường hợp cạnh thứ tư là lần lặp cuối cùng, trong đó mọi menu phải được chọn. Coi như```
2
5 5
5 5
5 5
5 5
```Sau lần lặp đầu tiên, có hai thực đơn đã chọn và hai thực đơn không sử dụng, tổng cộng là 10. Lần bổ sung bữa trưa và bữa tối thứ hai sẽ sử dụng hai thực đơn còn lại, mỗi thực đơn thêm 5 thực đơn. Câu trả lời cuối cùng là 20. Thuật toán không bao giờ cần đọc một menu không sử dụng không tồn tại vì ở đầu cặp thứ (k) có chính xác (2(N-k+1)) menu không sử dụng. 

Trường hợp cạnh thứ năm là số học lớn. Với (N=250000) và mọi mức giá đều bằng (10^9), câu trả lời cuối cùng là (500000\cdot10^9=5\times10^{14}). Logic vùng heap không thay đổi nhưng bộ tích lũy phải hỗ trợ các giá trị lớn hơn nhiều so với (2^{31}-1). Biểu diễn số nguyên của Python xử lý việc này một cách trực tiếp.
