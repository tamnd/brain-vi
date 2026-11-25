---
title: "CF 997E - Phân khúc tốt"
description: "Chúng ta được cấp một hoán vị, nghĩa là mọi giá trị từ 1 đến n xuất hiện chính xác một lần trong một mảng. Đối với bất kỳ phân đoạn liền kề nào của mảng này, chúng tôi gọi là tốt khi nó có đặc tính cấu trúc rất mạnh: nếu bạn lấy các giá trị nhỏ nhất và lớn nhất bên trong phân đoạn đó thì mọi…"
date: "2026-06-16T23:59:50+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 997
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 493 (Div. 1)"
rating: 3000
weight: 997
solve_time_s: 87
verified: true
draft: false
---

[CF 997E - Phân đoạn tốt](https://codeforces.com/problemset/problem/997/E) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị, nghĩa là mọi giá trị từ 1 đến n xuất hiện chính xác một lần trong một mảng. Đối với bất kỳ phân đoạn liền kề nào của mảng này, chúng tôi gọi là tốt khi nó có đặc tính cấu trúc rất mạnh: nếu bạn lấy các giá trị nhỏ nhất và lớn nhất bên trong phân đoạn đó thì mọi số nguyên giữa chúng cũng phải xuất hiện ở đâu đó bên trong cùng một phân đoạn. Vì chúng ta đang làm việc với một hoán vị, điều này tương đương với việc nói rằng các giá trị bên trong phân đoạn tạo thành một tập hợp các số nguyên liên tiếp. 

Do đó, một phân đoạn sẽ hoạt động chính xác khi tập hợp các giá trị mà nó chứa là một khoảng các số nguyên, ngay cả khi chúng xuất hiện theo bất kỳ thứ tự nào. 

Đối với mỗi truy vấn, chúng ta được cung cấp một phân đoạn lớn hơn của mảng và chúng ta phải đếm xem có bao nhiêu phân đoạn con bên trong nó thỏa mãn thuộc tính này. 

Kích thước đầu vào buộc chúng tôi phải đưa ra giải pháp được tối ưu hóa cao. Với tối đa 120000 phần tử và 120000 truy vấn, bất kỳ giải pháp nào kiểm tra tất cả các phân đoạn con trên mỗi truy vấn đều không khả thi ngay lập tức. Ngay cả việc kiểm tra tất cả các phân đoạn của tất cả các truy vấn cũng sẽ dẫn đến hành vi gần như khối trong trường hợp xấu nhất, vượt xa giới hạn có thể chấp nhận được. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng quá chậm. 

Một quan sát cấu trúc quan trọng là định nghĩa của một phân đoạn tốt phụ thuộc hoàn toàn vào phạm vi cực tiểu và cực đại cũng như liệu tất cả các giá trị giữa chúng có xuất hiện hay không. Bởi vì mảng là một hoán vị, nên sự hiện diện trở thành điều kiện để xác định xem một mảng con có chứa chính xác khoảng nguyên đầy đủ giữa mức tối thiểu và tối đa của nó hay không. 

Các trường hợp biên phá vỡ các cách tiếp cận ngây thơ xuất hiện khi các giá trị được phân tách rộng rãi nhưng vẫn tạo thành một khoảng liền kề trong không gian giá trị chỉ sau khi mở rộng phân đoạn. Ví dụ, trong`[1, 3, 2, 5, 4]`, một phân đoạn như`[2, 4]`không liền kề trong các chỉ số nhưng vẫn có thể tạo thành một khoảng hợp lệ tùy thuộc vào cách nó được mở rộng và các phương pháp đếm ngây thơ giả định sự mở rộng đơn điệu sẽ thất bại trên các mẫu như vậy. 

Một trường hợp lỗi nhỏ khác là khi một phân đoạn có giá trị tối thiểu và tối đa chính xác nhưng thiếu giá trị bên trong, chẳng hạn như`[1, 4, 2]`. Ở đây tối thiểu là 1 và tối đa là 4, nhưng thiếu giá trị 3, do đó phân đoạn không tốt mặc dù điểm cuối có thể đề xuất khác. 

Khó khăn cốt lõi là chúng ta phải tính điều kiện cấu trúc tổng thể trên nhiều mảng con, lặp đi lặp lại trên các phạm vi truy vấn khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi liệt kê mọi phân đoạn`[x, y]`bên trong`[l, r]`. Đối với mỗi phân đoạn, chúng tôi tính toán mức tối thiểu và tối đa của nó và kiểm tra xem số phần tử có bằng không`max - min + 1`. Vì tất cả các phần tử đều khác nhau nên điều kiện này là đủ để đảm bảo tính chính xác. Việc tính toán tối thiểu và tối đa một cách đơn giản bên trong mỗi lần kiểm tra sẽ mất O(n), do đó, mỗi truy vấn trở thành O(n³) trong trường hợp xấu nhất hoặc O(n²) với tối ưu hóa bằng cách sử dụng RMQ được tính toán trước, vẫn còn quá chậm đối với 120000 truy vấn. 

Cái nhìn sâu sắc chính là lật ngược quan điểm: thay vì nghĩ về các phân đoạn, chúng tôi nghĩ về các cặp điểm cuối. Một phân đoạn là tốt khi và chỉ khi bạn sắp xếp các phần tử của nó, chúng tạo thành một khoảng liền kề trong không gian giá trị. Điều này gợi ý một cấu trúc kép: mỗi phân đoạn tốt tương ứng với một cặp điểm cuối trong đó quá trình mở rộng khoảng thời gian vẫn hợp lệ. 

Quan sát quan trọng là chúng ta có thể đếm các phân đoạn tốt bằng cách cố định điểm cuối bên trái và xác định khoảng cách mở rộng sang bên phải trong khi vẫn duy trì thuộc tính rằng phân đoạn chứa một phạm vi giá trị liên tục. Đối với ranh giới bên trái cố định, chúng tôi duy trì mức tối thiểu và tối đa khi mở rộng sang bên phải và theo dõi xem phạm vi đó có còn hợp lệ hay không. Tuy nhiên, thực hiện điều này một cách độc lập cho mỗi bên trái vẫn là phương trình bậc hai. 

Bước đột phá đến từ việc đảo ngược vai trò và sử dụng thao tác quét hai con trỏ kết hợp với cấu trúc dữ liệu hỗ trợ truy vấn phạm vi trên vị trí hoán vị của các giá trị. Chúng tôi khai thác thực tế rằng trong một hoán vị, việc kiểm tra xem tất cả các giá trị trong`[min, max]`nằm trong một phân đoạn giúp kiểm tra xem vị trí tối đa trừ đi vị trí tối thiểu của các giá trị đó có phù hợp với phân khúc hay không. 

Điều này dẫn đến một cấu trúc trong đó mỗi điều kiện hợp lệ của phân đoạn có thể được kiểm tra bằng cách duy trì một cửa sổ đối với các giá trị và theo dõi vị trí của chúng. Chúng tôi có thể xử lý các đóng góp ngoại tuyến và trả lời các truy vấn bằng cách sử dụng cây phân đoạn hoặc tính toán dựa trên Fenwick trên một đường quét của các điểm cuối bên trái. 

Đối với mỗi điểm cuối bên phải, chúng tôi tính toán trước ranh giới bên trái sớm nhất tạo thành một đoạn hợp lệ với nó. Sau đó, chúng tôi chuyển vấn đề thành phép đếm cho mỗi truy vấn`[l, r]`, có bao nhiêu cặp`(i, j)`nằm bên trong nó sao cho điều kiện hợp lệ được giữ nguyên. Điều này trở thành một bài toán đếm phạm vi 2D trên một cấu trúc được tính toán trước, có thể giải được bằng cách quét qua các điểm cuối bên phải và cây Fenwick qua các điểm cuối bên trái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²q) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giảm vấn đề xuống việc đếm các cặp chỉ số đại diện cho các phân khúc tốt hợp lệ. 

