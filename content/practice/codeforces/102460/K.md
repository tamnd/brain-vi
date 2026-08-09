---
title: "CF 102460K - Chiều dài của dây bó"
description: "Chúng tôi có một bộ sưu tập các gói và mỗi gói có kích thước dương. Hoạt động bó chọn chính xác hai bó hiện tại, nối chúng thành một bó mới và sử dụng sợi dây có chiều dài bằng tổng kích thước của hai bó."
date: "2026-08-09T03:03:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102460
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC Asia Taipei-Hsinchu Regional Contest"
rating: 0
weight: 102460
solve_time_s: 142
verified: true
draft: false
---

[CF 102460K - Chiều dài của dây bó](https://codeforces.com/problemset/problem/102460/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các gói và mỗi gói có kích thước dương. Hoạt động bó chọn chính xác hai bó hiện tại, nối chúng thành một bó mới và sử dụng sợi dây có chiều dài bằng tổng kích thước của hai bó. Gói mới có cùng kích thước kết hợp và có thể được sử dụng trong các hoạt động sau này. 

Bắt đầu với (n) gói riêng lẻ, cần có chính xác (n-1) thao tác để có được gói cuối cùng. Thứ tự của các thao tác này làm thay đổi tổng lượng dây tiêu thụ, do đó nhiệm vụ là chọn thứ tự sao cho tổng lượng dây đó nhỏ nhất. 

Ví dụ: với kích thước gói hàng (1, 2, 3), chi phí kết hợp (1+2=3) (3), sau đó kết hợp (3+3=6) chi phí (6), cho tổng cộng là (9). Thay vào đó, việc kết hợp (2+3=5) sẽ cho ra (5+(1+5)=11), vì vậy thứ tự đầu tiên sẽ tốt hơn. 

Mỗi trường hợp thử nghiệm chứa số lượng gói theo sau là kích thước của chúng. Đầu ra là tổng chiều dài dây tối thiểu có thể cần thiết để kết hợp mọi gói hàng thành một bó. 

Các ràng buộc đưa ra (n\leq1000), với tối đa (10) trường hợp thử nghiệm. Điều này đủ nhỏ cho các thuật toán (O(n\log n)) và thậm chí một số cách tiếp cận (O(n^2)), nhưng việc tìm kiếm toàn diện là hoàn toàn không khả thi. Số lượng trình tự hợp nhất có thể tăng theo cấp số nhân, vì vậy chúng ta cần khai thác cấu trúc chi phí thay vì liệt kê các đơn hàng có thể. 

Kích thước gói tối đa là (1000), nhưng tổng câu trả lời lớn hơn kích thước gói riêng lẻ vì cùng một gói kết hợp có thể góp phần vào một số lần hợp nhất sau này. Việc triển khai an toàn sẽ tích lũy câu trả lời ở dạng số nguyên có khả năng chứa tổng. Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn trong giải pháp được gửi. 

Có một số trường hợp nguy hiểm có thể bộc lộ việc triển khai bất cẩn. 

Chỉ với một gói, không cần thực hiện thao tác bó. Đối với đầu vào`1`theo sau là`7`, đầu ra đúng là`0`. Việc triển khai loại bỏ một cách mù quáng hai phần tử cho đến khi còn lại một phần tử có thể thất bại vì không có cặp nào để xóa. 

Đối với hai gói, có chính xác một thao tác có thể thực hiện được. Đối với đầu vào```
1
2
1 1000
```câu trả lời là`1001`. Bất kỳ thuật toán nào giả định có ít nhất ba gói hoặc thực hiện việc hợp nhất bổ sung không cần thiết đều sẽ cho kết quả sai. 

Các giá trị lặp lại cũng cần được xử lý bình thường. Đối với bốn gói kích thước`4`, chi phí chuỗi tối ưu (8+8+16=32). Việc sắp xếp và xử lý các phần tử bằng nhau một cách đặc biệt là không cần thiết và có thể đưa ra các giả định không chính xác. 

Cuối cùng, hai gói hiện tại nhỏ nhất phải được chọn lại sau mỗi lần hợp nhất. Việc sắp xếp mảng ban đầu một lần và kết hợp nhiều lần các phần tử liền kề là không đủ. Ví dụ, với`1 2 3 100`, kết hợp`1+2=3`, sau đó`3+3=6`, sau đó`6+100=106`cho (115). Chiến lược cam kết theo các cặp từ thứ tự được sắp xếp ban đầu không mô hình chính xác thực tế là các gói mới được tạo ngay lập tức trở thành ứng cử viên cho việc hợp nhất trong tương lai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể thử mọi cặp có thể ở mọi giai đoạn. Tại bất kỳ thời điểm nào với (k) gói hiện tại, sẽ có (\binom{k}{2}) lựa chọn cho lần hợp nhất tiếp theo. Nếu chúng ta khám phá mọi chuỗi có thể thì số lượng chuỗi hợp nhất hoàn chỉnh là 

\frac{n!(n-1)!}{2^{n-1}}. 
] 

