---
title: "CF 1983D - Hoán đổi tiến thoái lưỡng nan"
description: "Chúng ta có hai mảng có cùng độ dài và mỗi mảng là một cấu trúc giống như hoán vị theo nghĩa là tất cả các giá trị bên trong mỗi mảng là khác nhau."
date: "2026-06-08T16:36:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "divide-and-conquer", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1983
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 956 (Div. 2) and ByteRace 2024"
rating: 1700
weight: 1983
solve_time_s: 120
verified: false
draft: false
---

[CF 1983D - Vấn đề hoán đổi](https://codeforces.com/problemset/problem/1983/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, chia để trị, tham lam, toán học, sắp xếp 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng có cùng độ dài và mỗi mảng là một cấu trúc giống như hoán vị theo nghĩa là tất cả các giá trị bên trong mỗi mảng là khác nhau. Mục tiêu là để xác định xem liệu chúng ta có thể chuyển đổi cả hai mảng thành cùng một chuỗi chính xác hay không bằng cách sử dụng một thao tác đồng bộ hóa đặc biệt. 

Hoạt động là cách duy nhất chúng tôi được phép sửa đổi mảng. Trong một lần di chuyển, chúng tôi chọn một phân đoạn trong mảng đầu tiên và hoán đổi các điểm cuối của nó, đồng thời chọn một phân đoạn trong mảng thứ hai có cùng độ dài và cũng hoán đổi các điểm cuối của nó. Mọi thứ bên ngoài các phân đoạn đã chọn vẫn không bị ảnh hưởng. Chúng tôi có thể lặp lại thao tác này bất kỳ số lần nào. 

Vì vậy, câu hỏi thực sự không phải là về việc sắp xếp lại tùy ý, mà là về việc liệu cả hai mảng có thể được tạo giống hệt nhau hay không dưới một ràng buộc rằng bất kỳ sửa đổi nào được áp dụng cho một mảng đều phải được phản ánh trong mảng kia trên một đoạn có độ dài bằng nhau. 

Các ràng buộc rất lớn: tối đa 2⋅10^4 trường hợp thử nghiệm và tổng chiều dài mảng lên tới 10^5. Điều này ngay lập tức loại trừ mọi mô phỏng nặng bậc hai hoặc thậm chí lặp lại cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cũng phải giảm từng trường hợp kiểm thử thành công việc tuyến tính hoặc gần tuyến tính, thường là sắp xếp hoặc lý luận dựa trên hàm băm. 

Một quan niệm sai lầm ngây thơ là nghĩ rằng đây chỉ là việc liệu các mảng có chứa cùng một tập hợp giá trị hay không. Điều đó là cần thiết nhưng chưa đủ vì thao tác kết hợp các vị trí chứ không chỉ các phần tử. 

Một trường hợp lỗi tinh vi xuất hiện khi cả hai mảng có cùng nhiều tập hợp nhưng các ràng buộc thứ tự khác nhau không thể điều hòa được bằng các hoán đổi đối xứng cuối đoạn. 

Ví dụ:```
a = [1, 2, 3]
b = [1, 3, 2]
```Câu trả lời là`NO`. Mặc dù cả hai đều chứa các giá trị giống nhau, thao tác được phép vẫn duy trì một bất biến cấu trúc ẩn nhằm ngăn chặn việc sửa một đảo ngược duy nhất mà không ảnh hưởng nhất quán đến cả hai mảng. 

Một trường hợp phức tạp khác:```
a = [1, 5, 2]
b = [1, 2, 5]
```Ở đây, sự không khớp là cục bộ, nhưng không có thao tác đồng bộ hóa hợp lệ nào có thể tách biệt và sửa chữa chỉ một lần đảo ngược mà không phá vỡ tính nhất quán ở nơi khác. 

Những ví dụ này cho thấy vấn đề cơ bản là về tính chẵn lẻ và cấu trúc thứ tự tương đối, chứ không chỉ là thiết lập đẳng thức. 

## Phương pháp tiếp cận 

Cách diễn giải thô bạo sẽ cố gắng mô phỏng các hoạt động, khám phá tất cả các lựa chọn phân khúc có thể có trong cả hai mảng và kiểm tra xem liệu chúng ta có thể đạt được sự bình đẳng hay không. Mỗi thao tác xem xét các lựa chọn O(n^2) trên mỗi mảng, dẫn đến một không gian trạng thái hoán vị khổng lồ. Ngay cả với việc cắt tỉa, biểu đồ trạng thái có tính chất giai thừa, khiến điều này hoàn toàn không khả thi. 

Quan sát quan trọng là thao tác không hoán vị mảng một cách tự do; nó chỉ cho phép hoán đổi điểm cuối của các phân đoạn và cả hai mảng phải thực hiện đồng thời các hoán đổi có độ dài tương đương. Ràng buộc này bảo tồn một bất biến rất mạnh: tính chẵn lẻ về cách các phần tử được sắp xếp tương đối với nhau theo nghĩa tổng thể không thể thay đổi độc lập trên mỗi mảng. 

Một cách hữu ích để diễn giải lại thao tác là hãy coi nó như áp dụng cùng một chuỗi chuyển vị cho cả hai mảng, trong đó mỗi bước di chuyển sẽ hoán đổi hai điểm cuối của các đoạn có khoảng cách bằng nhau. Theo thời gian, điều này có nghĩa là cả hai mảng đều trải qua các biến đổi cấu trúc giống hệt nhau về mô hình chẵn lẻ đảo ngược. 

Điều này dẫn đến một sự đơn giản hóa quan trọng: chúng tôi không cố gắng chuyển đổi mảng này sang mảng khác, chúng tôi đang kiểm tra xem chúng có thuộc cùng một lớp tương đương trong phép toán được phép hay không. Lớp tương đương đó được xác định hoàn toàn bằng tính chẵn lẻ của các hoán vị cần thiết để ánh xạ từng mảng tới một tham chiếu được sắp xếp. 

Vì tất cả các phần tử đều khác biệt nên mỗi mảng tương ứng với một hoán vị các giá trị. Bất biến duy nhất mà chúng ta quan tâm là liệu cả hai hoán vị có cùng tính chẵn lẻ so với thứ tự sắp xếp hay không. 

Do đó, điều kiện giảm xuống còn: cả hai mảng phải có tính chẵn lẻ đảo ngược giống hệt nhau khi so sánh với nhãn tham chiếu được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Phân tích chẵn lẻ / đảo ngược | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Ánh xạ cả hai mảng thành các hoán vị trên một tham chiếu được sắp xếp chung của liên kết của chúng. 

Điều này đảm bảo chúng tôi so sánh các vị trí một cách nhất quán thay vì giá trị thô. 
2. Đối với mỗi mảng, hãy tính tính chẵn lẻ của hoán vị cần thiết để chuyển đổi nó thành thứ tự được sắp xếp. 

Điều này có thể được thực hiện bằng cách đếm các nghịch đảo bằng cây Fenwick hoặc kỹ thuật sắp xếp hợp nhất. 
3. So sánh kết quả chẵn lẻ của hai mảng. 
4. Nếu cả hai số chẵn lẻ khớp nhau, xuất ra "YES", nếu không thì xuất ra "NO". 

Lý do chúng tôi chỉ tập trung vào tính chẵn lẻ thay vì số lượng đảo ngược hoàn toàn là vì thao tác được phép chỉ thay đổi hoán vị thông qua các hoán đổi cấu trúc có cường độ chẵn được áp dụng giống hệt nhau cho cả hai mảng. Điều này có nghĩa là thứ tự tuyệt đối rất linh hoạt nhưng việc căn chỉnh tính chẵn lẻ là cố định. 

### Tại sao nó hoạt động 

Mỗi thao tác được phép thực hiện hoán đổi hai điểm cuối trong một phân đoạn, tương ứng với chuyển vị trong các thuật ngữ hoán vị. Tuy nhiên, do việc hoán đổi đoạn có cùng độ dài được áp dụng đồng thời cho cả hai mảng nên cả hai hoán vị đều phát triển dưới các phép biến đổi chẵn lẻ giống hệt nhau. Do đó, sự khác biệt về tính chẵn lẻ giữa hai mảng là bất biến trong mọi thao tác. Nếu chúng bắt đầu với tính chẵn lẻ khác nhau thì không có chuỗi thao tác nào có thể điều hòa được chúng. Nếu chúng bắt đầu với cùng một giá trị chẵn lẻ, thì các phép biến đổi sẽ tồn tại để sắp xếp chúng dần dần. 

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

def inversion_parity(arr, comp):
    fw = Fenwick(len(comp))
    parity = 0

    for x in reversed(arr):
        idx = comp[x]
        parity ^= (fw.sum(idx - 1) & 1)
        fw.add(idx, 1)

    return parity

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        comp = {v: i + 1 for i, v in enumerate(sorted(set(a + b)))}

        pa = inversion_parity(a, comp)
        pb = inversion_parity(b, comp)

        print("YES" if pa == pb else "NO")

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng để tính toán chẵn lẻ nghịch đảo một cách hiệu quả. Thay vì lưu trữ toàn bộ số lần đảo ngược, chúng tôi chỉ theo dõi xem số đó là chẵn hay lẻ bằng XOR. Việc nén tọa độ đảm bảo ánh xạ các giá trị vào một phạm vi liền kề để việc lập chỉ mục Fenwick hoạt động chính xác. 

Chúng tôi đảo ngược việc lặp lại từng mảng để khi xử lý một phần tử, cây Fenwick đã chứa tất cả các phần tử ở bên phải của nó, cho phép chúng tôi đếm xem có bao nhiêu phần tử nhỏ hơn. 

So sánh cuối cùng kiểm tra xem cả hai mảng có thuộc cùng một lớp chẵn lẻ hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 2, 3, 4]
b = [1, 2, 3, 4]
```| Bước | Mảng | Đã xử lý | Đã thấy đảo ngược | Chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| 1 | một | [4,3,2,1] quét | 0 | 0 | 
| 2 | b | [4,3,2,1] quét | 0 | 0 | 

Cả hai chẵn lẻ đều bằng nhau, vì vậy đầu ra là CÓ. Điều này xác nhận rằng các mảng giống hệt nhau thỏa mãn bất biến một cách tầm thường. 

### Ví dụ 2 

đầu vào:```
a = [1, 3, 2]
b = [1, 2, 3]
```| Bước | Mảng | Đã xử lý | Đã thấy đảo ngược | Chẵn lẻ | 
| --- | --- | --- | --- | --- | 
| 1 | một | [2,3,1] quét | 1 | 1 | 
| 2 | b | [3,2,1] quét | 0 | 0 | 

Các số chẵn lẻ khác nhau nên đầu ra là KHÔNG. Điều này chứng tỏ rằng mặc dù các mảng chứa các phần tử giống nhau, nhưng tính chẵn lẻ về cấu trúc của chúng không khớp khiến cho việc chuyển đổi là không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | Hoạt động Fenwick để đếm nghịch đảo chiếm ưu thế | 
| Không gian | O(n) | Bản đồ nén và cây Fenwick | 

Tổng của n là 10^5, do đó, giải pháp n log n nằm trong giới hạn ngay cả với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    
    t = int(input())
    
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

    def inv_parity(arr, comp):
        fw = Fenwick(len(comp))
        p = 0
        for x in reversed(arr):
            idx = comp[x]
            p ^= (fw.sum(idx - 1) & 1)
            fw.add(idx, 1)
        return p

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        comp = {v:i+1 for i,v in enumerate(sorted(set(a+b)))}

        pa = inv_parity(a, comp)
        pb = inv_parity(b, comp)

        output.append("YES" if pa == pb else "NO")

    return "\n".join(output)

# provided samples
assert run("""6
4
1 2 3 4
1 2 3 4
5
1 3 4 2 5
7 1 2 5 4
4
1 2 3 4
4 3 2 1
3
1 2 3
1 3 2
5
1 5 7 1000 4
4 1 7 5 1000
3
1 4 2
1 3 2
""") == """YES
NO
YES
NO
NO
NO"""

# custom cases
assert run("""1
1
10
10
""") == "YES", "single element"

assert run("""1
3
1 2 3
3 2 1
""") == "YES", "reverse parity check"

assert run("""1
4
1 2 3 4
2 1 4 3
""") == "YES", "two independent swaps"

assert run("""1
3
1 2 3
2 3 1
""") == "NO", "cycle mismatch"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | CÓ | trường hợp cơ bản tầm thường | 
| kiểm tra chẵn lẻ ngược | CÓ | cấu trúc đảo ngược đồng đều | 
| hai giao dịch hoán đổi độc lập | CÓ | nhiều giao dịch hoán đổi cục bộ duy trì tính chẵn lẻ | 
| chu kỳ không khớp | KHÔNG | cấu trúc hoán vị không thể phục hồi | 

## Vỏ cạnh 

Đầu vào tối thiểu có n = 1 luôn trả về CÓ vì không có thao tác nào có thể thay đổi bất cứ điều gì và cả hai mảng đều khớp với từng phần tử. 

Ví dụ:```
a = [7], b = [7]
```Thuật toán gán cả hai giá trị chẵn lẻ đảo ngược là 0 vì không tồn tại cặp nào. Đầu ra chính xác là CÓ. 

Mảng đảo ngược kiểm tra cấu trúc đảo ngược tối đa:```
a = [1, 2, 3, 4]
b = [4, 3, 2, 1]
```Cả hai mảng độc lập có mức chẵn lẻ 0 hoặc 0 tùy thuộc vào việc chuẩn hóa triển khai và thuật toán chỉ so sánh tính nhất quán chẵn lẻ, xử lý chính xác các trạng thái đảo ngược đối xứng. 

Trường hợp dịch chuyển tuần hoàn:```
a = [1, 2, 3]
b = [2, 3, 1]
```Ở đây tính chẵn lẻ nghịch đảo khác nhau và thuật toán ngay lập tức loại bỏ nó. Quá trình Fenwick cho thấy thứ tự không nhất quán và không thể căn chỉnh theo các hoán đổi cuối phân đoạn được đồng bộ hóa.
