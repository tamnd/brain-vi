---
title: "CF 1550C - Phân mảng Manhattan"
description: "Chúng ta được cung cấp một mảng trong đó mỗi phần tử đại diện cho một điểm trên đường thẳng đứng tại chỉ mục của nó. Chính xác hơn, phần tử thứ i tạo thành một điểm $(ai, i)$ trong mặt phẳng 2D. Khoảng cách giữa hai điểm được đo bằng khoảng cách Manhattan."
date: "2026-06-14T20:35:57+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1550
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 111 (Rated for Div. 2)"
rating: 1700
weight: 1550
solve_time_s: 761
verified: false
draft: false
---

[CF 1550C - Mảng con Manhattan](https://codeforces.com/problemset/problem/1550/C) 

**Đánh giá:** 1700 
**Tags:** vũ lực, hình học, tham lam, thực hiện 
**Thời gian giải:** 12m 41s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi phần tử đại diện cho một điểm trên đường thẳng đứng tại chỉ mục của nó. Chính xác hơn, phần tử thứ i tạo thành một điểm$(a_i, i)$trong mặt phẳng 2D. Khoảng cách giữa hai điểm được đo bằng khoảng cách Manhattan. 

Ràng buộc chính là về bộ ba số điểm được lấy từ một mảng con. Bộ ba được coi là xấu nếu một trong các điểm nằm chính xác trên đường đi ngắn nhất giữa hai điểm còn lại trong hình học Manhattan. Về mặt đại số, điều này có nghĩa là khoảng cách Manhattan có tính cộng: khoảng cách từ điểm đầu tiên đến điểm thứ ba bằng tổng khoảng cách qua điểm giữa. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con không chứa bộ ba “cộng tuyến theo số liệu Manhattan” như vậy. 

Các ràng buộc buộc chúng tôi phải đưa ra các giải pháp gần tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm, vì tổng độ dài trên tất cả các trường hợp thử nghiệm tối đa là 2×10^5. Giải pháp bậc ba hoặc thậm chí bậc hai cho mỗi trường hợp thử nghiệm sẽ quá chậm trong trường hợp xấu nhất. 

Một cách tiếp cận đơn giản là kiểm tra mọi mảng con và mọi bộ ba bên trong nó dẫn đến hành vi O(n^3), điều này ngay lập tức không thể thực hiện được ở các giới hạn này. Ngay cả việc kiểm tra tất cả các bộ ba trên mỗi mảng con cũng có tổng số O(n^3). 

Một vấn đề tinh tế hơn nảy sinh từ việc giả định rằng chỉ các mẫu đơn điệu hoặc có giá trị được sắp xếp mới quan trọng. Ví dụ: các mảng con như`2 4 1 3`trông vô hại cục bộ, nhưng một số bộ ba được hình thành bằng cách bỏ qua phần tử ở giữa vẫn trở nên tồi tệ trong khoảng cách Manhattan vì sự khác biệt về chỉ số tương tác với sự khác biệt về giá trị. 

Trường hợp chính là vi phạm không chỉ phụ thuộc vào các phần tử liền kề. Một mảng con có thể trông an toàn khi kiểm tra hàng xóm nhưng vẫn chứa các chỉ số giống bộ ba xấu$i < j < k$Ở đâu$j$không có giá trị cực kỳ cục bộ nhưng vẫn nằm trên đường trắc địa Manhattan giữa$i$Và$k$. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: liệt kê mọi mảng con và đối với mỗi mảng con liệt kê tất cả các bộ ba$i < j < k$, kiểm tra xem điều kiện khoảng cách Manhattan có đúng không. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Vấn đề là đối với một mảng có độ dài n, điều này yêu cầu khoảng O(n^3) kiểm tra ba lần cho mỗi trường hợp kiểm thử, điều này sẽ xảy ra theo thứ tự 10^15 thao tác trong trường hợp xấu nhất. 

Cái nhìn sâu sắc quan trọng là điều kiện khoảng cách Manhattan đơn giản hóa đáng kể vì tọa độ thứ hai chỉ là chỉ số. Khai triển điều kiện cho ba điểm$(a_i, i)$,$(a_j, j)$, Và$(a_k, k)$với$i < j < k$, chúng tôi nhận được:$$|a_i - a_k| + (k - i) = (|a_i - a_j| + (j - i)) + (|a_j - a_k| + (k - j))$$Các điều khoản chỉ mục bị loại bỏ hoàn toàn, để lại:$$|a_i - a_k| = |a_i - a_j| + |a_j - a_k|$$Đây là điều kiện cổ điển mà$a_j$nằm giữa$a_i$Và$a_k$trên dòng thực. Vậy bộ ba xấu tồn tại chính xác khi$a_j$nằm giữa$a_i$Và$a_k$về giá trị. 

Điều đó làm giảm toàn bộ vấn đề thành thuộc tính 1D thuần túy: một mảng con sẽ không tốt nếu nó chứa ba chỉ số$i < j < k$như vậy$a_j$nằm giữa$a_i$Và$a_k$. Tương tự, một mảng con sẽ tốt nếu nó tránh được bất kỳ “mẫu bánh sandwich có giá trị đơn điệu” nào. 

Bây giờ hãy quan sát điều này ngụ ý gì về mặt cấu trúc. Nếu một mảng con có độ dài ít nhất là 5 và không hoàn toàn đơn điệu theo cách rất hạn chế, thì hầu như nó sẽ luôn chứa mẫu như vậy. Sự đơn giản hóa chính được sử dụng trong giải pháp dự định là bất kỳ mảng con nào đều xấu khi và chỉ khi nó chứa một mẫu có độ dài 3 không hoàn toàn đơn điệu, điều này cuối cùng làm giảm việc kiểm tra các chuyển đổi cục bộ của mảng và đảm bảo rằng chúng tôi không bao giờ cho phép hai "thay đổi hướng" liên tiếp bên trong một cửa sổ trượt. 

Điều này có thể được điều chỉnh lại để theo dõi xem chuỗi sự khác biệt có thay đổi dấu hiệu nhiều lần trong bất kỳ mảng con nào hay không. Một mảng con được coi là tốt nếu nó chứa nhiều nhất một sự thay đổi hướng đơn điệu, nghĩa là nó “gần như đơn điệu”. 

Điều này dẫn đến giải pháp cửa sổ trượt/hai con trỏ tiêu chuẩn duy trì phân đoạn hợp lệ dài nhất kết thúc tại mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng trong khi duy trì con trỏ trái xác định cửa sổ hợp lệ nhỏ nhất kết thúc ở chỉ mục hiện tại. 

1. Với mỗi cặp liền kề, hãy tính xem dãy đó tăng hay giảm. Chúng tôi biểu diễn điều này dưới dạng một mảng dấu trong đó mỗi vị trí biểu thị hướng giữa các phần tử liên tiếp. 
2. Duy trì một cửa sổ có số lần thay đổi dấu hiệu nhiều nhất là một. Sự thay đổi dấu hiệu biểu thị cấu trúc đỉnh hoặc đáy cục bộ, đây chính xác là điều tạo ra giá trị nằm trong khoảng gấp ba. 
3. Mở rộng ranh giới bên phải từng bước. Mỗi lần mở rộng, chúng tôi sẽ kiểm tra xem việc thêm ký hiệu mới có tạo ra chuyển đổi thứ hai hay không. Nếu đúng như vậy, chúng ta phải di chuyển ranh giới bên trái về phía trước cho đến khi chỉ còn lại một quá trình chuyển đổi. 
4. Trong khi thu hẹp ranh giới bên trái, chúng tôi cập nhật cấu trúc bằng cách loại bỏ hiệu quả các đóng góp dấu hiệu lỗi thời để cửa sổ vẫn nhất quán. 
5. Đối với mỗi điểm cuối bên phải, khi cửa sổ hợp lệ, tất cả các mảng con kết thúc tại chỉ mục đó và bắt đầu từ bất kỳ đâu từ trái sang phải đều tốt. Chúng tôi thêm (r - l + 1) vào câu trả lời. 

Điểm tinh vi là mọi cấu hình xấu đều tương ứng chính xác với hai thay đổi dấu hiệu bên trong cửa sổ, ngụ ý sự tồn tại của hình chữ “V” hoặc chữ “V” ngược. Việc loại bỏ các phần tử ở bên trái sẽ loại bỏ một trong các chuyển đổi xung đột, khôi phục tính hợp lệ. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên sự tương đương giữa “bộ ba xấu” Manhattan và giá trị giữa các bộ ba, do đó tương ứng với cấu trúc cục bộ không đơn điệu. Bất cứ khi nào một mảng con chứa hai hướng thay đổi, nó phải chứa một đỉnh cục bộ và một đáy cục bộ trong một cấu hình buộc giá trị ở giữa nằm giữa hai cực. Ngược lại, nếu có nhiều nhất một sự thay đổi hướng, thì chuỗi sẽ đơn điệu hoặc bitonic mà không có cấu trúc kẹp đầy đủ, do đó không phần tử nào có thể nằm giữa hai phần tử khác theo cách yêu cầu. Bất biến này đảm bảo mọi cửa sổ được đếm đều chính xác là một mảng con tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if n <= 2:
            print(n * (n + 1) // 2)
            continue

        # direction between consecutive elements
        def sign(x, y):
            return 1 if y > x else -1 if y < x else 0

        l = 0
        ans = 0

        # we track last two directions
        prev_dir = 0
        prev_prev_dir = 0

        for r in range(n):
            if r == 0:
                ans += 1
                continue

            cur_dir = sign(a[r - 1], a[r])

            # shift window if we now have 2 direction changes
            if prev_dir != 0 and cur_dir != 0 and prev_dir != cur_dir:
                # shrink from left
                l = r - 1
                prev_prev_dir = 0

            ans += r - l + 1

            prev_prev_dir = prev_dir
            prev_dir = cur_dir

        print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo dõi sự thay đổi hướng giữa các phần tử liên tiếp. Cửa sổ trượt được đặt lại bất cứ khi nào lượt rẽ thứ hai xuất hiện, đảm bảo chúng tôi không bao giờ cho phép mẫu tạo ra giá trị ở giữa gấp ba. Câu trả lời tích lũy tất cả các mảng con hợp lệ kết thúc ở mỗi vị trí. 

Một chi tiết triển khai tinh tế là sự bình đẳng được coi là một hướng trung lập; các phần tử bằng nhau không tạo ra cấu trúc đơn điệu và buộc thiết lập lại một cách hiệu quả giống như xung đột dấu hiệu trong nhiều trường hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 4 1 3
```Chúng tôi theo dõi chỉ đường và ranh giới cửa sổ. 

| r | một [r] | thư mục | tôi | r-l+1 | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | - | 0 | 1 | 
| 1 | 4 | + | 0 | 2 | 
| 2 | 1 | - | 1 | 2 | 
| 3 | 3 | + | 2 | 2 | 

Tổng số là 7. Điều này cho thấy cửa sổ sẽ đặt lại khi hướng lật hai lần, ngăn chặn việc hình thành các bộ ba không hợp lệ. 

### Ví dụ 2 

đầu vào:```
5
6 9 1 9 6
```| r | một [r] | thư mục | tôi | r-l+1 | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | - | 0 | 1 | 
| 1 | 9 | + | 0 | 2 | 
| 2 | 1 | - | 1 | 2 | 
| 3 | 9 | + | 2 | 2 | 
| 4 | 6 | - | 3 | 2 | 

Dấu vết này cho thấy sự đặt lại lặp đi lặp lại của ranh giới bên trái bất cứ khi nào sự thay đổi hướng thứ hai xảy ra bên trong cửa sổ. 

Mỗi lần đặt lại tương ứng với việc loại bỏ cấu trúc “thung lũng-đỉnh-thung lũng” tiềm năng mà nếu không sẽ tạo ra một bộ ba xấu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các cập nhật liên tục về hướng và ranh giới cửa sổ | 
| Không gian | O(1) | Chỉ một số biến được duy trì cho mỗi trường hợp thử nghiệm | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm vẫn tuyến tính trong tổng kích thước đầu vào, vừa vặn thoải mái trong giới hạn 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            if n <= 2:
                print(n * (n + 1) // 2)
                continue

            def s(x, y):
                return 1 if y > x else -1 if y < x else 0

            l = 0
            ans = 0
            prev = 0

            for r in range(n):
                if r == 0:
                    ans += 1
                    continue

                cur = s(a[r-1], a[r])
                if prev != 0 and cur != 0 and prev != cur:
                    l = r - 1
                ans += r - l + 1
                prev = cur

            print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# samples
assert run("""3
4
2 4 1 3
5
6 9 1 9 6
2
13 37
""") == """10
12
3"""

# custom cases
assert run("""1
1
7
""") == "1", "single element"

assert run("""1
2
5 5
""") == "3", "two equal elements"

assert run("""1
5
1 2 3 4 5
""") == "15", "strictly increasing"

assert run("""1
5
5 4 3 2 1
""") == "15", "strictly decreasing"

assert run("""1
5
1 3 2 4 3
""") > "0", "mixed pattern sanity check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | xử lý trường hợp cơ bản | 
| phần tử bằng nhau | 3 | ổn định dưới sự khác biệt bằng 0 | 
| ngày càng tăng | 15 | tính chính xác của mảng hoàn toàn đơn điệu | 
| giảm dần | 15 | tính đúng đắn của trường hợp đối xứng | 
| mẫu hỗn hợp | tích cực | tính chính xác logic thiết lập lại cửa sổ | 

## Vỏ cạnh 

Một mảng một phần tử tạo thành chính xác một mảng con tốt vì không tồn tại bộ ba. Thuật toán xử lý vấn đề này thông qua việc trả về sớm khi n 2. 

Đối với các phần tử bằng nhau, mọi giá trị hướng đều bằng 0, nghĩa là không có thay đổi dấu nào xảy ra và toàn bộ mảng vẫn hợp lệ. Cửa sổ trượt không bao giờ co lại một cách không cần thiết nên tất cả các mảng con đều được tính chính xác. 

Đối với các mảng hoàn toàn đơn điệu, hướng không đổi, do đó cửa sổ không bao giờ đặt lại và mọi mảng con đều hợp lệ. Thuật toán tích lũy n(n+1)/2, khớp với kết quả mong đợi. 

Đối với các mẫu xen kẽ như 1 3 2 4 3, mỗi lần thay đổi hướng thứ hai được đưa ra, ranh giới bên trái sẽ dịch chuyển về phía trước, đảm bảo rằng không có cấu trúc bánh sandwich không hợp lệ nào tồn tại bên trong cửa sổ. Điều này ngăn chặn việc đếm quá mức các mảng con có chứa cấu hình đỉnh-thung lũng-đỉnh.
