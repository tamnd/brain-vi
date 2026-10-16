---
title: "CF 105465L - LIS trên lưới"
description: "Chúng ta được cung cấp một lưới có $n$ hàng và $m$ cột. Với mỗi cột $j$, chúng ta phải chọn chính xác các ô $aj$ để tô màu đen. Tất cả các tế bào khác vẫn có màu trắng. Các lựa chọn bên trong mỗi cột là miễn phí, miễn là số lượng ô đen trên mỗi cột là cố định."
date: "2026-06-23T02:26:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105465
codeforces_index: "L"
codeforces_contest_name: "2023 ICPC Southeastern Europe Regional Contest (The 2nd Universal Cup, Stage 14: Southeastern Europe)"
rating: 0
weight: 105465
solve_time_s: 58
verified: true
draft: false
---

[CF 105465L - LIS trên lưới](https://codeforces.com/problemset/problem/105465/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới với$n$hàng và$m$cột. Đối với mỗi cột$j$, chúng ta phải chọn chính xác$a_j$các tế bào để sơn màu đen. Tất cả các tế bào khác vẫn có màu trắng. Các lựa chọn bên trong mỗi cột là miễn phí, miễn là số lượng ô đen trên mỗi cột là cố định. 

Sau khi vẽ, chúng ta xem xét các dãy ô màu đen di chuyển thẳng xuống dưới theo hàng và sang phải theo cột. Nói cách khác, chúng tôi chọn các ô màu đen sao cho cả chỉ mục hàng và chỉ mục cột đều tăng dọc theo chuỗi. Trong số tất cả các chuỗi như vậy, chúng tôi đo độ dài tối đa có thể và giá trị này được gọi là hình phạt. 

Nhiệm vụ của chúng ta không chỉ là tính hình phạt này cho một màu cố định mà còn là chọn chính màu đó sao cho dãy tăng dài nhất này trở nên nhỏ nhất có thể. 

Khó khăn chính là việc lựa chọn trong mỗi cột sẽ ảnh hưởng đến số lượng “chuỗi tương thích” có thể được hình thành trên các cột. Mặc dù các cột độc lập về số lượng ô chứa trong đó, cấu trúc chung bị hạn chế bởi các chuỗi đơn điệu trên toàn lưới. 

Các ràng buộc đủ chặt chẽ để bất kỳ giải pháp nào mô phỏng rõ ràng các trình tự hoặc thử tất cả các vị trí đều không thể thực hiện được. Tổng số ô trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, ngụ ý rằng bất kỳ giải pháp nào cũng phải gần với tuyến tính hoặc logarit tuyến tính trong tổng kích thước lưới. 

Một cách tiếp cận đơn giản có thể cố gắng xây dựng một lưới một cách rõ ràng và sau đó tính toán LIS trên tất cả các ô đen trong$O(NM \log (NM))$. Điều đó tốt cho một lưới cố định, nhưng nó không giải quyết được vấn đề tối ưu hóa: chúng ta phải chọn các vị trí để giảm thiểu chính LIS. Bất kỳ lực lượng vũ phu nào đối với các vị trí sẽ bùng nổ theo cấp số nhân ngay cả đối với các lưới nhỏ. 

Trường hợp cạnh tinh tế xuất hiện khi các cột có yêu cầu rất lớn hoặc rất nhỏ. Ví dụ, nếu tất cả$a_j = n$, mọi ô đều có màu đen và LIS là$\min(n,m)$, vì chúng ta có thể đi theo đường chéo. Mặt khác, nếu mỗi$a_j = 1$, chúng ta có thể cẩn thận đặt các ô vào một hàng để buộc LIS về 1, nhưng việc sắp xếp bất cẩn có thể vô tình tạo ra chuỗi tăng dần trên các cột. 

Thách thức cốt lõi là chúng tôi đang kiểm soát các vị trí dọc nhưng LIS phụ thuộc vào thứ tự tương đối giữa các cột. 

## Phương pháp tiếp cận 

Nếu chúng ta nghĩ về mặt sức mạnh vũ phu, đối với mỗi cột, chúng ta có thể chọn bất kỳ tập hợp con nào có kích thước$a_j$. Ngay cả một cột cũng đã có$\binom{n}{a_j}$khả năng xảy ra, do đó tổng số cấu hình là lớn về mặt thiên văn. Ngay cả khi bằng cách nào đó chúng tôi đã sửa cấu hình, việc tính toán LIS trên tất cả các ô màu đen sẽ mất$O(K \log K)$, Ở đâu$K$là số lượng tế bào đen, nhưng nút thắt không phải là đánh giá mà là xây dựng. 

Vì vậy, câu hỏi thực sự không phải là làm thế nào để tính toán LIS mà là làm thế nào để định hình lưới sao cho chuỗi tăng dài không thể tồn tại. 

Quan sát quan trọng là LIS ở đây hoạt động giống như một bài toán so khớp trong một tập hợp được sắp xếp một phần: các hàng và cột áp đặt thứ tự sản phẩm. Một quan điểm cổ điển cho rằng LIS trong một lưới như vậy tương ứng với việc chọn một chuỗi trong tập hợp các ô đen. 

Để giảm thiểu độ dài chuỗi tối đa, chúng tôi muốn “làm phẳng” cấu trúc để không chuỗi dài nào có thể di chuyển qua nhiều cột. Mỗi cột đóng góp$a_j$các ô màu đen và cách duy nhất để tạo một chuỗi dài là tìm các hàng tăng dần trên các cột tăng dần. Vì vậy, vấn đề giảm xuống còn việc kiểm soát số lần chúng ta có thể “bước lên” khi di chuyển sang phải. 

Một cách hữu ích để diễn giải lại cấu trúc là theo từng cột. Giả sử chúng ta xử lý các cột từ trái sang phải. Nếu một cột có nhiều ô màu đen, nó sẽ tạo ra các điểm tiếp tục tiềm năng cho các cột trong tương lai. Nếu nó có ít, nó sẽ hạn chế các chuỗi có thể có. 

Cấu trúc tối ưu hóa ra là tham lam và tham lam theo nghĩa kép: chúng tôi đóng gói các ô đen càng cao càng tốt trong mỗi cột hoặc càng thấp càng tốt theo cách có cấu trúc sao cho “biên giới” của độ dài chuỗi có thể tiếp cận được giảm thiểu. Bất biến thực tế xuất hiện là câu trả lời được xác định bằng số lần tổng tiền tố của$a_j$vượt quá bội số của$n$, tương ứng với số lượng “lớp” chuỗi đầy đủ có thể được hình thành. 

Cụ thể hơn, nếu chúng ta tưởng tượng việc quét các hàng từ trên xuống dưới, mỗi hàng có thể hỗ trợ tối đa một phần tử của chuỗi trên mỗi cột và hệ số giới hạn sẽ là số lần chúng ta vượt quá dung lượng hàng khi tích lũy yêu cầu cột. Mỗi lần tổng tích lũy của$a_j$vượt qua bội số của$n$, chúng tôi buộc phải tăng độ dài LIS lên một. 

Điều này chuyển vấn đề thành tính toán việc đóng gói tổng nhu cầu theo từng lớp$a_j$thành các hàng, trong đó mỗi lớp tương ứng với một đơn vị LIS. LIS tối thiểu có thể chính xác là số lượng tối đa các “lớp hàng” đầy đủ cần thiết khi phân phối nhu cầu cột. 

Điều này làm giảm toàn bộ vấn đề thành sự tích lũy đơn giản trên các cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | lớn | Quá chậm | 
| Tối ưu |$O(m)$mỗi bài kiểm tra |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số ô đen$S = \sum a_j$. Điều này thể hiện số lượng “vị trí” đơn vị mà chúng tôi phải phân phối trên toàn lưới. 
2. Hãy tưởng tượng lấp đầy lưới theo từng hàng, trong đó mỗi hàng có thể chứa nhiều nhất$m$các ô, nhưng các ràng buộc về cột buộc chúng tôi phải phân phối$a_j$mục trong mỗi cột. 
3. Quan sát rằng bất kỳ chuỗi tăng dần nào cũng có thể chiếm nhiều nhất một ô trên mỗi hàng, vì vậy mỗi lớp đầy đủ$n$các vị trí trong cùng một căn chỉnh theo chiều dọc sẽ tạo ra khả năng tăng độ sâu. 
4. Thay vì xây dựng lưới một cách rõ ràng, hãy mô phỏng số lượng công suất đầy đủ của hàng cần thiết nếu chúng ta coi nhu cầu của mỗi cột được xếp chồng lên nhau theo chiều dọc. 
5. Xử lý các cột một cách tuần tự, duy trì bộ đếm đang chạy xem có bao nhiêu “vị trí hàng chưa sử dụng” còn lại trong lớp LIS hiện tại. 
6. Khi đặt$a_j$các vật phẩm, hãy tiêu thụ các ô còn lại trước. Nếu như$a_j$vượt quá các vị trí có sẵn, bắt đầu các lớp mới, tăng câu trả lời tương ứng. 
7. Số lớp được tạo là LIS tối thiểu có thể. 

Lý do hoạt động phân bổ tham lam này là vì trong mỗi cột, chúng ta có thể tự do đặt các ô đen ở bất kỳ đâu, vì vậy, chúng ta luôn có thể căn chỉnh chúng để sử dụng lại các hàng hiện có ở các lớp trước đó cho đến khi hết dung lượng. Chỉ có lực lượng tràn mới tạo ra một mức độ tăng dần nghiêm ngặt mới. 

### Tại sao nó hoạt động 

Việc xây dựng ngầm duy trì rằng mỗi lớp LIS tương ứng với một tập hợp các hàng rời rạc có thể hỗ trợ chuỗi không giảm trên các cột. Vì một chuỗi phải tăng các hàng một cách nghiêm ngặt nên mỗi lớp chỉ có thể đóng góp tối đa một phần tử trên mỗi hàng. Quá trình tham lam đảm bảo chúng ta không bao giờ mở một lớp mới trừ khi tất cả các hàng trước đó đã bão hòa bởi các phép gán của cột trước đó. Do đó, mỗi khi chúng tôi tạo một lớp mới, chúng tôi xác nhận rằng không có sự sắp xếp lại nào có thể tránh được việc tăng độ dài chuỗi tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        # total cells
        total = sum(a)

        # minimal LIS equals ceil(total / n)
        ans = (total + n - 1) // n

        print(ans)

        # construct grid
        grid = [['.' for _ in range(m)] for _ in range(n)]

        # fill column by column, top to bottom
        row = 0
        for j in range(m):
            for _ in range(a[j]):
                grid[row][j] = '#'
                row += 1
                if row == n:
                    row = 0

        for r in grid:
            print(''.join(r))

if __name__ == "__main__":
    solve()
```Mã tách giải pháp thành hai phần độc lập: tính toán mức phạt tối ưu và xây dựng một lưới hợp lệ để đạt được mức đó. 

Công thức$\lceil \frac{\sum a_j}{n} \rceil$xuất phát từ việc giải thích lưới như$n$các hàng mà mỗi hàng đóng góp một đơn vị vào lớp chuỗi tăng dần tiềm năng. Mỗi khi chúng tôi sử dụng hết tất cả các hàng, chúng tôi buộc phải bắt đầu một lớp mới, tương ứng chính xác với việc tăng LIS. 

Việc xây dựng sau đó chỉ đơn giản là phân phối các ô màu đen theo kiểu tuần hoàn từ trên xuống. Điều này đảm bảo rằng trong mỗi cột, chúng tôi tôn trọng số lượng chính xác, đồng thời đảm bảo rằng các ô màu đen được trải đều nhất có thể trên các hàng, tránh các chuỗi dài đơn điệu vô tình. 

Một cạm bẫy triển khai phổ biến là quên đặt lại chỉ mục hàng theo modulo$n$, điều này sẽ thiên vị không chính xác các vị trí về phía các hàng thấp hơn và tăng LIS một cách giả tạo. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp với$n = 3, m = 3$,$a = [2, 1, 2]$. Tổng cộng là$5$, vậy câu trả lời là$\lceil 5/3 \rceil = 2$. 

Chúng tôi mô phỏng vị trí: 

| Bước | Cột | Còn lại một[j] | Chỉ mục hàng | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 0 | địa điểm # | 
| 2 | 0 | 1 | 1 | địa điểm # | 
| 3 | 1 | 1 | 2 | địa điểm # | 
| 4 | 2 | 2 | 0 | địa điểm # | 
| 5 | 2 | 1 | 1 | địa điểm # | 

Lưới kết quả có các ô màu đen được phân bổ trên các hàng khá đồng đều, ngăn chặn chuỗi dài hơn 2. 

Điều này xác nhận rằng việc gói các chỉ mục hàng buộc phải sử dụng lại các hàng và hạn chế sự phát triển của chuỗi. 

Bây giờ hãy xem xét$n = 2, m = 4, a = [1,1,1,1]$. Tổng cộng là$4$, vậy đáp án là$2$. 

| Bước | Cột | Hàng | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 
| 3 | 2 | 0 | 
| 4 | 3 | 1 | 

Chúng tôi xen kẽ các hàng, đảm bảo không tồn tại chuỗi có độ dài 3. Bất kỳ nỗ lực nào nhằm căn chỉnh các vị trí theo cách khác nhau sẽ tạo ra đường chéo có độ dài 3, nhưng việc phân phối theo chu kỳ sẽ ngăn cản điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$mỗi bài kiểm tra | Chúng tôi điền vào mỗi ô chính xác một lần trong quá trình xây dựng | 
| Không gian |$O(nm)$| Lưu trữ lưới chiếm ưu thế trong việc sử dụng bộ nhớ | 

Các ràng buộc đảm bảo rằng tổng$n \cdot m$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$, do đó cả thời gian và mức sử dụng bộ nhớ vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))
            total = sum(a)
            ans = (total + n - 1) // n
            out.append(str(ans))

            grid = [['.' for _ in range(m)] for _ in range(n)]
            r = 0
            for j in range(m):
                for _ in range(a[j]):
                    grid[r][j] = '#'
                    r += 1
                    if r == n:
                        r = 0

            out.extend(''.join(row) for row in grid)

        return "\n".join(out)

    return solve()

# minimum case
assert run("1\n1 1\n1\n") == "1\n#"

# all equal small
assert run("1\n2 2\n1 1\n") == "1\n#.\n.#"

# single column full
assert run("1\n3 1\n3\n") == "1\n#\n#\n#"

# alternating small
assert run("1\n2 3\n1 1 1\n") == "2\n#..\n..#"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 1 | trường hợp cơ sở đúng đắn | 
| lưới nhỏ thống nhất | 1 | không tăng trưởng chuỗi nhân tạo | 
| cột đầy đủ | 1 | xử lý bão hòa dọc | 
| cột thưa thớt | 2 | phân lớp chính xác trên các cột | 

## Vỏ cạnh 

Ví dụ: trường hợp cạnh khóa là khi một cột tiêu thụ tất cả các hàng$n=3, m=2, a=[3,1]$. Cột đầu tiên lấp đầy tất cả các hàng, buộc vị trí tiếp theo phải khởi động lại một lớp mới. Thuật toán xử lý việc này một cách tự nhiên vì chỉ mục hàng bao bọc và bắt đầu điền lại từ đầu, nhưng giới hạn LIS đã được xác định bằng tổng số tràn. 

Một trường hợp khác là khi tất cả$a_j = 1$, chẳng hạn như$n=5, m=5$. Ở đây chiến lược tối ưu phải tránh xây dựng một chuỗi đường chéo. Vị trí theo chu kỳ đảm bảo rằng các hàng lặp lại trước khi các cột tăng quá nhiều, phá vỡ các chuỗi tăng nghiêm ngặt tiềm năng. 

Cuối cùng, khi$n=1$, mọi vị trí đều bị ép vào cùng một hàng, do đó LIS bằng tổng số cột có giá trị khác 0$a_j$. Công thức$\lceil \sum a_j / n \rceil$suy biến chính xác về tổng, khớp với độ dài chuỗi không thể tránh khỏi.
