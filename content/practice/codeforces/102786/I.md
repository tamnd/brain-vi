---
title: "CF 102786I - \u041f\u0440\u043e\u0431\u043b\u0435\u043c\u0430 \u0441\u0432\u043e\u0431\u043e\u0434\u043d\u043e\u0433\u043e \u043c\u0435\u0441\u0442\u0430"
description: "Chỉnh sửa Chúng ta có một hàng gồm n hàng hóa, được biểu thị bằng một mảng trọng số. Hàng thay đổi theo thời gian: mỗi truy vấn hoán đổi các phần tử ở hai vị trí. Sau mỗi lần hoán đổi, chúng ta phải xác định xem toàn bộ mảng có được sắp xếp theo thứ tự không giảm hay không."
date: "2026-07-27T19:30:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102786
codeforces_index: "I"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u042f\u0440\u0413\u0423 \u0438\u043c. \u041f.\u0413. \u0414\u0435\u043c\u0438\u0434\u043e\u0432\u0430 Demidov Open IT Cup 2019"
rating: 0
weight: 102786
solve_time_s: 61
verified: true
draft: false
---

[CF 102786I - \u041f\u0440\u043e\u0431\u043b\u0435\u043c\u0430 \u0441\u0432\u043e\u0431\u043e\u0434\u043d\u043e\u0433\u043e \u043c\u0435\u0441\u0442\u0430](https://codeforces.com/problemset/problem/102786/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
Chỉnh sửa 

#Hiểu vấn đề 

Chúng tôi có một hàng`n`hàng hóa, được đại diện bởi một loạt các trọng lượng. Hàng thay đổi theo thời gian: mỗi truy vấn hoán đổi các phần tử ở hai vị trí. Sau mỗi lần hoán đổi, chúng ta phải xác định xem toàn bộ mảng có được sắp xếp theo thứ tự không giảm hay không. 

Câu trả lời không phụ thuộc vào cách mảng trở thành trạng thái hiện tại. Đối với mỗi thời điểm sau khi hoán đổi, chúng ta chỉ cần biết liệu mọi phần tử có lớn hơn phần tử ngay sau nó hay không. 

Các giới hạn buộc chúng ta phải tránh kiểm tra toàn bộ mảng sau mỗi truy vấn. Với`n`Và`q`cả hai đều đạt`100000`, một giải pháp quét tất cả`n`vị trí cho mỗi lần hoán đổi thực hiện lên tới`10^10`so sánh, vượt xa giới hạn 2 giây cho phép. Chúng tôi cần một giải pháp trong đó mỗi truy vấn chỉ thay đổi một lượng nhỏ thông tin được duy trì. 

Các trường hợp đặc biệt chính xuất phát từ thực tế là việc hoán đổi không phải lúc nào cũng chỉ ảnh hưởng đến hai vị trí được hoán đổi. Nó cũng có thể thay đổi mối quan hệ với các phần tử lân cận. Ví dụ:```
Input
3 1
1 3 2
2 3
```Sau khi hoán đổi, mảng trở thành`[1, 2, 3]`, do đó đầu ra là:```
Sorted!
```Một giải pháp chỉ kiểm tra xem bản thân các giá trị được hoán đổi có theo đúng thứ tự hay không sẽ bỏ sót thực tế là mối quan hệ lân cận tại vị trí`1`cũng thay đổi. 

Một trường hợp quan trọng khác là giá trị bằng nhau:```
Input
3 1
5 5 5
1 3
```Đầu ra là:```
Sorted!
```Một sự so sánh chặt chẽ như`a[i] < a[i+1]`sẽ đánh dấu không chính xác mảng này là chưa được sắp xếp vì cho phép các giá trị liền kề bằng nhau. 

Một tình huống khó khăn cuối cùng là khi mảng được sắp xếp trước các truy vấn sau:```
Input
4 2
1 3 2 4
2 3
1 4
```Sau lần hoán đổi đầu tiên, mảng được sắp xếp, nhưng lần hoán đổi thứ hai lại phá vỡ nó. Chúng tôi phải trả lời mọi truy vấn một cách độc lập với trạng thái hiện tại, không dừng lại sau khoảnh khắc thành công đầu tiên. 

# Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng mọi trao đổi và sau đó quét toàn bộ mảng để kiểm tra xem mọi cặp liền kề có được sắp xếp theo thứ tự hay không. Điều này đúng vì mảng không giảm chính xác là một mảng trong đó tất cả các phép so sánh liền kề đều thỏa mãn`a[i] <= a[i+1]`. 

Vấn đề là chi phí. Mỗi trong số`q`truy vấn yêu cầu lên tới`n-1`so sánh, dẫn đến`O(nq)`công việc. Trong trường hợp xấu nhất thì đây là về`10^10`hoạt động quá chậm. 

Cấu trúc của bài toán cho chúng ta một mục tiêu nhỏ hơn. Hoán đổi chỉ thay đổi giá trị ở hai vị trí, vì vậy hầu hết các cặp liền kề đều giữ nguyên thứ tự. Nếu chúng ta duy trì số cặp liền kề hiện đang sai thì một lần hoán đổi chỉ yêu cầu cập nhật các cặp chạm vào vị trí được hoán đổi. 

Mảng được sắp xếp chính xác khi số này bằng 0. Trước khi hoán đổi, chúng tôi loại bỏ phần đóng góp của các cặp lân cận bị ảnh hưởng, thực hiện hoán đổi, sau đó thêm phần đóng góp mới của chúng. Vì mỗi vị trí chỉ có hai vị trí lân cận nên mỗi truy vấn sẽ thay đổi một số lượng cặp không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(n+q) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Xây dựng bộ đếm các cặp liền kề xấu. Đối với mỗi chỉ số`i`, kiểm tra xem`a[i] > a[i+1]`. Nếu điều này đúng thì cặp này vi phạm thứ tự sắp xếp và làm tăng bộ đếm. 
2. Đối với mỗi truy vấn hoán đổi, hãy xác định hai vị trí được trao đổi. Các cặp liền kề duy nhất có thể thay đổi là các cặp liên quan đến vị trí hoán đổi, vì vậy hãy thu thập các chỉ số đó trước khi thực hiện bất kỳ sửa đổi nào. 
3. Loại bỏ phần đóng góp cũ của mọi cặp liền kề bị ảnh hưởng khỏi bộ đếm. Bước này là cần thiết vì các giá trị hiện tại sắp biến mất khỏi các vị trí đó. 
4. Hoán đổi hai giá trị mảng. 
5. Thêm phần đóng góp mới của mỗi cặp liền kề bị ảnh hưởng sau khi hoán đổi. Bộ đếm bây giờ mô tả lại mảng hiện tại. 
6. Nếu bộ đếm bằng 0, hãy in`Sorted!`. Ngược lại, in`Unsorted!`. 

Lý do điều này có tác dụng là vì mọi rối loạn có thể xảy ra trong một mảng không giảm đều xuất hiện dưới dạng một cặp liền kề xấu. Nếu không có cặp liền kề nào giảm đi thì mọi phần tử nhiều nhất là phần tử tiếp theo, có nghĩa là toàn bộ mảng đã được sắp xếp. Hoạt động hoán đổi không thể thay đổi bất kỳ cặp nào không liền kề với một trong các vị trí được hoán đổi, do đó, chỉ cập nhật những cặp đó sẽ bảo toàn số lượng vi phạm chính xác. 

#Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    bad = 0
    for i in range(n - 1):
        if a[i] > a[i + 1]:
            bad += 1

    ans = []

    def value_bad(i):
        return i >= 0 and i + 1 < n and a[i] > a[i + 1]

    for _ in range(q):
        x, y = map(int, input().split())
        x -= 1
        y -= 1

        affected = set()

        for pos in (x, y):
            affected.add(pos - 1)
            affected.add(pos)

        for pos in affected:
            if value_bad(pos):
                bad -= 1

        a[x], a[y] = a[y], a[x]

        for pos in affected:
            if value_bad(pos):
                bad += 1

        if bad == 0:
            ans.append("Sorted!")
        else:
            ans.append("Unsorted!")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biến`bad`lưu trữ số lượng đảo ngược liền kề. Hàm trợ giúp kiểm tra xem một cặp liền kề cụ thể có hợp lệ hay không đồng thời xử lý các ranh giới vì các vị trí bên ngoài mảng sẽ không bao giờ được tính. 

Tập hợp bị ảnh hưởng có thể chứa các bản sao khi các vị trí hoán đổi nằm cạnh nhau. Sử dụng một bộ sẽ tránh được việc loại bỏ hoặc thêm cùng một cặp liền kề hai lần. Điều này quan trọng đối với các giao dịch hoán đổi như vị trí`2`Và`3`, trong đó cả hai vị trí đều có chung một cặp lân cận. 

Mã chuyển đổi vị trí từ định dạng đầu vào, bắt đầu từ`1`, thành các chỉ mục Python bắt đầu từ`0`. Việc hoán đổi chỉ xảy ra sau khi phần đóng góp cũ bị xóa, vì việc đếm mảng đã sửa đổi mà không xóa trạng thái trước đó sẽ trộn lẫn hai phiên bản khác nhau của mảng. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

| Truy vấn | Mảng sau khi hoán đổi | Cặp liền kề xấu | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 2 5 3 4 | (5,3) | | 
| 3 4 | 1 2 3 5 4 | (5,4) | Chưa được sắp xếp! | 
| 4 5 | 1 2 3 4 5 | không | Đã sắp xếp! | 
| 1 5 | 5 2 3 4 1 | (5,2), (4,1) | Chưa được sắp xếp! | 
| 5 1 | 1 2 3 4 5 | không | Đã sắp xếp! | 

Ví dụ này cho thấy bộ đếm có thể di chuyển từ khác 0 sang 0 và ngược lại. Thuật toán không lưu trữ liệu mảng đã được sắp xếp trước đó hay chưa, nó sẽ tính toán lại thông tin bị ảnh hưởng sau mỗi lần sửa đổi. 

Đối với mẫu thứ hai: 

| Truy vấn | Mảng sau khi hoán đổi | Cặp liền kề xấu | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 2 | không | | 
| 1 2 | 2 1 | (2,1) | Chưa được sắp xếp! | 
| 1 2 | 1 2 | không | Đã sắp xếp! | 
| 1 2 | 2 1 | (2,1) | Chưa được sắp xếp! | 

Trường hợp này cho thấy sự hoán đổi lặp đi lặp lại của cùng một vị trí. Bộ đếm được duy trì tuân theo trạng thái mảng hiện tại và không phụ thuộc vào các câu trả lời trước đó. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+q) | Lần quét đầu tiên sẽ kiểm tra từng cặp liền kề một lần và mỗi truy vấn chỉ cập nhật một số lượng cặp không đổi. | 
| Không gian | O(n) | Mảng và một lượng thông tin bổ sung không đổi được lưu trữ. | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì tổng số thao tác tăng tuyến tính với kích thước đầu vào. Thậm chí tại`100000`hoán đổi, mỗi truy vấn chỉ thực hiện một số thao tác chèn, so sánh và truy cập mảng. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""5 4
1 2 5 3 4
3 4
4 5
1 5
5 1
""") == """Unsorted!
Sorted!
Unsorted!
Sorted!
""", "sample 1"

assert run("""2 3
1 2
1 2
1 2
1 2
""") == """Unsorted!
Sorted!
Unsorted!
""", "sample 2"

assert run("""2 1
7 7
1 2
""") == """Sorted!
""", "equal values"

assert run("""5 1
1 2 3 5 4
4 5
""") == """Sorted!
""", "boundary adjacent swap"

assert run("""5 1
5 4 3 2 1
1 5
""") == """Unsorted!
""", "large disorder after distant swap"

assert run("""100000 1
""" + " ".join(["1"] * 100000) + """
1 100000
""") == """Sorted!
""", "maximum size all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | Câu trả lời hỗn hợp | Xử lý hoán đổi chung và phục hồi theo thứ tự đã sắp xếp | 
| Mẫu 2 | Câu trả lời thay thế | Hoán đổi lặp đi lặp lại các vị trí giống nhau | 
| Giá trị bằng nhau | Đã sắp xếp! | So sánh không giảm phải cho phép bình đẳng | 
| Hoán đổi ranh giới liền kề | Đã sắp xếp! | Cập nhật chính xác gần các vị trí lân cận | 
| Hoán đổi thứ tự ngược | Chưa được sắp xếp! | Nhiều vi phạm hiện có | 
| Kích thước tối đa đều bằng nhau | Đã sắp xếp! | Tiền xử lý tuyến tính và sử dụng bộ nhớ | 

# Vỏ cạnh 

Đối với các giá trị bằng nhau, thuật toán sử dụng`>`còn hơn là`>=`khi đếm các cặp xấu. Với đầu vào:```
3 1
5 5 5
1 3
```các cặp liền kề bị ảnh hưởng được kiểm tra sau khi hoán đổi, nhưng cả hai so sánh vẫn hợp lệ vì`5 <= 5`. Bộ đếm vẫn bằng 0 và câu trả lời là`Sorted!`. 

Đối với một hoán đổi sửa toàn bộ mảng, thuật toán sẽ loại bỏ các cặp xấu cũ trước khi trao đổi và sau đó thêm các cặp xấu mới. Với:```
3 1
1 3 2
2 3
```cặp xấu ban đầu là`(3,2)`. Nó bị xóa, trao đổi tạo ra`[1,2,3]`, và không có cặp xấu nào được thêm vào. Bộ đếm trở thành 0, do đó đầu ra là`Sorted!`. 

Đối với một giao dịch hoán đổi làm thay đổi hai vị trí lân cận, các chỉ số bị ảnh hưởng trùng lặp không được xử lý hai lần. TRONG:```
4 1
1 4 2 3
2 3
```các vị trí chia sẻ cặp liền kề xung quanh chúng. Tập hợp các cạnh bị ảnh hưởng chỉ giữ cặp này một lần, ngăn chặn sự thay đổi không chính xác trong bộ đếm. 

Đối với các truy vấn lặp lại hoàn tác lẫn nhau, thuật toán luôn hoạt động với mảng hiện tại. TRONG:```
2 2
1 2
1 2
1 2
```truy vấn đầu tiên tạo ra một cặp xấu và truy vấn thứ hai sẽ loại bỏ nó. Các kết quả đầu ra là:```
Unsorted!
Sorted!
```Bộ đếm được duy trì mô tả trạng thái hiện tại sau mỗi thao tác, đây chính xác là thông tin cần thiết cho mỗi câu trả lời.```

```Tôi cũng có thể điều chỉnh bài xã luận này thành một phiên bản ngắn hơn theo phong cách Codeforces nếu bạn cần một phiên bản phù hợp với định dạng blog cuộc thi.
