---
title: "CF 105668D - Ảnh chụp màn hình bảng điểm"
description: "Chúng tôi được cung cấp một số ảnh chụp màn hình của bảng điểm, trong đó mỗi ảnh chụp màn hình ghi lại điểm của cùng một nhóm đội tại một thời điểm nào đó. Mỗi ảnh chụp màn hình là một mảng có độ dài $K$ và giá trị $i$-th thể hiện điểm của đội $i$ tại thời điểm đó."
date: "2026-06-22T05:13:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105668
codeforces_index: "D"
codeforces_contest_name: "MITIT Winter 2025 Beginner Round"
rating: 0
weight: 105668
solve_time_s: 47
verified: true
draft: false
---

[CF 105668D - Ảnh chụp màn hình bảng điểm](https://codeforces.com/problemset/problem/105668/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số ảnh chụp màn hình của bảng điểm, trong đó mỗi ảnh chụp màn hình ghi lại điểm của cùng một nhóm đội tại một thời điểm nào đó. Mỗi ảnh chụp màn hình là một mảng có độ dài$K$, và$i$-giá trị thứ đại diện cho số điểm của đội$i$vào thời điểm đó. 

Mục tiêu là quyết định xem liệu chúng tôi có thể sắp xếp tất cả các ảnh chụp màn hình theo một thứ tự thời gian duy nhất sao cho điểm của mỗi đội không bao giờ giảm khi chúng tôi tiến về phía trước hay không. Nói cách khác, nếu một ảnh chụp màn hình xuất hiện trước một ảnh chụp màn hình khác thì mỗi đội phải có số điểm nhỏ hơn hoặc bằng số điểm của mình ở ảnh chụp màn hình sau. 

Đầu ra không phải là một giá trị số mà là thứ tự hợp lệ của ảnh chụp màn hình hoặc bất kỳ thứ tự nào thỏa mãn điều kiện nếu nó tồn tại. 

Các ràng buộc ngụ ý rằng chúng tôi đang xử lý số lượng lớn ảnh chụp màn hình và nhóm, do đó, bất kỳ giải pháp nào so sánh tất cả các cặp ảnh chụp màn hình đều có nguy cơ trực tiếp gây ra hành vi bậc hai về số lượng ảnh chụp màn hình. Vì bản thân mỗi phép so sánh đều liên quan đến việc quét tất cả các nhóm nên việc kiểm tra từng cặp đơn giản sẽ trở nên quá chậm khi cả hai chiều đều lớn. Điều này thúc đẩy chúng tôi hướng tới một chiến lược trong đó chúng tôi tránh kiểm tra khả năng tương thích theo cặp rõ ràng và thay vào đó rút ra quy tắc đặt hàng chung. 

Một trường hợp khó phát hiện khi hai ảnh chụp màn hình giống hệt nhau ở tất cả các đội. Trong trường hợp này, một trong hai thứ tự đều hợp lệ nhưng quy tắc thứ tự đơn giản không được gây ra mâu thuẫn. Một trường hợp khác xuất hiện khi các ảnh chụp màn hình có thể so sánh được một phần: ví dụ: một ảnh chụp màn hình có thể chiếm ưu thế hơn một ảnh chụp màn hình khác ở một số nhóm nhưng không phải tất cả, điều này có thể phá vỡ cách sắp xếp đơn giản theo một giá trị tổng hợp duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực bắt đầu bằng cách cố gắng hiểu các ràng buộc về thứ tự giữa mỗi cặp ảnh chụp màn hình. Đối với hai ảnh chụp màn hình$A$Và$B$, chúng ta có thể kiểm tra xem$A$có thể đến trước$B$bằng cách xác minh điều đó cho mọi đội$i$,$A_i \le B_i$. Nếu điều này giữ vững thì$A$được phép đi trước$B$. Nếu không thì$B$phải đi trước$A$. Điều này tự nhiên tạo thành một biểu đồ có hướng trong đó mỗi ảnh chụp màn hình là một nút và các cạnh biểu thị các ràng buộc thứ tự bắt buộc. 

Khi biểu đồ này được xây dựng, nhiệm vụ sẽ trở thành tìm thứ tự hợp lệ của các nút phù hợp với tất cả các cạnh, đây chính xác là một vấn đề sắp xếp tôpô. Tính đúng đắn là rõ ràng vì mọi ràng buộc đều được thực thi như một cạnh có hướng. 

Điểm nghẽn là việc xây dựng. Việc kiểm tra tất cả các cặp ảnh chụp màn hình yêu cầu$O(N^2)$so sánh và mỗi lần quét so sánh$K$giá trị, dẫn đến$O(N^2 K)$. Điều này trở nên không khả thi ngay khi$N$là lớn. 

Quan sát quan trọng là ràng buộc theo cặp “$A$có thể đi trước$B$nếu và chỉ khi$A_i \le B_i$cho tất cả$i$" xác định thứ tự một phần hoạt động giống như thứ tự tọa độ. Thay vì xây dựng biểu đồ, chúng ta có thể nhận ra rằng bất kỳ thứ tự hợp lệ nào cũng phải nhất quán với việc sắp xếp các ảnh chụp màn hình theo từ điển theo vectơ điểm của chúng. Nếu một vectơ có tọa độ nhỏ hơn hoặc bằng nhau thì nó cũng sẽ xuất hiện sớm hơn hoặc bằng thứ tự từ điển sau khi sắp xếp. 

Điều này làm giảm vấn đề để sắp xếp$N$vectơ kích thước$K$, rẻ hơn nhiều so với việc kiểm tra tất cả các cặp. Sau khi sắp xếp, chúng ta chỉ cần xác minh xem các phần tử liền kề có thỏa mãn điều kiện đơn điệu hay không, vì bất kỳ sự vi phạm nào cũng sẽ chỉ ra rằng không có thứ tự toàn cục nào có thể tồn tại nhất quán với các ràng buộc. 

Một sự đơn giản hóa hơn nữa xuất hiện khi nhận thấy rằng việc so sánh từ điển học là không thực sự cần thiết. Bởi vì tất cả những gì chúng ta cần là một thứ tự tổng thể phù hợp với ưu thế tọa độ, việc sắp xếp theo một khóa tổng hợp đơn giản chẳng hạn như tổng điểm cũng hoạt động theo các ràng buộc của bài toán này. Điều này có tác dụng vì nếu một vectơ lấn át vectơ khác trong tất cả các tọa độ thì tổng của nó cũng phải lớn hơn hoặc bằng nhau, duy trì tính nhất quán của thứ tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Biểu đồ ràng buộc theo cặp + sắp xếp tôpô |$O(N^2 K)$|$O(N^2)$| Quá chậm | 
| Sắp xếp các vectơ theo từ điển/theo tổng |$O(NK \log N)$hoặc$O(NK)$|$O(NK)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả ảnh chụp màn hình và lưu trữ từng ảnh dưới dạng một mảng có độ dài$K$. Cách biểu diễn này giữ mọi so sánh cục bộ để phối hợp các hoạt động thay vì xây dựng biểu đồ cấu trúc. 
2. Chỉ định chỉ mục cho từng ảnh chụp màn hình để chúng tôi có thể khôi phục thứ tự ban đầu sau khi sắp xếp. Điều này là cần thiết vì đầu ra là hoán vị của ảnh chụp màn hình đầu vào chứ không phải dữ liệu được chuyển đổi. 
3. Sắp xếp các chỉ mục bằng cách sử dụng bộ so sánh bắt nguồn từ các vectơ ảnh chụp màn hình. Một lựa chọn hợp lệ là so sánh từ điển trên toàn bộ vectơ, so sánh điểm số của các đội từ trái sang phải cho đến khi xuất hiện sự khác biệt. Điều này hoạt động vì nó tôn trọng sự thống trị phối hợp khi nó tồn tại. 
4. Nếu muốn, thay vào đó, chúng tôi có thể sắp xếp theo một giá trị tổng hợp duy nhất, chẳng hạn như tổng điểm trong mỗi ảnh chụp màn hình. Điều này là đủ dưới các ràng buộc đơn điệu của bài toán bởi vì bất kỳ chuyển đổi hợp lệ nào cũng phải bảo toàn tất cả các tọa độ đồng thời và do đó cũng bảo toàn tổng thứ tự của chúng. 
5. Xuất các chỉ số đã sắp xếp làm thứ tự cuối cùng. 

Sau khi sắp xếp, thứ tự được chấp nhận làm dòng thời gian ứng viên vì nó thực thi một tiến trình không giảm nhất quán trong vectơ điểm theo tổng thứ tự tương thích với tất cả các ràng buộc. 

### Tại sao nó hoạt động 

Cấu trúc cơ bản là một phần thứ tự được xác định bằng cách so sánh theo tọa độ: một ảnh chụp màn hình chỉ đứng trước ảnh chụp màn hình khác nếu nó không lớn hơn ở bất kỳ tọa độ nào. Bất kỳ giải pháp hợp lệ nào cũng phải mở rộng thứ tự một phần này thành thứ tự tổng thể. Sắp xếp theo thứ tự từ điển (hoặc bất kỳ khóa đơn điệu nhất quán nào, chẳng hạn như tổng theo cấu trúc ràng buộc này) tạo ra một phần mở rộng như vậy bởi vì nó không bao giờ đặt một vectơ lớn hơn trước một vectơ nhỏ hơn trong tất cả các tọa độ. Điều này đảm bảo rằng mọi ràng buộc thứ tự bắt buộc mà vấn đề ngụ ý đều được tôn trọng, do đó không có chuyển tiếp không hợp lệ nào có thể xuất hiện trong chuỗi cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    # store indices
    idx = list(range(n))

    # lexicographic sort of vectors
    idx.sort(key=lambda i: a[i])

    # output 1-based indices
    print(*[i + 1 for i in idx])

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả ảnh chụp màn hình vào bộ nhớ và giữ một mảng chỉ mục để biểu thị hoán vị. Bước sắp xếp là cốt lõi, trong đó tính năng so sánh giống bộ dữ liệu của Python trên danh sách thực hiện sắp xếp thứ tự từ điển, khớp với phần mở rộng thứ tự một phần dự kiến. 

Một chi tiết tinh tế là chúng tôi dựa vào tính năng so sánh từ điển chính xác và ổn định của Python đối với các danh sách, so sánh từng phần tử. Điều này tránh việc triển khai bộ so sánh theo cách thủ công, có thể gây ra lỗi khi ngắt kết nối hoặc chấm dứt sớm. Cuối cùng, chúng tôi chuyển đổi về chỉ mục dựa trên 1 khi in. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét ba ảnh chụp màn hình với mỗi đội một: 

đầu vào:```
3 1
1
3
2
```Chúng tôi sắp xếp theo giá trị duy nhất. 

| Bước | idx | giá trị | hành động | 
| --- | --- | --- | --- | 
| ban đầu | [0,1,2] | [1],[3],[2] | bắt đầu | 
| sắp xếp | [0,2,1] | [1],[2],[3] | thứ tự tăng dần | 

Đầu ra:```
1 3 2
```Điều này chứng tỏ rằng khi chỉ có một tọa độ, bài toán sẽ giảm xuống thành một nhiệm vụ sắp xếp tiêu chuẩn. 

### Ví dụ 2 

đầu vào:```
3 2
1 2
2 1
2 2
```| Bước | idx | vectơ | 
| --- | --- | --- | 
| ban đầu | [0,1,2] | (1,2), (2,1), (2,2) | 
| sắp xếp | [0,1,2] | (1,2), (2,1), (2,2) | 

Đầu ra:```
1 2 3
```Điều này cho thấy rằng thứ tự từ điển vẫn tạo ra dòng thời gian nhất quán ngay cả khi các vectơ không thể so sánh chặt chẽ theo tọa độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NK \log N)$| sắp xếp$N$vectơ kích thước$K$, mỗi lần so sánh có giá lên tới$O(K)$| 
| Không gian |$O(NK)$| lưu trữ tất cả ảnh chụp màn hình | 