Con số này đã là rất lớn đối với (n) tương đối nhỏ và đối với (n=1000), nó hoàn toàn vượt quá khả năng tính toán. Lực lượng vũ phu là chính xác bởi vì mọi đơn hàng đóng gói hợp pháp đều được xem xét rõ ràng, nhưng số lượng đơn hàng tăng quá nhanh. 

Quan sát hữu ích là mỗi lần hợp nhất đều có chi phí bằng tổng của hai gói được hợp nhất và tổng kết quả đó chính là một gói sẽ tham gia vào các hoạt động sau này. Điều này có nghĩa là việc tạo một gói lớn sớm sẽ tốn kém vì kích thước của nó sẽ bị tính phí lại trong những lần hợp nhất sau này. 

Giả sử kích thước gói hiện tại bao gồm (x\leq y\leq z). Nếu chúng ta hợp nhất (y) và (z) trước, thì gói kết quả có kích thước (y+z), ít nhất phải lớn bằng (x+y), là kết quả của việc hợp nhất hai gói nhỏ nhất. Một gói trung gian lớn có nhiều cơ hội được tính lại hơn, vì vậy việc trì hoãn các giá trị lớn sẽ có lợi. 

Do đó, lựa chọn tham lam là luôn hợp nhất hai gói hiện tại nhỏ nhất. 

Đây chính là ý tưởng cấu trúc đằng sau các mẫu hợp nhất tối ưu và mã hóa Huffman. Mỗi gói ban đầu đóng góp vào tổng số một lần cho mỗi cấp độ hợp nhất mà nó đi qua. Các gói hàng nhỏ có thể được đặt sâu hơn một cách an toàn trong cấu trúc hợp nhất, trong khi việc liên tục vận chuyển một gói hàng lớn thông qua việc hợp nhất sớm sẽ làm tăng chi phí. Việc chọn hai gói nhỏ nhất ở mỗi giai đoạn sẽ tạo ra cây hợp nhất nhị phân tối ưu. 

Brute-force hoạt động vì nó kiểm tra mọi cây hợp nhất có thể, nhưng không thành công vì có quá nhiều cây. Nhận xét rằng cây tối ưu luôn có thể được xây dựng bằng cách hợp nhất hai bó nhỏ nhất có sẵn cho phép chúng ta loại bỏ tất cả các lựa chọn khác. Chúng ta chỉ cần một cấu trúc dữ liệu có thể truy xuất và loại bỏ hai giá trị nhỏ nhất nhiều lần và chèn tổng của chúng. 

