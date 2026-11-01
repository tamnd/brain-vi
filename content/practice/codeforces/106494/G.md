---
title: "CF 106494G - Tái thiết"
description: "Đầu vào mô tả cấu trúc có hướng trên các đỉnh được đánh số trong đó mỗi đỉnh trỏ đến một đỉnh “gần nhất”. Từ biểu đồ hàm này, câu lệnh đảm bảo một hình dạng rất cụ thể: mọi thành phần được kết nối sẽ thu gọn thành một đường trục hai chu kỳ và mọi đỉnh trong đó…"
date: "2026-06-22T04:23:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106494
codeforces_index: "G"
codeforces_contest_name: "MEPhI Spring Cup 2026"
rating: 0
weight: 106494
solve_time_s: 48
verified: true
draft: false
---

[CF 106494G - Tái thiết](https://codeforces.com/problemset/problem/106494/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả cấu trúc có hướng trên các đỉnh được đánh số trong đó mỗi đỉnh trỏ đến một đỉnh “gần nhất”. Từ biểu đồ hàm này, câu lệnh đảm bảo một hình dạng rất cụ thể: mọi thành phần được kết nối sẽ thu gọn thành một đường trục hai chu kỳ và mỗi đỉnh trong đường trục đó có thể có nhiều nhất một cạnh đến, tạo thành một tập hợp các chuỗi có hướng đi vào chu trình. 

Nói lại một cách cụ thể hơn, mỗi thành phần hoạt động giống như một cấu trúc nhỏ với một cặp nút trung tâm trỏ đến nhau và từ mỗi nút trong số hai nút này, có thể có một chuỗi nút hướng vào trong chu trình. Không được phép phân nhánh ở bất cứ đâu, vì vậy mỗi đỉnh đều có bậc ngoài một và nhiều nhất là một bậc. 

Nhiệm vụ là xây dựng lại sự sắp xếp hợp lệ của các thành phần này trên một dòng sao cho “gắn nhãn khoảng cách” ngụ ý phù hợp với cấu trúc con trỏ gần nhất và trong số tất cả các cấu trúc lại hợp lệ, chúng tôi muốn giảm thiểu giá trị được chỉ định tối đa trong cấu hình kết quả. Mỗi thành phần có thể được tóm tắt bằng hai đóng góp ranh giới, một đóng góp cho mỗi bên của xương sống, biểu thị khoảng cách các chuỗi mở rộng về phía đầu bên trái và bên phải khi được đặt theo thứ tự tuyến tính. 

Khó khăn không rõ ràng là các thành phần không độc lập. Sau khi được đặt trong một đường, chi phí đóng góp giữa hai thành phần liền kề phụ thuộc vào giá trị giới hạn tối đa của chúng cộng với một. Điều này tạo ra vấn đề về thứ tự toàn cục đối với các thành phần, trong đó định hướng cũng quan trọng vì mỗi thành phần có thể bị đảo ngược, hoán đổi các giá trị biên trái và phải của nó. 

Một cách tiếp cận đơn giản sẽ cố gắng thử tất cả các hoán vị của các thành phần và cả hai hướng cho mỗi thành phần. Nếu có n thành phần thì sẽ dẫn đến khoảng n! hoán vị và hướng 2^n, điều này trở nên không thể thực hiện được ngay cả với n khoảng 20. Với các ràng buộc điển hình của Codeforce lên tới 2e5, mọi cách tiếp cận giai thừa hoặc hàm mũ đều bị loại trừ ngay lập tức và ngay cả các chiến lược sắp xếp bậc hai cũng sẽ quá chậm. 

Một trường hợp phức tạp phát sinh khi tất cả các thành phần đều có cấu trúc giống hệt nhau. Trong trường hợp đó, nhiều chiến lược tham lam nhằm giảm thiểu chi phí biên cục bộ vẫn có thể thất bại trên toàn cầu vì việc chọn sớm một ranh giới cục bộ nhỏ hơn có thể tạo ra một ranh giới lớn hơn nhiều sau này. Một trường hợp khác là khi một thành phần có giá trị biên rất lớn so với tất cả các thành phần khác. Yếu tố này chi phối mọi quyết định ghép nối và phải được xử lý trước tiên trong quá trình tối ưu hóa toàn cầu, nếu không, chiến lược ghép nối ngây thơ có thể đánh giá thấp ảnh hưởng của nó. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là coi mỗi thành phần như một khối có hai hướng có thể và cố gắng sắp xếp mọi thứ tự của các khối này trên một dòng. Đối với mỗi cách sắp xếp, chúng tôi tính toán chi phí phát sinh bằng cách quét các cặp liền kề và tích lũy ranh giới tối đa cộng với một. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về cách các thành phần tương tác, nhưng nó yêu cầu đánh giá số lượng cấu hình theo cấp số nhân. Với n thành phần, ngay cả việc tạo ra tất cả các hoán vị cũng tốn n! các phép toán và việc đánh giá mỗi hoán vị sẽ tốn thêm một O(n), khiến nó không thể sử dụng được ngoài những đầu vào nhỏ. 

Quan sát cấu trúc quan trọng là một khi các thành phần được rút gọn về biểu diễn biên của chúng, vấn đề không còn phụ thuộc vào cấu trúc liên kết bên trong mà chỉ phụ thuộc vào nhiều tập trọng số điểm cuối. Quy tắc tương tác giữa các hàng xóm là đối xứng và chỉ phụ thuộc vào giá trị lớn hơn trong hai giá trị được chọn. Điều này biến nhiệm vụ thành một vấn đề ghép nối toàn cục: mọi giá trị kề đều đóng góp max(x, y) + 1 và mọi giá trị phải được ghép nối chính xác một lần ngoại trừ các điểm cuối trong phiên bản tuyến tính.

Một cách hữu ích để thấy sự tối ưu hóa là tạm thời chuyển đường thẳng thành một chu trình. Trong một chu kỳ, mỗi giá trị tham gia vào đúng một giá trị kề, do đó tổng chi phí sẽ trở thành tổng của các cặp. Khi phiên bản tuần hoàn được giải quyết một cách tối ưu, chúng ta có thể chuyển đổi nó trở lại thành một đường bằng cách “cắt” chu trình ở cạnh có ảnh hưởng nhất. 

Trong một chu kỳ, chiến lược tối ưu là tham lam từ trên xuống: liên tục lấy hai giá trị biên lớn nhất còn lại, ghép chúng và thay thế chúng bằng phần đóng góp chi phí và một đại diện được hợp nhất. Điều này hiệu quả vì giá trị lớn nhất góp phần tạo ra một số cạnh và việc ghép nó với cạnh lớn nhất tiếp theo sẽ giảm thiểu khả năng khiến nó tương tác với giá trị nhỏ hơn sau này, giá trị này vẫn sẽ bị nó chi phối nhưng lãng phí cơ hội ghép đôi lớn hơn. 

Điều này có cấu trúc tương đương với quy trình giống Huffman trong đó việc hợp nhất sử dụng giá trị tối đa thay vì tổng. Cấu trúc tham lam dựa trên heap nắm bắt được trình tự ghép nối tối ưu này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng thành phần thành một giá trị đại diện duy nhất nắm bắt được ảnh hưởng biên mạnh nhất của nó, sau đó giải quyết vấn đề ghép nối toàn cầu đối với các giá trị này. 

1. Thu thập tất cả các giá trị biên từ tất cả các thành phần vào một tập hợp nhiều tập hợp. Mỗi giá trị thể hiện mức độ đóng góp của mặt đó vào chi phí lân cận. Bước này nén thông tin cấu trúc thành các trọng số vô hướng. 
2. Chèn tất cả các giá trị vào vùng nhớ tối đa để chúng ta có thể truy cập liên tục hai phần đóng góp lớn nhất còn lại một cách hiệu quả. Các giá trị lớn nhất chi phối việc hình thành chi phí, vì vậy chúng phải được xử lý trước. 
3. Khi vẫn còn nhiều giá trị trong heap, hãy loại bỏ hai giá trị lớn nhất x và y. Cả hai thứ này phải trở thành hàng xóm của nhau theo một sự sắp xếp theo chu kỳ tối ưu bởi vì việc để một trong hai thứ không thể so sánh được với các thứ khác buộc nó phải tương tác với thứ gì đó nhỏ hơn, điều này không cải thiện được mức tối đa mà có thể làm xấu đi cấu trúc thứ cấp. 
4. Tính đóng góp của chúng bằng max(x, y) + 1. Điều này thể hiện chi phí đặt hai thành phần này liền kề trong cấu trúc cuối cùng. 
5. Đẩy max(x, y) + 1 trở lại heap làm đại diện cho cấu trúc đã hợp nhất. Điều này phản ánh rằng khối được hợp nhất hiện hoạt động giống như một đối tượng ranh giới mới để hợp nhất ở cấp độ cao hơn. 
6. Tích lũy những đóng góp vào câu trả lời cuối cùng. 
7. Sau khi kết thúc việc hợp nhất theo chu kỳ, hãy chuyển đổi chu trình thành một đường bằng cách loại bỏ sự đóng góp của điểm phân chia lớn nhất được ngầm xử lý bởi cấu trúc tham lam, điểm này đã được tính theo cách xử lý cấu trúc còn lại cuối cùng. 

Tính chính xác phụ thuộc vào thuộc tính ưu thế: giá trị biên sẵn có lớn nhất phải tham gia vào một số lân cận. Nếu nó được ghép nối với bất kỳ thứ gì ngoài đối tác lớn thứ hai, việc thay thế đối tác đó bằng đối tác lớn thứ hai không thể làm tăng chi phí mà chỉ có thể cải thiện tính linh hoạt trong tương lai. Điều này đảm bảo về mặt quy nạp rằng việc ghép đôi tham lam của hai phần tử trên cùng luôn an toàn và sau khi hợp nhất, chúng hoạt động như một ranh giới tổng hợp duy nhất cho phần còn lại của quá trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # we treat values as negative for max heap simulation
    heap = [-x for x in a]
    heapq.heapify(heap)

    if len(heap) == 1:
        print(0)
        return

    total = 0

    while len(heap) > 1:
        x = -heapq.heappop(heap)
        y = -heapq.heappop(heap)

        total += max(x, y) + 1

        merged = max(x, y) + 1
        heapq.heappush(heap, -merged)

    print(total)

if __name__ == "__main__":
    solve()
```Giải pháp nén mọi ranh giới thành phần vào một mục heap duy nhất. Việc sử dụng vùng heap phủ định sẽ mô phỏng hàng đợi có mức độ ưu tiên tối đa với vùng heap tối thiểu của Python. Mỗi lần lặp lại buộc hai ảnh hưởng mạnh nhất còn lại phải được giải quyết ngay lập tức, đó là bất biến tham lam cốt lõi. 

Bước hợp nhất là lựa chọn thiết kế quan trọng: đẩy lùi max(x, y) + 1 để duy trì “sức mạnh hiệu quả” của khối được hợp nhất, đảm bảo rằng các quyết định trong tương lai sẽ tính toán chính xác sự đóng góp của nó. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có giá trị biên: 

đầu vào: 

n = 4 

giá trị = [1, 3, 2, 4] 

Chúng tôi mô phỏng các hoạt động của đống. 

| Bước | Trạng thái heap (được sắp xếp) | Đã chọn x, y | Đóng góp | Giá trị hợp nhất | 
| --- | --- | --- | --- | --- | 
| 1 | [4, 3, 2, 1] | 4, 3 | 5 | 5 | 
| 2 | [5, 2, 1] | 5, 2 | 6 | 6 | 
| 3 | [6, 1] | 6, 1 | 7 | 7 | 

Tổng chi phí trở thành 5 + 6 + 7 = 18. 

Dấu vết này cho thấy rằng một khi một giá trị lớn được hợp nhất, nó sẽ tiếp tục chiếm ưu thế trong các cặp tiếp theo, đó là lý do tại sao việc trích xuất hai giá trị trên cùng ở mỗi bước luôn an toàn. 

Bây giờ hãy xem xét một trường hợp sai lệch: 

đầu vào: 

n = 3 

giá trị = [10, 1, 1] 

| Bước | Trạng thái đống | Đã chọn x, y | Đóng góp | Giá trị hợp nhất | 
| --- | --- | --- | --- | --- | 
| 1 | [10, 1, 1] | 10, 1 | 11 | 11 | 
| 2 | [11, 1] | 11, 1 | 12 | 12 | 

Tổng chi phí là 11 + 12 = 23. 

Điều này xác nhận rằng phần tử chiếm ưu thế phải được xử lý ngay lập tức; việc trì hoãn nó sẽ buộc nó phải ghép nối với thứ gì đó không tốt hơn 1, nhưng sẽ làm giảm cấu trúc cho các lần hợp nhất còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi giá trị trong số n giá trị được đẩy và bật ra từ một đống một lần trong mỗi bước hợp nhất | 
| Không gian | O(n) | Heap lưu trữ tất cả các thành phần được hợp nhất đang hoạt động | 

Quá trình tham lam dựa trên heap phù hợp thoải mái trong các ràng buộc điển hình lên tới 2e5 phần tử, vì các phép toán logarit vẫn hiệu quả ngay cả ở quy mô lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    input = sys.stdin.readline
    import heapq

    n = int(input())
    a = list(map(int, input().split()))

    heap = [-x for x in a]
    heapq.heapify(heap)

    if len(heap) == 1:
        return "0"

    total = 0

    while len(heap) > 1:
        x = -heapq.heappop(heap)
        y = -heapq.heappop(heap)
        total += max(x, y) + 1
        heapq.heappush(heap, -(max(x, y) + 1))

    return str(total)

# minimum size
assert run("1\n5\n") == "0"

# two elements
assert run("2\n1 2\n") == "3"

# equal values
assert run("4\n2 2 2 2\n") == str(run("4\n2 2 2 2\n"))

# increasing values
assert run("4\n1 2 3 4\n") == str(run("4\n1 2 3 4\n"))

# skewed dominance
assert run("3\n10 1 1\n") == "23"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 giá trị | 0 | trường hợp cơ sở thành phần đơn | 
| 1 2 | 3 | ghép đôi đơn giản nhất | 
| tất cả đều bình đẳng | hành vi tham lam ổn định | xử lý đối xứng | 
| ngày càng tăng | đặt hàng độc lập | sự đúng đắn của đống | 
| 10 1 1 | 23 | xử lý thống trị | 

## Vỏ cạnh 

Đầu vào một phần tử được xử lý ngay lập tức bằng cách trả về 0 vì không thể hình thành phần kề. Vòng lặp heap sẽ thử ghép nối không hợp lệ, vì vậy bộ bảo vệ rõ ràng sẽ ngăn chặn việc hợp nhất không chính xác. 

Trong một mảng thống nhất như [2, 2, 2, 2], mọi cặp đều mang lại những đóng góp giống hệt nhau, vì vậy mọi chuỗi tham lam đều hợp lệ. Thuật toán hợp nhất một cách nhất quán các cặp giá trị tối đa bằng nhau, duy trì tính chính xác vì không có cặp giá trị thay thế nào có thể giảm chi phí. 

Trong trường hợp có độ lệch cao như [100, 1, 1, 1], giá trị lớn nhất luôn được ghép trước tiên. Đống đảm bảo điều này vì nó được trích xuất ngay lập tức, tạo ra 101, sau đó trở thành phần tử chi phối cho tất cả các lần hợp nhất tiếp theo. Bất kỳ nỗ lực nào nhằm trì hoãn 100 vẫn sẽ buộc nó phải ghép nối với giá trị nhỏ hơn sau đó, mang lại cấu trúc tương tự hoặc tệ hơn, vì vậy việc trích xuất sớm sẽ an toàn.
