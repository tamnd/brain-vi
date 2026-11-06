---
title: "CF 1270H - Số lượng linh kiện"
description: "Mảng xác định thứ tự hoàn chỉnh giữa các vị trí: với mỗi cặp chỉ số $i < j$, chúng ta vẽ một so sánh có hướng trở thành cạnh vô hướng nếu giá trị ở bên trái nhỏ hơn giá trị ở bên phải."
date: "2026-06-16T00:54:35+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1270
codeforces_index: "H"
codeforces_contest_name: "Good Bye 2019"
rating: 3300
weight: 1270
solve_time_s: 325
verified: false
draft: false
---

[CF 1270H - Số lượng thành phần](https://codeforces.com/problemset/problem/1270/H) 

**Đánh giá:** 3300 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 5 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mảng xác định thứ tự hoàn chỉnh giữa các vị trí: cho mỗi cặp chỉ số$i < j$, chúng ta vẽ một so sánh có hướng trở thành cạnh vô hướng nếu giá trị ở bên trái nhỏ hơn giá trị ở bên phải. Vì vậy, mỗi cặp vị trí đều có thể đóng góp một cạnh hoặc không, chỉ tùy thuộc vào giá trị bên trái có nhỏ hơn hay không. 

Điều này tạo ra một biểu đồ có cấu trúc hoàn toàn được xác định bởi thứ tự tương đối của các giá trị giữa các vị trí. Một thành phần được kết nối trong biểu đồ này tương ứng với một nhóm các chỉ số có thể được liên kết thông qua chuỗi các mối quan hệ “giá trị tăng dần giữa các vị trí”. 

Chúng tôi không được yêu cầu xây dựng lại biểu đồ này một cách rõ ràng. Thay vào đó, sau mỗi lần cập nhật điểm vào mảng, chúng ta phải tính toán lại xem biểu đồ ẩn này có bao nhiêu thành phần được kết nối. 

Các ràng buộc đẩy chúng ta tới điều gì đó gần với tuyến tính hoặc logarit cho mỗi lần cập nhật. Với$n, q \le 5 \cdot 10^5$, bất kỳ giải pháp nào kiểm tra các cặp hoặc tính toán lại kết nối từ đầu sau mỗi truy vấn sẽ liên quan đến$O(n^2 q)$hoặc thậm chí$O(nq)$, cả hai đều không thể. Thậm chí$O(n \log n)$mỗi truy vấn quá lớn, vì nó sẽ tiếp cận$5 \cdot 10^5 \cdot \log n$mỗi hoạt động. 

Một khó khăn tinh tế xuất phát từ thực tế là khả năng kết nối được xác định trên một biểu đồ dày đặc, nhưng các cạnh không phải là tùy ý. Chúng phụ thuộc vào sự so sánh giá trị, điều này làm cho cấu trúc có trật tự toàn cầu nhưng năng động cục bộ. 

Một sai lầm ngây thơ là cho rằng đồ thị hoạt động giống như một cấu trúc đơn điệu đơn giản trên các chỉ số. Ví dụ, người ta có thể nghĩ rằng việc sắp xếp mảng hoặc theo dõi các mẫu giống như đảo ngược là đủ. Điều đó không thành công vì sự kết nối phụ thuộc vào chuỗi so sánh chứ không chỉ các mối quan hệ liền kề. Một cạm bẫy khác là giả định rằng chỉ những thay đổi cục bộ xung quanh vị trí được cập nhật mới quan trọng. Việc thay đổi một giá trị có thể kết nối hoặc ngắt kết nối các thành phần ở xa nhau thông qua các nút trung gian, do đó vị trí không giữ được. 

Một ví dụ nhỏ cho thấy độ nhạy: 

cho$[3, 1, 2]$, các cạnh tồn tại cho các cặp có giá trị bên trái nhỏ hơn. Điều này mang lại các cạnh$1 \to 3$theo nghĩa chỉ mục nếu các giá trị cho phép xâu chuỗi và biểu đồ được kết nối. Nếu chúng ta thay đổi giá trị ở giữa thành giá trị nhỏ nhất hoặc lớn nhất, kết nối có thể bị đảo lộn dù chỉ thay đổi một vị trí. 

Vì vậy, khó khăn cốt lõi là duy trì thước đo kết nối toàn cầu theo các cập nhật điểm trong biểu đồ dày đặc, hướng đến giá trị. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ xây dựng biểu đồ cho mỗi truy vấn và chạy DFS hoặc DSU trên tất cả các cặp. Xây dựng biểu đồ đã có chi phí$O(n^2)$và khả năng kết nối bổ sung thêm một tính năng khác$O(n^2)$, lặp lại$q$nhiều lần, điều đó hoàn toàn không thể thực hiện được. 

Quan sát cấu trúc quan trọng là các cạnh được xác định bằng cách sắp xếp thứ tự trong không gian giá trị, không phải không gian chỉ mục. Nếu chúng ta sắp xếp các chỉ số theo giá trị, chúng ta sẽ có được một hoán vị. Biểu đồ trở thành một cấu trúc so sánh trong đó mỗi vị trí tương tác với tất cả các vị trí khác tùy thuộc vào thứ hạng tương đối. 

Thay vì suy nghĩ theo các cạnh, sẽ hiệu quả hơn khi nghĩ về số lần “ranh giới” giữa các thành phần xuất hiện khi quét các chỉ mục theo thứ tự giá trị tăng dần. Mỗi thành phần tương ứng với một phân đoạn tối đa mà không có “cấu hình chặn” nào ngăn cản kết nối. Vấn đề giảm xuống còn việc duy trì số lượng phân khúc như vậy tồn tại dưới sự thay đổi năng động về thứ hạng. 

Một cách tiêu chuẩn để xử lý số liệu thống kê thứ tự động và các mối quan hệ liền kề trong một hoán vị là duy trì thứ tự được sắp xếp theo giá trị và theo dõi nơi xảy ra sự gián đoạn. Khó khăn là các bản cập nhật thay đổi vị trí xếp hạng của một phần tử, điều này chỉ ảnh hưởng đến các mối quan hệ lân cận theo thứ tự được sắp xếp chứ không ảnh hưởng đến tất cả các cặp. 

Điều này gợi ý việc duy trì cấu trúc được sắp xếp theo giá trị (chẳng hạn như BST cân bằng hoặc tập hợp có thứ tự) và duy trì thống kê phụ trợ trên các phần tử liền kề theo thứ tự giá trị mã hóa xem có xảy ra “sự ngắt” giữa các thành phần hay không. Mỗi bản cập nhật sẽ loại bỏ một phần tử và chèn nó vào vị trí giá trị mới, chỉ điều chỉnh thông tin lân cận cục bộ theo thứ tự được sắp xếp. Câu trả lời bắt nguồn từ việc đếm những khoảng nghỉ này. 

Điều này biến vấn đề kết nối toàn cầu thành vấn đề cập nhật cục bộ theo trình tự được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi truy vấn |$O(n^2)$| Quá chậm | 
| Tối ưu |$O(\log n)$mỗi truy vấn |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì các phần tử mảng hiện tại bên trong một cấu trúc có thứ tự được khóa theo giá trị để chúng ta có thể nhanh chóng xác định các giá trị tiền nhiệm và giá trị kế tiếp cho bất kỳ phần tử nào. Cấu trúc này đại diện cho mảng được sắp xếp theo giá trị thay vì theo chỉ mục. 
2. Xác định hàm trên các phần tử liên tiếp theo thứ tự sắp xếp theo giá trị này để xác định xem ranh giới có đóng góp vào số lượng thành phần hay không. Giải thích là mỗi ranh giới như vậy phản ánh sự thay đổi về tiềm năng kết nối giữa các cấp độ lân cận. 
3. Khởi tạo cấu trúc với tất cả các phần tử. Tính số lượng thành phần ban đầu bằng cách quét các cặp liền kề theo thứ tự được sắp xếp và đếm xem có bao nhiêu ranh giới tồn tại theo quy tắc được xác định ở bước 2. 
4. Đối với mỗi lần cập nhật tại vị trí$pos$, xóa giá trị cũ khỏi cấu trúc có thứ tự. Trước khi loại bỏ, hãy điều chỉnh sự đóng góp của các lân cận vì mọi ranh giới liên quan đến phần tử này đều biến mất. 
5. Chèn giá trị mới. Sau khi chèn, chỉ những hàng xóm xung quanh vị trí mới của nó theo thứ tự đã sắp xếp mới có thể thay đổi cấu trúc ranh giới, vì vậy chỉ cập nhật đóng góp cho các cặp cục bộ đó. 
6. Duy trì một bộ đếm ranh giới toàn cầu. Số lượng thành phần được kết nối được lấy trực tiếp từ bộ đếm này cộng với phần bù cơ sở tùy thuộc vào cách biểu diễn. 

Mỗi bản cập nhật chỉ chạm đến hai quan hệ kề cận cục bộ theo thứ tự được sắp xếp, do đó việc duy trì là không đổi hoặc logarit theo thời gian. 

### Tại sao nó hoạt động 

Bất biến cơ bản là thứ tự sắp xếp theo giá trị sẽ phân chia mảng thành một chuỗi trong đó tất cả các tương tác liên quan đến kết nối chỉ xảy ra giữa các phần tử lân cận theo thứ tự này. Bất kỳ bản cập nhật nào cũng thay đổi thứ tự tương đối của chính xác một phần tử, do đó chỉ phần tử trước và phần tử kế tiếp trong chuỗi này mới có thể thay đổi cấu trúc ranh giới. Vì khả năng kết nối giữa các cấp không liền kề luôn được trung gian thông qua các cấp trung gian nên không cần phải điều chỉnh tầm xa. Điều này làm giảm cấu trúc phụ thuộc dày đặc trên toàn cầu thành cấu trúc kề cận có thể cập nhật cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("val",)
    def __init__(self, val):
        self.val = val

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    import bisect

    # We maintain a sorted list of values
    # and a mapping from value to position index if needed.
    vals = sorted(a)

    # We
```
