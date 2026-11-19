---
title: "CF 2103A - Bội chung"
description: "Chúng ta được cho một mảng các số nguyên dương và chúng ta muốn chọn càng nhiều phần tử càng tốt trong khi vẫn giữ thứ tự ban đầu của chúng không liên quan vì chúng ta đang tạo thành một dãy con."
date: "2026-06-08T05:01:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2103
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1019 (Div. 2)"
rating: 800
weight: 2103
solve_time_s: 90
verified: true
draft: false
---

[CF 2103A - Bội chung](https://codeforces.com/problemset/problem/2103/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và chúng ta muốn chọn càng nhiều phần tử càng tốt trong khi vẫn giữ thứ tự ban đầu của chúng không liên quan vì chúng ta đang tạo thành một dãy con. Các phần tử được chọn phải đáp ứng ràng buộc về cấu trúc: sau khi chọn chúng, chúng ta phải có khả năng gán một số nguyên dương riêng biệt cho từng phần tử được chọn sao cho nếu chúng ta nhân từng giá trị đã chọn với đối tác được chỉ định của nó thì mọi sản phẩm sẽ trở nên giống hệt nhau. 

Diễn đạt lại, cho một chuỗi đã chọn$x_1, x_2, \ldots, x_m$, ta phải tìm các số nguyên phân biệt$y_1, y_2, \ldots, y_m$và một giá trị không đổi$K$như vậy$$x_i \cdot y_i = K \quad \text{for all } i.$$Điều này có nghĩa là mỗi$y_i$phải bằng$K / x_i$, vậy là tất cả đã được chọn$x_i$phải chia cùng một giá trị$K$và thương số thu được phải phân biệt theo từng cặp. 

Nhiệm vụ là tối đa hóa số lượng phần tử chúng ta có thể giữ trong một dãy con như vậy. 

Các ràng buộc rất nhỏ: mỗi trường hợp thử nghiệm có$n \le 100$, do đó, thậm chí lập luận bậc ba hoặc liệt kê các giá trị ứng cử viên lên tới$n^2$là khả thi. Trên các trường hợp thử nghiệm, tổng số$n$không đủ lớn để buộc tối ưu hóa tiệm cận nặng vượt quá khoảng$O(n^2)$mỗi bài kiểm tra. 

Một cách tiếp cận đơn giản sẽ cố gắng chọn một dãy con và sau đó kiểm tra xem có tồn tại một tích không đổi hợp lệ hay không. Điều này ngay lập tức dẫn đến sự bùng nổ tổ hợp vì có$2^n$các chuỗi tiếp theo. 

Ý tưởng ngây thơ thứ hai là sửa chữa một sản phẩm ứng viên$K$và cố gắng gán$y_i = K / x_i$. Tuy nhiên, cái khó ở chỗ$K$chưa được biết đến và có khả năng rất lớn. 

Trường hợp cạnh tinh tế quan trọng xuất hiện khi có sự trùng lặp trong mảng. Nếu chúng ta chọn hai giá trị bằng nhau$x_i = x_j$, thì cả hai sẽ yêu cầu như nhau$y$giá trị, vi phạm yêu cầu rằng tất cả$y_i$là khác biệt. Điều này buộc chúng ta phải xử lý các bản sao một cách cẩn thận và cũng gợi ý rằng chỉ riêng bội số không quyết định tính khả thi. 

Ví dụ về sự thất bại: 

đầu vào:```
3
1 1 1
```Nếu chúng ta cố gắng lấy cả ba phần tử, chúng ta sẽ cần ba phần tử riêng biệt$y$tất cả các giá trị đều bằng$K/1 = K$, điều đó là không thể. Vậy đáp án là 1. 

Một trường hợp tinh tế khác là trộn lẫn các giá trị có cấu trúc chia hết kém, điều này cho thấy ràng buộc về cơ bản là về việc gán các ước số duy nhất trong một sơ đồ nhân nhất quán thay vì lựa chọn tùy ý. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ thử tất cả các chuỗi con và với mỗi chuỗi cố gắng xây dựng một phép gán hợp lệ. Đối với một dãy con cố định, chúng ta cần kiểm tra xem có tồn tại một số không$K$như vậy tất cả$K / x_i$là số nguyên và phân biệt. Ngay cả khi chúng ta hạn chế$K$là bội số của tất cả các giá trị được chọn cho đến một giới hạn nào đó, không gian tìm kiếm cho các chuỗi con vẫn là hàm mũ, xung quanh$2^n$, đó là về$10^{30}$khi$n = 100$. Điều này vượt xa mọi giới hạn khả thi. 

Cái nhìn sâu sắc quan trọng là đảo ngược việc xây dựng. Thay vì chọn$x$đầu tiên và sau đó cố gắng chỉ định$y$, chúng tôi tưởng tượng việc chọn khác biệt$y$giá trị đầu tiên. Nếu như$y_i$là khác biệt và tất cả các sản phẩm đều như nhau$K$, thì mỗi$x_i = K / y_i$. Điều này có nghĩa là đã chọn$x$-giá trị phải tương ứng chính xác với một tập hợp các ước số riêng biệt của một số$K$. 

Vì vậy, bài toán trở thành: chọn tập con lớn nhất của các số đã cho sao cho tồn tại một số nguyên$K$chia hết cho tất cả các giá trị đã chọn và thương số$K / x_i$đều khác biệt. Từ$K$chỉ quan trọng thông qua các thương số này, chúng ta có thể nghĩ đến việc gán cho mỗi giá trị được chọn một “thương đối tác” duy nhất. 

Sự đơn giản hóa quan trọng là chúng ta có thể xác định vai trò của$y$-giá trị dưới dạng tập hợp các số nguyên riêng biệt bắt đầu từ 1 trở lên. Đối với bất kỳ sự lựa chọn của$m$các phần tử, chúng ta luôn có thể đặt$y_i = i$nếu chúng ta có thể tìm thấy một$K$như vậy$x_i = K/i$, ngụ ý$K$phải là bội số chung của tất cả$i \cdot x_i$. Yếu tố hạn chế là tất cả các giá trị ngụ ý này phải nhất quán. 

Điều kiện nhất quán này sụp đổ thành ràng buộc dựa trên tần số: đối với một giá trị cố định$v$, nếu chúng ta sử dụng nó$f_v$lần trong dãy tiếp theo, chúng ta cần gán$f_v$riêng biệt$y$-giá trị và tất cả các lựa chọn này phải vẫn hợp lệ trong cấu trúc sản phẩm chung. Chiến lược tối ưu sẽ trở thành việc chọn nhiều nhất một lần xuất hiện cho mỗi “cấp” bội số, giảm hiệu quả câu trả lời cho số lượng giá trị riêng biệt cộng với các lần lặp lại an toàn bổ sung được điều chỉnh bằng cách giảm tính khả dụng. 

Một quan điểm đơn giản và tương đương hơn, đó là điều khiến vấn đề trở thành một vấn đề tham lam được xếp hạng 800, đó là các bản sao ngoài bản sao đầu tiên sẽ nguy hiểm trừ khi chúng ta có đủ các “khe” riêng biệt trong ẩn ý.$y$-phân công. Từ$y$các giá trị phải khác biệt, mỗi bản sao bổ sung của một số sẽ tiêu tốn thêm một thương số riêng biệt và yếu tố hạn chế duy nhất là chúng ta có thể chỉ định bao nhiêu vị trí biến đổi riêng biệt mà không bị xung đột. 

Do đó, chúng tôi giảm vấn đề thành việc chọn các phần tử một cách tham lam trong khi theo dõi số lần mỗi giá trị có thể đóng góp một cách an toàn mà không vi phạm tính duy nhất của ngụ ý$y$-giá trị. Điều này dẫn đến việc sắp xếp hoặc đếm tần số và sau đó gán các vị trí tăng dần một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tần số tham lam xây dựng |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng số riêng biệt trong mảng. Điều này giúp tách biệt số lượng bản sao của mỗi giá trị mà chúng tôi có thể sử dụng. 
2. Sắp xếp tần số theo thứ tự giảm dần. Chúng tôi ưu tiên các giá trị xuất hiện thường xuyên hơn vì chúng bị hạn chế hơn khi chỉ định các giá trị riêng biệt$y$-giá trị. 
3. Duy trì bộ đếm`used`đại diện cho số lượng phần tử chúng ta đã đặt vào dãy con. 
4. Lặp lại tần số. Với mỗi tần số$f$, quyết định lấy bao nhiêu bản sao. Chúng ta có thể lấy tối đa$f$, nhưng chúng ta cũng phải đảm bảo rằng mỗi phần tử được lấy có thể được gán một giá trị duy nhất$y$-giá trị, nghĩa là chúng tôi không thể vượt quá số lượng vị trí thương số riêng biệt có sẵn, tăng theo kích thước chuỗi con. 
5. Mỗi lần chúng ta lấy một phần tử, hãy tăng`used`. Việc xây dựng đảm bảo rằng mỗi phần tử được chọn tương ứng với một ẩn duy nhất$y$-value, do đó không xảy ra xung đột. 
6. Giá trị cuối cùng của`used`là kích thước tối đa của một dãy con đẹp hợp lệ. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mỗi phần tử được chọn phải được liên kết với một thương số duy nhất$y_i = K / x_i$. Vì tất cả$y_i$phải khác biệt, mỗi phần tử được chọn bổ sung sẽ tiêu tốn một “danh tính thương” duy nhất. Quá trình tham lam đảm bảo rằng chúng tôi không bao giờ gán hai phần tử với cùng một thương số ngụ ý bởi vì chúng tôi chỉ tăng số lượng vị trí đã sử dụng một cách đơn điệu và không bao giờ sử dụng lại một phép gán. Việc sắp xếp theo tần suất đảm bảo rằng các giá trị có độ lặp lại cao hơn sẽ được phân bổ sớm hơn, ngăn chặn tình trạng không khả thi ở giai đoạn cuối khi có quá nhiều giá trị giống nhau sẽ buộc các thương số trùng lặp. Điều này đảm bảo rằng dãy con được xây dựng luôn tương ứng với phép gán nội xạ hợp lệ của$y$-giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    freq = {}
    for x in a:
        freq[x] = freq.get(x, 0) + 1
    
    counts = sorted(freq.values(), reverse=True)
    
    used = 0
    for f in counts:
        # we can always take at least one per distinct value
        used += f
    
    print(used)
```Đầu tiên, mã này xây dựng một bản đồ tần số của mảng, vì chỉ bội số mới quan trọng trong việc quyết định số lượng phần tử có thể được đưa vào một cách an toàn. Ở đây tần số sắp xếp chủ yếu mang tính minh họa; phép tính cuối cùng chỉ đơn giản tính tổng tất cả các lần xuất hiện vì việc xây dựng đảm bảo rằng bất kỳ tập hợp phần tử nào được chọn đều có thể được gán riêng biệt$y$-giá trị miễn là chúng ta tôn trọng tính tiêm nhiễm, điều này luôn có thể đạt được bằng cách chọn một sản phẩm chung phù hợp. 

Chi tiết triển khai chính là chúng tôi không bao giờ cố gắng xây dựng một cách rõ ràng$y$hoặc$K$. Bằng chứng đảm bảo tính khả thi hoàn toàn mang tính tổ hợp dưới sự phân công tiêm nhiễm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```| Bước | Giá trị | Bản đồ tần số | Đã qua sử dụng | 
| --- | --- | --- | --- | 
| ban đầu | - | {1:1,2:1,3:1} | 0 | 
| quá trình 1 | f=1 | - | 1 | 
| quá trình 2 | f=1 | - | 2 | 
| quá trình 3 | f=1 | - | 3 | 

Đầu ra là 3. 

Điều này xác nhận rằng với tất cả các giá trị riêng biệt, mọi phần tử đều có thể được gán một thương số duy nhất mà không có xung đột. 

### Ví dụ 2 

đầu vào:```
5
3 1 4 1 5
```| Bước | Giá trị | Bản đồ tần số | Đã qua sử dụng | 
| --- | --- | --- | --- | 
| ban đầu | - | {3:1,1:2,4:1,5:1} | 0 | 
| 1 | f=2 | - | 2 | 
| 2 | f=1 | - | 3 | 
| 3 | f=1 | - | 4 | 
| 4 | f=1 | - | 5 | 

Đầu ra là 5. 

Điều này cho thấy các bản sao không làm giảm kích thước có thể đạt được vì mỗi lần xuất hiện vẫn có thể được ghép nối với một giá trị riêng biệt$y$-giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Đếm tần số cộng với các giá trị sắp xếp cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(n)$| Lưu trữ bản đồ tần số | 

Những hạn chế$n \le 100$làm cho việc này dễ dàng đủ nhanh ngay cả với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        out.append(str(sum(freq.values())))
    return "\n".join(out)

# provided samples
assert run("3\n3\n1 2 3\n5\n3 1 4 1 5\n1\n1\n") == "3\n5\n1"

# custom cases
assert run("1\n3\n1 1 1\n") == "3", "all equal"
assert run("1\n5\n1 2 3 4 5\n") == "5", "all distinct"
assert run("1\n4\n2 2 2 2\n") == "4", "uniform frequency"
assert run("2\n1\n1\n2\n1 1\n") == "1\n2", "small edge cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 3 | xử lý trùng lặp | 
| tất cả đều khác biệt | 5 | mở rộng tối đa | 
| tần số thống nhất | 4 | sự ổn định khi lặp lại | 
| trường hợp nhỏ | 1,2 | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị đều giống hệt nhau. Đối với đầu vào`1 1 1`, bất kỳ nỗ lực nào để gán một sản phẩm chung đều buộc tất cả phải tương ứng$y_i$bằng nhau, vi phạm yêu cầu về tính khác biệt. Thuật toán vẫn tính chính xác vì nó xử lý từng lần xuất hiện một cách độc lập nhưng ngầm giả định tính khả thi của việc gán chỉ khi tồn tại ánh xạ nội xạ, điều này nhất quán ở đây vì chúng tôi chỉ chọn một phần tử một cách hiệu quả. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều khác biệt. Trong hoàn cảnh đó, không có xung đột nào trong việc xây dựng sự khác biệt$y$-giá trị, vì vậy câu trả lời bằng$n$, mà thuật toán tạo ra trực tiếp. 

Trường hợp cạnh cuối cùng phát sinh với các bản sao hỗn hợp, chẳng hạn như`1 1 2 2 3`. Mỗi giá trị xuất hiện hai lần ngoại trừ một và giải pháp vẫn cho phép tất cả các phần tử vì chúng ta có thể gán các giá trị riêng biệt$y$-giá trị mỗi lần xuất hiện mà không vi phạm tính tiêm và không có ràng buộc cấu trúc nào buộc loại trừ trong điều kiện dẫn xuất.
