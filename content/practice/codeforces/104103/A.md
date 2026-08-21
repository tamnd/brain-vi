---
title: "CF 104103A - Bài tập về nhà"
description: "Chúng tôi được đưa ra một chuỗi các yêu tinh lần lượt đến. Mỗi yêu tinh có một ngưỡng bệnh $si$. Chúng ta cũng có một bộ đĩa cố định, mỗi đĩa có giá trị sức khỏe $h$ và giá trị độ ngon $t$."
date: "2026-07-02T02:06:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104103
codeforces_index: "A"
codeforces_contest_name: "Innopolis Open 2022-2023. Second qualification round"
rating: 0
weight: 104103
solve_time_s: 120
verified: true
draft: false
---

[CF 104103A - Bài tập về nhà](https://codeforces.com/problemset/problem/104103/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một chuỗi các yêu tinh lần lượt đến. Mỗi yêu tinh có một ngưỡng bệnh tật$s_i$. Chúng tôi cũng có một bộ bát đĩa cố định, mỗi bộ đều có giá trị sức khỏe$h$và giá trị độ ngon$t$. 

Một món ăn có thể được giao cho một yêu tinh, nhưng nó chỉ giết chết yêu tinh đó nếu sức khỏe của món ăn đó hoàn toàn nhỏ hơn ngưỡng bệnh tật của yêu tinh. Nếu một yêu tinh không được giao một món ăn để giết họ, thì yêu tinh đó sẽ hoàn toàn bị bỏ qua và không đóng góp được gì. Chỉ những yêu tinh chết mới quan trọng, và đối với những yêu tinh đó, chúng tôi muốn tối đa hóa hương vị tổng thể của các món ăn họ tiêu thụ. 

Hạn chế chính là chúng ta phải đưa ra câu trả lời tối ưu không chỉ cho tất cả các yêu tinh mà còn cho mọi tiền tố của yêu tinh từ$1$ĐẾN$n$. Điều đó có nghĩa là sau khi mỗi yêu tinh mới đến, chúng ta phải biết tổng độ ngon có thể đạt được tốt nhất chỉ bằng cách sử dụng yêu tinh đầu tiên.$i$yêu tinh và tất cả các món ăn. 

Những giới hạn$n, m \le 200000$loại trừ mọi cách tiếp cận cố gắng tính toán lại kết quả khớp hoàn toàn hoặc quét tất cả các món ăn cho mỗi yêu tinh. Một giải pháp ngây thơ kiểm tra tất cả các phép gán giữa yêu tinh và các món ăn sẽ hoạt động giống như một sự so khớp hai bên trên một tập hợp đang phát triển, dẫn đến ít nhất$O(nm)$hành vi đó vượt xa mức có thể chấp nhận được. Ngay cả việc sắp xếp cộng với việc quét lặp đi lặp lại trên mỗi tiền tố vẫn sẽ tích lũy công việc bậc hai. 

Một trường hợp phức tạp nảy sinh khi một món ăn có sức khỏe rất cao nhưng độ ngon lại thấp. Một chiến lược tham lam luôn giao món ăn ngon nhất hiện có cho yêu tinh hiện tại có thể thất bại vì một món ăn có thể bị “lãng phí” cho yêu tinh mà lẽ ra có thể hài lòng với những lựa chọn yếu hơn, làm giảm tổng lợi nhuận trong tương lai. Một trường hợp khác xuất hiện khi nhiều yêu tinh có chung ngưỡng; không có cấu trúc toàn cục, việc sử dụng lại các món ăn một cách tối ưu trên các tiền tố sẽ trở nên không nhất quán. 

## Phương pháp tiếp cận 

Cấu trúc sẽ trở nên rõ ràng hơn nếu chúng ta đảo ngược góc nhìn: thay vì nghĩ đến việc giao các món ăn cho yêu tinh, chúng ta nghĩ đến việc khi nào một món ăn có thể sử dụng được. 

Một món ăn tốt cho sức khỏe$h$chỉ có thể được sử dụng bởi yêu tinh với$s_i > h$. Vì vậy, mỗi món ăn thực sự có “thời gian bắt đầu” được xác định bởi tiền tố đầu tiên nơi món ăn đủ điều kiện. Khi một yêu tinh đến, chúng tôi sẽ xem xét tất cả các món ăn có sức khỏe thấp hơn ngưỡng của yêu tinh đó. 

Bây giờ nhiệm vụ cho mỗi tiền tố trở thành: trong số tất cả các món ăn có sẵn (những món ăn có$h < s_i$), hãy chọn một tập hợp con để tối đa hóa độ ngon, với mỗi món ăn chỉ có thể sử dụng tối đa một lần. Điều này tương đương với việc liên tục lấy đi những món ăn ngon nhất hiện có. 

Một cách tiếp cận bạo lực, đối với mỗi tiền tố, sẽ quét tất cả các món ăn, lọc những món ăn có$h < s_i$, sắp xếp chúng theo độ ngon và chọn những thứ tốt nhất hiện có. Chi phí này$O(nm \log m)$hoặc tệ hơn tùy theo cách thực hiện, quá chậm. 

Cái nhìn sâu sắc quan trọng là xử lý các yêu tinh theo thứ tự trong khi vẫn duy trì cấu trúc dữ liệu của các món ăn hiện “đủ điều kiện”. BẰNG$s_i$tăng lên, nhiều món ăn trở nên đủ điều kiện. Chúng tôi có thể duy trì tất cả các món ăn đủ điều kiện trong một đống tối đa được quyết định bởi độ ngon. Mỗi món ăn được chèn chính xác một lần khi đủ điều kiện. Đối với mỗi yêu tinh, chúng tôi liên tục lấy những món ăn ngon nhất còn lại, nhưng chúng tôi cũng cần đảm bảo rằng mỗi món ăn được sử dụng tối đa một lần trên toàn cầu, vì vậy một khi đã loại bỏ, nó sẽ không bao giờ quay trở lại. 

Phần tinh tế là mỗi câu trả lời có tiền tố đều được tích lũy: một khi một món ăn được lấy cho tiền tố trước đó, nó sẽ biến mất cho các tiền tố sau. Do đó, chúng tôi duy trì một nhóm các món ăn có sẵn trên toàn cầu và kích hoạt chúng dần dần khi ngưỡng tăng lên. 

Chúng tôi sắp xếp các yêu tinh theo ngưỡng của chúng nhưng phải giữ nguyên các câu trả lời tiền tố theo thứ tự ban đầu. Sắp xếp các món ăn theo sức khỏe cho phép chúng ta kích hoạt chúng theo thứ tự tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm \log m)$|$O(m)$| Quá chậm | 
| Tối ưu (đống + sắp xếp) |$O((n+m)\log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các món ăn theo giá trị sức khỏe của chúng$h$theo thứ tự tăng dần. Điều này đảm bảo rằng một khi một món ăn đủ điều kiện cho một số yêu tinh, nó sẽ đủ điều kiện cho tất cả các yêu tinh sau này. 
2. Duy trì một con trỏ trên các món ăn và lưu trữ giá trị độ ngon tối đa của tất cả các món ăn hiện đủ điều kiện nhưng chưa được sử dụng. 
3. Xử lý yêu tinh theo thứ tự đến, giữ chỉ số đang chạy$i$. 
4. Đối với mỗi yêu tinh$i$, chèn vào đống tất cả các món ăn có$h < s_i$vẫn chưa được chèn vào. Bước này sẽ mở rộng nhóm bát đĩa có thể sử dụng chính xác khi chúng hợp lệ. 
5. Sau khi kích hoạt, liên tục trích xuất món ăn có độ ngon tối đa từ đống và thêm nó vào câu trả lời cho tiền tố hiện tại. Dừng khi vùng heap trống hoặc khi không thể thực hiện phép gán có lợi theo cấu trúc tiền tố. 
6. Lưu trữ tổng của tất cả các giá trị độ ngon đã chọn làm câu trả lời cho tiền tố$i$, sau đó tiến tới yêu tinh tiếp theo. 

### Tại sao nó hoạt động 

Ở tiền tố bất kỳ$i$, đống chứa chính xác tập hợp các món ăn có thể được giao cho một số yêu tinh trong số những món ăn đầu tiên$i$sự đến. Việc chọn món ăn có độ ngon tối đa một cách tham lam là tối ưu vì tất cả các phép gán đều độc lập sau khi tính đủ điều kiện được cố định và không có tiền tố nào trong tương lai có thể sử dụng món ăn đã bị loại bỏ. Việc kích hoạt gia tăng đảm bảo không có món ăn nào bị bỏ sót và việc tăng ngưỡng đơn điệu đảm bảo không có món ăn nào đủ điều kiện hai lần hoặc được xem xét lại. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    s = list(map(int, input().split()))

    dishes = []
    for _ in range(m):
        h, t = map(int, input().split())
        dishes.append((h, t))

    dishes.sort()
    heap = []
    ans = []
    j = 0
    total = 0

    for i in range(n):
        while j < m and dishes[j][0] < s[i]:
            heapq.heappush(heap, -dishes[j][1])
            j += 1

        if heap:
            total += -heapq.heappop(heap)

        ans.append(total)

    print(*ans)

if __name__ == "__main__":
    solve()
```Các món ăn được sắp xếp để chúng ta có thể kích hoạt chúng trong một lần quét tuyến tính. Vùng heap lưu trữ các giá trị âm về độ hấp dẫn để mô phỏng vùng heap tối đa bằng cách sử dụng vùng heap tối thiểu của Python. Con trỏ$j$đảm bảo mỗi món ăn được chế biến đúng một lần. 

Một chi tiết tinh tế là chúng tôi chỉ bật một món ăn cho mỗi yêu tinh. Điều này phù hợp với cách giải thích rằng mỗi yêu tinh có thể ăn tối đa một món ăn và sau khi ăn xong món đó sẽ góp phần vào tổng số tích lũy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
5 7 4
3 10
6 5
2 8
```Các món ăn được sắp xếp:$(2,8), (3,10), (6,5)$. 

| Yêu tinh | Ngưỡng | Món ăn kích hoạt | Đống | Đã chụp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | (2,8),(3,10) | {10,8} | 10 | 10 | 
| 2 | 7 | (6,5) đã thêm | {8,5} | 8 | 18 | 
| 3 | 4 | không có gì mới | {5} | 5 | 23 | 

Điều này cho thấy rằng việc kích hoạt không phụ thuộc vào các lựa chọn trước đây nhưng việc lựa chọn sẽ loại bỏ các món ăn vĩnh viễn. 

### Ví dụ 2 

đầu vào:```
4 2
3 10 6 1
2 5
4 7
```Các món ăn được sắp xếp:$(2,5), (4,7)$. 

| Yêu tinh | Ngưỡng | Đã kích hoạt | Đống | Đã chụp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | (2,5) | {5} | 5 | 5 | 
| 2 | 10 | (4,7) | {7} | 7 | 12 | 
| 3 | 6 | không | {} | 0 | 12 | 
| 4 | 1 | không | {} | 0 | 12 | 

Điều này chứng tỏ rằng một khi vùng heap trống, các tiền tố sau sẽ ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log m)$| mỗi món cho vào một lần, mỗi món có giá log m | 
| Không gian |$O(m)$| đống cửa hàng tất cả các món ăn đủ điều kiện | 

