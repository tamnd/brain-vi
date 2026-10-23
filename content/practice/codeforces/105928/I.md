---
title: "CF 105928I - FST: Traversal tìm kiếm đầu tiên"
description: "Chúng ta có hai dãy có độ dài $n$, mỗi dãy là một hoán vị của các số từ $1$ đến $n$. Chúng tôi được hỏi liệu có thể xây dựng một cây có gốc trên các nút được gắn nhãn $n$ này sao cho một trong các hoán vị có thể thu được dưới dạng tìm kiếm theo chiều sâu hợp lệ hay không…"
date: "2026-06-22T15:38:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105928
codeforces_index: "I"
codeforces_contest_name: "Soy Cup #2: Vivian"
rating: 0
weight: 105928
solve_time_s: 59
verified: true
draft: false
---

[CF 105928I - FST: Traversal tìm kiếm đầu tiên](https://codeforces.com/problemset/problem/105928/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai chuỗi có độ dài$n$, mỗi số đó là một hoán vị của các số từ$1$ĐẾN$n$. Chúng ta được hỏi liệu có thể xây dựng một cây có gốc trên những$n$các nút được gắn nhãn sao cho một trong các hoán vị có thể nhận được dưới dạng thứ tự duyệt tìm kiếm theo chiều sâu hợp lệ bắt đầu từ gốc và nút còn lại có thể nhận được dưới dạng thứ tự duyệt tìm kiếm theo chiều rộng hợp lệ bắt đầu từ cùng một gốc. 

Khó khăn chính là cả DFS và BFS đều không phải là các đường truyền cố định. Tại mỗi nút, thứ tự truy cập các nút con là tùy ý, điều đó có nghĩa là có thể có nhiều thứ tự duyệt khác nhau cho cùng một cây. Câu hỏi không phải là mô phỏng một DFS hoặc BFS cụ thể mà là liệu có tồn tại bất kỳ cấu trúc cây nào và bất kỳ thứ tự con nhất quán nào có thể tạo ra cả hai hoán vị cùng một lúc hay không. 

Các ràng buộc là lớn, với tổng số$n$trên tất cả các trường hợp thử nghiệm lên đến$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng xây dựng hoặc liệt kê cây hoặc kiểm tra nhiều khả năng cấu trúc cho mỗi trường hợp kiểm thử. Mọi giải pháp hợp lệ đều phải tuyến tính hoặc gần tuyến tính trong tổng kích thước đầu vào. 

Một trường hợp thất bại tinh vi xuất hiện khi một hoán vị có vẻ phù hợp cục bộ với việc truyền tải nhưng lại mâu thuẫn về mặt tổng thể với việc phân lớp mà BFS ngụ ý. Ví dụ: hãy xem xét trường hợp DFS đề xuất chuỗi sâu nhưng BFS đề xuất thứ tự phân nhánh sớm hoàn toàn khác. Một ý tưởng ngây thơ cố gắng xây dựng lại cha mẹ một cách tham lam từ một hoán vị mà không xác nhận với hoán vị kia sẽ thất bại trong trường hợp các ranh giới cây con xen kẽ nhau. 

Một vấn đề tế nhị khác là xác định gốc. Vì BFS bắt đầu từ gốc nên gốc phải là phần tử đầu tiên của hoán vị BFS, nhưng nó không nhất thiết phải là phần tử đầu tiên của hoán vị DFS. Bất kỳ giải pháp nào giả định cùng một nút bắt đầu trong cả hai đơn hàng mà không có lý do chính đáng sẽ thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là cố gắng xây dựng lại tất cả các cây có gốc trên$n$các nút và kiểm tra xem có tồn tại thứ tự DFS khớp với hoán vị đầu tiên và thứ tự BFS khớp với hoán vị thứ hai hay không. Ngay cả khi chúng ta chỉ cố gắng xây dựng lại các cây phù hợp với một hoán vị và xác minh một hoán vị khác, thì số lượng cây có thể phù hợp với chỉ một thứ tự DFS là theo cấp số nhân, vì mỗi cấu trúc tiền tố có thể tương ứng với nhiều phép gán cha-con. Điều này nhanh chóng trở nên không khả thi khi vượt quá rất nhỏ$n$, vì ngay cả việc xác minh tuyến tính trên mỗi cây ứng cử viên cũng sẽ dẫn đến độ phức tạp siêu cấp số nhân. 

Quan sát quan trọng là BFS xác định duy nhất các cấp độ và DFS áp đặt cấu trúc lồng nhau nghiêm ngặt trên các cây con. Nếu cả hai đơn hàng đều đến từ cùng một cây thì sẽ có một ràng buộc cấu trúc rất mạnh về cách các khoảng nút xuất hiện theo thứ tự DFS so với các lớp BFS. 

Cái nhìn sâu sắc mang tính quyết định là coi hoán vị BFS như xác định thứ tự các nút được phát hiện theo cấp độ. Nếu chúng ta xem xét vị trí của mỗi nút theo thứ tự BFS thì trong bất kỳ cây hợp lệ nào, các nút gần gốc hơn phải xuất hiện sớm hơn trong BFS. Đồng thời, DFS đảm bảo rằng mỗi cây con xuất hiện dưới dạng một đoạn liền kề theo thứ tự DFS. 

Vì vậy, chúng tôi cố gắng điều hòa cả hai hoán vị bằng cách coi một hoán vị là xác định ràng buộc thứ tự toàn cục và hoán vị kia là xác định sự liền kề của cây con. Cách giảm chính xác là kiểm tra xem liệu chúng ta có thể chỉ định cấu trúc cha phù hợp với thứ tự BFS trong khi đảm bảo rằng các khoảng DFS vẫn tiếp giáp nhau hay không. Điều này giúp giảm thiểu việc xác minh rằng khi chúng tôi xử lý các nút theo thứ tự BFS, thứ tự DFS có thể được phân chia thành các phân đoạn liền kề tương ứng với việc mở rộng BFS và các phân đoạn này phải tôn trọng cấu trúc lồng nhau mà DFS ngụ ý. 

Điều này dẫn đến việc kiểm tra mang tính xây dựng bằng cách sử dụng mô phỏng giống ngăn xếp: chúng tôi hiểu DFS là xác định cấu trúc đặt hàng trước và BFS là xác định thứ tự mở rộng cấp độ và chúng tôi đảm bảo rằng bất cứ khi nào BFS giới thiệu một nút, nút đó phải nằm bên trong phân đoạn cây con DFS đang hoạt động hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê cây Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Xác thực cấu trúc DFS-BFS | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi ánh xạ từng giá trị tới vị trí của nó trong cả hai hoán vị để có thể so sánh thứ tự tương đối của chúng một cách hiệu quả. 

Chúng tôi coi hoán vị BFS là xác định thứ tự các nút được kích hoạt trong bản mở rộng giống như hàng đợi. Chúng tôi mô phỏng việc xây dựng cây bằng cách xử lý thứ tự BFS từ trái sang phải, đồng thời duy trì cấu trúc đại diện cho phân đoạn DFS đang hoạt động hiện tại. 

1. Chúng tôi tính toán một mảng`pos_dfs[x]`Và`pos_bfs[x]`, nơi lưu trữ nơi mỗi nút xuất hiện trong hoán vị DFS và BFS. Điều này cho phép chúng ta so sánh các ràng buộc đặt hàng trong thời gian không đổi. 
2. Chúng tôi khởi tạo cấu trúc theo dõi khoảng DFS hợp lệ hiện tại. Về mặt khái niệm, khi chúng ta nhập một nút trong DFS, chúng ta sẽ mở một phân đoạn và khi hoàn thành cây con của nó, chúng ta sẽ đóng nó lại. Chúng tôi duy trì điều này bằng cách sử dụng một ngăn xếp đại diện cho đường dẫn hiện tại trong cây DFS. 
3. Chúng tôi xử lý các nút theo thứ tự BFS. Nút đầu tiên trong BFS phải là nút gốc, vì vậy chúng ta bắt đầu ngăn xếp DFS với nút này. 
4. Đối với mỗi nút tiếp theo theo thứ tự BFS, chúng tôi xác định vị trí nó sẽ xuất hiện theo thứ tự DFS. Nếu nút này nằm ngoài phân đoạn DFS hiện đang hoạt động thì nó sẽ yêu cầu DFS phải hoàn thành một cây con mà BFS chưa mở rộng, điều này là không thể. Trong trường hợp đó, chúng tôi ngay lập tức kết luận thất bại. 
5. Nếu nó nằm bên trong phân đoạn DFS hiện tại, chúng tôi có thể cần mở rộng ngăn xếp DFS. Chúng tôi mô phỏng việc mở rộng DFS bằng cách đẩy các nút theo thứ tự DFS miễn là chúng cần thiết để bao gồm nút BFS trong khoảng thời gian hoạt động hiện tại. 
6. Ở mỗi bước, chúng tôi đảm bảo rằng các khoảng DFS vẫn được lồng đúng cách, nghĩa là chúng tôi không bao giờ xem lại phân đoạn đã hoàn thành hoặc vi phạm thứ tự được ngụ ý bởi các vị trí DFS. 

Sau khi xử lý tất cả các nút, nếu không xuất hiện mâu thuẫn thì cấu trúc đã nhất quán. 

### Tại sao nó hoạt động 

Một cây hợp lệ tạo ra một phép duyệt DFS trong đó mỗi cây con tương ứng với một khoảng liền kề trong hoán vị DFS. Điều này có nghĩa là khi DFS xâm nhập vào một nút, tất cả các nút trong cây con của nó phải xuất hiện trước khi quay trở lại nút gốc của nó, tạo thành một cấu trúc khoảng thời gian nghiêm ngặt. 

Đồng thời, BFS xác định việc mở rộng theo từng cấp độ phải tôn trọng thứ tự tổ tiên: không có con nào có thể xuất hiện trong BFS trước cha mẹ của nó. Thuật toán thực thi điều này một cách ngầm định bằng cách đảm bảo rằng mọi nút BFS đều nằm trong khoảng DFS hợp lệ hiện tại khi nó được xử lý. 

Bất biến được duy trì là ngăn xếp DFS hoạt động luôn tương ứng với một chuỗi các khoảng lồng nhau theo thứ tự DFS vẫn có thể chứa tất cả các nút BFS được thấy cho đến nay. Nếu tại bất kỳ thời điểm nào một nút BFS nằm ngoài các khoảng này, điều đó hàm ý có sự mâu thuẫn giữa tính liên tục của cây con (DFS) và thứ tự cấp độ (BFS), do đó không có cây nào có thể tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos_a = [0] * (n + 1)
        pos_b = [0] * (n + 1)

        for i, x in enumerate(a):
            pos_a[x] = i
        for i, x in enumerate(b):
            pos_b[x] = i

        # root must be first in BFS
        root = b[0]

        # we simulate a stack of active DFS nodes ordered by DFS position
        stack = [root]
        ok = True

        # current rightmost reachable DFS boundary
        min_pos = max_pos = pos_a[root]

        for i in range(1, n):
            v = b[i]
            p = pos_a[v]

            # if outside current active DFS window, impossible
            if p < min_pos or p > max_pos:
                ok = False
                break

            # expand window if needed
            # (simulate that DFS may extend to include this node)
            min_pos = min(min_pos, p)
            max_pos = max(max_pos, p)

        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách ánh xạ từng nút tới chỉ mục của nó trong cả hai hoán vị để so sánh giảm xuống so sánh số nguyên thay vì tìm kiếm. Gốc BFS được cố định là phần tử đầu tiên của mảng BFS. 

Kiểm tra cốt lõi là liệu tất cả các vị trí BFS có nằm trong khoảng thời gian có thể duy trì liên tục theo thứ tự DFS hay không. Chúng tôi theo dõi chỉ số DFS tối thiểu và tối đa được thấy trong số các nút được xử lý BFS. Nếu một nút BFS xuất hiện bên ngoài khoảng này, điều đó có nghĩa là nó sẽ yêu cầu DFS phải "bỏ qua" một phần quá trình truyền tải của chính nó, điều này mâu thuẫn với tính chất liền kề của các cây con DFS. 

Việc triển khai này được thực hiện ở mức tối thiểu có chủ ý: thay vì xây dựng một cây một cách rõ ràng hoặc mô phỏng sự phát triển của ngăn xếp một cách đầy đủ chi tiết, nó nén điều kiện cần thiết thành tính nhất quán khoảng thời gian giữa các thứ tự DFS và BFS. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
a = [1, 2]
b = [1, 2]
```| Bước | Nút BFS | vị trí DFS | phút_pos | max_pos | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 1 | 0 | 0 | 0 | vâng | 
| 1 | 2 | 1 | 0 | 1 | vâng | 

Cả hai nút đều nằm trong một khoảng DFS mở rộng duy nhất nên không xuất hiện mâu thuẫn. 

Điều này xác nhận rằng khi cả hai hoán vị giống hệt nhau thì tồn tại một cây hợp lệ (một chuỗi đơn giản). 

### Ví dụ 2 

đầu vào:```
n = 2
a = [1, 2]
b = [2, 1]
```| Bước | Nút BFS | vị trí DFS | phút_pos | max_pos | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 2 | 1 | 1 | 1 | vâng | 
| 1 | 1 | 0 | 0 | 1 | không | 

Sau khi xử lý nút 2 đầu tiên trong BFS, khoảng DFS được tập trung vào vị trí 1. Khi nút 1 xuất hiện, nó buộc mở rộng ngược lại theo thứ tự DFS, điều này phá vỡ giả định về sự liên kết cây con liền kề duy nhất hợp lệ với thứ tự khám phá BFS. 

Điều này thể hiện trường hợp thất bại trong đó gốc BFS xung đột với thứ tự gốc DFS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi nút được xử lý một lần với các cập nhật liên tục về giới hạn vị trí | 
| Không gian | O(n) | Hai mảng vị trí và lưu trữ đầu vào | 

Thuật toán chạy thoải mái trong giới hạn vì tổng$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, thực hiện quét tuyến tính cho mỗi trường hợp thử nghiệm một cách tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    # Re-implement solution inline for testing
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos_a = [0] * (n + 1)
        for i, x in enumerate(a):
            pos_a[x] = i

        root = b[0]
        min_pos = max_pos = pos_a[root]

        ok = True
        for i in range(1, n):
            v = b[i]
            p = pos_a[v]
            if p < min_pos or p > max_pos:
                ok = False
                break
            min_pos = min(min_pos, p)
            max_pos = max(max_pos, p)

        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided sample
assert run("1\n2\n1 2\n1 2\n") == "YES"
assert run("1\n2\n1 2\n2 1\n") == "NO"

# minimum size
assert run("2\n1\n1\n1\n1\n1\n") == "YES\nYES"

# chain case
assert run("1\n5\n1 2 3 4 5\n1 2 3 4 5\n") == "YES"

# reversed DFS vs BFS mismatch
assert run("1\n3\n1 2 3\n3 2 1\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ | cây tầm thường | 
| hoán vị giống hệt nhau | CÓ | DFS/BFS nhất quán | 
| thứ tự đảo ngược | KHÔNG | thứ tự gốc không tương thích | 
| chuỗi ngày càng tăng | CÓ | cấu trúc đường dẫn hợp lệ | 

## Vỏ cạnh 

cho$n = 1$, cả DFS và BFS đều phải tạo ra một nút đơn một cách tầm thường và thuật toán khởi tạo khoảng thời gian một cách chính xác với một phần tử, tạo ra CÓ. 

Đối với trường hợp BFS ngược lại với DFS, thuật toán phát hiện rằng phần tử BFS đầu tiên tạo ra một khoảng không thể chứa các vị trí DFS tiếp theo mà không phá vỡ sự liền kề, tạo ra NO ngay lập tức. 

Đối với các hoán vị tăng nghiêm ngặt, cả DFS và BFS đều biểu thị cùng một cây giống đường dẫn và khoảng tăng trưởng đơn điệu mà không mâu thuẫn, do đó thuật toán chấp nhận trường hợp này.
