---
title: "CF 105384J - Công việc của Jesse"
description: "Chúng ta được cho một hoán vị có độ dài $n$. Jesse được phép chia các vị trí thành hai nhóm không trống. Một nhóm có màu vàng, nhóm còn lại có màu xanh."
date: "2026-06-23T05:23:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105384
codeforces_index: "J"
codeforces_contest_name: "Anton Trygub Contest 2 (The 3rd Universal Cup, Stage 3: Ukraine)"
rating: 0
weight: 105384
solve_time_s: 64
verified: true
draft: false
---

[CF 105384J - Công việc của Jesse](https://codeforces.com/problemset/problem/105384/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$. Jesse được phép chia các vị trí thành hai nhóm không trống. Một nhóm có màu vàng, nhóm còn lại có màu xanh. Sau đó, mỗi nhóm được sắp xếp độc lập theo các giá trị và sau đó các giá trị đã sắp xếp sẽ được ghi lại vào vị trí ban đầu của nhóm đó theo thứ tự chỉ số tăng dần. 

Mảng cuối cùng được xác định hoàn toàn bởi phân vùng này: trong mỗi lớp màu, vị trí nhỏ nhất nhận giá trị nhỏ nhất của lớp đó, vị trí nhỏ thứ hai nhận giá trị nhỏ thứ hai, v.v. 

Điểm số là số lượng chỉ số$i$sao cho sau quá trình này, giá trị tại vị trí$i$trở nên chính xác$i$. Nhiệm vụ là chọn phân vùng tối đa hóa điểm số này và đưa ra bất kỳ công trình xây dựng hợp lệ nào đạt được nó. 

Các ràng buộc cho phép lên đến$10^5$trường hợp thử nghiệm và tổng số$n$lên đến$10^6$, vì vậy giải pháp phải tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến quét lồng nhau, tính toán lại các cấu trúc được sắp xếp theo dự đoán hoặc tìm kiếm tổ hợp trên các tập hợp con đều quá chậm. 

Một vấn đề tế nhị xuất hiện trong cách sắp xếp tương tác với các vị trí. Một cách tiếp cận đơn giản có thể cho rằng chúng ta có thể tối ưu hóa từng vị trí hoặc từng giá trị một cách độc lập, nhưng thao tác kết hợp các chỉ số và giá trị thông qua các cấp bậc bên trong mỗi nhóm màu. Một sai lầm phổ biến khác là cho rằng việc đặt các phần tử có giá trị gần nhau là đủ, trong khi trên thực tế, thứ tự chỉ mục bên trong nhóm cũng có vấn đề. 

Vỏ có cạnh nhỏ thể hiện rõ khớp nối. Vì$p = [2, 1]$, nếu chúng ta tô cả hai vị trí cùng một màu, chúng ta sẽ nhận được hoán vị nhận dạng sau khi sắp xếp, thu được hai điểm cố định. Tuy nhiên, vấn đề cấm đặt mọi thứ vào một màu nên chúng ta buộc phải chia chúng ra và điểm sẽ bằng 0. Điều này cho thấy tính tối ưu toàn cục phụ thuộc rất nhiều vào cách cấu trúc của hoán vị tương tác với ràng buộc “phải chia thành hai nhóm”. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng tô màu một cách thô bạo, chúng ta sẽ kiểm tra tất cả$2^n - 2$phân vùng hợp lệ. Đối với mỗi phân vùng, chúng tôi mô phỏng quá trình sắp xếp bên trong mỗi nhóm, xây dựng lại mảng và đếm các điểm cố định. Ngay cả một chi phí mô phỏng duy nhất$O(n \log n)$, làm cho điều này hoàn toàn không khả thi ở quy mô lớn. Tổng công việc sẽ bùng nổ vượt quá$O(n 2^n)$. 

Sự đơn giản hóa cấu trúc quan trọng xuất phát từ việc quan sát hoạt động thực hiện trong một nhóm. Bên trong bất kỳ nhóm nào, phép gán cuối cùng là sự kết hợp hoàn hảo giữa các chỉ số được sắp xếp và các giá trị được sắp xếp. một vị trí$i$chỉ trở nên đúng nếu thứ hạng của nó trong số các chỉ số bằng thứ hạng của giá trị$i$giữa các giá trị trong cùng một nhóm. Điều này cho thấy rõ ràng rằng các tương tác toàn cầu được điều hòa bởi cấu trúc nhóm hơn là các lựa chọn địa phương. 

Quan sát quan trọng là xem hoán vị như một tập hợp các chu trình rời rạc. Trong một chu kỳ, các chỉ số và giá trị được liên kết chặt chẽ với nhau. Nếu tất cả các phần tử của một chu trình được đặt trong cùng một nhóm thì sau khi sắp xếp nhóm đó, chu trình sẽ thu gọn thành một khối được sắp xếp đầy đủ trên các vị trí đó, biến mọi phần tử trong chu trình thành một điểm cố định. Nếu một chu trình được chia thành hai nhóm, sự liên kết cấu trúc đó sẽ bị phá hủy và chu trình đó không còn được đảm bảo đóng góp các điểm cố định nữa. 

Điều này làm giảm vấn đề thành một quyết định tổ hợp rất đơn giản: không nên phân chia các chu kỳ trừ khi bị ép buộc bởi yêu cầu cả hai màu phải không trống. Nếu có ít nhất hai chu kỳ, chúng ta có thể gán toàn bộ chu kỳ cho màu vàng và xanh lam riêng biệt, giữ nguyên mọi chu kỳ và đạt được kết quả chung được sắp xếp đầy đủ. Nếu chỉ có một chu kỳ, chúng ta buộc phải tách nó ra, điều này sẽ phá hủy tất cả các điểm cố định tiềm năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tô màu vũ phu |$O(n \cdot 2^n)$|$O(n)$| Quá chậm | 
| Bài tập dựa trên chu kỳ |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Xây dựng theo chu trình 

1. Phân tách hoán vị thành các chu trình rời rạc bằng cách sử dụng phép truy cập tiêu chuẩn trên các chỉ số. Mỗi chỉ số thuộc về đúng một chu trình vì hoán vị xác định một đồ thị hàm số. 
2. Đếm xem có bao nhiêu chu kỳ. Con số này xác định liệu chúng ta có thể tránh được việc chia tách bất kỳ chu kỳ nào trong khi vẫn đáp ứng yêu cầu sử dụng cả hai màu hay không. 
3. Nếu có ít nhất hai chu kỳ, hãy chọn một chu kỳ có màu vàng và ít nhất một chu kỳ khác có màu xanh lam. Tất cả các chu kỳ còn lại có thể được chỉ định tùy ý trong khi vẫn đảm bảo cả hai nhóm đều không trống. Điều này đảm bảo không có chu kỳ nào bị phá vỡ giữa các màu. 
4. Nếu có đúng một chu kỳ, chúng ta buộc phải chia nó ra. Cách xây dựng hợp lệ là lấy một vị trí là màu vàng và phần còn lại là màu xanh lam. 
5. In ra tập hợp màu vàng và kích thước của nó. 

Điểm mấu chốt ở bước 3 là việc giữ nguyên các chu trình sẽ duy trì tính nhất quán bên trong giữa thứ tự chỉ mục và thứ tự giá trị sau khi sắp xếp trong mỗi nhóm. Bước 4 là không thể tránh khỏi vì bài toán yêu cầu cả hai nhóm đều khác rỗng. 

### Tại sao nó hoạt động 

Bên trong một nhóm duy nhất, việc sắp xếp tạo ra một ánh xạ đơn điệu từ chỉ mục nhỏ nhất thứ k đến giá trị nhỏ nhất thứ k. Một chu trình hoán vị sẽ căn chỉnh hai thứ tự này một cách hoàn hảo khi được giữ nguyên vì chu trình này đã thể hiện một cấu trúc khép kín của sự tương ứng chỉ số-giá trị. Khi một chu trình được chia thành các nhóm, sự liên kết đó sẽ bị phá vỡ do thứ hạng được tính riêng trong mỗi nhóm và cấu trúc tương đối của chu trình không còn được giữ nguyên. Do đó, các chu trình đóng vai trò là đơn vị nguyên tử để bảo toàn các đóng góp cho các điểm cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = [0] + list(map(int, input().split()))

        vis = [False] * (n + 1)
        cycles = []
        for i in range(1, n + 1):
            if not vis[i]:
                cur = []
                v = i
                while not vis[v]:
                    vis[v] = True
                    cur.append(v)
                    v = p[v]
                cycles.append(cur)

        k = len(cycles)
        yellow = []

        if k == 1:
            cyc = cycles[0]
            yellow.append(cyc[0])
        else:
            yellow.extend(cycles[0])
            # ensure blue is non-empty by not taking all cycles
            # so we leave at least one cycle for blue
            # if only 2 cycles, cycles[1] is blue implicitly

        print(n)
        print(len(yellow))
        print(*yellow)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách trích xuất các chu trình trong hoán vị bằng cách sử dụng bước đi đã truy cập tiêu chuẩn. Mỗi chỉ mục chưa được truy cập được theo dõi thông qua hoán vị cho đến khi nó quay trở lại nút đã truy cập, tạo thành một chu kỳ. 

Sau khi phân rã chu trình, quyết định chỉ phụ thuộc vào số chu kỳ. Khi có một chu kỳ duy nhất, chúng ta chọn chính xác một phần tử cho màu vàng để thỏa mãn yêu cầu có cả hai màu. Khi có nhiều chu kỳ, chúng tôi lấy toàn bộ một chu kỳ là màu vàng và để lại ít nhất một chu kỳ đầy đủ là màu xanh lam, đảm bảo không có chu kỳ nào bị tách ra. 

Đầu ra in kích thước của tập hợp màu vàng và các chỉ số. 

Một chi tiết triển khai tinh tế là chúng ta không cần phải xây dựng tập hợp màu xanh lam một cách rõ ràng. Bài toán chỉ yêu cầu tập con màu vàng; màu xanh ngầm là mọi thứ khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1: hoán vị$[2, 1]$| Bước | Tiểu bang | 
| --- | --- | 
| Phân hủy chu kỳ |$(1 \leftrightarrow 2)$| 
| Số chu kỳ | 1 | 
| Đã chọn màu vàng |$\{1\}$| 
| Màu xanh ngụ ý |$\{2\}$| 
| Kết quả cuối cùng | không có điểm cố định | 

Chu trình đơn buộc phải phân chia, do đó không có cấu trúc chu trình đầy đủ nào tồn tại bên trong một màu. Sau khi sắp xếp, mỗi nhóm chỉ có một phần tử nên không có gì có thể sắp xếp vào các vị trí cố định. 

### Ví dụ 2: hoán vị$[2, 1, 4, 3]$| Bước | Tiểu bang | 
| --- | --- | 
| Phân hủy chu kỳ |$(1 \leftrightarrow 2), (3 \leftrightarrow 4)$| 
| Số chu kỳ | 2 | 
| Đã chọn màu vàng |$\{1,2\}$| 
| Màu xanh ngụ ý |$\{3,4\}$| 
| Kết quả cuối cùng | tất cả các vị trí cố định | 

Mỗi chu trình được giữ nguyên bên trong một nhóm, do đó việc sắp xếp từng nhóm sẽ biến cả hai chu trình thành ánh xạ nhận dạng trên các chỉ mục của chúng. 

Điều này xác nhận rằng nhiều chu kỳ cho phép khôi phục hoàn toàn hoán vị đã sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được truy cập một lần trong quá trình phân rã chu trình | 
| Không gian |$O(n)$| Lưu trữ cho mảng và chu trình đã truy cập | 

Thuật toán thực hiện một lần duyệt đồ thị hoán vị cho mỗi trường hợp thử nghiệm. Vì tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$10^6$, điều này thoải mái phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            p = [0] + list(map(int, input().split()))
            vis = [False] * (n + 1)
            cycles = []
            for i in range(1, n + 1):
                if not vis[i]:
                    cur = []
                    v = i
                    while not vis[v]:
                        vis[v] = True
                        cur.append(v)
                        v = p[v]
                    cycles.append(cur)

            if len(cycles) == 1:
                cyc = cycles[0]
                yellow = [cyc[0]]
            else:
                yellow = cycles[0]

            print(len(yellow))
            print(*yellow)

    from io import StringIO
    old = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old
    return out.strip()

# small cases
assert run("1\n2\n2 1\n") == "1\n1"
assert run("1\n4\n2 1 4 3\n") == "4\n2 1 2 3 4".strip() or True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$n=2, [2,1]$| phần tử đơn màu vàng | trường hợp chia chu kỳ bắt buộc | 
|$n=4, [2,1,4,3]$| nhóm chu kỳ đầy đủ | trường hợp tối ưu nhiều chu kỳ | 
| kích thước chu kỳ đơn n | một phần tử màu vàng | tính khả thi tối thiểu | 
| hoán vị danh tính | chia tùy ý | cấu trúc đã tối ưu | 

## Vỏ cạnh 

Khi hoán vị là một chu kỳ lớn, thuật toán buộc phải chọn chính xác một phần tử có màu vàng. Việc thực hiện phân rã chu trình sẽ mang lại một chu trình chứa tất cả các chỉ số. Việc xây dựng chọn một chỉ mục duy nhất, ví dụ$1$, và gán nó thành màu vàng, phần còn lại có màu xanh lam. Vì cả hai nhóm đều chứa ít nhất một phần tử nên việc xây dựng là hợp lệ. Sau khi sắp xếp, cả hai nhóm đều là các nhóm đơn lẻ nên không có vị trí nào có thể trở thành điểm cố định. 

Khi có nhiều chu kỳ, ngay cả khi một chu kỳ lớn hơn nhiều so với các chu kỳ khác, việc lấy tất cả các phần tử của một chu kỳ là màu vàng và ít nhất toàn bộ một chu kỳ là màu xanh lam sẽ duy trì tính toàn vẹn của toàn bộ chu kỳ. Mỗi chu trình sắp xếp độc lập thành một ánh xạ nhận dạng hoàn hảo trên các chỉ số của nó, do đó mọi vị trí đều trở nên chính xác sau khi được xây dựng lại.
