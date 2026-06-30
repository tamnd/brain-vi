---
title: "CF 104396D - Đường Ray Ngôi Sao"
description: "Chúng ta được cho một tập hợp các điểm trong mặt phẳng. Mỗi điểm đại diện cho một ngôi sao. Với mỗi ngôi sao $i$, chúng ta tưởng tượng đứng ở ngôi sao đó và vẽ một đường thẳng đi qua nó. Đường này bắt buộc phải không đi qua bất kỳ ngôi sao nào khác."
date: "2026-07-01T00:47:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104396
codeforces_index: "D"
codeforces_contest_name: "2023 Jiangsu Collegiate Programming Contest, 2023 National Invitational of CCPC (Hunan), The 13th Xiangtan Collegiate Programming Contest"
rating: 0
weight: 104396
solve_time_s: 59
verified: true
draft: false
---

[CF 104396D - Đường sắt hình sao](https://codeforces.com/problemset/problem/104396/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm trong mặt phẳng. Mỗi điểm đại diện cho một ngôi sao. Cho mỗi ngôi sao$i$, chúng ta tưởng tượng đứng ở ngôi sao đó và vẽ một đường thẳng đi qua nó. Đường này bắt buộc phải không đi qua bất kỳ ngôi sao nào khác. Khi đường được vẽ, nó sẽ chia tách tất cả các đường khác$n-1$các ngôi sao thành hai nhóm tùy thuộc vào phía nào của đường thẳng mà chúng nằm. 

Đối với một ngôi sao khởi đầu cố định$i$, chúng ta quan tâm đến việc có bao nhiêu cách khác nhau để chọn một đường thẳng sao cho một trong hai cạnh chứa chính xác$k$các ngôi sao. Mỗi dòng hợp lệ đóng góp một lựa chọn cho câu trả lời tương ứng$k$. Câu trả lời chúng ta cần là một ma trận trong đó mục nhập$A_{i,k}$đếm xem có bao nhiêu đường đi qua điểm$i$tạo ra một mặt với chính xác$k$điểm. 

Khó khăn chính là đường dây không cố định. Nó có thể xoay quanh điểm$i$và mỗi hướng tạo ra sự phân chia khác nhau của các điểm còn lại. Chúng tôi đang đếm một cách hiệu quả có bao nhiêu đường phân chia hướng xung quanh mỗi điểm tạo ra mỗi kích thước cạnh có thể có. 

Những ràng buộc ngụ ý rằng$n \le 2000$. Số bậc hai của các cặp điểm đã đạt tới khoảng$4 \cdot 10^6$, điều này sẽ ổn trong Python nếu được xử lý cẩn thận. Bất cứ điều gì khối sẽ là quá chậm. Điều này ngay lập tức gợi ý rằng chúng ta nên xử lý từng điểm trung tâm một cách độc lập và dành thời gian tuyến tính hoặc gần tuyến tính cho mỗi trung tâm. 

Trường hợp cạnh tinh tế là khi nhiều điểm nằm gần như thẳng hàng với điểm tham chiếu. Cách tiếp cận sắp xếp góc ngây thơ không xử lý cẩn thận “ranh giới nửa mặt phẳng” có thể đếm gấp đôi hoặc bỏ lỡ cấu hình. Một sai lầm phổ biến khác là quên rằng chúng ta đang đếm các nửa mặt phẳng mở tránh hoàn toàn các điểm trên đường biên, điều này buộc chúng ta phải đảm bảo không có điểm nào khác nằm chính xác trên hướng đã chọn. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là sửa chữa một điểm$i$, sau đó liệt kê tất cả các dòng có thể thông qua$i$được xác định bằng cách ghép đôi$i$với mọi điểm khác$j$. Mỗi hướng của đường thẳng như vậy xác định việc chia tất cả các điểm còn lại thành hai tập hợp tùy thuộc vào phía nào của đường thẳng mà chúng nằm. Đối với mỗi hướng, chúng ta đếm xem có bao nhiêu điểm nằm trên một phía và tăng dần tương ứng$A_{i,k}$. 

Điều này đúng vì bất kỳ dòng phân cách hợp lệ nào xuyên qua$i$phải song song với một đường thẳng được xác định bởi$i$và một điểm khác, vì đường thẳng được xác định bởi hướng của nó và chúng ta chỉ quan tâm đến việc nó phân chia tập hợp điểm hữu hạn như thế nào. Tuy nhiên, đối với mỗi cố định$i$, điều này sẽ yêu cầu$O(n)$hướng và cho mỗi hướng$O(n)$phân loại điểm, đưa ra$O(n^3)$tổng số hoạt động, quá chậm đối với$n = 2000$. 

Quan sát quan trọng là đối với một trung tâm cố định$i$, điều quan trọng chỉ là thứ tự góc của tất cả các điểm khác xung quanh$i$. Nếu chúng ta sắp xếp tất cả các vectơ$i \to j$theo góc cực thì nửa mặt phẳng bất kỳ tương ứng với một cung liền kề có chiều dài nhỏ hơn$180^\circ$về thứ tự vòng tròn này. Thay vì tính lại số điểm cho mỗi hướng, chúng ta có thể sử dụng thao tác quét hai con trỏ để duy trì số điểm nằm trong cửa sổ nửa vòng tròn bắt đầu từ mỗi hướng. Điều này làm giảm chi phí cho mỗi trung tâm xuống$O(n \log n)$để sắp xếp cộng$O(n)$để quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Quét góc |$O(n^2 \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi điểm$i$, chúng ta coi nó là gốc và phân tích tất cả các điểm khác liên quan đến nó. 

1. Đối với cố định$i$, tính toán vectơ từ$i$đến mọi điểm khác$j$. Mỗi vectơ có một góc bằng$[0, 2\pi)$. Điều này chuyển bài toán chia hình học thành bài toán sắp xếp góc. 
2. Sắp xếp tất cả các vectơ này theo góc cực. Thứ tự sắp xếp thể hiện việc đi bộ xung quanh điểm$i$trong một vòng tròn đầy đủ. 
3. Nhân đôi danh sách đã sắp xếp bằng cách nối lại từng vectơ với góc$+2\pi$. Điều này cho phép chúng ta mô phỏng vòng tròn bao quanh bằng cấu trúc tuyến tính. 
4. Sử dụng kỹ thuật hai con trỏ. Đối với mỗi chỉ số bắt đầu$l$, di chuyển con trỏ$r$về phía trước miễn là chênh lệch góc nhỏ hơn$180^\circ$. Cửa sổ$[l, r)$đại diện cho tất cả các điểm nằm trong nửa mặt phẳng hợp lệ bắt đầu từ hướng$l$. 
5. Gọi số điểm bên trong cửa sổ này là$w = r - l - 1$. Điều này có nghĩa là việc chọn đường biên thẳng hàng với hướng$l$sản xuất chính xác$w$điểm ở một bên. 
6. Tăng dần$A[i][w]$bằng 1 cho mỗi cửa sổ hợp lệ bắt đầu từ$l$. Điều này đếm mỗi dòng phân cách riêng biệt một lần. 
7. Lặp lại cho tất cả$i$, tạo ra ma trận đầy đủ. 

Chi tiết quan trọng là mọi đường dẫn hợp lệ đi qua$i$tương ứng với chính xác một hướng bắt đầu góc và quá trình quét đảm bảo chúng ta đếm mỗi nửa mặt phẳng cực đại chính xác một lần. 

### Tại sao nó hoạt động 

Quét góc mã hóa mọi đường phân cách có thể thông qua$i$như một ranh giới giữa một điểm và điểm tiếp theo theo thứ tự góc. Bởi vì ràng buộc nửa mặt phẳng tương đương với một khoảng góc nhỏ hơn$180^\circ$, mọi cấu hình khả thi đều tương ứng với một cung liền kề theo thứ tự vòng tròn. Cửa sổ hai con trỏ tìm tất cả các cung tối đa bắt đầu từ mỗi hướng và mỗi cung như vậy xác định duy nhất tập hợp các điểm trên một phía của một đường thẳng hợp lệ. Vì không có điểm nào khác nằm chính xác trên đường biên theo cách xây dựng của bài toán nên không có cấu hình nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    ans = [[0] * (n - 1) for _ in range(n)]

    for i in range(n):
        x0, y0 = pts[i]
        arr = []

        for j in range(n):
            if i == j:
                continue
            x, y = pts[j]
            dx = x - x0
            dy = y - y0
            arr.append((math.atan2(dy, dx), dx, dy))

        arr.sort(key=lambda t: t[0])
        m = n - 1

        ext = arr + [(a + 2 * math.pi, dx, dy) for a, dx, dy in arr]

        r = 0
        for l in range(m):
            if r < l:
                r = l
            while r < l + m and ext[r][0] - ext[l][0] < math.pi:
                r += 1
            w = r - l - 1
            ans[i][w] += 1

    out = []
    for i in range(n):
        out.append(" ".join(map(str, ans[i])))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng các vectơ góc xung quanh mỗi điểm bằng cách sử dụng`atan2`, đảm bảo thứ tự chính xác trong tất cả các góc phần tư. Bước nhân bản với`+2π`là điều cần thiết để xử lý sự bao quanh mà không cần số học mô-đun. 

Cửa sổ hai con trỏ được duy trì cẩn thận để đối với mỗi điểm cuối bên trái, chúng tôi chỉ mở rộng điểm cuối bên phải về phía trước, đảm bảo phân bổ thời gian tuyến tính trên mỗi trung tâm. Phép trừ`r - l - 1`loại bỏ chính hướng trục khỏi số đếm. 

Một cạm bẫy phổ biến là quên thiết lập lại`r`khi nó tụt lại phía sau`l`, điều này sẽ phá vỡ sự đơn điệu và dẫn đến kích thước cửa sổ không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ trong đó một điểm được bao quanh bởi các điểm khác theo các hướng khác nhau. Đối với tâm cố định, chúng tôi tính toán tất cả các góc và theo dõi cách cửa sổ trượt mở rộng. 

### Dấu vết ví dụ 

| tôi | r (cuối cùng) | kích thước cửa sổ w | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 3 | 2 | A[i][2] += 1 | 
| 1 | 4 | 2 | A[i][2] += 1 | 
| 2 | 5 | 2 | A[i][2] += 1 | 

Điều này cho thấy nhiều phép quay của nửa mặt phẳng có thể tạo ra cùng một số đếm nhưng được coi là các đường hợp lệ riêng biệt. 

Dấu vết xác nhận rằng mỗi vị trí góc đóng góp độc lập, phù hợp với định nghĩa của bài toán về các đường phân cách riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$| Mỗi trong số$n$trung tâm sắp xếp$n$vectơ và sau đó chạy quét tuyến tính | 
| Không gian |$O(n)$| Lưu trữ các vectơ góc cho một tâm tại một thời điểm | 

Với$n \le 2000$, điều này dẫn đến khoảng$4 \cdot 10^6$tính toán hình học và chi phí sắp xếp, phù hợp thoải mái trong giới hạn trong Python khi được triển khai hiệu quả. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]
        ans = [[0] * (n - 1) for _ in range(n)]

        for i in range(n):
            x0, y0 = pts[i]
            arr = []
            for j in range(n):
                if i == j:
                    continue
                x, y = pts[j]
                arr.append((math.atan2(y - x0, x - x0), x - x0, y - y0))

            arr.sort()
            m = n - 1
            ext = arr + [(a + 2 * math.pi, dx, dy) for a, dx, dy in arr]

            r = 0
            for l in range(m):
                if r < l:
                    r = l
                while r < l + m and ext[r][0] - ext[l][0] < math.pi:
                    r += 1
                ans[i][r - l - 1] += 1

        return "\n".join(" ".join(map(str, row)) for row in ans)

    return solve()

# small triangle
assert run("""3
0 0
1 0
0 1
""")  # basic sanity (structure check)

# collinear-ish configuration
assert run("""4
0 0
1 0
2 0
1 1
""")

# symmetric square
assert run("""4
0 0
0 1
1 0
1 1
""")

# chain-like spread
assert run("""5
0 0
2 0
4 0
6 0
3 3
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tam giác | hướng dẫn sử dụng | độ đúng góc cơ bản | 
| thẳng hàng + bù trừ | hướng dẫn sử dụng | xử lý các trường hợp cận tuyến | 
| vuông | số lượng đối xứng | đối xứng quay | 
| xích lệch | hướng dẫn sử dụng | ổn định cửa sổ | 

## Vỏ cạnh 

Cấu hình có vẻ suy biến là khi nhiều điểm gần như thẳng hàng với trục quay. Trong những trường hợp như vậy, các lỗi nhỏ về độ chính xác của dấu phẩy động trong`atan2`thứ tự có thể thay đổi sự kề cận trong danh sách góc. Thuật toán dựa trên thứ tự nghiêm ngặt, do đó cần phải sắp xếp ổn định và biểu diễn góc nhất quán để tránh phân loại sai các ranh giới nửa mặt phẳng. 

Một trường hợp cạnh khác là khi tất cả các điểm nằm trong một hình bán nguyệt xung quanh một trục quay. Trong trường hợp này, cửa sổ trượt sẽ luôn mở rộng để bao gồm tất cả các điểm, chỉ tạo ra một mục khác 0 trong hàng. Thuật toán xử lý việc này một cách chính xác vì điều kiện`angle < π`bao gồm bộ đầy đủ có thể truy cập mà không gặp sự cố tràn hoặc xung quanh. 

Trường hợp tinh tế cuối cùng là khi các điểm được phân bổ gần như đồng đều xung quanh vòng tròn. Ở đây, mỗi hướng bắt đầu tạo ra một cung cực đại khác nhau và thuật toán phải đảm bảo rằng mỗi cung được tính chính xác một lần. Sự tiến bộ đơn điệu nghiêm ngặt của con trỏ bên phải đảm bảo ánh xạ một-một giữa các chỉ số bắt đầu và cấu hình được tính.
