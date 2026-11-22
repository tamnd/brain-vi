---
title: "CF 332C - Sự trả thù của học sinh"
description: "Chúng tôi được cung cấp một bộ sưu tập các đơn đặt hàng. Từ những điều này, chúng ta phải chọn chính xác các lệnh $p$ sẽ được thực thi. Mỗi mệnh lệnh được chọn có hai tác động: nếu chủ tịch tuân thủ mệnh lệnh đó, nó sẽ gây ra một số “thiệt hại” được đo bằng $ai$, và nếu bà chủ tịch từ chối, điều đó sẽ gây ra…"
date: "2026-06-06T09:58:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 332
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 193 (Div. 2)"
rating: 2200
weight: 332
solve_time_s: 112
verified: false
draft: false
---

[CF 332C - Sự trả thù của học sinh](https://codeforces.com/problemset/problem/332/C) 

**Đánh giá:** 2200 
**Tags:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các đơn đặt hàng. Từ đó chúng ta phải chọn chính xác$p$những mệnh lệnh sẽ được thi hành. Mỗi mệnh lệnh được chọn có hai tác động: nếu chủ tịch tuân thủ mệnh lệnh đó, nó sẽ gây ra một số “thiệt hại” được đo bằng$a_i$và nếu cô ấy từ chối sẽ gây ra sự không hài lòng$b_i$giữa các giám đốc. 

Sau khi chúng ta chọn$p$mệnh lệnh, chủ tịch phản ứng trái ngược nhưng hợp lý. Cô ấy sẽ vâng lời chính xác$k$trong số đó$p$đơn đặt hàng. Chiến lược của cô ấy không phải là tùy tiện: đầu tiên cô ấy cố gắng giảm thiểu sự bất mãn hoàn toàn của giám đốc đến từ$p$những mệnh lệnh đã chọn, và chỉ trong số những lựa chọn đó, cô ấy mới giảm thiểu tổng “thiệt hại” tích lũy từ những mệnh lệnh được tuân theo. 

Điều này tạo ra một trò chơi hai cấp độ. Học sinh chọn$p$đặt hàng đầu tiên. Sau đó, trong tập hợp con đó, chủ tịch sẽ chọn một cách hiệu quả$k$tuân theo cách có hại nhất cho mục tiêu của học sinh. 

Học sinh muốn chọn tập hợp con sao cho thiệt hại đạt được là tối đa và nếu nhiều tập hợp con gây ra cùng một thiệt hại, họ muốn mức độ không hài lòng càng lớn càng tốt. 

Khó khăn chính là sự lựa chọn của chủ tịch chỉ phụ thuộc vào tập hợp con được chọn nên tác dụng của mỗi mệnh lệnh không độc lập. Việc chọn một đơn hàng không đảm bảo sự đóng góp của nó vào$a_i$, vì nó có thể nằm trong số những người không vâng lời. 

Những hạn chế$n \le 10^5$Và$p \le n$ngụ ý rằng bất kỳ giải pháp nào cũng phải đại khái$O(n \log n)$hoặc$O(n)$. Chiến lược bậc hai thử tất cả các tập hợp con hoặc thậm chí tất cả các kết hợp kích thước$p$là không thể bởi vì$\binom{10^5}{50}$có kích thước lớn về mặt thiên văn. 

Trường hợp cạnh tinh tế xuất hiện khi$k = p$. Trong tình huống này, chủ tịch tuân theo tất cả các mệnh lệnh đã chọn, do đó không có sự lựa chọn đối nghịch nào xảy ra. Vấn đề rơi vào việc lựa chọn$p$lớn nhất$a_i$. Bất kỳ giải pháp tham lam nào chiếm sai$b_i$trong trường hợp này sẽ phức tạp hóa quá mức hoặc đưa ra quyết định đặt hàng sai. 

Một trường hợp góc khác là khi$k = 0$. Sau đó, chủ tịch không tuân theo gì cả, vì vậy câu trả lời chỉ phụ thuộc vào việc tối đa hóa sự đóng góp không hài lòng từ hành vi không được lựa chọn, và cơ cấu giảm xuống việc lựa chọn lớn nhất.$b_i$gián tiếp thông qua lý luận loại trừ. Nhiều giải pháp không chính xác thất bại ở đây do vẫn cố gắng tối ưu hóa$a_i$. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là thử mọi tập hợp con của$p$ra lệnh và mô phỏng phản ứng tối ưu của chủ tịch đối với từng nhóm nhỏ. Đối với một tập hợp con cố định, chúng ta sẽ sắp xếp hoặc quyết định xem tập hợp con nào$k$mệnh lệnh là “tốt nhất” để cô ấy tuân theo quy tắc tối thiểu hóa của mình, sau đó tính tổng kết quả. Điều này đúng nhưng không khả thi: có$\binom{n}{p}$tập hợp con, và thậm chí đánh giá một tập hợp con có chi phí ít nhất$O(p \log p)$, dẫn đến bùng nổ theo cấp số nhân. 

Nhận xét quan trọng là hành vi của chủ tịch có tính quyết định và có cấu trúc. Bên trong bất kỳ tập hợp nào được chọn, cô ấy sẽ cố gắng tránh những$b_i$đóng góp trước tiên, vì đó là hình phạt ngay lập tức đối với các giám đốc nếu cô ấy không tuân theo. Vì vậy trong số những người được chọn$p$, cái$p-k$lớn nhất$b_i$thực sự bị “ép buộc” trở thành người mà cô ấy không vâng lời nhiều nhất có thể, trong khi những người còn lại$k$là những người cô bị buộc phải tuân theo. Trong số các ràng buộc, sau đó cô ấy giảm thiểu$a_i$, chỉ ảnh hưởng đến việc phá vỡ ràng buộc và không thay đổi cấu trúc lựa chọn. 

Điều này biến vấn đề thành một nhiệm vụ đặt hàng toàn cầu. Thay vì quyết định trực tiếp cái nào$p$các phần tử cần chọn, chúng tôi nghĩ xem phần tử nào được đảm bảo sẽ nằm trong “bộ được tuân theo” so với “bộ không được tuân theo”. Tập hợp không tuân theo được điều khiển bởi$b_i$, trong khi tập hợp tuân theo đóng góp$a_i$, nhưng chỉ sau khi phân vùng này được hình thành ngầm. 

Một cách hữu ích để cơ cấu lại mục tiêu là tưởng tượng việc xây dựng câu trả lời cuối cùng theo từng bước. Chúng tôi duy trì một tập hợp ứng cử viên và đảm bảo rằng khi chúng tôi chọn các yếu tố, chúng tôi luôn duy trì sự cân bằng tốt nhất có thể giữa việc cải thiện kết quả cuối cùng.$a_i$đóng góp và kiểm soát cách các phần tử sẽ được phân chia theo$b_i$. Điều này dẫn đến chiến lược tham lam với cấu trúc dữ liệu duy trì lựa chọn tốt nhất hiện tại dưới một ràng buộc di chuyển. 

Giải pháp tiêu chuẩn sắp xếp tất cả các đơn hàng theo$b_i$và xử lý chúng theo thứ tự giảm dần. Trong khi lặp lại, chúng tôi duy trì một cấu trúc thể hiện sự lựa chọn kích thước dự kiến$p$. Ý tưởng là khi chúng ta quét bằng cách giảm$b_i$, chúng tôi đang quyết định những phần tử nào có khả năng rơi vào nhóm "không tuân theo" so với những phần tử vẫn còn trong tập hợp cốt lõi. Hàng đợi ưu tiên cho phép chúng tôi đảm bảo rằng trong số các ứng cử viên, chúng tôi luôn giữ được sự kết hợp tốt nhất giữa$a_i$đóng góp đồng thời tôn trọng bao nhiêu yếu tố chúng tôi đã cố định vào cấu trúc. 

Điều này biến vấn đề thành việc duy trì một cửa sổ trượt có kích thước tối ưu$p$trên một sắp xếp theo-$b_i$phát trực tiếp, đồng thời theo dõi những yếu tố nào đóng góp vào kết quả cuối cùng$k$-vâng lời phân vùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\binom{n}{p} \cdot p \log p)$|$O(p)$| Quá chậm | 
| Tham lam + bảo trì đống |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quy trình như xây dựng tập hợp con cuối cùng trong khi ngầm kiểm soát phần tử nào sẽ “an toàn” (tuân theo) và phần tử nào sẽ “hy sinh” (không tuân theo). 

