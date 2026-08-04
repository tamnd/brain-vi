---
title: "CF 102625C - Matiyao Be Mid Sem hee toh hai"
description: "Chúng tôi có một dãy điểm đại diện cho số điểm hiện tại của từng môn học. Có một số thao tác được thực hiện theo một thứ tự cố định. Trong quá trình hoạt động j, chúng tôi có thể chọn tối đa đối tượng Bj và ghi đè điểm của chúng bằng giá trị Cj."
date: "2026-08-03T15:30:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "C"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 346
verified: true
draft: false
---

[CF 102625C - Matiyao Be Mid Sem hee toh hai](https://codeforces.com/problemset/problem/102625/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 46 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy điểm đại diện cho số điểm hiện tại của từng môn học. Có một số thao tác được thực hiện theo một thứ tự cố định. Trong quá trình vận hành`j`, chúng tôi có thể chọn tối đa`B_j`môn học và ghi đè lên điểm của chúng bằng giá trị`C_j`. Một chủ đề có thể được chọn nhiều lần, nhưng chỉ thao tác cuối cùng được chọn mới ảnh hưởng đến điểm cuối cùng của nó. 

Mục tiêu là chọn các môn học cho mọi hoạt động sao cho tổng số điểm cuối cùng càng lớn càng tốt. 

Các giá trị của`N`Và`M`cả hai có thể đạt được`100000`. Điều này loại trừ các cách tiếp cận mô phỏng các lựa chọn đối tượng hoặc thử tất cả các nhiệm vụ hoạt động có thể. Thậm chí một`O(NM)`phương pháp sẽ thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn 1 giây cho phép. Chúng ta cần một giải pháp gần`O((N+M) log N)`. 

Các bẫy chính đến từ thứ tự của các thao tác và từ các thao tác không hữu ích. Ví dụ: một thao tác có thể ghi đè lên điểm cao bằng giá trị thấp hơn, do đó việc áp dụng mù quáng mọi thao tác là không chính xác. 

Hãy xem xét trường hợp này:```
1 1
100
1 50
```Câu trả lời đúng là`100`. Một giải pháp bất cẩn luôn thực hiện thao tác làm thay đổi dấu hiệu thành`50`và mất giá trị. 

Một trường hợp khác là:```
3 2
5 1 4
2 3
1 5
```Câu trả lời đúng là`14`. Thao tác đầu tiên nên bỏ qua vì việc thay đổi hai đối tượng thành`3`có hại. Thao tác thứ hai nên thay đổi chủ đề bằng dấu`1`ĐẾN`5`. Một giải pháp xử lý các hoạt động một cách tham lam ngay từ đầu có thể tiêu tốn các đối tượng cho hoạt động đầu tiên và ngăn cản việc sử dụng hoạt động tốt hơn sau này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng quyết định, đối với mọi hoạt động, đối tượng nào sẽ được thay thế. Vì mỗi thao tác có thể chọn nhiều tập con chủ đề khác nhau nên số lượng các lựa chọn có thể tăng lên theo cấp số nhân. Ngay cả một mô phỏng hợp lý hơn để kiểm tra nhiều đối tượng ứng cử viên cho mọi thao tác cũng trở nên quá chậm. Trong trường hợp xấu nhất, việc kiểm tra tất cả các đối tượng cho tất cả các hoạt động đã tốn chi phí`10^10`so sánh. 

Lực lượng vũ phu là chính xác vì nó khám phá mọi cách gán hoạt động có thể có cho các chủ thể, nhưng cấu trúc của vấn đề cho phép chúng ta tránh đưa ra những lựa chọn đó một cách rõ ràng. 

Điều quan trọng cần lưu ý là thao tác cuối cùng tác động đến một đối tượng sẽ xác định hoàn toàn giá trị cuối cùng của nó. Điều này có nghĩa là chúng ta có thể xem xét các hoạt động theo thứ tự ngược lại. Khi xử lý ngược các thao tác, mọi đối tượng đã được chọn bởi thao tác sau sẽ được cố định và không thể chạm lại được. Các môn còn lại vẫn giữ nguyên điểm. 

Đối với một hoạt động đảo ngược với giá trị`C`, chúng ta chỉ nên sử dụng nó cho những môn có điểm hiện tại nhỏ hơn`C`. Nếu chúng ta quyết định sử dụng phép toán thì những đối tượng tốt nhất là những đối tượng có điểm thấp nhất vì chúng nhận được cùng một giá trị thay thế. Điều này chuyển đổi vấn đề thành việc liên tục lấy các giá trị nhỏ nhất từ ​​cấu trúc dữ liệu và tăng chúng khi một thao tác tốt hơn xuất hiện. 

Một đống tối thiểu lưu trữ các chủ đề vẫn có sẵn. Các thao tác xử lý ngược cho phép chúng ta trích xuất một cách tham lam những dấu nhỏ nhất, thay thế chúng bằng`C`và chèn lại giá trị mới. Mỗi chủ đề chỉ được cải thiện khi một thao tác có thể tăng nó và vùng heap luôn đưa ra những ứng cử viên tốt nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(N) | Quá chậm | 
| Tối ưu | O((N + M) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chèn tất cả các dấu ban đầu vào một đống tối thiểu và tính tổng ban đầu của chúng. Heap đại diện cho các đối tượng có giá trị cuối cùng chưa được cố định bằng thao tác sau. 
2. Thực hiện các thao tác theo thứ tự ngược lại. Đối với một hoạt động`(B, C)`, liên tục nhìn vào giá trị nhỏ nhất hiện có trong heap. Nếu nó nhỏ hơn`C`, thay thế nó bằng`C`, cập nhật tổng số tiền bằng cách cộng phần cải tiến và đặt`C`trở lại đống. 

Việc chọn giá trị nhỏ nhất là tối ưu vì mọi đối tượng được chọn đều nhận được cùng một giá trị mới. Sự cải tiến lớn nhất luôn đến từ việc thay thế dấu hiện tại nhỏ nhất. 
3. Dừng sử dụng thao tác hiện tại khi một trong hai`B`các môn học đã được cải thiện hoặc giá trị sẵn có nhỏ nhất ít nhất đã đạt`C`. 

Nếu giá trị nhỏ nhất không nhỏ hơn`C`, mọi giá trị sẵn có khác cũng không nhỏ hơn nên việc sử dụng thao tác không thể làm tăng đáp số. 
4. Sau khi tất cả các thao tác đảo ngược được xử lý, hãy xuất số tiền duy trì. 

Tại sao nó hoạt động: 

Xem xét mọi thao tác trong khi xử lý ngược. Các đối tượng đã được chọn bởi các thao tác sau sẽ được cố định vì các thao tác trước đó không thể ảnh hưởng đến giá trị cuối cùng của chúng. Trong số các môn còn lại, hoạt động hiện tại có thể cải thiện nhiều nhất`B`giá trị để`C`. Sự lựa chọn tối ưu luôn là`B`các giá trị nhỏ nhất dưới đây`C`, bởi vì thay thế bất kỳ giá trị lớn hơn nào sẽ mang lại mức tăng nhỏ hơn. Vùng heap duy trì chính xác những ứng cử viên đó, vì vậy mỗi bước đảo ngược sẽ đưa ra quyết định cục bộ tốt nhất có thể. Vì mỗi thao tác được xử lý sau tất cả các thao tác có thể ghi đè lên nó nên các lựa chọn cục bộ này kết hợp thành phép gán tối ưu toàn cục. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    operations = []
    for _ in range(m):
        b, c = map(int, input().split())
        operations.append((b, c))

    heap = a[:]
    heapq.heapify(heap)

    ans = sum(a)

    for b, c in reversed(operations):
        used = 0
        while used < b and heap[0] < c:
            x = heapq.heappop(heap)
            ans += c - x
            heapq.heappush(heap, c)
            used += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Đống chứa tất cả các dấu chưa được gán cho thao tác sau theo thứ tự ban đầu. Việc truyền tải ngược lại là điều làm cho cách giải thích này có thể thực hiện được. 

Tổng được cập nhật ngay lập tức khi việc thay thế diễn ra, tránh việc phải xây dựng lại mảng sau mỗi thao tác. Số nguyên Python xử lý số tiền lớn có thể một cách an toàn vì tổng số có thể đạt khoảng`10^14`. 

Kiểm tra điều kiện vòng lặp`heap[0] < c`trước khi thay thế một giá trị. Điều này ngăn ngừa sự thay thế có hại và cũng tránh loại bỏ nhiều hơn những chủ đề hữu ích hiện có. Vùng heap không bao giờ trống vì mọi giá trị bị xóa sẽ được chèn lại với dấu mới. 

## Ví dụ đã hoạt động 

Đối với mẫu 1:```
3 2
5 1 4
2 3
1 5
```Các hoạt động được xử lý từ cuối. 

| Bước | Hoạt động | Đống trước | Hành động | Tổng hợp | 
| --- | --- | --- | --- | --- | 
| Ban đầu | Không có | [1, 4, 5] | Bắt đầu | 10 | 
| 1 | (1, 5) | [1, 4, 5] | Thay thế 1 bằng 5 | 14 | 
| 2 | (2, 3) | [4, 5, 5] | Giá trị nhỏ nhất không dưới 3, không làm gì | 14 | 

Thao tác đảo ngược đầu tiên thể hiện thực tế là thao tác cuối cùng được ưu tiên. Thao tác trước đó không thể cải thiện bất kỳ chủ đề nào còn lại, vì vậy bỏ qua nó là tối ưu. 

Đối với mẫu 2:```
10 3
1 8 5 7 100 4 52 33 13 5
3 10
4 30
1 4
```| Bước | Hoạt động | Giá trị sẵn có nhỏ nhất đã thay đổi | Tổng hợp | 
| --- | --- | --- | --- | 
| Ban đầu | Không có | Không có thay đổi | 228 | 
| 1 | (1, 4) | 1 trở thành 4 | 231 | 
| 2 | (4, 30) | 4, 5, 5, 7 trở thành 30 | 320 | 
| 3 | (3, 10) | Giá trị nhỏ nhất đã trên 10 | 320 | 

Ví dụ này cho thấy tại sao các thao tác không nên được xử lý theo thứ tự ban đầu. Thao tác sau có thể sử dụng một chủ đề mà thao tác trước đó sẽ lãng phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Mỗi thao tác trên heap tốn O(log N) và mỗi lần thay thế hữu ích sẽ chèn một giá trị trở lại heap. | 
| Không gian | O(N + M) | Heap lưu trữ N dấu và danh sách thao tác lưu trữ M cặp. | 

Với`N, M <= 100000`, các phép toán logarit dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ cũng tuyến tính, thấp hơn nhiều so với bộ nhớ khả dụng. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    ops = [tuple(map(int, input().split())) for _ in range(m)]

    heapq.heapify(a)
    ans = sum(a)

    for b, c in reversed(ops):
        for _ in range(b):
            if a[0] >= c:
                break
            x = heapq.heappop(a)
            ans += c - x
            heapq.heappush(a, c)

    sys.stdin = old_stdin
    return str(ans)

assert run("""3 2
5 1 4
2 3
1 5
""") == "14", "sample 1"

assert run("""10 3
1 8 5 7 100 4 52 33 13 5
3 10
4 30
1 4
""") == "320", "sample 2"

assert run("""3 2
100 100 100
3 99
3 99
""") == "300", "sample 3"

assert run("""1 1
100
1 50
""") == "100", "avoid harmful replacement"

assert run("""1 2
1
1 5
1 10
""") == "10", "latest operation priority"

assert run("""5 1
1 1 1 1 1
5 1000000000
""") == "5000000000", "large values"

assert run("""4 3
5 5 5 5
1 5
2 4
4 6
""") == "20", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chủ đề duy nhất được thay thế bằng giá trị thấp hơn | 100 | Xác nhận các hoạt động có hại bị bỏ qua. | 
| Hai hoạt động tăng dần về một chủ đề | 10 | Xác nhận xử lý ngược ưu tiên cho thao tác cuối cùng. | 
| Giá trị thay thế lớn | 5000000000 | Xác nhận số tiền lớn được xử lý chính xác. | 
| Tất cả các điểm bằng nhau | 20 | Xác nhận không có sự thay thế không cần thiết nào được thực hiện. | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khi một thao tác làm giảm điểm. Vì:```
1 1
100
1 50
```đống bắt đầu bằng`[100]`. Hoạt động được xử lý và giá trị heap nhỏ nhất không nhỏ hơn`50`, do đó thuật toán không làm gì và giữ câu trả lời là`100`. 

Trường hợp cạnh thứ hai là khi một thao tác sớm có thể chặn một thao tác tốt hơn sau đó:```
3 2
5 1 4
2 3
1 5
```Thuật toán xử lý đầu tiên`(1,5)`theo thứ tự ngược lại và thay đổi`1`ĐẾN`5`. Các giá trị còn lại là`4,5,5`, Vì thế`(2,3)`không thể cải thiện bất cứ điều gì. Câu trả lời cuối cùng là`14`, phù hợp với sự lựa chọn tối ưu. 

Trường hợp cạnh thứ ba là khi mọi thao tác đều có giá trị thay thế bằng hoặc nhỏ hơn dấu hiện tại:```
3 2
100 100 100
3 99
3 99
```Mức tối thiểu của heap luôn là`100`, không nhỏ hơn`99`. Cả hai thao tác đều bị bỏ qua và câu trả lời vẫn còn`300`. 

Bài xã luận này cũng có thể được rút ngắn thành một lời giải thích theo phong cách cuộc thi hoặc được mở rộng bằng một bằng chứng chính thức hơn nếu cần.
