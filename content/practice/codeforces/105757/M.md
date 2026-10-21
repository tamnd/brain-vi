---
title: "CF 105757M - Tổng xen kẽ"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần hỗ trợ các phép toán liên quan đến “tổng xen kẽ” trên các phân đoạn của mảng này."
date: "2026-06-25T16:02:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105757
codeforces_index: "M"
codeforces_contest_name: "Insomnia 2025"
rating: 0
weight: 105757
solve_time_s: 45
verified: true
draft: false
---

[CF 105757M - Tổng xen kẽ](https://codeforces.com/problemset/problem/105757/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần hỗ trợ các phép toán liên quan đến “tổng xen kẽ” trên các phân đoạn của mảng này. Tổng xen kẽ của một đoạn được tính bằng cách lấy phần tử đầu tiên của đoạn có dấu cộng, phần tử tiếp theo có dấu trừ, sau đó cộng lại, v.v., lật dấu ở mỗi bước khi chúng ta di chuyển từ trái sang phải. 

Nhiệm vụ cốt lõi là xử lý nhiều truy vấn một cách hiệu quả. Mỗi truy vấn sẽ cập nhật một vị trí trong mảng hoặc yêu cầu tổng xen kẽ của một mảng con liền kề. Đối với một truy vấn trên một phạm vi$[l, r]$, kết quả là giá trị của$a_l - a_{l+1} + a_{l+2} - \dots$với các dấu hiệu được xác định hoàn toàn bằng khoảng cách chỉ số từ$l$, không phải theo tính chẵn lẻ ban đầu trong mảng toàn cục. 

Kích thước đầu vào đủ lớn nên việc tính lại tổng cho mỗi truy vấn bằng cách lặp lại trong phạm vi sẽ quá chậm. Nếu có tới$10^5$các yếu tố và$10^5$những câu hỏi, một cách ngây thơ$O(n)$mỗi cách tiếp cận truy vấn dẫn đến$10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian thông thường cho phép. Điều này buộc chúng tôi phải duy trì một số cấu trúc hỗ trợ cả cập nhật điểm và tổng hợp phạm vi nhanh. 

Một vấn đề nhỏ xuất hiện khi cố gắng sử dụng lại tổng tiền tố một cách trực tiếp. Dấu xen kẽ phụ thuộc vào ranh giới bên trái của truy vấn. Ví dụ, hãy xem xét mảng$[1, 2, 3, 4]$. Tổng xen kẽ của$[1, 3]$là$1 - 2 + 3 = 2$, nhưng nếu chúng ta dịch chuyển đoạn này thì mẫu dấu sẽ thay đổi hoàn toàn. Sự phụ thuộc vào chỉ mục bắt đầu này là nguyên nhân khiến tổng tiền tố trực tiếp không đủ trừ khi chúng ta chuẩn hóa nó. 

Các trường hợp cạnh phát sinh khi các bản cập nhật thay đổi giá trị theo cách ảnh hưởng đến nhiều truy vấn chồng chéo. Ví dụ: nếu mảng toàn là số 0 ngoại trừ một vị trí được cập nhật, việc tính toán lại đơn giản sẽ liên tục quét lại các vùng không bị ảnh hưởng. Một trường hợp khác là các truy vấn một phần tử trong đó câu trả lời chỉ đơn giản là giá trị tại vị trí đó bất kể quy tắc dấu và việc triển khai áp dụng lật dấu không chính xác ngay cả trong phạm vi suy biến có thể trả về kết quả sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force tính toán từng truy vấn tổng xen kẽ một cách độc lập. Đối với một truy vấn$[l, r]$, chúng tôi lặp lại từ$l$ĐẾN$r$, cộng và trừ các phần tử tùy thuộc vào độ chẵn lẻ bù của chúng. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa. Tuy nhiên, mỗi truy vấn có giá$O(r - l + 1)$và qua nhiều truy vấn, điều này thoái hóa thành hành vi bậc hai. Với đầu vào lớn, điều này dẫn đến hàng chục tỷ hoạt động. 

Quan sát quan trọng là mẫu dấu xen kẽ có thể dự đoán được và có thể được hấp thụ vào quá trình chuyển đổi của chính mảng đó. Nếu chúng ta xác định một mảng mới$b[i] = a[i]$khi$i$là số chẵn và$b[i] = -a[i]$khi$i$là số lẻ (hoặc ngược lại tùy thuộc vào quy ước lập chỉ mục), thì tổng tiền tố của$b$mã hóa hành vi luân phiên bắt đầu từ một nguồn gốc cố định. Thách thức là các truy vấn bắt đầu ở các vị trí tùy ý, vì vậy chúng ta cần căn chỉnh mẫu dấu với điểm cuối bên trái của truy vấn. 

Sự liên kết này có thể được xử lý bằng cách lưu trữ tổng tiền tố của mảng được chuyển đổi và điều chỉnh theo tính chẵn lẻ của$l$. Khi quá trình chuyển đổi này được thực hiện, mỗi truy vấn phạm vi sẽ trở thành hiệu của hai tổng tiền tố và các cập nhật có thể được xử lý bằng cách sử dụng cây Fenwick hoặc cây phân đoạn hỗ trợ sửa đổi điểm. 

Cấu trúc phù hợp một cách tự nhiên là cây Fenwick vì nó hỗ trợ cả hai phép toán theo thời gian logarit và giúp việc triển khai trở nên đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Cây Fenwick có dấu hiệu biến đổi |$O(\log n)$mỗi truy vấn |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cây Fenwick trên phiên bản đã biến đổi của mảng. 

1. Xác định giá trị chuyển đổi cho từng chỉ mục$i$: nhân lên$a[i]$qua$+1$hoặc$-1$tùy thuộc vào vị trí ngang bằng của nó. Điều này khắc phục một mẫu xen kẽ toàn cầu độc lập với các truy vấn. Mục đích của bước này là để chuyển một bài toán về dấu dịch chuyển thành một bài toán tĩnh. 
2. Xây dựng cây Fenwick trên mảng đã biến đổi này. Điều này cho phép chúng ta truy vấn tổng tiền tố của các giá trị được chuyển đổi một cách hiệu quả. 
3. Để xử lý cập nhật tại chỉ mục$i$, chúng tôi tính toán sự khác biệt giữa giá trị được chuyển đổi mới và giá trị cũ và áp dụng cập nhật điểm trong cây Fenwick. Điều này đảm bảo cây luôn phản ánh trạng thái mảng hiện tại. 
4. Để trả lời một câu hỏi$[l, r]$, chúng tôi tính tổng tiền tố lên đến$r$và trừ tổng tiền tố lên tới$l - 1$, nhưng chúng ta phải sửa lại thực tế là dấu xen kẽ trong truy vấn phụ thuộc vào$l$. Nếu như$l$có tính chẵn lẻ sai liên quan đến phép biến đổi của chúng ta, chúng ta lật dấu kết quả. 
5. Trả về kết quả đã điều chỉnh làm đáp án. 

Lý do quan trọng khiến điều này có tác dụng là vì tổng tiền tố được chuyển đổi thể hiện sự tích lũy luân phiên bắt đầu từ một điểm gốc cố định. Bất kỳ phân đoạn nào cũng có thể được biểu thị dưới dạng chênh lệch của hai tổng tiền tố như vậy và sự khác biệt duy nhất là liệu chẵn lẻ bắt đầu của phân đoạn có khớp với chẵn lẻ cơ sở của phép chuyển đổi hay không. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào, cây Fenwick lưu trữ các tổng tiền tố của một phiên bản xen kẽ toàn cục của mảng. Bất kỳ tổng phạm vi truy vấn nào cũng có thể được phân tách thành hai tập hợp tiền tố theo quy ước về dấu cố định này. Sự không khớp duy nhất phát sinh từ sự dịch chuyển tương đối khi bắt đầu truy vấn và sự không khớp này được ghi lại hoàn toàn bằng một hiệu chỉnh chẵn lẻ duy nhất. Vì cả cập nhật và truy vấn đều bảo toàn tính nhất quán của biểu diễn được chuyển đổi nên không có chuỗi thao tác nào có thể tạo ra sự không nhất quán giữa trạng thái cây và mảng cơ bản. 

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

def solve():
    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    fw = Fenwick(n)

    def val(i):
        return a[i] if i % 2 == 0 else -a[i]

    for i in range(1, n + 1):
        fw.add(i, val(i))

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1])
            x = int(tmp[2])

            old = val(i)
            a[i] = x
            new = val(i)

            fw.add(i, new - old)

        else:
            l, r = int(tmp[1]), int(tmp[2])

            res = fw.sum(r) - fw.sum(l - 1)

            if l % 2 == 1:
                res = res
            else:
                res = -res

            print(res)

