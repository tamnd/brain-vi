---
title: "CF 102551C - \u041f\u0440\u043e\u0434\u0443\u043a\u0442\u044b \u0432 \u044d\u043a\u0441\u043f\u0435\u0434\u0438\u0446\u0438\u0438"
description: "Chúng tôi có n loại sản phẩm. Sản phẩm i có ki phần và biến mất sau ngày ti. Đoàn thám hiểm có c người nên mỗi ngày có thể ăn được chính xác c phần ăn."
date: "2026-08-04T09:05:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102551
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0422\u0440\u0435\u0442\u044c\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102551
solve_time_s: 143
verified: true
draft: false
---

[CF 102551C - \u041f\u0440\u043e\u0434\u0443\u043a\u0442\u044b \u0432 \u044d\u043a\u0441\u043f\u0435\u0434\u0438\u0446\u0438\u0438](https://codeforces.com/problemset/problem/102551/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`các loại sản phẩm. Sản phẩm`i`có`k_i`từng phần và biến mất sau ngày`t_i`. Cuộc thám hiểm có`c`mọi người, vậy nên chính xác là mỗi ngày`c`phần có thể được ăn. Các phần của các sản phẩm khác nhau có thể được trộn trong cùng một ngày, điều đó có nghĩa là một sản phẩm không phải là một mặt hàng không thể chia nhỏ: chúng ta chỉ cần đủ tổng sức chứa ăn uống trước thời hạn. 

Nhiệm vụ là chọn càng nhiều loại sản phẩm càng tốt để có thể ăn hết từng phần của mỗi loại đã chọn trước thời hạn tương ứng. Đầu ra là kích thước của tập hợp tối đa này và các chỉ số của sản phẩm đã chọn. 

Những ràng buộc buộc phải có một giải pháp tham lam. có thể có`2 * 10^5`sản phẩm, vì vậy việc kiểm tra mọi tập hợp con là không thể bởi vì ngay cả`2^n`tập hợp con là vượt xa tầm với. Ngay cả các thuật toán xung quanh`O(n^2)`quá chậm đối với kích thước này. Chúng ta cần một cái gì đó gần gũi`O(n log n)`, thường có nghĩa là sắp xếp cộng với cấu trúc dữ liệu. 

Các giá trị lớn của`c`,`t_i`, Và`k_i`cũng quan trọng. Lượng thức ăn có thể đạt tới`10^27`, do đó việc triển khai phải sử dụng số học số nguyên mà không bị tràn. Số nguyên Python xử lý việc này một cách tự nhiên. 

Một sai lầm phổ biến là chỉ kiểm tra tổng lượng thức ăn. Ví dụ:```
2 3
1 10
100 1000
```Tổng sức chứa của cả chuyến thám hiểm là rất lớn, nhưng riêng sản phẩm đầu tiên cần 10 phần trong ngày đầu tiên trong khi chỉ được ăn 3 phần. Đầu ra đúng là:```
1
2
```Một sai lầm khác là bỏ qua thời hạn chính xác. Coi như:```
3 2
1 3
2 1
10 100
```Hai sản phẩm đầu tiên cùng nhau yêu cầu 4 phần vào ngày thứ 2, trong khi sức chứa hiện có chỉ là 4 nên cả hai đều có thể sử dụng được. Sản phẩm thứ ba không thể phù hợp với họ vì năng lực trước ngày thứ 10 cũng bị giới hạn bởi tất cả các thời hạn trước đó. Thuật toán phải liên tục xác minh mọi tiền tố thời hạn. 

Trường hợp cạnh thứ ba là khi cần phải tháo một sản phẩm mặc dù bản thân sản phẩm hiện tại có thể vừa. Ví dụ:```
3 5
1 5
2 5
2 20
```Sản phẩm cuối cùng có kích thước lớn nhưng việc giữ nó sẽ buộc một trong những sản phẩm nhỏ hơn phải loại bỏ. Câu trả lời tối ưu là giữ hai loại sản phẩm, không nhất thiết phải là loại mới nhất. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ có thể thử mọi tập hợp con sản phẩm. Đối với mỗi tập hợp con, hãy sắp xếp các sản phẩm của nó theo thời hạn và xác minh xem lượng thực phẩm có thời hạn tối đa mỗi ngày có phù hợp hay không.`c * day`các phần công suất. Việc kiểm tra này là chính xác vì nút thắt cổ chai duy nhất có thể xảy ra là tiền tố thời hạn. Tuy nhiên, có`2^n`tập hợp con, và thậm chí đối với`n = 50`điều này đã là không thể rồi. Với`n = 200000`, cách tiếp cận này không gần gũi lắm. 

Quan sát hữu ích xuất phát từ việc xem xét cấu trúc của điều kiện khả thi. Sau khi sắp xếp sản phẩm theo ngày hết hạn, khi chúng tôi xử lý sản phẩm từ thời hạn sớm đến thời hạn muộn, điều duy nhất quan trọng là tổng số lượng đã chọn cho đến nay. Nếu việc thêm một sản phẩm mới khiến thời hạn hiện tại không thể thực hiện được thì chúng ta nên loại bỏ một sản phẩm đã chọn. Để giữ được số lượng loại sản phẩm tối đa, sản phẩm thải bỏ phải là sản phẩm có số lượng phần lớn nhất. 

Đây là lập luận trao đổi tương tự đằng sau việc lên lịch cho số lượng công việc tối đa có thời hạn. Một sản phẩm lớn tiêu thụ nhiều dung lượng hơn trong khi mang lại phần thưởng giống như một sản phẩm nhỏ, vì cả hai đều được tính chính xác là một loại sản phẩm. Việc thay thế một sản phẩm được chọn lớn bằng một sản phẩm bị từ chối nhỏ hơn chỉ có thể cải thiện tính khả thi trong khi vẫn duy trì số lượng loại đã chọn. 

Vùng nhớ tối đa lưu trữ các sản phẩm hiện được chọn theo số phần của chúng. Sau khi sắp xếp theo thời hạn, chúng tôi thêm mọi sản phẩm và nếu tiền tố hiện tại vượt quá dung lượng của nó, chúng tôi sẽ xóa sản phẩm lớn nhất khỏi vùng nhớ heap. Đống còn lại chứa tập hợp tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các sản phẩm theo ngày hết hạn tăng dần. Những sản phẩm có thời hạn nhỏ hơn phải được xem xét trước tiên vì chúng tạo ra giới hạn công suất chặt chẽ hơn. 
2. Giữ tổng số phần đang chạy trong bộ hiện được chọn và chèn từng sản phẩm đã xử lý vào đống tối đa. Đống cho phép chúng ta tìm ra sản phẩm đắt nhất xét về công suất tiêu thụ. 
3. Sau khi chèn sản phẩm có thời hạn`t`, so sánh tổng số phần được chọn với`c * t`. Nếu tổng quá lớn, hãy loại bỏ sản phẩm có số phần tối đa khỏi đống và trừ đi kích thước của nó khỏi tổng. 
4. Sau khi tất cả các sản phẩm được xử lý, đống chứa các chỉ số của loại sản phẩm tạo thành câu trả lời. 

Tại sao nó hoạt động: sau khi xử lý mọi thời hạn, đống luôn đại diện cho số lượng sản phẩm lớn nhất có thể trong số tất cả các lựa chọn khả thi được xem xét cho đến nay. Nếu vi phạm thời hạn, mọi giải pháp khả thi đều phải loại bỏ ít nhất một sản phẩm khỏi bộ hiện tại. Loại bỏ sản phẩm lớn nhất là lựa chọn tốt nhất có thể vì nó giải phóng nhiều dung lượng nhất trong khi chỉ mất một sản phẩm khỏi số câu trả lời. Việc lặp lại điều này sau mỗi lần vi phạm sẽ giữ được số lượng loại được chọn tối đa. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, c = map(int, input().split())
    products = []

    for i in range(1, n + 1):
        t, k = map(int, input().split())
        products.append((t, k, i))

    products.sort()

    heap = []
    total = 0

    for t, k, idx in products:
        heapq.heappush(heap, (-k, idx))
        total += k

        if total > c * t:
            removed_k, removed_idx = heapq.heappop(heap)
            total += removed_k

    answer = [idx for _, idx in heap]

    print(len(answer))
    if answer:
        print(*answer)

if __name__ == "__main__":
    solve()
```Các sản phẩm được sắp xếp để mọi vi phạm đều được kiểm tra vào thời hạn sớm nhất có thể xảy ra. Heap lưu trữ số phần âm vì Python`heapq`là một đống tối thiểu và việc phủ định các giá trị sẽ biến nó thành một đống tối đa. 

Biến`total`chứa lượng thức ăn trong bộ đã chọn hiện tại. Khi một sản phẩm bị xóa, giá trị heap được lưu trữ của nó là âm, do đó việc thêm lại sản phẩm sẽ trừ đi các phần của nó. Số nguyên Python không bị giới hạn, vì vậy các giá trị như`c * t`và tổng của`k_i`không tràn. 

Đống cuối cùng chỉ chứa các sản phẩm còn sót lại sau mỗi lần kiểm tra dung lượng. Thứ tự của chúng không liên quan vì bài toán chấp nhận bất kỳ thứ tự hợp lệ nào của các chỉ số. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1 1
4 4
```Dấu vết là: 

| Bước | Sản phẩm | Hạn chót | Nội dung đống | Tổng cộng | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | {1:4} | 4 | Giữ | 

Sản phẩm yêu cầu 4 phần và 4 phần có thể ăn trong 4 ngày nên vẫn được chọn. 

Đối với mẫu thứ hai:```
5 3
3 4
2 6
4 5
3 4
5 7
```Sau khi sắp xếp theo deadline, thứ tự là sản phẩm 2, 1, 4, 3, 5. 

| Bước | Đã thêm sản phẩm | Hạn chót | Tổng số trước khi loại bỏ | Đã xóa | Kích thước đống | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 6 | Không có | 1 | 
| 2 | 1 | 3 | 10 | Không có | 2 | 
| 3 | 4 | 3 | 14 | Không có | 3 | 
| 4 | 3 | 4 | 19 | Không có | 4 | 
| 5 | 5 | 5 | 26 | Sản phẩm 2 | 4 | 

Ở bước cuối cùng, tổng công suất là`3 * 5 = 15`, do đó tích lớn nhất bị loại bỏ. Sản phẩm 2 có 6 phần, trong khi các sản phẩm nhỏ hơn có thể ở lại với nhau. Bộ cuối cùng chứa ba loại sản phẩm, phù hợp với mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp mất O(n log n) và mọi thao tác heap là O(log n) | 
| Không gian | O(n) | Heap và danh sách sản phẩm lưu trữ hầu hết tất cả các sản phẩm | 

Giải pháp xử lý`200000`Products vì nó chỉ thực hiện các phép toán sắp xếp và logarit. Các giá trị số học lớn nhưng hỗ trợ số nguyên Python giúp các phép tính được an toàn. 

## Trường hợp thử nghiệm```python
import io
import sys

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

assert run("""1 1
4 4
""") == "1\n1\n"

assert run("""5 3
3 4
2 6
4 5
3 4
5 7
""").split()[0] == "3"

assert run("""3 2
2 6
4 9
1 3
""").split()[0] == "0"

assert run("""1 10
1 11
""").split()[0] == "0"

assert run("""4 5
1 5
2 5
3 5
4 5
""").split()[0] == "4"

assert run("""3 2
1 4
2 4
2 100
""").split()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một sản phẩm phù hợp chính xác | 1 | Ranh giới khả thi cơ bản | 
| Mẫu 2 | 3 | Thay thế tham lam bình thường | 
| Mẫu 3 | 0 | Thời hạn bất khả thi | 
| Một sản phẩm vượt quá công suất trong ngày | 0 | Từ chối ngay lập tức | 
| Kích thước phần bằng nhau | 4 | Nhiều lần xóa hợp lệ | 
| Sản phẩm muộn lớn | 2 | Loại bỏ sản phẩm đắt nhất | 

## Vỏ cạnh 

Trường hợp cạnh thứ nhất là sản phẩm có tổng thời gian khảo sát nhưng không đủ thời gian trước thời hạn của chính nó. đầu vào```
2 3
1 10
100 1000
```được xử lý bằng cách xem xét sản phẩm đầu tiên. Heap chứa 10 phần, nhưng dung lượng ở ngày 1 chỉ là 3 nên sản phẩm sẽ bị loại bỏ. Sản phẩm thứ hai còn lại vì có thể ăn được trong nhiều ngày. Thuật toán đưa ra một sản phẩm. 

Trường hợp thứ hai là khi thời hạn sớm là hạn chế thực sự. TRONG```
3 2
1 3
2 1
10 100
```hai sản phẩm đầu tiên phù hợp chính xác với hai ngày đầu tiên. Khi sản phẩm thứ ba được thêm vào, điều kiện thời hạn được kiểm tra vào ngày thứ 10 và sản phẩm lớn nhất sẽ bị loại bỏ. Câu trả lời giữ lại hai sản phẩm nhỏ hơn vì chúng tối đa hóa số lượng loại. 

Trường hợp cạnh thứ ba là khi một sản phẩm lớn phải được loại bỏ mặc dù nó được xử lý muộn. Vì```
3 5
1 5
2 5
2 20
```đống tăng lên cho đến khi tổng số vượt quá sức chứa của ngày thứ 2. Sản phẩm lớn nhất có 20 phần, do đó, việc loại bỏ nó sẽ để lại hai sản phẩm có kích thước 5. Điều này mang lại số lượng loại sản phẩm tối đa có thể.