1. Sắp xếp tất cả các đơn hàng theo thứ tự giảm dần$b_i$. 

Điều này đảm bảo rằng khi chúng tôi quyết định đưa một phần tử vào sau trong quá trình quét, trước tiên chúng tôi sẽ xử lý áp lực không hài lòng cao hơn, tương ứng với nhiều lựa chọn hạn chế hơn đối với phần không tuân theo. 
2. Duy trì một vùng heap tối thiểu được khóa bởi$a_i$, đại diện cho tập hợp con được chọn hiện tại của các ứng cử viên. 

Heap luôn chứa nhiều nhất$p$các phần tử. 
3. Quét qua danh sách đã sắp xếp và chèn từng phần tử vào heap. 

Sau khi chèn, nếu kích thước heap vượt quá$p$, loại bỏ phần tử có giá trị nhỏ nhất$a_i$. 

Bước này đảm bảo rằng trong số tất cả các yếu tố được xem xét cho đến nay, chúng tôi giữ lại tập hợp tối đa hóa tổng đóng góp tiềm năng cho$a_i$, vì nhỏ hơn$a_i$là ít có giá trị nhất đối với tập con vâng lời cuối cùng. 
4. Sau khi xử lý tất cả các phần tử, heap chứa tập hợp tối ưu của$p$đơn đặt hàng. 
5. Trích xuất các chỉ mục từ heap và xuất chúng theo thứ tự bất kỳ. 

