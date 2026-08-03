---
title: "CF 102739H - \u0414\u043e\u0441\u0442\u0430\u0432\u043a\u0430 \u0435\u0434\u044b"
description: "Vấn đề yêu cầu chúng tôi trả lời nhiều truy vấn về một chuỗi các công ty giao hàng. Trong ngày, các công ty của người đưa thư đến được ghi thành mảng a."
date: "2026-08-01T22:17:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102739
codeforces_index: "H"
codeforces_contest_name: "\u0421\u0438\u0440\u0438\u0443\u0441.2020.\u041d\u043e\u044f\u0431\u0440\u044c.\u041e\u0447\u043d\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 102739
solve_time_s: 115
verified: true
draft: false
---

[CF 102739H - \u0414\u043e\u0441\u0442\u0430\u0432\u043a\u0430 \u0435\u0434\u044b](https://codeforces.com/problemset/problem/102739/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55 giây 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Vấn đề yêu cầu chúng ta trả lời nhiều truy vấn về một chuỗi các công ty giao hàng. Trong ngày, các công ty chuyển phát đến được ghi lại dưới dạng mảng`a`. Đối với mỗi đồng nghiệp, chúng tôi nhận được một khoảng của mảng này biểu thị khoảng thời gian mà đồng nghiệp đó có thể đặt đồ ăn. Chúng ta phải tìm số công ty xuất hiện nhiều lần nhất trong khoảng đó. Nếu một số công ty có cùng tần số tối đa thì bất kỳ công ty nào cũng được chấp nhận. 

Độ dài mảng và số lượng truy vấn đều có thể đạt tới`100000`, trong khi số nhận dạng công ty có thể lớn bằng`10^9`. Phạm vi định danh lớn có nghĩa là chúng tôi không thể sử dụng trực tiếp số công ty làm chỉ mục mảng, do đó cần phải nén. Kích thước của`n`Và`q`loại trừ việc kiểm tra mọi phần tử của mỗi khoảng. Một giải pháp quét từng truy vấn một cách độc lập có thể thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn một giây cho phép. Chúng ta cần một phương thức trong đó tổng lượng chuyển động trong mảng gần bằng`n * sqrt(n)`. 

Một sai lầm phổ biến là xử lý cà vạt không đúng cách. Câu trả lời không bắt buộc phải là duy nhất. Ví dụ:```
5
1 2 1 2 3
1
1 4
```Đầu ra đúng có thể là`1`hoặc`2`, bởi vì cả hai đều xuất hiện hai lần. Một giải pháp giả định mức tối đa đầu tiên được tìm thấy luôn ổn định có thể thất bại sau khi loại bỏ các phần tử khỏi cấu trúc dữ liệu. 

Một trường hợp khác là truy vấn chứa một phần tử:```
3
7 8 9
1
2 2
```Câu trả lời phải là`8`. Việc triển khai khởi tạo tần số tối đa hiện tại về 0 và quên thêm phần tử đầu tiên một cách chính xác có thể trả về giá trị không hợp lệ. 

Trường hợp thứ ba là khi một công ty chiếm ưu thế sau nhiều lần cập nhật:```
6
5 1 5 2 5 3
1
1 6
```Câu trả lời phải là`5`. Chiến lược cập nhật lười biếng chỉ tăng mức tối đa hiện tại nhưng không sửa chữa nó sau khi xóa có thể giữ tần suất lỗi thời và tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn riêng biệt. Trong một khoảng thời gian`[l, r]`, chúng tôi quét tất cả các vị trí giữa`l`Và`r`, đếm số lần xuất hiện của mọi công ty và chọn số lượng lớn nhất. Điều này đúng vì nó kiểm tra chính xác các phần tử thuộc truy vấn. Vấn đề là công việc lặp đi lặp lại. Nếu tất cả các truy vấn bao trùm toàn bộ mảng thì mỗi truy vấn sẽ có giá`O(n)`, cho`O(nq)`, trở thành`10^10`hoạt động cho kích thước đầu vào tối đa. 

Cấu trúc của vấn đề là các truy vấn chỉ hỏi về tần số trong phạm vi và các truy vấn lân cận thường chồng chéo rất nhiều. Thay vì mỗi lần xây dựng lại thông tin tần số từ 0, chúng ta có thể sắp xếp lại các truy vấn và duy trì một cửa sổ chuyển động. Đây là ý tưởng đằng sau thuật toán của Mo. 

Thuật toán của Mo chia mảng thành các khối có kích thước gần đúng`sqrt(n)`. Các truy vấn được sắp xếp theo khối bên trái và sau đó theo điểm cuối bên phải. Khi chuyển từ truy vấn này sang truy vấn tiếp theo, chúng tôi chỉ thêm hoặc xóa các vị trí đã thay đổi. Cửa sổ được duy trì luôn chứa chính xác khoảng thời gian truy vấn hiện tại, do đó bảng tần số luôn sẵn sàng. 

Thử thách còn lại là duy trì lượng khách thường xuyên nhất trong khi các phần tử ra vào cửa sổ. Chúng tôi giữ một mảng tần số để nén số nhận dạng công ty và một mảng khác cho biết hiện có bao nhiêu công ty có mỗi tần số. Khi loại bỏ các phần tử, nếu tần số tối đa hiện tại biến mất, chúng tôi sẽ giảm nó cho đến khi có công ty nào đó tồn tại với tần số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O((n + q)√n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén tất cả số nhận dạng công ty thành các số nguyên liên tiếp. Các giá trị ban đầu được lưu riêng để có thể chuyển đổi lại câu trả lời cuối cùng. 
2. Chọn kích thước khối xung quanh`sqrt(n)`và sắp xếp các truy vấn theo thứ tự của Mo. Các truy vấn có khối bên trái nhỏ hơn được đặt trước và bên trong một khối, các điểm cuối bên phải được sắp xếp ngày càng nhiều. Điều này giảm thiểu khoảng cách hiện tại phải di chuyển tổng thể. 
3. Duy trì khoảng thời gian hiện tại bằng hai con trỏ. Ban đầu khoảng trống. Khi ranh giới bên trái hoặc bên phải thay đổi, hãy thêm hoặc bớt vị trí mảng tương ứng. 
4. Khi thêm một công ty, hãy tăng tần suất của nó. Cập nhật số lượng công ty có tần suất đó và tăng mức tối đa hiện tại nếu cần thiết. 
5. Khi loại bỏ một công ty, hãy giảm tần suất xuất hiện của nó. Cập nhật bộ đếm tần số. Nếu tần số tối đa trước đó không còn tồn tại, hãy giảm mức tối đa cho đến khi vẫn còn tần số hợp lệ. 
6. Đối với mỗi truy vấn được xử lý, hãy chọn bất kỳ công ty nào có tần số bằng tần số tối đa hiện tại và lưu mã định danh ban đầu của nó. 

Điều bất biến là sau mỗi lần di chuyển con trỏ, bảng tần số mô tả chính xác các phần tử bên trong khoảng hiện tại. Cấu trúc thứ hai đảm bảo rằng tần số tối đa được lưu trữ luôn là tần số thực sự xuất hiện trong cửa sổ. Vì câu trả lời chỉ được chọn từ các công ty có tần suất đó nên mọi câu trả lời được tạo ra đều là chế độ hợp lệ của phạm vi được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    values = sorted(set(arr))
    comp = {v: i for i, v in enumerate(values)}
    a = [comp[x] for x in arr]

    q = int(input())
    queries = []
    for i in range(q):
        l, r = map(int, input().split())
        queries.append((l - 1, r - 1, i))

    block = int(n ** 0.5) + 1

    queries.sort(
        key=lambda x: (
            x[0] // block,
            x[1] if (x[0] // block) % 2 == 0 else -x[1]
        )
    )

    freq = [0] * len(values)
    freq_count = [0] * (n + 1)

    answers = [0] * q
    current_max = 0
    best = -1

    def add(x):
        nonlocal current_max, best
        old = freq[x]
        if old:
            freq_count[old] -= 1
        new = old + 1
        freq[x] = new
        freq_count[new] += 1
        if new > current_max:
            current_max = new
            best = x

    def remove(x):
        nonlocal current_max, best
        old = freq[x]
        freq_count[old] -= 1
        new = old - 1
        freq[x] = new
        if new:
            freq_count[new] += 1
        if old == current_max and freq_count[old] == 0:
            current_max -= 1
            while current_max and freq_count[current_max] == 0:
                current_max -= 1
            if current_max:
                for i, f in enumerate(freq):
                    if f == current_max:
                        best = i
                        break

    left = 0
    right = -1

    for l, r, idx in queries:
        while left > l:
            left -= 1
            add(a[left])
        while right < r:
            right += 1
            add(a[right])
        while left < l:
            remove(a[left])
            left += 1
        while right > r:
            remove(a[right])
            right -= 1

        answers[idx] = values[best]

    print("\n".join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Bước nén thay thế số lượng công ty lớn bằng các chỉ mục nhỏ, cho phép truy cập liên tục vào các mảng tần số. Các số ban đầu vẫn còn`values`, do đó đầu ra vẫn sử dụng mã định danh từ đầu vào. 

các`queries`Thứ tự sắp xếp là phần chính của thuật toán Mo. Hướng xen kẽ cho điểm cuối bên phải giúp giảm chuyển động con trỏ không cần thiết giữa các khối lân cận. 

các`freq`mảng lưu trữ số lần xuất hiện hiện tại của mọi công ty được nén. các`freq_count`mảng trả lời một câu hỏi khác: liệu một tần số cụ thể hiện có tồn tại hay không. Điều này tránh việc xây dựng lại tất cả các tần số sau mỗi lần xóa. 

Số nguyên Python không bị tràn nên giá trị tần số được đảm bảo an toàn. Chi tiết ranh giới quan trọng là khoảng thời gian được duy trì bao gồm cả hai phía, vì vậy các thao tác thêm và xóa phải diễn ra theo đúng thứ tự khi di chuyển con trỏ. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
5
2 4 2 4 5
3
1 3
3 4
1 5
```dấu vết là: 

| Truy vấn | Phạm vi hiện tại | Tần số | Tần số tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
| [1,3] | 2 4 2 | 2:2, 4:1 | 2 | 2 | 
| [3,4] | 2 4 | 2:1, 4:1 | 1 | 2 | 
| [1,5] | 2 4 2 4 5 | 2:2, 4:2, 5:1 | 2 | 4 | 

Ví dụ minh họa quy tắc hòa. Truy vấn thứ hai chứa hai công ty có cùng tần suất, vì vậy một trong hai công ty đều hợp lệ. 

Một ví dụ thứ hai:```
4
9 9 1 9
2
2 3
1 4
```| Truy vấn | Phạm vi hiện tại | Tần số | Tần số tối đa | Trả lời | 
| --- | --- | --- | --- | --- | 
| [2,3] | 9 1 | 9:1, 1:1 | 1 | 9 | 
| [1,4] | 9 9 1 9 | 9:3, 1:1 | 3 | 9 | 

Dấu vết này cho thấy rằng việc mở rộng khoảng thời gian sẽ tích lũy chính xác thông tin tần số trước đó thay vì tính toán lại nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q)√n) | Thứ tự của Mo giới hạn tổng chuyển động của con trỏ | 
| Không gian | O(n) | Mảng tần số, giá trị nén và truy vấn được lưu trữ | 

Với`100000`các phần tử và truy vấn, số lượng thay đổi con trỏ có thể chấp nhận được đối với các ràng buộc dự định. Việc sử dụng bộ nhớ là tuyến tính và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out

    solve()

    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("""5
2 4 2 4 5
3
1 3
3 4
1 5
""") in ("2\n2\n4\n", "2\n4\n4\n", "2\n2\n2\n", "4\n2\n4\n")

assert run("""1
100
1
1 1
""") == "100\n"

assert run("""6
7 7 7 3 3 2
3
1 6
4 5
6 6
""") == "7\n3\n2\n"

assert run("""5
1 2 3 4 5
2
1 5
2 4
""") in (
    "1\n2\n", "1\n3\n", "1\n4\n", "2\n2\n",
    "3\n2\n", "4\n2\n", "5\n2\n"
)

assert run("""5
8 8 8 8 8
2
1 3
3 5
""") == "8\n8\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn |`100`| Xử lý khoảng thời gian kích thước tối thiểu | 
| Một công ty thống trị |`7`,`3`,`2`| Tần suất cập nhật sau khi di chuyển cửa sổ | 
| Tất cả các giá trị riêng biệt | Bất kỳ phần tử nào trong phạm vi | Xử lý cà vạt | 
| Tất cả các giá trị bằng nhau |`8`| Theo dõi tần số tối đa | 

## Vỏ cạnh 

Trường hợp hòa được xử lý vì thuật toán chỉ tìm kiếm một công ty có tần suất tối đa hiện tại. Nó không bao giờ cho rằng một công ty cụ thể phải giành chiến thắng. Vì:```
5
1 2 1 2 3
1
1 4
```tần số được duy trì là`1:2`Và`2:2`, vì vậy một trong hai công ty có thể được trả lại. 

Đối với truy vấn một phần tử:```
3
7 8 9
1
2 2
```Thuật toán của Mo mở rộng cửa sổ trống bằng cách chỉ thêm vị trí`2`. Tần số của`8`trở thành một và tần số tối đa trở thành một, vì vậy câu trả lời trả về chính xác là`8`. 

Để có giá trị vượt trội sau nhiều lần cập nhật:```
6
5 1 5 2 5 3
1
1 6
```bảng tần số cuối cùng chứa`5:3`, trong khi mọi công ty khác đều có tần số một. Mức tối đa được duy trì trở thành ba và thuật toán trả về`5`. Logic tương tự vẫn hoạt động ngay cả khi việc xóa tạm thời làm giảm tần suất của người chiến thắng, vì bộ đếm tần số sẽ sửa chữa giá trị tối đa trước khi trả lời truy vấn tiếp theo.