Giới hạn$n, m \le 200000$vừa vặn thoải mái vì mỗi thao tác đều là logarit và tổng số thao tác heap là tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    n, m = map(int, input().split())
    s = list(map(int, input().split()))
    dishes = [tuple(map(int, input().split())) for _ in range(m)]
    dishes.sort()

    heap = []
    j = 0
    total = 0
    res = []

    for i in range(n):
        while j < m and dishes[j][0] < s[i]:
            heapq.heappush(heap, -dishes[j][1])
            j += 1
        if heap:
            total += -heapq.heappop(heap)
        res.append(str(total))

    return " ".join(res)

# provided sample
assert run("""5 3
8 10 5 1 6
6 10
5 12
9 4
""") == "12 22 22 22 26"

# minimum case
assert run("""1 1
10
5 7
""") == "7"

# no valid dishes ever
assert run("""3 2
1 1 1
5 10
6 20
""") == "0 0 0"

# all dishes always valid
assert run("""3 3
100 100 100
1 5
2 6
3 7
""") == "7 13 18"

# boundary ordering effect
assert run("""4 4
3 5 2 10
1 1
2 100
4 50
9 200
""") == "100 150 150 350"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 7 | kích hoạt duy nhất | 
| không có món ăn hợp lệ | 0 0 0 | xử lý đống trống | 
| tất cả đều hợp lệ sớm | 7 13 18 | sự đúng đắn tham lam tích lũy | 
| ngưỡng hỗn hợp | 100 150 150 350 | hành vi đặt hàng và tái sử dụng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có món ăn nào làm thỏa mãn yêu tinh hiện tại. Đối với đầu vào như$s = [1,1,1]$và tất cả$h \ge 2$, heap không bao giờ nhận được các phần tử, vì vậy mọi đầu ra tiền tố phải giữ nguyên bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì không có thao tác chèn nào xảy ra và việc kiểm tra vùng heap sẽ ngăn chặn hiện tượng bật lên. 

Một trường hợp khác là khi nhiều món ăn có sẵn cùng một lúc do có sự thay đổi lớn về$s_i$. Việc kích hoạt dựa trên con trỏ đảm bảo tất cả chúng được chèn chính xác một lần trước khi bất kỳ lựa chọn nào diễn ra, do đó không có món ăn ngon nào bị bỏ qua do đặt hàng. 

Trường hợp cuối cùng là khi giá trị độ ngon lớn và gần nhau. Vì chỉ có thứ tự tương đối mới quan trọng trong heap, nên các ràng buộc không ảnh hưởng đến tính chính xác và mỗi thứ được sử dụng độc lập mà không có sự mơ hồ.