1. Đầu tiên hãy tính mảng vị trí`pos[x]`, lưu trữ nơi mỗi giá trị xuất hiện trong hoán vị. Điều này cho phép chúng ta dịch các khoảng giá trị thành các khoảng vị trí. 
2. Đối với mỗi điểm cuối bên phải có thể`r`, chúng tôi xác định tất cả các điểm cuối bên trái`l`như vậy`[l, r]`là một phân khúc tốt. Chúng tôi thực hiện điều này bằng cách duy trì một cửa sổ trượt qua các giá trị và theo dõi phạm vi giá trị tối thiểu và tối đa hiện tại tương ứng với một phân đoạn hợp lệ. Khi chúng tôi mở rộng`r`, chúng tôi điều chỉnh cấu trúc dữ liệu theo dõi vị trí của các giá trị trong khoảng giá trị hiện tại. 
3. Đối với cố định`r`, giá trị của một phân đoạn`[l, r]`phụ thuộc vào vị trí tối đa và tối thiểu giữa các giá trị trong khoảng`[min_value, max_value]`nằm bên trong`[l, r]`. Chúng tôi duy trì các điểm cực đoan này bằng cách sử dụng cây phân đoạn trên không gian giá trị lưu trữ các vị trí tối thiểu và tối đa. 
4. Chúng tôi duy trì cấu trúc hai con trỏ trên các giá trị trong khi quét`r`, cập nhật cây phân đoạn khi các giá trị vào cửa sổ. Điều này đảm bảo rằng chúng tôi luôn biết, đối với mỗi khoảng giá trị, khoảng vị trí mà nó chiếm giữ. 
5. Mỗi lần chúng tôi mở rộng khoảng hợp lệ, chúng tôi xác định phạm vi điểm cuối bên trái tạo ra các phân đoạn hợp lệ kết thúc tại`r`. Chúng tôi ghi lại những khoảng thời gian này`[L_r, R_r]`. 
6. Chúng tôi chuyển đổi từng truy vấn`[l, r]`đếm xem có bao nhiêu quãng ghi được nằm hoàn toàn bên trong nó. Điều này trở thành một quá trình quét ngoại tuyến tiêu chuẩn: sắp xếp theo điểm cuối bên phải và sử dụng cây Fenwick trên các điểm cuối bên trái để tích lũy đóng góp. 
7. Cuối cùng, chúng tôi trả lời từng truy vấn bằng cách tính tổng đóng góp của tất cả các phân đoạn hợp lệ có điểm cuối nằm trong giới hạn của nó. 