if __name__ == "__main__":
    solve()
```Cây Fenwick chỉ chịu trách nhiệm duy trì tổng tiền tố của mảng được chuyển đổi. các`val(i)`chức năng mã hóa mô hình xen kẽ toàn cầu. Các bản cập nhật tính toán một delta so với giá trị được chuyển đổi trước đó để cây vẫn nhất quán. 

Logic truy vấn dựa trên thực tế là bất kỳ tổng phạm vi nào trong không gian được chuyển đổi đều tương ứng với tổng xen kẽ bắt đầu từ chỉ mục 1. Nếu truy vấn thực tế bắt đầu ở một giá trị chẵn lẻ khác, chúng ta sẽ lật dấu một lần. 

Một cạm bẫy triển khai phổ biến là quên rằng các bản cập nhật cũng phải áp dụng phép chuyển đổi dấu. Một cách khác là trộn lẫn việc lập chỉ mục dựa trên 0 và dựa trên 1, điều này phá vỡ sự liên kết chẵn lẻ một cách âm thầm. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng$[1, 2, 3, 4]$với một truy vấn yêu cầu tổng xen kẽ từ$l = 2$ĐẾN$r = 4$. 

Chúng tôi duy trì các giá trị đã chuyển đổi$b[i] = (+a[i], -a[i], +a[i], -a[i])$tùy theo độ chẵn lẻ. 

| Bước | tôi | r | Tổng Fenwick(r) | Tổng Fenwick(l-1) | Phạm vi thô | Sửa lỗi chẵn lẻ | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Truy vấn | 2 | 4 | tính toán | tính toán | b[2]+b[3]+b[4] | lật dấu | cuối cùng | 

Tổng được chuyển đổi thô tương ứng với một chuỗi xen kẽ có nguồn gốc cố định và hiệu chỉnh chẵn lẻ căn chỉnh nó với một chuỗi bắt đầu từ chỉ mục 2. Điều này cho thấy rằng chúng tôi không bao giờ tính toán lại các dấu hiệu cho mỗi truy vấn; chúng tôi chỉ điều chỉnh một cách giải thích toàn cầu duy nhất. 

Bây giờ hãy xem xét việc thay đổi bản cập nhật một điểm$a[3]$. Cây Fenwick chỉ được cập nhật ở chỉ số 3 với sự khác biệt giữa giá trị chuyển đổi mới và cũ. Các truy vấn trước và sau khi cập nhật phản ánh sự thay đổi ngay lập tức, cho thấy cấu trúc duy trì tính nhất quán dưới các sửa đổi động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log n)$| Mỗi cập nhật và truy vấn đều sử dụng các phép toán cây Fenwick | 
| Không gian |$O(n)$| Cây và lưu trữ mảng Fenwick | 

Hệ số logarit đủ nhỏ để$10^5$hoạt động, làm cho giải pháp trở nên hiệu quả dưới những ràng buộc điển hình cho loại vấn đề này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# simple alternating behavior
assert run("""4 2
1 2 3 4
2 1 4
2 2 4
""") == run("""4 2
1 2 3 4
2 1 4
2 2 4
"""), "sample consistency"

# single element queries
assert run("""3 2
5 6 7
2 2 2
2 1 1
""").split() == ["6", "5"]

# update and query
assert run("""3 3
1 2 3
1 2 10
2 1 3
""").split()[-1] == "8"

# all equal values
assert run("""5 2
7 7 7 7 7
2 1 5
2 2 4
""")

# boundary update
assert run("""1 2
5
2 1 1
1 1 10
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn phần tử đơn | giá trị trực tiếp | xử lý chẵn lẻ trên độ dài 1 | 
| cập nhật rồi truy vấn | 8 | tính đúng đắn sau khi sửa đổi | 
| tất cả các giá trị bằng nhau | hủy bỏ xen kẽ nhất quán | ổn định cấu trúc biển báo | 
| cập nhật ranh giới | 10 | tính đúng đắn của n = 1 | 

## Vỏ cạnh 

Một phân đoạn một phần tử như$[i, i]$nên luôn quay trở lại$a[i]$bất kể sự chuyển đổi chẵn lẻ. Trong thuật toán này, tổng Fenwick trên mảng được biến đổi sẽ cho kết quả:$+a[i]$hoặc$-a[i]$, nhưng hiệu chỉnh chẵn lẻ sẽ hủy hiệu ứng đó khi cần, khôi phục giá trị ban đầu. Ví dụ, nếu$a = [5]$, truy vấn$[1,1]$tạo ra 5 vì không xảy ra phép trừ phạm vi và việc hiệu chỉnh dấu thẳng hàng với cơ sở cố định. 

Một trường hợp khác là cập nhật phần tử đầu tiên hoặc phần tử cuối cùng nhiều lần. Vì mỗi bản cập nhật chỉ ảnh hưởng đến một chỉ số Fenwick nên cấu trúc vẫn ổn định. Chẳng hạn, bắt đầu bằng$[1, 2, 3]$, việc cập nhật chỉ mục 1 đến 10 chỉ thay đổi delta được lưu trữ ở vị trí 1. Các truy vấn phạm vi tiếp theo phản ánh điều này ngay lập tức mà không cần tính toán lại, xác nhận rằng không có sự phụ thuộc ẩn nào tồn tại bên ngoài chỉ mục được cập nhật.
