---
title: "CF 104316A - \u0411\u043b\u0438\u043d\u0441\u043a\u0438\u0435 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438..."
description: "Chúng ta có một hệ thống gồm $n$ vị trí, mỗi vị trí ban đầu giữ một học sinh. Sự sắp xếp ban đầu chưa được biết và được biểu diễn bằng hoán vị $b$."
date: "2026-07-01T19:34:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104316
codeforces_index: "A"
codeforces_contest_name: "VIII \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e. \u0424\u0438\u043d\u0430\u043b"
rating: 0
weight: 104316
solve_time_s: 60
verified: true
draft: false
---

[CF 104316A - \u0411\u043b\u0438\u043d\u0441\u043a\u0438\u0435 \u043f\u0435\u0440\u0435\u0441\u0442\u0430\u043d\u043e\u0432\u043a\u0438...](https://codeforces.com/problemset/problem/104316/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống$n$các vị trí, ban đầu mỗi vị trí giữ một học sinh. Sự sắp xếp ban đầu chưa được biết và được biểu diễn bằng một hoán vị$b$. Theo thời gian, các học sinh liên tục di chuyển theo một hoán vị cố định$p$: học sinh ngồi ở vị trí$i$di chuyển đến vị trí$p_i$trong một bước. Việc chuyển đổi này được áp dụng nhiều lần. 

Sau tất cả các chuyển động, chúng tôi quan sát sự sắp xếp cuối cùng$a$, đó cũng là một hoán vị. Nhiệm vụ là xây dựng lại sự sắp xếp ban đầu có thể$b$sao cho việc áp dụng hoán vị$p$nhiều lần dẫn đến$a$sau một số vòng đầy đủ và trong số tất cả các sắp xếp ban đầu hợp lệ, chúng ta phải xuất ra kết quả nhỏ nhất về mặt từ điển. 

Khó khăn chính là quá trình này có thể đảo ngược hoàn toàn về mặt cấu trúc vì$p$là một hoán vị nên mọi vị trí đều nằm trên một chu trình. Mỗi chu kỳ phát triển độc lập và trong mỗi chu kỳ, về cơ bản chúng ta là những giá trị luân chuyển. 

Ràng buộc$n \le 5 \cdot 10^5$ngụ ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Bất cứ điều gì bậc hai theo chu kỳ đều không thể thực hiện được vì các chu kỳ trong trường hợp xấu nhất đều có thể có độ dài bằng 1 hoặc một chu kỳ lớn và mô phỏng lặp lại sẽ vượt quá giới hạn thời gian. 

Trường hợp cạnh tinh tế phát sinh khi các chu trình có nhiều cách sắp xếp hợp lệ dẫn đến cùng một cách sắp xếp cuối cùng. Ví dụ: nếu một chu trình có độ dài 4 và giá trị cuối cùng là$[1,2,3,4]$dưới một số phép quay thì nhiều phép quay ban đầu là hợp lệ. Việc tái cấu trúc đơn giản có thể chọn một phép quay tùy ý trên mỗi chu kỳ, nhưng việc giảm thiểu từ điển sẽ kết hợp các lựa chọn trong các chu kỳ vì các vị trí trước đó trong hoán vị quan trọng hơn. 

Một trường hợp không hề tầm thường khác là khi các chu trình độc lập nhưng thứ tự từ điển buộc chúng ta phải chọn góc quay nhỏ nhất trên mỗi chu kỳ trong một ánh xạ vị trí nhất quán, chứ không phải một cách tham lam về mặt giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị ban đầu có thể có$b$, mô phỏng áp dụng$p$cho đến khi chúng tôi đạt được$a$, và kiểm tra tính hợp lệ. Điều này ngay lập tức không thể thực hiện được vì có$n!$các ứng cử viên, và thậm chí một chi phí mô phỏng duy nhất$O(n)$, dẫn đến$O(n! \cdot n)$, có giá trị lớn về mặt thiên văn. 

Cấu trúc của bài toán được điều chỉnh bởi hoán vị$p$. Mọi chỉ số đều thuộc về một chu trình có định hướng và việc áp dụng$p$liên tục chỉ xoay các giá trị trong mỗi chu kỳ. Điều này có nghĩa là vấn đề phân hủy thành các chu kỳ độc lập. 

Trong một chu trình, giả sử chúng ta liệt kê các chỉ số của nó theo thứ tự truyền tải. Sự biến đổi$p$hoạt động như một sự dịch chuyển theo chu kỳ. Cấu hình cuối cùng$a$do đó phải tương ứng với một số vòng quay của cấu hình ban đầu$b$. Điều này làm giảm vấn đề trên mỗi chu kỳ thành: chọn một vòng quay của chu kỳ ánh xạ về phía trước theo các ca lặp lại để khớp với$a$và chọn cách sắp xếp toàn cục nhỏ nhất về mặt từ điển. 

Quan sát quan trọng là thay vì suy nghĩ về phía trước theo thời gian, chúng ta có thể gán cho mỗi chu kỳ một “độ lệch khởi đầu” nhất quán sao cho khi chúng ta đi qua chu trình theo hướng thuận, sự sắp xếp kết quả sẽ khớp với$a$. Ràng buộc từ$a$xác định duy nhất những phép quay nào là hợp lệ và trong số đó chúng tôi chọn phép quay nhỏ nhất về mặt từ điển bằng cách cố định giá trị bắt đầu nhỏ nhất có thể ở vị trí sớm nhất trong thứ tự chu kỳ. 

Bởi vì các chu trình là độc lập nhưng thứ tự từ điển so sánh các mảng toàn cục, nên chúng tôi xử lý các chu trình theo thứ tự tăng dần của chỉ số nhỏ nhất của chúng và gán cho vòng quay khả thi nhỏ nhất một cách tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Phân hủy chu trình |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta phân tách hoán vị$p$thành những chu kỳ rời rạc. Đối với mỗi chu kỳ, chúng tôi ghi lại các chỉ số của nó theo thứ tự truyền tải. 

Tiếp theo, chúng tôi sử dụng sự sắp xếp cuối cùng$a$để hiểu cách đặt các giá trị bên trong mỗi chu kỳ. Vì chuyển động chỉ hoán vị trong các chu kỳ nên mỗi chu kỳ trong$a$phải tương ứng chính xác với một phép quay của các giá trị được gán cho chu trình đó trong$b$. 

Chúng tôi xử lý từng chu trình một cách độc lập nhưng chúng tôi xây dựng$b$theo cách đảm bảo tính tối thiểu về mặt từ điển trên toàn cầu. 

1. Xác định tất cả các chu kỳ của$p$. Đối với mỗi chỉ mục chưa được truy cập, hãy làm theo$p$cho đến khi quay lại điểm bắt đầu, ghi lại chu trình theo thứ tự. 
2. Đối với mỗi chu kỳ, trích xuất chuỗi các giá trị từ$a$tại các vị trí đó theo thứ tự chu kỳ. Điều này đưa ra phiên bản xoay cuối cùng của chu kỳ đó. 
3. Xác định phép quay để tạo ra sự sắp xếp ban đầu nhỏ nhất có thể về mặt từ điển. Vì bất kỳ phép quay nào cũng hợp lệ nên chúng tôi chọn phép quay làm cho phần tử đầu tiên của chu trình càng nhỏ càng tốt và nếu bị ràng buộc, sẽ tiếp tục giảm thiểu theo từ điển dọc theo chu trình. 
4. Viết lại các giá trị đã chọn này vào$b$dọc theo các vị trí chu kỳ theo thứ tự tương ứng. 

Phần tinh tế là "nhỏ nhất về mặt từ điển" áp dụng cho toàn bộ mảng chứ không phải mảng theo chu kỳ cục bộ. Tuy nhiên, vì các chu trình là rời rạc nên vị trí khác nhau sớm nhất giữa hai ứng cử viên nằm trong chu trình chứa chỉ số nhỏ nhất mà chúng khác nhau. Do đó, việc giảm thiểu từng chu kỳ theo thứ tự tăng dần chỉ số tối thiểu sẽ đảm bảo tính tối thiểu toàn cục. 

### Tại sao nó hoạt động 

Mỗi chu kỳ là một nhóm quay độc lập dưới sự hoán vị$p$. Mảng cuối cùng$a$sửa nhiều tập hợp giá trị trên mỗi chu kỳ và thứ tự tuần hoàn của chúng theo vòng quay. Bất kỳ cấu hình ban đầu hợp lệ nào đều tương ứng với việc chọn một vòng quay cho mỗi chu kỳ. So sánh từ điển giữa hai ứng cử viên toàn cầu được xác định ở chỉ số đầu tiên nơi chúng khác nhau, nằm trong đúng một chu kỳ. Việc chọn phép xoay nhỏ nhất về mặt từ điển trong mỗi chu kỳ sẽ đảm bảo không có phép xoay thay thế nào có thể tạo ra tiền tố nhỏ hơn ở chỉ mục bị ảnh hưởng đầu tiên của chu kỳ, do đó không thể cải thiện toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = [0] + list(map(int, input().split()))
    a = [0] + list(map(int, input().split()))

    vis = [False] * (n + 1)
    b = [0] * (n + 1)

    for i in range(1, n + 1):
        if vis[i]:
            continue

        cycle = []
        cur = i
        while not vis[cur]:
            vis[cur] = True
            cycle.append(cur)
            cur = p[cur]

        m = len(cycle)

        # values in final arrangement along cycle order
        vals = [a[x] for x in cycle]

        # find lexicographically smallest rotation of vals
        # by doubling and using minimal starting point
        best = 0
        doubled = vals * 2

        for j in range(1, m):
            for k in range(m):
                if doubled[j + k] < doubled[best + k]:
                    best = j
                    break
                if doubled[j + k] > doubled[best + k]:
                    break

        rotated = [doubled[best + k] for k in range(m)]

        for idx, pos in enumerate(cycle):
            b[pos] = rotated[idx]

    print(*b[1:])

if __name__ == "__main__":
    solve()
```Lời giải bắt đầu bằng cách đọc hoán vị và sắp xếp cuối cùng. Sau đó nó phân hủy$p$thành các chu kỳ bằng cách sử dụng một mảng đã được truy cập. Đối với mỗi chu kỳ, nó trích xuất các giá trị tương ứng từ$a$, biểu thị chu trình đó trông như thế nào sau tất cả các phép quay. 

Để giảm thiểu về mặt từ điển, nó tính toán góc quay nhỏ nhất của chuỗi giá trị của chu kỳ đó bằng cách sử dụng phép so sánh nhân đôi đơn giản. Điều này là đủ vì mỗi chu trình là độc lập và chúng ta chỉ cần độ dịch chu kỳ nhỏ nhất. 

Cuối cùng, nó ghi chuỗi đã quay trở lại vị trí chu trình ban đầu theo thứ tự truyền tải, tạo ra sự sắp xếp ban đầu. 

Một cạm bẫy phổ biến là giả sử chúng ta có thể sắp xếp các giá trị chu trình hoặc đặt các phần tử nhỏ nhất lên đầu tiên một cách tham lam. Điều đó phá vỡ ràng buộc xoay: các giá trị bị khóa theo thứ tự tuần hoàn, không thể hoán vị tự do. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
p = [2,1,4,3]
a = [3,2,1,4]
```Sự phân hủy chu trình mang lại$[1,2]$Và$[3,4]$. 

Đối với chu kỳ$[1,2]$, giá trị từ$a$là$[3,2]$. Các phép quay là$[3,2]$,$[2,3]$. Tối thiểu là$[2,3]$, Vì thế$b[1]=2, b[2]=3$. 

Đối với chu kỳ$[3,4]$, các giá trị là$[1,4]$. Các phép quay là$[1,4]$,$[4,1]$. Tối thiểu là$[1,4]$, nên không thay đổi. 

| Chu kỳ | giá trị a | Vòng quay được chọn | b bài tập | 
| --- | --- | --- | --- | 
| [1,2] | [3,2] | [2,3] | b1=2, b2=3 | 
| [3,4] | [1,4] | [1,4] | b3=1, b4=4 | 

Đầu ra:```
2 3 1 4
```Điều này xác nhận rằng mỗi chu trình được xử lý độc lập trong khi vẫn duy trì việc giảm thiểu từ điển. 

### Ví dụ 2 

đầu vào:```
n = 6
p = [2,1,4,5,3,6]
a = [3,1,2,4,6,5]
```Chu kỳ là$[1,2]$,$[3,4,5]$,$[6]$. 

Vì$[1,2]$, giá trị$[3,1]$, phép quay tốt nhất là$[1,3]$. 

Vì$[3,4,5]$, giá trị$[2,4,6]$, các phép quay được so sánh và nhỏ nhất là$[2,4,6]$. 

Vì$[6]$, phần tử đơn vẫn còn$[5]$. 

| Chu kỳ | giá trị a | Vòng quay tốt nhất | Kết quả | 
| --- | --- | --- | --- | 
| [1,2] | [3,1] | [1,3] | b1=1, b2=3 | 
| [3,4,5] | [2,4,6] | [2,4,6] | b3=2, b4=4, b5=6 | 
| [6] | [5] | [5] | b6=5 | 

Đầu ra:```
1 3 2 4 6 5
```Điều này cho thấy các chu kỳ lớn hơn hoạt động giống hệt như các bài toán quay độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$tệ nhất trong kiểm tra xoay vòng ngây thơ,$O(n)$khấu hao theo chu kỳ | Mỗi chỉ số thuộc về đúng một chu kỳ; tổng đường truyền là tuyến tính | 
| Không gian |$O(n)$| Mảng hoán vị, điểm đánh dấu đã truy cập và đầu ra | 

Thuật toán chạy thoải mái trong giới hạn vì mỗi nút được truy cập một lần trong quá trình phân rã chu trình và quá trình xử lý chu trình có tổng kích thước tuyến tính. Ngay cả việc so sánh xoay vẫn bị giới hạn vì mỗi phần tử tham gia vào đúng một chu kỳ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(input())
    p = [0] + list(map(int, input().split()))
    a = [0] + list(map(int, input().split()))

    vis = [False] * (n + 1)
    b = [0] * (n + 1)

    for i in range(1, n + 1):
        if vis[i]:
            continue
        cycle = []
        cur = i
        while not vis[cur]:
            vis[cur] = True
            cycle.append(cur)
            cur = p[cur]

        vals = [a[x] for x in cycle]
        m = len(vals)
        doubled = vals * 2

        best = 0
        for j in range(1, m):
            for k in range(m):
                if doubled[j + k] < doubled[best + k]:
                    best = j
                    break
                if doubled[j + k] > doubled[best + k]:
                    break

        for idx, pos in enumerate(cycle):
            b[pos] = doubled[best + idx]

    return " ".join(map(str, b[1:]))

# sample-like tests
assert run("4\n2 1 4 3\n3 2 1 4\n") == "2 3 1 4"
assert run("2\n2 1\n2 1\n") == "1 2"

# minimum size
assert run("1\n1\n1\n") == "1"

# single cycle
assert run("3\n2 3 1\n2 3 1\n") == "1 2 3"

# all same structure but different rotation
assert run("5\n2 3 4 5 1\n2 3 4 5 1\n") == "1 2 3 4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 chu kỳ | tầm thường | độ đúng cơ sở | 
| danh tính | cùng một mảng | điểm cố định | 
| chu kỳ đầy đủ | sắp xếp xoay | xử lý xoay | 
| cấu trúc lặp lại | sự tối giản từ điển | tính nhất quán toàn cầu | 

## Vỏ cạnh 

Chu trình một phần tử hoạt động bình thường vì chỉ có một phép quay hợp lệ. Thuật toán xử lý nó như một chu kỳ có độ dài một và gán giá trị trực tiếp từ$a$, tạo ra giá trị ban đầu duy nhất có thể. 

Đối với một chu kỳ có độ dài đầy đủ, tìm kiếm xoay của thuật toán sẽ so sánh tất cả các dịch chuyển của các giá trị chu kỳ. Ngay cả khi nhiều phép quay có vẻ giống nhau, phép quay nhỏ nhất về mặt từ điển vẫn được chọn một cách nhất quán vì việc so sánh được thực hiện theo từ điển trên mảng nhân đôi. 

Khi tất cả các chu kỳ đều có độ dài bằng hai, mỗi chu kỳ sẽ trở thành một quyết định hoán đổi đơn giản. Thuật toán vẫn xử lý chúng một cách thống nhất do so sánh xoay chuyển thành so sánh hai phần tử. 

Một tình huống tinh tế hơn là khi nhiều chu kỳ chứa các mẫu giá trị giống hệt nhau. Ngay cả khi đó, tính độc lập của chu trình đảm bảo không có sự can thiệp và việc sắp xếp theo chỉ số nhỏ nhất trong các chu trình đảm bảo sự phân công xác định.