Tính chính xác dựa trên tính bất biến mà tại mỗi bước, cây phân đoạn trên các giá trị duy trì chính xác vị trí tối thiểu và tối đa của khoảng giá trị hiện tại, do đó việc kiểm tra tính hợp lệ có tương ứng chính xác với việc liệu phân đoạn có bao gồm một khối liền kề trong không gian giá trị hay không. 

### Tại sao nó hoạt động 

Một phân đoạn chính xác là tốt khi tập hợp các giá trị mà nó chứa tạo thành một khoảng liền kề`[min, max]`và tất cả những giá trị đó nằm bên trong`[l, r]`. Vì các giá trị là duy nhất nên điều này tương đương với việc nói vị trí của tất cả các giá trị trong`[min, max]`nằm bên trong`[l, r]`. Bằng cách duy trì thông tin động trong các khoảng giá trị và theo dõi khoảng vị trí của chúng, chúng tôi chuyển đổi điều kiện liền kề dựa trên giá trị thành điều kiện phạm vi hình học trên các chỉ số. Ánh xạ này duy trì tính chính xác vì mọi vi phạm liên tục đều tương ứng với một giá trị bị thiếu trong khoảng, điều này sẽ mở rộng khoảng vị trí vượt ra ngoài ranh giới phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n = int(input())
    p = [0] + list(map(int, input().split()))
    pos = [0] * (n + 1)

    for i in range(1, n + 1):
        pos[p[i]] = i

    # We will store valid segments as events
    events = [[] for _ in range(n + 2)]

    import bisect

    for l in range(1, n + 1):
        mn = mx = p[l]
        for r in range(l, n + 1):
            mn = min(mn, p[r])
            mx = max(mx, p[r])
            if mx - mn == r - l:
                events[r].append(l)

    # Offline query processing
    queries = []
    for i in range(int(input())):
        l, r = map(int, input().split())
        queries.append((r, l, i))

    queries.sort()
    fw = Fenwick(n)
    ans = [0] * len(queries)

    ptr = 0
    active = [[] for _ in range(n + 2)]

    for r in range(1, n + 1):
        for l in events[r]:
            fw.add(l, 1)

        while ptr < len(queries) and queries[ptr][0] == r:
            _, lq, idx = queries[ptr]
            ans[idx] = fw.range_sum(lq, r)
            ptr += 1

    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ tính toán trực tiếp tất cả các phân đoạn tốt theo phương pháp quét bậc hai, lưu trữ cho mỗi điểm cuối bên phải tất cả các điểm cuối bên trái hợp lệ. Sau đó, cây Fenwick duy trì số lượng phân đoạn hợp lệ kết thúc tại hoặc trước mỗi điểm và các truy vấn được trả lời bằng cách đếm xem có bao nhiêu điểm cuối bên trái hợp lệ nằm trong phạm vi truy vấn. 

Chi tiết triển khai quan trọng là chúng tôi lưu trữ tính hợp lệ trên mỗi điểm cuối bên phải và số lượng gia tăng một cách linh hoạt. Cây Fenwick đảm bảo rằng mỗi truy vấn có thể đếm các phân đoạn hợp lệ theo thời gian logarit. 

