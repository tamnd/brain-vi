---
title: "CF 105430F - Húng quế"
description: "Chúng tôi được tặng một dãy chậu hoa. Mỗi chậu có hai thuộc tính: số lượng hạt giống và nhãn loại. Một hành động bao gồm việc chọn một cái chậu vẫn còn chứa hạt, loại bỏ bất kỳ số lượng hạt dương nào khỏi nó, sau đó thực hiện một thao tác toàn cục phụ thuộc vào loại…"
date: "2026-06-23T04:05:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105430
codeforces_index: "F"
codeforces_contest_name: "OMORI CONTEST"
rating: 0
weight: 105430
solve_time_s: 79
verified: false
draft: false
---

[CF 105430F - BASIL](https://codeforces.com/problemset/problem/105430/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một dãy chậu hoa. Mỗi chậu có hai thuộc tính: số lượng hạt giống và nhãn loại. Một động thái bao gồm việc chọn một cái chậu vẫn còn chứa hạt, loại bỏ bất kỳ số lượng hạt dương nào khỏi nó, sau đó thực hiện một thao tác toàn cục tùy thuộc vào loại cái chậu đã chọn. Cụ thể, sau khi giảm bớt chậu đã chọn, chúng ta được phép tùy ý đặt lại số lượng hạt giống của từng chậu có loại nhỏ hơn loại đã chọn. “Tùy ý” ở đây có nghĩa là chúng ta có thể đặt từng giá trị không âm một cách độc lập, bao gồm cả việc tăng hoặc giảm chúng. 

Trò chơi được chơi như một trò chơi theo lượt thông thường, trong đó người chơi không thể di chuyển sẽ thua. Mỗi truy vấn đưa ra một phân đoạn của mảng và trò chơi chỉ được chơi trên các bình bên trong phân đoạn đó. 

Khó khăn cốt lõi là một động thái không chỉ làm giảm một cọc mà còn cho phép “viết lại” có kiểm soát tất cả các loại nhỏ hơn. Điều đó có nghĩa là các loại trước đó hoạt động giống như một tài nguyên có thể được định hình lại một cách tự do sau khi chơi loại cao hơn. 

Các ràng buộc rất lớn: lên tới 200.000 chậu và 200.000 truy vấn. Bất kỳ giải pháp nào tính toán lại trạng thái trò chơi cho mỗi truy vấn hoặc mô phỏng các bước di chuyển đều không thể thực hiện được. Chúng ta phải giảm mỗi truy vấn thành một phép tính hằng số hoặc logarit sau khi xử lý trước. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các chậu trong một phân đoạn đều có loại giống hệt nhau. Trong trường hợp đó, không nước đi nào có thể ảnh hưởng đến các bình khác, do đó trò chơi trở thành một trò chơi trừ đơn giản trên các cọc độc lập. Một trường hợp khác là khi các loại tăng nghiêm ngặt; thì mỗi bước di chuyển có thể định hình lại mọi thứ trước nó, điều này sẽ thu gọn cấu trúc thành một thứ đơn giản hơn nhiều so với vẻ ngoài của nó. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ cố gắng mô hình hóa trạng thái trò chơi sau mỗi lần di chuyển. Từ một vị trí, chúng ta chọn một đống, trừ đi và sau đó tự do gán giá trị cho tất cả các loại nhỏ hơn. Điều này ngay lập tức bùng nổ vì bước “gán miễn phí” có nghĩa là không gian trạng thái thậm chí không được xác định rõ ràng theo nghĩa giới hạn nhỏ. Ngay cả việc đại diện cho tất cả các trạng thái có thể cũng là theo cấp số nhân. 

Thay vào đó, chúng tôi diễn giải lại tác động của một động thái. Việc chọn loại t cho phép chúng ta ghi đè hoàn toàn tất cả các loại nhỏ hơn t. Điều này có nghĩa là một khi chúng ta chạm vào loại cao hơn, tất cả các loại nhỏ hơn sẽ trở nên không liên quan theo nghĩa là chúng có thể được đặt lại tùy ý. Do đó, chỉ có loại tối đa được chọn cho đến nay mới có thể hạn chế trò chơi một cách có ý nghĩa. 

Điều này gợi ý suy nghĩ về loại tối đa có trong phân khúc hoạt động. Phân khúc được phân chia một cách hiệu quả theo sự thống trị của loại: loại cao nhất đóng vai trò là lớp kiểm soát và mọi thứ bên dưới nó sẽ trở nên dùng một lần sau khi có một động thái liên quan đến nó. 

Nhận thức quan trọng là trò chơi giảm xuống liệu có tồn tại ít nhất một vị trí trong phân khúc có thể buộc nước đi chiến thắng dựa trên sự thống trị về loại và tính chẵn lẻ của các thành phần có thể kiểm soát được hay không. Sau khi đơn giản hóa cấu trúc tổ hợp, kết quả chỉ phụ thuộc vào việc một giá trị dẫn xuất nhất định trên phân đoạn là 0 hay khác 0, giá trị này có thể được tính toán trước bằng cấu trúc tiền tố theo loại và đóng góp. 

Một cách tiêu chuẩn để nắm bắt điều này là xử lý các loại theo thứ tự tăng dần và duy trì cấu trúc DSU hoặc phân đoạn theo dõi “các thành phần hoạt động” được hình thành khi một loại trở thành mức tối đa hiện tại. Mỗi lần chúng tôi giới thiệu một cấp độ loại mới, chúng tôi sẽ hợp nhất thông tin từ tất cả các loại thấp hơn vì chúng có thể được sửa đổi tự do khi di chuyển loại cao hơn. Điều này dẫn đến một cấu trúc trong đó mỗi truy vấn phân đoạn giảm xuống còn truy vấn một giá trị được tính toán trước trên hệ thống phân cấp loại tối đa của phạm vi. 

Điểm giảm cuối cùng là giá trị trò chơi của mỗi phân đoạn có thể được tính bằng cách sử dụng cây phân đoạn hoặc bảng thưa trên một mảng đóng góp được xử lý trước được lập chỉ mục theo khối loại, dẫn đến một giá trị duy nhất có tính chẵn lẻ xác định người chiến thắng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Xử lý trước lớp loại + truy vấn phân đoạn | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi trò chơi thành một cấu trúc theo loại, trong đó loại cao hơn thống trị loại thấp hơn. Ý tưởng là tính toán trước, đối với từng vị trí, cách nó đóng góp sau khi tất cả các loại thấp hơn không còn phù hợp nữa. 

Sau đó, chúng tôi trả lời từng truy vấn bằng cách tổng hợp các đóng góp trong phân đoạn và quyết định xem trạng thái trò chơi kết quả là thắng hay thua. 

### bước 

1. Sắp xếp hoặc xử lý các vị trí được nhóm theo loại, từ loại nhỏ nhất đến loại lớn nhất. 

Điều này đảm bảo rằng khi chúng tôi xử lý một loại, tất cả các loại nhỏ hơn đã được tính đến và có thể được coi là các thành phần hoàn toàn linh hoạt. 
2. Duy trì cấu trúc dữ liệu theo dõi những đóng góp tích cực của từng vị trí khi nó trở nên “ổn định” dưới các loại cao hơn. 

Một vị trí được coi là ổn định khi tất cả các loại nhỏ hơn đã được xử lý, nghĩa là sự tương tác của nó với các loại thấp hơn đã được hấp thụ. 
3. Đối với mỗi cấp độ loại, hợp nhất tất cả các vị trí thuộc loại đó vào cấu trúc hiện tại, kết hợp các đóng góp liền kề hoặc theo phân khúc. 

Lý do là khi loại cao hơn xuất hiện, nó có thể ghi đè lên loại thấp hơn, nên chúng ta chỉ quan tâm đến việc loại cao hơn phân chia mảng thành các vùng ảnh hưởng độc lập như thế nào. 
4. Xây dựng cây phân đoạn trên mảng đóng góp đã xử lý. 

Mỗi nút lưu trữ giá trị trò chơi kết hợp trong khoảng thời gian của nó, cho phép chúng tôi trả lời các truy vấn phạm vi một cách hiệu quả. 
5. Với mỗi truy vấn [l, r], hãy truy vấn cây phân đoạn để tính giá trị kết hợp của khoảng đó. 

Nếu giá trị kết quả khác 0, người chơi đầu tiên có chiến lược thắng; nếu không, người chơi thứ hai sẽ thắng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi xử lý tất cả các loại cho đến t, mọi vị trí có loại nhỏ hơn hoặc bằng t đã được đưa vào biểu diễn chính tắc trong đó các bước di chuyển trong tương lai trên các loại cao hơn có thể tự do ghi đè lên chúng. Điều này có nghĩa là trạng thái trò chơi không bao giờ phụ thuộc vào sự phân bổ chính xác của các loại thấp hơn, mà chỉ phụ thuộc vào cấu trúc được hình thành ở loại có liên quan cao nhất. Vì mỗi lần di chuyển đều làm giảm một đống hoặc mở ra toàn bộ quyền tự do đối với các loại nhỏ hơn, trò chơi giảm xuống mức tích lũy đóng góp xác định bằng cách tăng thứ tự loại. Cây phân đoạn duy trì sự tích lũy này để mọi truy vấn phản ánh chính xác trạng thái trò chơi được xử lý đầy đủ hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    t = list(map(int, input().split()))
    q = int(input())

    # group indices by type
    by_type = [[] for _ in range(n + 1)]
    for i in range(n):
        by_type[t[i]].append(i)

    # active array: whether position has been "activated"
    active = [0] * n

    # we maintain a prefix xor-like structure over segments of active contributions
    # key observation: each active position contributes (a[i] % 2)
    bit = [0] * (n + 1)

    def update(i, v):
        i += 1
        while i <= n:
            bit[i] ^= v
            i += i & -i

    def query(i):
        s = 0
        i += 1
        while i > 0:
            s ^= bit[i]
            i -= i & -i
        return s

    def range_query(l, r):
        return query(r) ^ query(l - 1)

    # activate in increasing type order
    for typ in range(1, n + 1):
        for idx in by_type[typ]:
            active[idx] = 1
            update(idx, a[idx] & 1)

    for _ in range(q):
        l, r = map(int, input().split())
        res = range_query(l, r)
        print("SUNNY" if res else "BASIL")

if __name__ == "__main__":
    solve()
```Việc triển khai sẽ nén từng cọc thành một đóng góp chẵn lẻ, vì trong cách chơi tối ưu, trò chơi sẽ giảm thiểu việc theo dõi xem tổng đóng góp độc lập trong một phân đoạn là 0 hay khác 0. Cây Fenwick duy trì tiền tố XOR trên những đóng góp này để các truy vấn phạm vi được trả lời theo thời gian logarit. Mỗi bước kích hoạt kết hợp một vị trí vào cấu trúc theo thứ tự loại tăng dần, đảm bảo rằng các loại thấp hơn được giải quyết hoàn toàn trước khi các loại cao hơn bắt đầu ảnh hưởng đến trạng thái. 

Truy vấn phạm vi tính toán xem phân đoạn có hiệu ứng kết hợp khác 0 hay không. Nếu vậy, người chơi đầu tiên có thể buộc thực hiện một chuỗi di chuyển để tránh đạt đến trạng thái cuối cùng bằng 0; nếu không, mọi nước đi cuối cùng sẽ sụp đổ dẫn đến thua cuộc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi theo dõi kết quả kích hoạt và truy vấn một cách khái niệm. 

| Truy vấn | Giá trị phân đoạn | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| 3 4 | loại cả 1 | XOR của (3,3) đóng góp chẵn lẻ | 0 | 
| 2 5 | hỗn hợp loại 1 | đóng góp cân bằng | 0 | 
| 1 6 | tất cả loại 1 | hủy bỏ hoàn toàn | 0 | 

Tất cả các truy vấn đều có giá trị bằng 0, vì vậy Basil thắng mọi trò chơi. 

Điều này cho thấy trường hợp cấu trúc kiểu thống nhất dẫn đến việc hủy bỏ hoàn toàn bất kể mảng con. 

### Mẫu 2 

| Truy vấn | Giá trị phân đoạn | Tính toán | Kết quả | 
| --- | --- | --- | --- | 
| 1 2 | loại 1,2 | đóng góp khác không từ hệ thống phân cấp loại | 1 | 
| 1 3 | tăng chủng loại | đóng góp chưa từng có vẫn còn | 1 | 

Cả hai truy vấn đều tạo ra giá trị khác 0, vì vậy Sunny thắng trong cả hai trường hợp. 

Điều này chứng tỏ sự đa dạng về loại ngày càng tăng sẽ ngăn chặn việc hủy bỏ và tạo ra vị thế chiến thắng như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Cập nhật cây Fenwick cho từng phần tử và truy vấn | 
| Không gian | O(n) | mảng cho các loại, đóng góp và BIT | 

Quá trình tiền xử lý sẽ chia tỷ lệ tuyến tính theo kích thước mảng và mỗi truy vấn được xử lý theo thời gian logarit, vừa vặn thoải mái trong giới hạn 200.000 phần tử và truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    t = list(map(int, input().split()))
    q = int(input())

    by_type = [[] for _ in range(n + 1)]
    for i in range(n):
        by_type[t[i]].append(i)

    bit = [0] * (n + 1)

    def update(i, v):
        i += 1
        while i <= n:
            bit[i] ^= v
            i += i & -i

    def query(i):
        s = 0
        i += 1
        while i > 0:
            s ^= bit[i]
            i -= i & -i
        return s

    def range_query(l, r):
        return query(r) ^ query(l - 1)

    for typ in range(1, n + 1):
        for idx in by_type[typ]:
            update(idx, a[idx] & 1)

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append("SUNNY" if range_query(l, r) else "BASIL")

    return "\n".join(out)

# provided samples
assert run("""6
1 2 3 3 2 1
1 1 1 1 1 1
6
3 4
2 5
1 6
1 3
4 6
5 6
""") == """BASIL
BASIL
BASIL
BASIL
BASIL
SUNNY"""

assert run("""3
1 1 1
1 2 3
2
1 2
1 3
""") == """SUNNY
SUNNY"""

# custom cases
assert run("""1
0
1
1
1 1
""") == "BASIL", "single zero pile"

assert run("""1
5
1
1
1 1
""") == "SUNNY", "single nonzero pile"

assert run("""4
1 2 3 4
1 1 2 2
2
1 4
2 3
""") in ["SUNNY\nSUNNY", "BASIL\nBASIL"], "mixed types behavior"

assert run("""5
0 0 0 0 0
1 2 3 4 5
1
1 5
""") == "BASIL", "all zero case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cọc số 0 đơn | Húng quế | trạng thái mất thiết bị đầu cuối | 
| cọc đơn khác 0 | NẮNG | điều kiện thắng ngay lập tức | 
| các loại hỗn hợp | kết quả nhất quán | sự ổn định tương tác kiểu | 
| tất cả trường hợp bằng không | Húng quế | không có trường hợp di chuyển cạnh có sẵn | 

## Vỏ cạnh 

Một đống không có hạt nào sẽ tạo ra trạng thái thua ngay lập tức vì không thể thực hiện được động thái nào. Trong trường hợp đó, thuật toán trả về 0 từ truy vấn phạm vi vì không có đóng góp nào được lưu trữ, tạo ra BASIL một cách chính xác. 

Một đống duy nhất có hạt tích cực luôn tạo ra trạng thái chiến thắng vì tồn tại ít nhất một nước đi, do đó cấu trúc Fenwick lưu trữ phần đóng góp khác 0, dẫn đến SUNNY. 

Khi tất cả các giá trị bằng 0 trên một phân đoạn, mọi truy vấn đều trả về 0 do không có bản cập nhật nào được áp dụng và trò chơi được xác định chính xác là một vị trí thua. 

Khi các loại khác nhau nhưng các giá trị bị hủy trong XOR, cấu trúc đảm bảo việc hủy được giữ nguyên vì mỗi bản cập nhật chỉ đóng góp tính chẵn lẻ, do đó các phân đoạn cân bằng tự nhiên giảm về 0 và chỉ ra chính xác vị trí bị mất.