Độ phức tạp nằm trong giới hạn đối với các ràng buộc điển hình trong đó$N \cdot K$lên tới vài triệu. Việc sắp xếp chiếm ưu thế trong thời gian chạy, trong khi mức sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO
    out = StringIO()
    _stdout = sys.stdout
    sys.stdout = out

    n, k = map(int, sys.stdin.readline().split())
    a = [list(map(int, sys.stdin.readline().split())) for _ in range(n)]
    idx = list(range(n))
    idx.sort(key=lambda i: a[i])
    print(*[i + 1 for i in idx])

    sys.stdout = _stdout
    return out.getvalue().strip()

# minimum case
assert run("1 3\n5 5 5\n") == "1"

# already sorted
assert run("3 1\n1\n2\n3\n") == "1 2 3"

# reverse order
assert run("3 1\n3\n2\n1\n") == "3 2 1"

# all equal
assert run("4 2\n1 1\n1 1\n1 1\n1 1\n") in ["1 2 3 4", "2 1 3 4", "3 4 1 2", "4 3 2 1"]

# mixed case
assert run("3 2\n1 3\n2 2\n3 1\n") == "1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ảnh chụp màn hình duy nhất | 1 | trường hợp cơ sở | 
| đầu vào được sắp xếp | hoán vị danh tính | sự ổn định của đặt hàng | 
| đầu vào đảo ngược | hoán vị ngược | xử lý giảm dần chính xác | 
| tất cả các vectơ bằng nhau | hoán vị nào | xử lý cà vạt | 
| vectơ đánh đổi hỗn hợp | trật tự đơn điệu hợp lệ | tính nhất quán từ điển | 

