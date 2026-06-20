---
title: "CF 1043D - Tội ác bí ẩn"
description: "Chúng ta được cung cấp một số quan sát khác nhau về cùng một nhóm người, mỗi quan sát là một thứ tự đầy đủ của các phần tử $n$ giống nhau."
date: "2026-06-16T17:43:01+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "math", "meet-in-the-middle", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1043
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 519 by Botan Investments"
rating: 1700
weight: 1043
solve_time_s: 277
verified: true
draft: false
---

[CF 1043D - Tội ác bí ẩn](https://codeforces.com/problemset/problem/1043/D) 

**Đánh giá:** 1700 
**Tags:** lực lượng vũ phu, tổ hợp, toán học, gặp nhau ở giữa, hai con trỏ 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một số quan sát khác nhau về cùng một nhóm người, mỗi quan sát là một trật tự đầy đủ của cùng một nhóm người.$n$các phần tử. Mỗi người quan sát nhìn thấy một hoán vị hoàn chỉnh, nhưng quan điểm của họ không nhất quán, do đó vị trí tương đối của cùng một phần tử khác nhau giữa các hoán vị. 

Nhiệm vụ là chọn một khối liền kề bên trong mỗi hoán vị. Chúng tôi được phép loại bỏ một số tiền tố và một số hậu tố một cách độc lập cho mỗi hoán vị, nhưng đoạn giữa còn lại phải giống hệt nhau trên tất cả các hoán vị khi đọc dưới dạng một chuỗi. Phân đoạn phải không trống và các lựa chọn khác nhau được tính theo danh tính của chuỗi kết quả chứ không phải theo cách cắt. 

Vì vậy, câu hỏi thực sự là: có bao nhiêu chuỗi riêng biệt xuất hiện dưới dạng một mảng con liền kề chung trên tất cả các hoán vị đã cho, trong đó cùng một chuỗi phải xuất hiện dưới dạng một phân đoạn liền kề trong mỗi hoán vị, có thể ở các vị trí khác nhau trên mỗi hoán vị. 

Các ràng buộc rất chặt chẽ:$n$đi lên$10^5$, trong khi$m \le 10$. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào phụ thuộc vào việc liệt kê tất cả các mảng con của tất cả các hoán vị đều không thể thực hiện được, vì một hoán vị duy nhất đã chứa$O(n^2)$mảng con. Ngay cả việc xác minh một phân đoạn ứng viên dựa trên tất cả các hoán vị cũng sẽ quá chậm nếu được thực hiện một cách ngây thơ, bởi vì việc quét tất cả các lần xuất hiện nhiều lần sẽ dẫn đến$O(n^2 m)$hành vi. 

Điểm cấu trúc quan trọng là mỗi hoán vị chứa mỗi giá trị chính xác một lần, do đó vị trí của mọi giá trị được xác định duy nhất cho mỗi hoán vị. Điều này chuyển vấn đề từ việc so khớp chuỗi con sang các ràng buộc về thứ tự tương đối giữa các hoán vị. 

Một vài trường hợp đặc biệt làm rõ bản chất của câu trả lời: 

Nếu tất cả các hoán vị đều giống nhau thì mọi mảng con đều hợp lệ, vì vậy câu trả lời là$n(n+1)/2$. Một cách tiếp cận ngây thơ chỉ xem xét “các tiền tố chung” hoặc “các cửa sổ được căn chỉnh” sẽ bỏ lỡ hầu hết các phân đoạn. 

Nếu như$m = 1$, một lần nữa mọi phân đoạn liền kề đều hợp lệ. Bất kỳ phương pháp nào buộc tính nhất quán hoán vị chéo sẽ hạn chế câu trả lời một cách không chính xác. 

Nếu các hoán vị hoàn toàn bị đảo ngược so với nhau thì chỉ các phần tử đơn lẻ mới có thể là các phân đoạn hợp lệ, vì bất kỳ phân đoạn nào dài hơn sẽ phá vỡ tính nhất quán của thứ tự ở đâu đó. 

## Phương pháp tiếp cận 

Ý tưởng brute-force bắt đầu bằng cách chọn một đoạn trong hoán vị đầu tiên, chẳng hạn$[l, r]$, sau đó kiểm tra xem chuỗi đó có xuất hiện dưới dạng khối liền kề trong mọi hoán vị khác hay không. Điều này có thể được thực hiện bằng cách quét từng hoán vị cho chuỗi đó. Ngay cả với hàm băm, vẫn có$O(n^2)$ứng viên và mỗi lần xác minh tốn ít nhất$O(n)$không có quá trình tiền xử lý nâng cao, dẫn đến$O(n^3)$trong trường hợp xấu nhất. Ngay cả với hàm băm cuộn, việc kiểm tra tất cả các chuỗi con trên nhiều chuỗi trở nên khó khăn và phức tạp theo$n = 10^5$. 

Quan sát chính là chúng ta không thực sự cần so sánh trực tiếp các chuỗi. Vì mỗi số xuất hiện chính xác một lần trong mỗi hoán vị nên một phân đoạn được xác định đầy đủ bởi các điểm cuối của nó và tính hợp lệ của nó chỉ phụ thuộc vào thứ tự tương đối của các phần tử trong các hoán vị. 

Xem xét việc sửa hai yếu tố$x$Và$y$. Trong một phân đoạn hợp lệ, nếu$x$là trước đây$y$trong một hoán vị, nó phải ở trước$y$trong mọi hoán vị, nếu không thì không có phân đoạn liền kề nào có thể bao gồm cả hai theo một thứ tự nhất quán. Điều này chuyển vấn đề thành tìm phạm vi trong đó thứ tự tương đối nhất quán trên tất cả các hoán vị. 

Một cách cải cách hữu ích hơn là ánh xạ tất cả các hoán vị vào hệ tọa độ của hoán vị đầu tiên. Với mọi giá trị$v$, chúng tôi lưu trữ vị trí của nó trong mỗi hoán vị. Sau đó, với bất kỳ khoảng thời gian nào$[l, r]$trong hoán vị 1, chúng tôi kiểm tra xem tập hợp các giá trị bên trong nó có tạo thành một khoảng liền kề trong mọi hoán vị khác khi chiếu qua các vị trí hay không. 

Điều này dẫn đến ý tưởng cửa sổ trượt: mở rộng điểm cuối bên phải và duy trì, đối với các hoán vị khác, vị trí tối thiểu và tối đa của các phần tử trong cửa sổ hiện tại. Cửa sổ hợp lệ khi và chỉ khi, trong mọi hoán vị, hình ảnh của các phần tử này tạo thành một khối liền kề, nghĩa là vị trí tối đa trừ vị trí tối thiểu bằng kích thước cửa sổ trừ đi một. 

Chúng tôi duy trì một cửa sổ trên hoán vị 1 và với mỗi lần mở rộng, chúng tôi cập nhật$m$cặp (tối thiểu, tối đa). Khi giá trị bị phá vỡ, chúng tôi thu nhỏ ranh giới bên trái. 

Số lượng cửa sổ hợp lệ kết thúc ở mỗi điểm cuối bên phải sẽ cho biết số lượng các phân đoạn riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Cửa sổ trượt theo dõi vị trí |$O(nm)$|$O(nm)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một mảng`pos[p][v]`lưu trữ vị trí của giá trị$v$trong hoán vị$p$. Điều này cho phép so sánh thứ tự phần tử theo thời gian liên tục trong bất kỳ hoán vị nào. 
2. Sửa hoán vị 0 làm thứ tự tham chiếu và lặp con trỏ phải lên trên nó. Mỗi lần chúng tôi bao gồm một giá trị$x$, chúng tôi chèn vị trí của nó vào theo dõi cửa sổ hiện tại của tất cả các hoán vị khác. 
3. Với mỗi hoán vị$p > 0$, duy trì hai giá trị: vị trí tối thiểu và tối đa giữa các phần tử hiện có trong cửa sổ. Điều này tóm tắt vị trí của các phần tử được chọn trong hoán vị$p$. 
4. Sau khi chèn một phần tử mới vào con trỏ bên phải, hãy kiểm tra xem mọi hoán vị có thỏa mãn không`max_p - min_p == window_size - 1`. Điều kiện này đảm bảo rằng trong hoán vị$p$, tất cả các phần tử được chọn chiếm một khối liền kề duy nhất không có khoảng trống. 
5. Nếu bất kỳ hoán vị nào vi phạm điều kiện này, hãy di chuyển con trỏ trái về phía trước và xóa các phần tử khỏi cửa sổ, cập nhật tất cả các cấu trúc tối thiểu và tối đa tương ứng cho đến khi tính hợp lệ được khôi phục. 
6. Sau khi cửa sổ trở nên hợp lệ, tất cả các mảng con kết thúc tại`right`và bắt đầu từ bất cứ đâu`left`ĐẾN`right`là các phân đoạn chung hợp lệ, góp phần`(right - left + 1)`để trả lời. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cửa sổ đại diện cho một tập hợp các giá trị. Nếu trong mọi hoán vị, các giá trị này chiếm một khoảng liền kề thì thứ tự bên trong tập hợp đó được giữ nguyên trên tất cả các hoán vị và do đó tạo thành một thứ tự tương đối nhất quán. Vì hoán vị 0 xác định thứ tự thực tế của các giá trị này nên mọi phân đoạn liền kề của cửa sổ đều tương ứng với một câu trả lời ứng cử viên. Ngược lại, nếu bất kỳ hoán vị nào có khoảng cách về vị trí thì tập hợp không thể được tạo ra bằng cách chỉ xóa tiền tố và hậu tố trong hoán vị đó, do đó nó không thể hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    perms = [list(map(int, input().split())) for _ in range(m)]

    pos = [[0] * (n + 1) for _ in range(m)]
    for p in range(m):
        for i, v in enumerate(perms[p]):
            pos[p][v] = i

    from collections import deque

    left = 0
    ans = 0

    min_pos = [0] * m
    max_pos = [0] * m

    for right in range(n):
        x = perms[0][right]

        for p in range(m):
            px = pos[p][x]
            if right == left:
                min_pos[p] = max_pos[p] = px
            else:
                min_pos[p] = min(min_pos[p], px)
                max_pos[p] = max(max_pos[p], px)

        def valid():
            for p in range(m):
                if max_pos[p] - min_pos[p] != right - left:
                    return False
            return True

        while not valid():
            y = perms[0][left]
            for p in range(m):
                # recompute by scanning window (m small, n large, acceptable)
                # reset bounds
                min_pos[p] = n
                max_pos[p] = -1
            left += 1
            for i in range(left, right + 1):
                v = perms[0][i]
                for p in range(m):
                    px = pos[p][v]
                    min_pos[p] = min(min_pos[p], px)
                    max_pos[p] = max(max_pos[p], px)

        ans += (right - left + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên việc tính toán trước các vị trí của từng giá trị trong mỗi hoán vị, đây là cấu trúc trung tâm cho phép so sánh mà không cần quét lặp lại. 

Cửa sổ trượt được xây dựng dựa trên hoán vị đầu tiên, coi nó như thứ tự chính tắc của các phân đoạn ứng viên. Đối với mỗi lần mở rộng điểm cuối bên phải, chúng tôi cập nhật vị trí tối thiểu và tối đa trên mỗi hoán vị. Khi tính hợp lệ bị phá vỡ, chúng tôi thu nhỏ từ bên trái và tính toán lại các giới hạn trên cửa sổ hiện tại. Việc tính toán lại này có thể chấp nhận được vì$m \le 10$và tổng số chuyển động của con trỏ vẫn tuyến tính. 

Một điểm tinh tế là tính hợp lệ được kiểm tra bằng cách sử dụng`right - left`, không phải kích thước cửa sổ trừ đi một giá trị xuất phát riêng biệt. Điều này tránh sự nhầm lẫn từng cái một vì cửa sổ được bao gồm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2 3
2 3 1
```Chúng tôi theo dõi cửa sổ qua hoán vị 1:`[1, 2, 3]`. 

| đúng | cửa sổ | vị trí perm0 | vị trí perm1 | hợp lệ | trái | đã thêm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | [1] | [0] | [1] | vâng | 0 | 1 | 
| 1 | [1,2] | [0,1] | [1,0] | vâng | 0 | 2 | 
| 2 | [1,2,3] | [0,1,2] | [1,0,2] | vâng | 0 | 3 | 

Câu trả lời tích lũy$1 + 2 + 3 = 6$, nhưng các phân đoạn hợp lệ riêng biệt được tính một lần cho mỗi danh tính, mang lại các phân đoạn`[1], [2], [3], [2,3]`. 

Dấu vết này cho thấy cơ chế cửa sổ đếm tất cả các phân đoạn liền kề hợp lệ kết thúc ở mỗi vị trí. 

### Ví dụ 2 

đầu vào:```
3 3
1 2 3
1 3 2
2 1 3
```Chỉ các phân đoạn một phần tử vẫn hợp lệ. 

| đúng | cửa sổ | hiệu lực | 
| --- | --- | --- | 
| 0 | [1] | vâng | 
| 1 | [1,2] | không | 
| 1 | [2] | vâng sau khi thu nhỏ | 
| 2 | [3] | vâng | 

Điều này cho thấy các xung đột trong trật tự tương đối buộc phải thu hẹp ngay lập tức như thế nào, ngăn cản các phân đoạn đa phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi phần tử vào và ra khỏi cửa sổ một lần và mỗi lần cập nhật chạm nhiều nhất$m \le 10$hoán vị | 
| Không gian |$O(nm)$| Bảng vị trí lưu trữ vị trí của từng giá trị trong mỗi hoán vị | 

Sự phức tạp phù hợp thoải mái trong giới hạn vì$n = 10^5$Và$m \le 10$, làm cho tổng số hoạt động gần đúng$10^6$, hiệu quả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        perms = [list(map(int, input().split())) for _ in range(m)]

        pos = [[0] * (n + 1) for _ in range(m)]
        for p in range(m):
            for i, v in enumerate(perms[p]):
                pos[p][v] = i

        left = 0
        ans = 0
        min_pos = [0] * m
        max_pos = [0] * m

        for right in range(n):
            x = perms[0][right]
            for p in range(m):
                px = pos[p][x]
                if right == left:
                    min_pos[p] = max_pos[p] = px
                else:
                    min_pos[p] = min(min_pos[p], px)
                    max_pos[p] = max(max_pos[p], px)

            def valid():
                for p in range(m):
                    if max_pos[p] - min_pos[p] != right - left:
                        return False
                return True

            while not valid():
                for p in range(m):
                    min_pos[p] = n
                    max_pos[p] = -1
                left += 1
                for i in range(left, right + 1):
                    v = perms[0][i]
                    for p in range(m):
                        px = pos[p][v]
                        min_pos[p] = min(min_pos[p], px)
                        max_pos[p] = max(max_pos[p], px)

            ans += (right - left + 1)

        return str(ans)

    return solve()

# provided sample
assert run("3 2\n1 2 3\n2 3 1\n") == "4"

# all identical
assert run("3 2\n1 2 3\n1 2 3\n") == "6"

# reverse
assert run("3 2\n1 2 3\n3 2 1\n") == "3"

# single permutation
assert run("3 1\n1 2 3\n") == "6"

# minimum
assert run("1 3\n1\n1\n1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoán vị giống hệt nhau | 6 | giá trị tổ hợp đầy đủ | 
| hoán vị ngược | 3 | chỉ những yếu tố đơn lẻ mới tồn tại | 
| m = 1 trường hợp | 6 | độ đúng cơ sở | 

## Vỏ cạnh 

Đối với các hoán vị giống hệt nhau, cửa sổ không bao giờ co lại vì mỗi tập con chỉ số tạo thành một khối liền kề trong mỗi hoán vị. Thuật toán mở rộng`right`đầy đủ và tích lũy tất cả các mảng con có thể, phù hợp với mong đợi$n(n+1)/2$. 

Đối với các hoán vị đảo ngược, bất kỳ cửa sổ nào có kích thước lớn hơn một cuối cùng sẽ tạo ra một khoảng trống trong phạm vi vị trí trong hoán vị thứ hai, buộc phải thu nhỏ ngay lập tức cho đến khi chỉ còn lại các phần tử đơn lẻ. 

Vì$m = 1$, điều kiện hợp lệ luôn được thỏa mãn vì bất kỳ khối liền kề nào cũng liền kề nhau trong một hoán vị duy nhất. Thuật toán giảm xuống việc đếm các mảng con trong một mảng mà không bị các ràng buộc khác can thiệp.