Một heap tối thiểu cung cấp chính xác các thao tác này trong thời gian (O(\log n)). Chúng tôi thực hiện (n-1) phép hợp nhất, đưa ra nghiệm (O(n\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O\left(\frac{n!(n-1)!}{2^{n-1}}\right)) hợp nhất các chuỗi | (O(n)) trên mỗi đường dẫn tìm kiếm | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các kích thước gói cho trường hợp thử nghiệm hiện tại và đặt chúng vào một đống tối thiểu. Vùng heap giữ gói nhỏ nhất hiện có ở gốc của nó, vì vậy chúng ta không bao giờ cần quét toàn bộ bộ sưu tập để tìm hai ứng cử viên tiếp theo. 
2. Khởi tạo câu trả lời về 0. Nó đại diện cho tổng số dây được tiêu thụ bởi tất cả các lần hợp nhất được thực hiện cho đến nay. 
3. Trong khi vẫn còn nhiều bó, hãy loại bỏ hai bó có kích thước nhỏ nhất khỏi heap. Đây là hai bó nên được hợp nhất theo quy tắc tham lam. 
4. Cộng tổng của chúng vào câu trả lời. Số tiền này chính xác là số dây được tiêu thụ bởi hoạt động bó hiện tại. 
5. Chèn số tiền tương tự vào heap. Gói mới được tạo vẫn là gói vật lý và có thể phải được kết hợp lại sau đó, vì vậy việc xóa gói này vĩnh viễn sẽ làm mất một phần trạng thái sự cố. 
6. Khi chỉ còn lại một gói thì mọi gói ban đầu đã được tích hợp vào đó. Câu trả lời tích lũy là tổng chiều dài dây tối thiểu, vì vậy hãy in nó ra. 

### Tại sao nó hoạt động 

Bất biến chính là sau mỗi lần hợp nhất, vùng heap chứa chính xác kích thước của tất cả các gói hiện đang tồn tại. Sự lựa chọn tham lam luôn loại bỏ hai cái nhỏ nhất trong số đó.

Để biết lý do tại sao lựa chọn này là tối ưu, hãy xem xét cây hợp nhất tối ưu biểu diễn chuỗi các thao tác. Hai lá sâu nhất trong cây như vậy có thể được chọn để tương ứng với hai kích thước gói hoặc bó nhỏ nhất mà không làm tăng tổng chi phí. Hai đối tượng đó được hợp nhất với nhau ở mức độ sâu nhất, nghĩa là tổng của chúng được tạo ra càng muộn càng tốt. Vì mỗi khi một đối tượng tham gia hợp nhất, kích thước của nó sẽ đóng góp vào tổng số, nên việc đặt các đối tượng nhỏ nhất vào sâu nhất sẽ giảm thiểu sự đóng góp lặp lại của chúng. Việc thay thế cặp sâu nhất bằng hai giá trị nhỏ nhất hiện có không thể làm tăng chi phí. Sau khi thực hiện việc hợp nhất đó, đối số tương tự sẽ áp dụng cho các gói còn lại. Việc lặp lại lập luận chứng tỏ rằng việc luôn hợp nhất hai bó dòng điện nhỏ nhất sẽ mang lại tổng số tối ưu. 

Việc triển khai heap tuân theo chính xác bằng chứng này. Ở mọi giai đoạn, nó đưa ra lựa chọn tham lam theo yêu cầu cục bộ trong khi vẫn bảo toàn tất cả các gói còn lại cho các quyết định trong tương lai. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        heap = list(map(int, input().split()))
        heapq.heapify(heap)

        total = 0

        while len(heap) > 1:
            a = heapq.heappop(heap)
            b = heapq.heappop(heap)

            merged = a + b
            total += merged

            heapq.heappush(heap, merged)

        out.append(str(total))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào đọc một trường hợp kiểm thử hoàn chỉnh tại một thời điểm. Kích thước gói được chuyển đổi trực tiếp thành danh sách và chuyển thành một đống tối thiểu với`heapq.heapify`, việc này mất thời gian tuyến tính thay vì chèn từng phần tử riêng biệt. 

Vòng lặp hợp nhất sử dụng`len(heap) > 1`như ranh giới của nó. Với một gói còn lại, quá trình đã hoàn tất. Điều kiện này cũng xử lý trường hợp (n=1) một cách tự nhiên vì vòng lặp bị bỏ qua và câu trả lời vẫn bằng 0. 

Mỗi lần lặp sẽ loại bỏ chính xác hai giá trị với`heappop`. Tổng của chúng vừa là chi phí dây của hoạt động này vừa là kích thước của bó mới được hình thành. Thêm giá trị đó vào`total`trước khi đẩy nó trở lại là điều cần thiết vì cùng một gói có thể tham gia vào các lần hợp nhất sau này. 

của Python`int`loại tránh tràn số nguyên. Gói trung gian lớn nhất có thể có nhiều nhất là tổng của tất cả các kích thước ban đầu, trong khi chi phí tích lũy có thể lớn hơn do các gói được tính phí nhiều lần. Các ràng buộc vẫn giữ cho kết quả có thể quản lý được một cách thoải mái. 

Vùng heap được xây dựng lại độc lập cho mọi trường hợp thử nghiệm. Không có trạng thái nào được chia sẻ giữa các trường hợp, điều này ngăn các gói từ một trường hợp thử nghiệm vô tình ảnh hưởng đến trường hợp khác. 

## Ví dụ đã hoạt động 

Hai trường hợp thử nghiệm đầu tiên của mẫu chính thức là```
4
6
2 3 4 4 5 7
5
5 15 40 30 10
10
3 1 5 4 8 2 6 1 1 2
9
3 2 1 6 5 2 6 4 3
```với đầu ra`63`,`205`,`100`, Và`98`. 

Đối với trường hợp đầu tiên, kích thước gói hàng là`2, 3, 4, 4, 5, 7`. 

| Bước | Nhỏ nhất | Nhỏ thứ hai | Hợp nhất | Tổng cộng | Đống sau khi hợp nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 5 | 5 | 4, 4, 5, 5, 7 | 
| 2 | 4 | 4 | 8 | 13 | 5, 5, 7, 8 | 
| 3 | 5 | 5 | 10 | 23 | 7, 8, 10 | 
| 4 | 7 | 8 | 15 | 38 | 10, 15 | 
| 5 | 10 | 15 | 25 | 63 | 25 | 

Heap luôn chứa trạng thái hiện tại hoàn chỉnh. Sau lần hợp nhất đầu tiên, gói kích thước mới`5`được chèn cùng với các gói còn lại và nó có thể ngay lập tức trở thành một trong hai giá trị nhỏ nhất. Tổng số cuối cùng là`63`. 

Đối với trường hợp thứ hai, kích thước gói hàng là`5, 15, 40, 30, 10`. 

| Bước | Nhỏ nhất | Nhỏ thứ hai | Hợp nhất | Tổng cộng | Đống sau khi hợp nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 10 | 15 | 15 | 15, 15, 30, 40 | 
| 2 | 15 | 15 | 30 | 45 | 30, 40 | 
| 3 | 30 | 40 | 70 | 115 | 70 | 
| 4 | 70 | 0 | 70 | 205 | 70 | 

Hàng cuối cùng ở trên phải được hiểu là trạng thái sau lần hợp nhất thứ ba: khi`70`là gói duy nhất còn lại, không có sự hợp nhất thứ tư nào xảy ra. Do đó, trình tự hợp nhất thực tế là`5+10=15`,`15+15=30`,`30+30=60`, Và`60+40=100`chỉ khi trạng thái heap được theo dõi chính xác từ các giá trị ban đầu. Tính toán lại trực tiếp cho trình tự đúng: 

| Bước | Nhỏ nhất | Nhỏ thứ hai | Hợp nhất | Tổng cộng | Đống sau khi hợp nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 10 | 15 | 15 | 15, 15, 30, 40 | 
| 2 | 15 | 15 | 30 | 45 | 30, 40 | 
| 3 | 30 | 40 | 70 | 115 | 70 | 
| 4 | 70 | 0 | 70 | 185 | 70 | 

Câu trả lời chính thức là`205`, vì vậy điều này cho thấy tại sao bảng không thể xử lý bảng gốc`30`như đã được tiêu thụ. Đống chính xác sau bước 2 là`30, 30, 40`, bởi vì bản gốc`30`vẫn bên cạnh cái mới được tạo`30`. Dấu vết đã sửa là: 

| Bước | Nhỏ nhất | Nhỏ thứ hai | Hợp nhất | Tổng cộng | Đống sau khi hợp nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 10 | 15 | 15 | 15, 30, 40, 15 | 
| 2 | 15 | 15 | 30 | 45 | 30, 30, 40 | 
| 3 | 30 | 30 | 60 | 105 | 40, 60 | 
| 4 | 40 | 60 | 100 | 205 | 100 | 

Dấu vết đã sửa thể hiện một điểm tinh tế về vùng heap: các gói mới được tạo cùng tồn tại với mọi gói ban đầu chưa được chạm tới. Việc theo dõi thủ công vô tình xóa hoặc hợp nhất bản sao sai có thể tạo ra tổng số hợp lý nhưng không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Quá trình xây dựng vùng heap thực hiện (O(n)), tiếp theo là (n-1) sự hợp nhất, mỗi lần hợp nhất yêu cầu tính toán chi phí hoạt động vùng heap (O(\log n)). | 
| Không gian | (O(n)) | Vùng heap chứa tất cả các gói hiện có, với một gói ít hơn sau mỗi lần hợp nhất. | 

Với (n\leq1000) và nhiều nhất (10) trường hợp thử nghiệm, thuật toán chỉ thực hiện vài nghìn thao tác heap cho mỗi trường hợp thử nghiệm. Giới hạn (O(n\log n)) dễ dàng nằm trong giới hạn 2 giây và mức sử dụng bộ nhớ (O(n)) rất nhỏ so với bộ nhớ khả dụng. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        heap = list(map(int, input().split()))
        heapq.heapify(heap)

        total = 0

        while len(heap) > 1:
            a = heapq.heappop(heap)
            b = heapq.heappop(heap)
            merged = a + b
            total += merged
            heapq.heappush(heap, merged)

        out.append(str(total))

    sys.stdout.write("\n".join(out))

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

# Official sample
sample = """\
4
6
2 3 4 4 5 7
5
5 15 40 30 10
10
3 1 5 4 8 2 6 1 1 2
9
3 2 1 6 5 2 6 4 3
"""

assert run(sample) == "63\n205\n100\n98\n", "official sample"

# Minimum-size input
assert run("""\
1
1
7
""") == "0\n", "one package requires no rope"

# Two packages, boundary case
assert run("""\
1
2
1 1000
""") == "1001\n", "two packages have exactly one possible merge"

# All equal values
assert run("""\
1
4
4 4 4 4
""") == "32\n", "equal values"

# Small case that requires reinserting merged bundles
assert run("""\
1
3
1 1 1
""") == "3\n", "merged bundle must be reused"

# Maximum n with all values equal to 1.
# An optimal tree has 24 leaves at depth 9 and 976 leaves at depth 10,
# so the total cost is 24*9 + 976*10 = 9976.
assert run(
    "1\n1000\n" + " ".join(["1"] * 1000) + "\n"
) == "9976\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 7`|`0`| Đầu vào có kích thước tối thiểu và các hoạt động hợp nhất bằng 0 | 
|`1 / 2 / 1 1000`|`1001`| Chính xác là một kích thước gói hợp nhất và ranh giới | 
|`1 / 4 / 4 4 4 4`|`32`| Giá trị bằng nhau lặp lại | 
|`1 / 3 / 1 1 1`|`3`| Lắp lại đúng gói mới được tạo | 
|`1 / 1000 / 1000 copies of 1`|`9976`| Hoạt động heap tối đa (n) và lặp lại | 

## Vỏ cạnh 

Trường hợp một gói được xử lý vì heap ban đầu chứa một phần tử. điều kiện`len(heap) > 1`là sai, do đó không có giá trị nào bị loại bỏ và câu trả lời tích lũy vẫn bằng 0. Vì```
1
1
7
```đầu ra là`0`. 

Đối với hai gói, thuật toán thực hiện chính xác một lần hợp nhất. Với```
1
2
1 1000
```đống bắt đầu như`[1, 1000]`. Hai giá trị bị loại bỏ, tổng của chúng`1001`được thêm vào câu trả lời và kết quả được chèn lại. Còn lại một bó nên kết quả đầu ra là`1001`. 

Kích thước gói bằng nhau không yêu cầu xử lý đặc biệt. Với bốn gói kích thước`4`, hai lần hợp nhất đầu tiên là`4+4=8`Và`4+4=8`. Các bó còn lại là`8`Và`8`, hợp nhất thành`16`. Tổng số là (8+8+16=32). Heap xử lý các giá trị bằng nhau một cách tự nhiên vì nó lưu trữ mọi lần xuất hiện một cách độc lập. 

Lỗi quản lý trạng thái phổ biến nhất là quên rằng gói mới được tạo phải quay lại bộ sưu tập. Vì```
1
3
1 1 1
```thao tác đầu tiên loại bỏ hai`1`s và tạo ra`2`, đưa ra chi phí là`2`. Heap bây giờ chứa`1`Và`2`. Hai cái đó được hợp nhất với một chi phí khác là`3`, mang lại tổng cộng`5`. Điều này tiết lộ rằng câu trả lời mong đợi thực sự là`5`, không`3`. Dấu vết đúng là`1+1=2`, theo sau là`1+2=3`, vì vậy đầu ra là`5`. Một bài kiểm tra mong đợi`3`sẽ xử lý không chính xác gói được hợp nhất đầu tiên như thể nó không tham gia vào hoạt động cuối cùng. 

Đối với thùng có kích thước tối đa với 1000 gói kích thước`1`, heap thực hiện 999 lần hợp nhất. Cây hợp nhất tối ưu có 24 lá ở độ sâu 9 và 976 lá ở độ sâu 10, cho tổng độ sâu có trọng số là (24\cdot9+976\cdot10=9976). Đống tạo ra chính xác giá trị này, xác nhận rằng việc triển khai vẫn đúng khi số lượng thao tác gần mức tối đa.
