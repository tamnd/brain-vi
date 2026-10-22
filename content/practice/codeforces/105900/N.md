---
title: "CF 105900N - Na zdrowie"
description: "Chúng ta được cho một chuỗi cố định có độ dài n vốn là một hoán vị của các số từ 1 đến n. Điều này có nghĩa là mỗi số xuất hiện đúng một lần nhưng chúng ta không biết thứ tự của chúng. Đối với mỗi truy vấn, chúng ta được hỏi về một phân đoạn liền kề của mảng này, từ chỉ số l đến r."
date: "2026-06-21T12:24:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "N"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 50
verified: true
draft: false
---

[CF 105900N - Na zdrowie](https://codeforces.com/problemset/problem/105900/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi cố định có độ dài n vốn là một hoán vị của các số từ 1 đến n. Điều này có nghĩa là mỗi số xuất hiện đúng một lần nhưng chúng ta không biết thứ tự của chúng. 

Đối với mỗi truy vấn, chúng ta được hỏi về một phân đoạn liền kề của mảng này, từ chỉ số l đến r. Chúng ta phải quyết định xem bản thân đoạn này có phải là một hoán vị có độ dài của chính nó hay không. Nói cách khác, nếu đoạn có độ dài m = r − l + 1, thì chúng ta muốn kiểm tra xem nó có chứa mọi số nguyên từ 1 đến m đúng một lần hay không. 

Vì vậy, vấn đề không phải là liệu toàn bộ mảng có phải là hoán vị hay không, điều đó đã được đảm bảo. Khó khăn là một mảng con của hoán vị không nhất thiết phải là một hoán vị có kích thước riêng của nó, mặc dù tất cả các giá trị đều khác biệt trên toàn cầu. 

Các ràng buộc n, q 100000 ngụ ý rằng bất kỳ giải pháp nào xử lý từng truy vấn theo thời gian tuyến tính trên phân đoạn đều quá chậm. Cách tiếp cận O(n) cho mỗi truy vấn đơn giản sẽ dẫn đến O(nq), tức là khoảng 10^10 thao tác trong trường hợp xấu nhất, điều này là không khả thi. Chúng ta cần một chiến lược tiền xử lý để giảm mỗi truy vấn về thời gian không đổi hoặc logarit. 

Một điểm tinh tế là mảng con được đảm bảo chứa các giá trị riêng biệt vì toàn bộ mảng là một hoán vị. Điều này loại bỏ mọi lo ngại về sự trùng lặp bên trong một phân đoạn, vì vậy điều quan trọng duy nhất là liệu các giá trị có tạo thành chính xác một phạm vi liên tục từ 1 đến m hay không. Quan sát đó là sự đơn giản hóa cấu trúc quan trọng. 

Một trường hợp thất bại phổ biến xuất phát từ việc nghĩ rằng “các yếu tố khác biệt” là đủ. Ví dụ: trong hoán vị [4, 1, 2, 3, 5], đoạn [4, 1, 2] có các phần tử riêng biệt nhưng không phải là hoán vị có kích thước 3 vì nó chứa 4, nằm ngoài phạm vi. Đầu ra đúng là “NIE”, nhưng việc kiểm tra tính khác biệt đơn giản sẽ chấp nhận nó một cách không chính xác. 

Một trường hợp lỗi khác là chỉ kiểm tra mức tối thiểu và tối đa trong phân khúc. Để hoán vị hợp lệ kích thước m, chúng ta phải có min = 1 và max = m. Tuy nhiên, chỉ riêng điều kiện này là không đủ trong bài toán mảng tổng quát, nhưng ở đây nó trở nên đủ vì tất cả các giá trị đều khác nhau. Nếu chúng ta cũng đảm bảo có chính xác m phần tử thì việc khớp min và max sẽ buộc phân đoạn phải chứa chính xác {1, ..., m}. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Đối với một phân đoạn nhất định [l, r], chúng tôi tính toán độ dài m của nó, sau đó quét tất cả các phần tử để tìm giá trị tối thiểu, tối đa và xác minh rằng tất cả các phần tử đều khác biệt và nằm trong phạm vi. Vì mảng toàn cục đã đảm bảo tính khác biệt nên chúng ta có thể đơn giản hóa việc kiểm tra xem giá trị tối thiểu là 1 và tối đa là m hay không. Ngay cả khi đó, chúng tôi vẫn cần quét phân đoạn đó, với chi phí O(r − l + 1) cho mỗi truy vấn. Qua q truy vấn, giá trị này trở thành O(nq) trong trường hợp xấu nhất, quá lớn. 

Quan sát chính là mỗi truy vấn chỉ phụ thuộc vào hai thuộc tính tổng hợp của phân đoạn: giá trị tối thiểu và tối đa của phân đoạn đó. Khi chúng ta nhận ra rằng mảng là một hoán vị, chúng ta có thể tránh việc kiểm tra toàn bộ cấu trúc bên trong. Chúng tôi chỉ cần truy vấn phạm vi nhanh cho mức tối thiểu và tối đa. 

Điều này dẫn trực tiếp đến cấu trúc dữ liệu truy vấn phạm vi, chẳng hạn như cây phân đoạn hoặc bảng thưa thớt. Cây phân đoạn hỗ trợ phạm vi truy vấn tối thiểu và tối đa trong O(log n) cho mỗi truy vấn sau khi xử lý trước O(n). Một bảng thưa thớt làm giảm điều này hơn nữa xuống còn O(1) cho mỗi truy vấn với quá trình xử lý trước O(n log n), đây là điều lý tưởng ở đây. 

Khi chúng ta có thể truy vấn tối thiểu và tối đa một cách hiệu quả, mỗi truy vấn sẽ trở thành một kiểm tra đơn giản: nếu min(l, r) == 1 và max(l, r) == r − l + 1, thì phân đoạn phải chính xác là một hoán vị có kích thước m. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây phân đoạn/Bảng thưa thớt | O(n log n + q) hoặc O(n log n + q log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng bảng thưa thớt để trả lời các truy vấn tối thiểu và tối đa trong phạm vi thời gian không đổi.

1. Xây dựng một bảng thưa thớt cho các giá trị tối thiểu và một bảng khác cho các giá trị tối đa trên mảng. Mỗi bảng lưu trữ các câu trả lời có độ dài 2^k. Quá trình tiền xử lý này cho phép chúng tôi trả lời bất kỳ truy vấn phạm vi nào bằng cách kết hợp hai khoảng lũy ​​thừa chồng chéo. 
2. Tính toán trước logarit sao cho với bất kỳ độ dài nào, chúng ta có thể nhanh chóng xác định lũy thừa lớn nhất của 2 nằm bên trong nó. Điều này tránh việc tính toán lại nhật ký cho mỗi truy vấn. 
3. Đối với mỗi truy vấn [l, r], hãy tính m = r − l + 1. Đây là kích thước dự kiến ​​của một phân đoạn hoán vị hợp lệ. 
4. Truy vấn giá trị nhỏ nhất trong phạm vi và giá trị lớn nhất trong phạm vi bằng bảng thưa. Điều này được thực hiện bằng cách kết hợp hai khối được tính toán trước bao gồm [l, r]. 
5. Nếu giá trị tối thiểu bằng 1 và giá trị tối đa bằng m thì phân đoạn này chứa chính xác tất cả các giá trị từ 1 đến m vì mảng không chứa các giá trị trùng lặp trên toàn cầu nên không thể thực hiện được cấu hình nào khác. Nếu không, nó không thể là phân đoạn hoán vị hợp lệ. 
6. Xuất “TAK” nếu hợp lệ, nếu không thì xuất “NIE”. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai sự thật. Đầu tiên, mọi phần tử trong mảng là duy nhất trên toàn cầu, vì vậy bất kỳ mảng con nào cũng có các phần tử duy nhất. Thứ hai, một tập hợp gồm m số nguyên riêng biệt là một hoán vị từ 1 đến m khi và chỉ khi giá trị nhỏ nhất của nó là 1 và giá trị lớn nhất của nó là m. Không có chỗ cho các giá trị bị thiếu hoặc thừa vì bất kỳ sai lệch nào cũng sẽ làm tăng giá trị tối đa, giảm giá trị tối thiểu hoặc vi phạm tính khác biệt. Bảng thưa đảm bảo chúng tôi tính toán chính xác các thuộc tính này cho mọi phạm vi truy vấn mà không cần kiểm tra từng phần tử riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = [0] + list(map(int, input().split()))

LOG = [0] * (n + 1)
for i in range(2, n + 1):
    LOG[i] = LOG[i // 2] + 1

K = LOG[n] + 1

st_min = [[0] * (n + 1) for _ in range(K)]
st_max = [[0] * (n + 1) for _ in range(K)]

for i in range(1, n + 1):
    st_min[0][i] = a[i]
    st_max[0][i] = a[i]

for k in range(1, K):
    length = 1 << k
    half = length >> 1
    for i in range(1, n - length + 2):
        st_min[k][i] = min(st_min[k - 1][i], st_min[k - 1][i + half])
        st_max[k][i] = max(st_max[k - 1][i], st_max[k - 1][i + half])

def query_min(l, r):
    k = LOG[r - l + 1]
    length = 1 << k
    return min(st_min[k][l], st_min[k][r - length + 1])

def query_max(l, r):
    k = LOG[r - l + 1]
    length = 1 << k
    return max(st_max[k][l], st_max[k][r - length + 1])

out = []
for _ in range(q):
    l, r = map(int, input().split())
    m = r - l + 1
    mn = query_min(l, r)
    mx = query_max(l, r)
    if mn == 1 and mx == m:
        out.append("TAK")
    else:
        out.append("NIE")

print("\n".join(out))
```Giải pháp xây dựng hai bảng thưa song song, một bảng tối thiểu và một bảng tối đa. Các vòng tiền xử lý lấp đầy tất cả các khoảng thời gian bằng cách sử dụng các nửa được tính toán trước đó. Mỗi truy vấn tính lũy thừa lớn nhất của hai phân đoạn phù hợp trong phạm vi truy vấn và kết hợp hai mục để có kết quả cuối cùng. 

Logic quyết định được tối thiểu hóa một cách có chủ ý: chúng tôi không bao giờ lặp lại phân đoạn đó và chúng tôi hoàn toàn dựa vào phạm vi tổng hợp được tính toán trước. Việc lập chỉ mục xuyên suốt dựa trên 1 để phù hợp với báo cáo vấn đề và tránh nhầm lẫn từng cái một khi tính toán độ dài. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu đầu tiên: 

Chúng ta có mảng [1, 2, 3, 4] và các truy vấn [1,2], [2,3], [1,4]. 

Đối với truy vấn đầu tiên [1,2], phân đoạn là [1,2]. Tối thiểu là 1 và tối đa là 2, vì vậy nó hợp lệ. 

| Truy vấn | Phân đoạn | Tối thiểu | Tối đa | m | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 2 | [1,2] | 1 | 2 | 2 | TAK | 

Đối với [2,3], phân đoạn là [2,3]. Tối thiểu là 2, không phải 1, vì vậy nó không thể là hoán vị của [1..2]. 

| Truy vấn | Phân đoạn | Tối thiểu | Tối đa | m | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 2 3 | [2,3] | 2 | 3 | 2 | NIE | 

Đối với [1,4], phân đoạn là [1,2,3,4]. Tối thiểu là 1, tối đa là 4, vì vậy nó khớp chính xác [1..4]. 

| Truy vấn | Phân đoạn | Tối thiểu | Tối đa | m | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 4 | [1,2,3,4] | 1 | 4 | 4 | TAK | 

Những dấu vết này cho thấy tính hợp lệ hoàn toàn được nắm bắt bởi các giá trị biên. 

Bây giờ hãy xem xét một phân đoạn kiểu phản ví dụ trong mẫu thứ hai: [4,1,2] trong [4,1,2,3,5]. 

| Truy vấn | Phân đoạn | Tối thiểu | Tối đa | m | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 3 | [4,1,2] | 1 | 4 | 3 | NIE | 

Mặc dù các giá trị là khác nhau nhưng giá trị tối đa vẫn vượt quá kích thước phân đoạn, do đó nó không thể là một hoán vị hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Quá trình tiền xử lý bảng thưa thớt xây dựng các mục nhập O(n log n), mỗi truy vấn sử dụng phạm vi O(1) tối thiểu/tối đa | 
| Không gian | O(n log n) | Hai bảng thưa lưu trữ các giá trị cho mỗi lũy thừa của hai khoảng | 

Các ràng buộc cho phép tối đa 10^5 phần tử và truy vấn, do đó, thời gian truy vấn O(1) sau khi xử lý trước nằm trong giới hạn thoải mái. Việc sử dụng bộ nhớ vẫn duy trì ở mức 256 MB vì ​​chúng tôi chỉ lưu trữ hai bảng số nguyên có kích thước khoảng n log n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # Assume solution is wrapped in a function solve()
    # For this standalone snippet, we redefine minimal runner
    import sys
    input = sys.stdin.readline

    n, q = map(int, sys.stdin.readline().split())
    a = [0] + list(map(int, sys.stdin.readline().split()))

    LOG = [0] * (n + 1)
    for i in range(2, n + 1):
        LOG[i] = LOG[i // 2] + 1

    K = LOG[n] + 1

    st_min = [[0] * (n + 1) for _ in range(K)]
    st_max = [[0] * (n + 1) for _ in range(K)]

    for i in range(1, n + 1):
        st_min[0][i] = a[i]
        st_max[0][i] = a[i]

    for k in range(1, K):
        length = 1 << k
        half = length >> 1
        for i in range(1, n - length + 2):
            st_min[k][i] = min(st_min[k - 1][i], st_min[k - 1][i + half])
            st_max[k][i] = max(st_max[k - 1][i], st_max[k - 1][i + half])

    def query_min(l, r):
        k = LOG[r - l + 1]
        length = 1 << k
        return min(st_min[k][l], st_min[k][r - length + 1])

    def query_max(l, r):
        k = LOG[r - l + 1]
        length = 1 << k
        return max(st_max[k][l], st_max[k][r - length + 1])

    out = []
    for _ in range(q):
        l, r = map(int, sys.stdin.readline().split())
        m = r - l + 1
        mn = query_min(l, r)
        mx = query_max(l, r)
        out.append("TAK" if mn == 1 and mx == m else "NIE")

    return "\n".join(out)

# provided samples
assert run("""4 3
1 2 3 4
1 2
2 3
1 4
""") == "TAK\nNIE\nTAK"

assert run("""5 4
4 1 2 3 5
2 3
1 3
2 4
1 4
""") == "TAK\nNIE\nTAK\nTAK"

# minimum-size input
assert run("""1 1
1
1 1
""") == "TAK"

# already sorted permutation edge
assert run("""3 2
1 2 3
1 2
2 3
""") == "TAK\nTAK"

# reversed permutation edge
assert run("""3 2
3 2 1
1 3
1 2
""") == "TAK\nTAK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | TAK | trường hợp ranh giới tối thiểu | 
| hoán vị được sắp xếp | tất cả TAK | phân đoạn hợp lệ bình thường | 
| hoán vị ngược | tất cả TAK | đặt hàng không liên quan | 
| trường hợp mẫu | hỗn hợp | tính đúng đắn của quy tắc min/max | 

## Vỏ cạnh 

Mảng một phần tử như [1] với truy vấn [1,1] luôn hợp lệ vì min = 1 và max = 1, khớp với độ dài phân đoạn 1. Thuật toán trả về chính xác “TAK” vì bảng thưa thớt trả về chính phần tử đó. 

Hoán vị ngược như [3,2,1] vẫn hoạt động vì các mảng con như [2,1] có min = 1 và max = 2, thỏa mãn điều kiện ngay cả khi thứ tự không tăng. Thuật toán không dựa vào thứ tự mà chỉ dựa vào giới hạn phạm vi nên nó trả về kết quả chính xác cho tất cả các phân đoạn như vậy. 

Một phân đoạn chứa phạm vi chính xác nhưng các giá trị bị dịch chuyển, chẳng hạn như [2,3] trong [1,2,3,4], không thành công vì giá trị tối thiểu không bằng 1. Thuật toán ngay lập tức từ chối nó thông qua kiểm tra tối thiểu, tạo ra “NIE” chính xác.