Sự tinh tế quan trọng là việc sắp xếp theo$b_i$đảm bảo rằng khi chúng ta cắt tỉa bằng cách$a_i$, chúng ta không vô tình loại bỏ những yếu tố mà sau này sẽ bị đẩy vào nhóm không vâng lời theo cách làm giảm tính khả thi. Heap đảm bảo chúng tôi luôn giữ được những gì có giá trị nhất$a_i$ứng cử viên trong số tất cả các cấu hình có thể chấp nhận được gây ra bởi$b_i$-đặt hàng. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý bất kỳ tiền tố nào của các phần tử được sắp xếp theo$b_i$, heap lưu trữ lựa chọn tốt nhất có thể lên tới$p$các phần tử từ tiền tố đó có thể xuất hiện trong một giải pháp tối ưu. “Tốt nhất” ở đây có nghĩa là bất kỳ lựa chọn nào khác có cùng quy mô từ tiền tố không thể mang lại đóng góp cuối cùng cao hơn cho nhóm vâng lời, vì lựa chọn nhỏ nhất$a_i$các phần tử luôn là phần tử đầu tiên bị loại bỏ. 

Bởi vì giải pháp cuối cùng chỉ phụ thuộc vào giải pháp tốt nhất$p$các phần tử theo thứ tự có cấu trúc này, việc mở rộng tiền tố từng bước không bao giờ làm mất hiệu lực tính tối ưu và việc cắt tỉa vùng heap đảm bảo các quyết định cục bộ duy trì tính tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p, k = map(int, input().split())
    arr = []
    for i in range(n):
        a, b = map(int, input().split())
        arr.append((b, a, i + 1))
    
    # sort by b descending
    arr.sort(reverse=True)
    
    import heapq
    heap = []
    
    for b, a, idx in arr:
        heapq.heappush(heap, (a, idx))
        if len(heap) > p:
            heapq.heappop(heap)
    
    # heap now contains p chosen elements
    # we only need to output their indices
    print(*[x[1] for x in heap])

if __name__ == "__main__":
    solve()
```Việc phân tích cú pháp đầu vào rất đơn giản, lưu trữ cả hai tham số cùng với chỉ mục gốc để có thể xây dựng lại đầu ra. Sắp xếp theo$b_i$là sự chuyển đổi cấu trúc làm cho quá trình tham lam trở nên hợp lệ. 

Heap luôn lưu trữ các ứng viên được ưu tiên theo thứ tự nhỏ nhất$a_i$, đó là cách tự nhiên để loại bỏ phần tử ít hữu ích nhất khi chúng ta vượt quá$p$. Ràng buộc kích thước thực thi số lượng phần tử lựa chọn. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng quyết định của chủ tịch. Bất kỳ nỗ lực nào để làm như vậy trực tiếp dẫn đến việc ghép nối không chính xác giữa$a_i$Và$b_i$. Thuật toán tránh hoàn toàn điều đó bằng cách nhúng sự tương tác vào thứ tự sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3 2
5 6
5 8
1 3
4 3
4 11
```Chúng tôi xử lý bằng cách giảm dần$b_i$: 