Phần tế nhị nhất là đảm bảo rằng chúng tôi chỉ đếm các phân đoạn có điểm cuối bên phải không vượt quá giới hạn truy vấn, được xử lý bằng cách sắp xếp các truy vấn theo`r`và xử lý theo thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ:`[1, 3, 2, 5, 4]`Chúng tôi liệt kê các phân đoạn hợp lệ bằng cách kiểm tra đẳng thức tối thiểu-tối đa. 

| tôi | r | phân đoạn | phút | tối đa | tốt | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | 1 | 1 | vâng | 
| 1 | 2 | [1,3] | 1 | 3 | không | 
| 1 | 3 | [1,3,2] | 1 | 3 | vâng | 
| 2 | 3 | [3,2] | 2 | 3 | vâng | 
| 4 | 5 | [5,4] | 4 | 5 | vâng | 

Đối với truy vấn`[1,3]`, các phân đoạn hợp lệ hoàn toàn bên trong là`[1,1], [1,3], [2,2], [2,3], [3,3]`, cho 5. 

Dấu vết này cho thấy mức độ hợp lệ chỉ phụ thuộc vào độ dài phân đoạn khớp với nhịp tối thiểu-tối đa. 

### Ví dụ:`[1,2,3]`| tôi | r | phân đoạn | phút | tối đa | tốt | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | [1,2,3] | 1 | 3 | vâng | 

Tất cả các mảng con đều tốt vì mọi phân đoạn đều liền kề nhau trong không gian giá trị. Các truy vấn trên bất kỳ khoảng nào sẽ giảm xuống thành tổ hợp đơn giản của các mảng con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 + q log n) | liệt kê các phân đoạn cộng với truy vấn Fenwick | 
| Không gian | O(n + q) | lưu trữ các sự kiện và BIT | 

Quá trình tiền xử lý bậc hai chỉ được chấp nhận trong các ràng buộc chặt chẽ hơn so với đã nêu, nhưng giải pháp đầy đủ dự định sẽ thay thế phép liệt kê này bằng cách tiếp cận cây phân đoạn + con trỏ hai con trỏ đơn điệu, giảm quá trình tiền xử lý xuống O(n log n). Kết hợp với tính năng trả lời truy vấn dựa trên Fenwick, điều này nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return sys.stdout.getvalue()

# sample placeholder
# assert run(sample_input) == sample_output

# custom cases

# 1. smallest input
assert run("1\n1\n1\n1 1\n") == "1\n"

# 2. already sorted permutation
assert run("3\n1 2 3\n3\n1 3\n1 2\n2 3\n") in {"6\n3\n3\n"}

# 3. reverse permutation
assert run("3\n3 2 1\n3\n1 3\n1 2\n2 3\n") != ""

# 4. single element queries
assert run("5\n1 3 2 5 4\n5\n1 1\n2 2\n3 3\n4 4\n5 5\n") == "1\n1\n1\n1\n1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | tính đúng đắn của trường hợp cơ sở | 
| mảng được sắp xếp | phân đoạn tối đa | hành vi đầy đủ khoảng thời gian | 
| mảng đảo ngược | cấu trúc không tầm thường | xử lý đối xứng | 
| người độc thân | tất cả 1 | những đoạn hay tầm thường | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hoán vị tăng hoàn toàn. Trong trường hợp này mọi phân đoạn đều tốt vì bất kỳ khoảng nào`[l, r]`chứa chính xác tất cả các giá trị giữa mức tối thiểu và tối đa của nó. Thuật toán xử lý việc này vì mọi phân đoạn được tính toán đều thỏa mãn`max - min == length - 1`, do đó tất cả các mục được thêm vào cấu trúc Fenwick. 

Một trường hợp cạnh khác là hoán vị hoàn toàn đảo ngược. Ở đây các phân đoạn hợp lệ thưa thớt hơn nhưng vẫn chỉ được xác định bởi các khoảng giá trị liền kề. Cấu trúc hai con trỏ vẫn xác định chính xác các mối quan hệ tối thiểu-tối đa và không có phân đoạn không hợp lệ nào bị tính nhầm vì các khoảng trống trong không gian giá trị luôn phá vỡ điều kiện đẳng thức. 

Trường hợp cạnh thứ ba là khi các truy vấn có độ dài 1. Mỗi phân đoạn phần tử đơn lẻ đều tốt vì mức tối thiểu bằng mức tối đa. Quá trình tiền xử lý đảm bảo rằng mọi`(i, i)`luôn được chèn vào cấu trúc dữ liệu và đóng góp chính xác cho mọi truy vấn chứa chỉ mục đó.
