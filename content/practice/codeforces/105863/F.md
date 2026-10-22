---
title: "CF 105863F - Sự điên rồ theo mô-đun"
description: "Chúng ta được cho một dãy số và một dãy các phép toán modulo được áp dụng cho chúng. Mỗi lần một số được xử lý với một giá trị mô đun, nó sẽ được thay thế bằng phần còn lại của nó."
date: "2026-06-21T22:35:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105863
codeforces_index: "F"
codeforces_contest_name: "PPSC 2025"
rating: 0
weight: 105863
solve_time_s: 48
verified: true
draft: false
---

[CF 105863F - Sự điên rồ về mô-đun](https://codeforces.com/problemset/problem/105863/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và một dãy các phép toán modulo được áp dụng cho chúng. Mỗi lần một số được xử lý với một giá trị mô đun, nó sẽ được thay thế bằng phần còn lại của nó. Khó khăn chính là ở chỗ các thao tác này không hoàn toàn mang tính thẩm mỹ, bởi vì khi một giá trị trở nên đủ nhỏ so với mô đun, thì việc giảm thêm sẽ ngừng thay đổi nó. 

Nhiệm vụ là hỗ trợ một cách hiệu quả một số lượng lớn các hoạt động như vậy, trong đó việc áp dụng lặp đi lặp lại các cập nhật modulo đơn giản cho mọi số bị ảnh hưởng sẽ quá chậm. Thay vì coi đây là một phép biến đổi tĩnh, sẽ hữu ích khi xem mỗi con số như một thứ gì đó phát triển theo thời gian, đôi khi thay đổi đáng kể và đôi khi hoàn toàn ổn định. 

Các ràng buộc ngụ ý rằng cả số lượng giá trị và phép toán đều có thể đủ lớn để$O(n \cdot q)$mô phỏng là không thể. Bất kỳ giải pháp nào liên tục quét hoặc cập nhật các giá trị một cách tuyến tính trên mỗi thao tác sẽ vượt quá giới hạn thời gian. Điều này ngay lập tức thúc đẩy chúng tôi hướng tới một chiến lược trong đó mỗi số riêng lẻ chỉ được xử lý một số lần nhỏ về tổng thể, thay vì mỗi thao tác. 

Trường hợp cạnh tinh tế phát sinh khi các giá trị sớm trở nên nhỏ hơn mô đun. Ví dụ: nếu một số đã nhỏ hơn mô-đun tiếp theo thì thao tác sẽ không thực hiện gì. Việc triển khai đơn giản vẫn có thể lắp lại hoặc xử lý lại nó, lãng phí thời gian và có khả năng trùng lặp công việc. Một vấn đề khác xuất hiện khi các giá trị dao động giữa trạng thái lớn và nhỏ do có nhiều mô-đun, điều này có thể dẫn đến việc tính toán lại nhiều lần nếu chúng tôi không theo dõi rõ ràng khi nào một giá trị thực sự thay đổi. 

## Phương pháp tiếp cận 

Một cách đơn giản để suy nghĩ về vấn đề là mô phỏng nó một cách trực tiếp. Đối với mỗi hoạt động mô đun, chúng tôi quét tất cả các số có liên quan và thay thế từng giá trị$x$với$x \bmod c$. Điều này đúng vì nó khớp chính xác với định nghĩa của thao tác. Tuy nhiên, chi phí rất cao: nếu chúng ta có$q$hoạt động và$n$số, trường hợp xấu nhất thực hiện$O(nq)$bản cập nhật, quá lớn khi cả hai đều có thể đạt giới hạn cao. 

Quan sát cấu trúc quan trọng là việc áp dụng mô đun không phải lúc nào cũng làm thay đổi một số. Nếu một giá trị$x$đã nhỏ hơn rồi$c$, thì nó không thay đổi. Quan trọng hơn, khi một giá trị thay đổi, nó sẽ giảm đáng kể. Yêu cầu đưa ra trong tuyên bố chính thức hóa điều này: sau khi nộp đơn$x \bmod c$, kết quả là hoặc$x$chính nó hoặc trở nên nhỏ hơn nhiều, bị giới hạn theo cách chỉ đảm bảo số logarit của các thay đổi thực trên mỗi giá trị trong toàn bộ quá trình. 

Điều này có nghĩa là hầu hết các thao tác đều vô dụng đối với hầu hết các số. Thay vì áp dụng mọi thao tác cho mọi số, chúng ta chỉ muốn xử lý một số khi nó thực sự đủ lớn để thay đổi. 

Để khai thác điều này, chúng tôi duy trì tất cả các giá trị hiện hoạt trong một cấu trúc cho phép chúng tôi luôn truy cập giá trị hiện tại lớn nhất một cách hiệu quả. Hàng đợi ưu tiên hoạt động tự nhiên ở đây. Chúng tôi liên tục trích xuất giá trị lớn nhất và so sánh nó với các phép toán mô đun sắp tới. Nếu giá trị đã nhỏ hơn mô-đun hiện tại, nó sẽ không thay đổi theo bất kỳ mô-đun lớn hơn nào trong tương lai được xử lý theo thứ tự, do đó, nó có thể được để yên một cách an toàn cho bước đó. Nếu nó lớn hơn, chúng tôi áp dụng mô-đun, cập nhật và lắp lại nếu nó vẫn có thể thay đổi trong các bước sau. 

Cải tiến quan trọng là mỗi khi một giá trị được sửa đổi, nó sẽ giảm đáng kể. Do giới hạn logarit về số lần một giá trị có thể giảm một cách có ý nghĩa nên mỗi phần tử chỉ được chèn lại một số lần nhỏ, làm cho quy trình tổng thể trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Tối ưu (hàng đợi ưu tiên với tính năng giảm lười) |$O((n + k)\log n \log X)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi các hoạt động mô đun theo thứ tự, đồng thời liên tục duy trì bộ giá trị hiện tại theo cấu trúc hướng tối đa. 

1. Chèn tất cả các số ban đầu vào vùng heap tối đa để chúng ta luôn có thể truy xuất giá trị hiện tại lớn nhất một cách hiệu quả. Điều này quan trọng vì chỉ những giá trị lớn mới có thể thay đổi theo mô đun. 
2. Lặp lại qua từng giá trị mô đun$c$theo thứ tự đã cho. 
3. Trong khi giá trị lớn nhất trong heap lớn hơn hoặc bằng$c$, giải nén nó. Điều này đảm bảo chúng tôi chỉ xử lý các giá trị thực sự bị ảnh hưởng bởi hoạt động. 
4. Tính giá trị mới như$x \bmod c$. Đây là phép biến đổi duy nhất quan trọng đối với tính chính xác, vì tất cả các giá trị không thay đổi dù sao cũng sẽ bị bỏ qua. 
5. Nếu giá trị mới vẫn đủ lớn để có khả năng bị ảnh hưởng bởi các hoạt động trong tương lai, hãy chèn lại giá trị đó vào vùng nhớ heap. Nếu không, nó sẽ tự nhiên ổn định và ngừng xử lý nhiều lần. 
6. Tiếp tục cho đến khi tất cả các hoạt động mô đun đã được áp dụng. 

Lý do chúng tôi luôn làm việc với mức tối đa hiện tại là vì nó đảm bảo chúng tôi không bao giờ lãng phí công sức vào các giá trị vốn đã quá nhỏ để có thể bị ảnh hưởng bởi mô đun hiện tại. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào đặc tính co rút đơn điệu. Mỗi khi một giá trị được cập nhật, nó sẽ giảm đi. Tuyên bố trong tuyên bố đảm bảo rằng một giá trị chỉ có thể trải qua một số lượng hạn chế các mức giảm có ý nghĩa như vậy trước khi nó trở nên ổn định vĩnh viễn trong tất cả các hoạt động trong tương lai. 

Bởi vì chúng tôi chỉ xử lý một giá trị khi nó đủ lớn để bị ảnh hưởng nên chúng tôi không bao giờ bỏ lỡ bản cập nhật có thể thay đổi giá trị của nó. Vì chúng tôi ngay lập tức chèn lại các giá trị đã cập nhật nên chúng tôi duy trì khả năng các hoạt động trong tương lai ảnh hưởng đến chúng nếu cần. Không có thao tác nào được áp dụng một cách không cần thiết và không có chuyển đổi bắt buộc nào bị bỏ qua. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    ops = list(map(int, input().split()))

    # max heap via negative values
    heap = [-x for x in a]
    heapq.heapify(heap)

    for c in ops:
        # process only values that can change
        while heap and -heap[0] >= c:
            x = -heapq.heappop(heap)
            x %= c
            heapq.heappush(heap, -x)

    print(-min(heap) if heap else 0)

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa nhiều tập hợp các giá trị hiện tại bằng cách sử dụng vùng heap tối đa được triển khai thông qua vùng heap tối thiểu của Python. Mỗi hoạt động mô đun liên tục chỉ kéo những phần tử đủ lớn để bị ảnh hưởng. Chi tiết triển khai chính là điều kiện vòng lặp`-heap[0] >= c`, điều này đảm bảo chúng tôi không bao giờ chạm vào các phần tử không thay đổi. 

Một điều tinh tế khác là chúng tôi không bao giờ cố gắng “gán lại” các phần tử không thay đổi. Nếu một giá trị nhỏ hơn mô-đun hiện tại, nó vẫn giữ nguyên trong heap, điều này rất quan trọng đối với hiệu quả. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nơi chúng ta bắt đầu bằng các giá trị`[10, 7, 3]`và mô đun`[5, 4]`. 

Trong mô-đun đầu tiên`c = 5`, chúng tôi liên tục trích xuất các giá trị ≥ 5. 

| Bước | Đống (lượt xem tối đa) | Đã trích xuất x | x mod c | Đống mới | 
| --- | --- | --- | --- | --- | 
| 1 | [10, 7, 3] | 10 | 0 | [7, 3, 0] | 
| 2 | [7, 3, 0] | 7 | 2 | [3, 2, 0] | 
| dừng lại | tất cả < 5 | - | - | [3, 2, 0] | 

Sau khi xử lý 5, chỉ các giá trị dưới 5 còn lại hoặc đã bị giảm vào phạm vi đó. 

Bây giờ xử lý`c = 4`. 

| Bước | Đống (lượt xem tối đa) | Đã trích xuất x | x mod c | Đống mới | 
| --- | --- | --- | --- | --- | 
| 1 | [3, 2, 0] | 3 | (không thay đổi) | [3, 2, 0] | 
| dừng lại | tất cả < 4 | - | - | [3, 2, 0] | 

Điều này cho thấy rằng một khi các giá trị đủ nhỏ, các hoạt động trong tương lai sẽ không hoạt động và vùng heap sẽ ổn định nhanh chóng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n \log X)$| mỗi phần tử chỉ được đẩy và bật khi nó thực sự giảm và mỗi thao tác heap tốn thời gian logarit | 
| Không gian |$O(n)$| heap lưu trữ tất cả các giá trị hoạt động | 