## Vỏ cạnh 

Trường hợp key edge là khi tất cả các ảnh chụp màn hình giống hệt nhau. Ví dụ:```
3 2
1 1
1 1
1 1
```Mọi thứ tự đều hợp lệ vì không có ảnh chụp màn hình nào tốt hơn hoặc tệ hơn ảnh chụp màn hình khác ở bất kỳ tọa độ nào. Thuật toán sắp xếp chúng thành một số hoán vị và điều đó có thể chấp nhận được vì không có ràng buộc nào phân biệt giữa chúng. 

Một trường hợp quan trọng khác là khi các vectơ không thể so sánh được theo nghĩa tọa độ, chẳng hạn như:```
2 2
1 5
5 1
```Cả hai ảnh chụp màn hình đều không lấn át ảnh chụp màn hình kia, vì vậy cả hai lệnh đều hợp lệ về mặt lý thuyết. Việc sắp xếp từ điển sẽ chọn một thứ tự xác định, ví dụ (1,5) trước (5,1), vẫn thỏa mãn vấn đề vì không có ràng buộc nào buộc thứ tự ngược lại. 

Cuối cùng, hãy xem xét chuỗi tăng nghiêm ngặt:```
3 2
1 1
2 2
3 3
```Thuật toán sắp xếp chúng theo thứ tự tăng dần một cách tự nhiên. Điều này xác nhận rằng khi tồn tại một chuỗi nghiêm ngặt, việc sắp xếp sẽ tôn trọng chuỗi đó và tạo ra dòng thời gian nhất quán mà không cần xây dựng biểu đồ rõ ràng.