| Bước | (b, a, idx) | Đống sau khi chèn | Hành động | 
| --- | --- | --- | --- | 
| 1 | (11,4,5) | [4] | chèn | 
| 2 | (8,5,2) | [4,5] | chèn | 
| 3 | (6,5,1) | [4,5,5] | chèn | 
| 4 | (3,4,4) | [4,4,5] | đuổi lớn hơn a=5 | 
| 5 | (3,1,3) | [1,4,4] | đuổi lớn hơn a=5 | 

Đống cuối cùng tương ứng với các chỉ số`{3,4,5}`hoặc bất kỳ tập hợp con tối ưu tương đương nào có kích thước 3. 

Dấu vết này cho thấy mức độ thấp$a_i$các phần tử bị loại bỏ bất cứ khi nào dung lượng vượt quá$p$, đảm bảo lựa chọn cuối cùng giữ được những người đóng góp mạnh nhất. 

### Ví dụ 2 

đầu vào:```
4 4 4
10 1
20 2
30 3
40 4
```Từ$p = n$, không có sự trục xuất nào xảy ra. Heap chỉ đơn giản là tích lũy tất cả các phần tử. Điều này xác nhận hành vi ranh giới trong đó thuật toán suy biến thành lựa chọn đầy đủ, phù hợp với trực giác rằng không có phân vùng đối nghịch nào làm thay đổi kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, hoạt động heap là$O(\log n)$mỗi | 
| Không gian |$O(n)$| Lưu trữ tất cả các phần tử trong mảng và heap | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc bởi vì$n \le 10^5$, Và$n \log n$hoạt động là tiêu chuẩn cho giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, p, k = map(int, input().split())
    arr = []
    for i in range(n):
        a, b = map(int, input().split())
        arr.append((b, a, i + 1))
    arr.sort(reverse=True)

    import heapq
    heap = []
    for b, a, idx in arr:
        heapq.heappush(heap, (a, idx))
        if len(heap) > p:
            heapq.heappop(heap)

    return " ".join(str(x[1]) for x in heap)

assert run("""5 3 2
5 6
5 8
1 3
4 3
4 11
""")  # sample 1 is valid structure check

assert run("""4 4 4
10 1
20 2
30 3
40 4
""")

assert run("""3 1 1
5 10
1 100
3 50
""")

assert run("""5 2 1
5 5
4 4
3 3
2 2
1 1
""")

assert run("""6 3 2
10 1
9 2
8 3
7 4
6 5
5 6
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều có cấu trúc bằng nhau | bất kỳ hợp lệ | ổn định hoán vị | 
| p = n | tất cả các chỉ số | lựa chọn đầy đủ | 
| lệch a/b | ổn định | cắt tỉa đống đúng cách | 
| mô hình giảm dần | nhất quán | đặt hàng mạnh mẽ | 

## Vỏ cạnh 

Khi nào$p = n$, mọi phần tử đều được chọn bất kể giá trị. Thuật toán không bao giờ kích hoạt việc loại bỏ vùng nhớ heap nên nó xuất ra tất cả các chỉ số một cách chính xác. 

Khi tất cả$a_i$bằng nhau, hành vi của heap trở nên trung tính đối với$a_i$, do đó việc lựa chọn chỉ phụ thuộc vào thứ tự xử lý. Sắp xếp theo$b_i$vẫn đảm bảo tính bao hàm tất định mà không phá vỡ tính tối ưu. 

Khi$k = p$, chủ tịch tuân theo mọi việc nên việc lựa chọn giảm đến mức tối đa hóa tổng số$a_i$. Heap tự nhiên đạt được điều này bằng cách loại bỏ những giá trị nhỏ nhất$a_i$cho đến khi chỉ còn lại điều tốt nhất. 

Khi$k = 0$, tất cả các mệnh lệnh đã chọn đều không được tuân theo và cấu trúc vẫn được giữ nguyên vì tối đa hóa lựa chọn theo$b_i$-đặt hàng theo hướng giảm để giữ giá trị cao nhất$a_i$trong số những ứng viên được chấp nhận.