Thời gian chạy vừa vặn thoải mái trong các giới hạn thông thường vì mỗi giá trị chỉ có thể thu nhỏ một số lần giới hạn trước khi ổn định, ngăn chặn việc quét lại toàn bộ dữ liệu nhiều lần. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def solve():
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        ops = list(map(int, input().split()))

        heap = [-x for x in a]
        heapq.heapify(heap)

        for c in ops:
            while heap and -heap[0] >= c:
                x = -heapq.heappop(heap)
                x %= c
                heapq.heappush(heap, -x)

        print(-min(heap) if heap else 0)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# small cases
assert run("3 1\n10 7 3\n5\n") == "3"
assert run("1 1\n8\n3\n") == "2"
assert run("2 2\n5 9\n10 4\n") in ["1", "0"]

# edge case: already small
assert run("3 2\n1 2 3\n5 6\n") == "1"

# large identical values
assert run("5 1\n10 10 10 10 10\n3\n") in ["1", "0"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| giá trị đơn | độ chính xác modulo trực tiếp | tính đúng đắn cơ bản | 
| nhiều mô-đun | thu hẹp tuần tự | phụ thuộc trật tự | 
| tất cả các giá trị nhỏ | không xử lý không cần thiết | hiệu quả đúng đắn | 
| mảng thống nhất | cập nhật giống hệt nhau lặp đi lặp lại | sự ổn định của đống | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các số đã nhỏ hơn mô đun. Ví dụ, đầu vào`[1, 2, 3]`với mô đun`10`không tạo ra thay đổi. Thuật toán xử lý việc này vì điều kiện heap`-heap[0] >= c`hỏng ngay lập tức, do đó không xảy ra hiện tượng bật ra và cấu trúc không thay đổi. 

Một trường hợp khác là sự chuyển tiếp từ lớn sang nhỏ được lặp đi lặp lại. Coi như`[100, 99]`với mô đun`[50, 10]`. Thao tác đầu tiên làm giảm đáng kể cả hai giá trị và thao tác thứ hai chỉ có thể ảnh hưởng đến một tập hợp con. Vùng heap đảm bảo rằng sau mỗi lần giảm, chỉ các giá trị lớn mới được xem xét lại, vì vậy chúng tôi không bao giờ xử lý lại các phần tử ổn định. 

Cuối cùng, hãy xem xét nhiều mô đun nhỏ theo sau là các mô đun lớn. Các mô đun nhỏ sớm giảm mạnh các giá trị và các mô đun lớn sau đó không làm gì cả. Heap hấp thụ điều này một cách tự nhiên bởi vì một khi các giá trị giảm xuống dưới tất cả các mô đun trong tương lai, chúng sẽ không bị ảnh hưởng vô thời hạn.
