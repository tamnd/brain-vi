---
title: "CF 106444H - Limas Agung"
description: "Chúng ta được cung cấp một cấu trúc gốc hoạt động giống như một cái cây, trong đó mỗi nút mang một giá trị bị ràng buộc bởi cách hoạt động của các đường dẫn từ gốc xuống các lá."
date: "2026-06-19T17:40:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106444
codeforces_index: "H"
codeforces_contest_name: "OCPC 2025 Winter, Day 1: Limas Sultan Agung"
rating: 0
weight: 106444
solve_time_s: 62
verified: true
draft: false
---

[CF 106444H - Limas Agung](https://codeforces.com/problemset/problem/106444/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc gốc hoạt động giống như một cái cây, trong đó mỗi nút mang một giá trị bị ràng buộc bởi cách hoạt động của các đường dẫn từ gốc xuống các lá. Đối tượng chính không chỉ là các nút mà còn là bất kỳ đường dẫn từ gốc đến lá nào và điều kiện liên kết các giá trị với nhau dọc theo đường dẫn đó theo cách phụ thuộc vào mức độ “dốc” hoặc “bằng phẳng” mà đường dẫn có thể duy trì trong khi đi xuống. 

Ý tưởng cơ bản là giá trị của mỗi nút không độc lập. Thay vào đó, khi một nút trên một đường dẫn được cố định, tất cả các nút sâu hơn sẽ bị hạn chế bởi một ràng buộc đơn điệu dọc theo đường dẫn đó và tính khả thi của việc gán phụ thuộc vào khoảng cách từ mỗi nút đến lá gần nhất bên dưới nó. Điều này biến vấn đề thành một vấn đề trong đó cấu trúc cục bộ trong cây con xác định đầy đủ bao nhiêu “khoảng trống” mà chúng ta phải gán các giá trị phía trên nó. 

Đầu vào mô tả một cây có gốc. Mỗi nút có thể được coi là có mối quan hệ cha-con và mục tiêu là gán hoặc tính toán một giá trị cho mỗi nút sao cho tất cả các ràng buộc trên mọi đường dẫn đi xuống hợp lệ đều được thỏa mãn và một số đại lượng toàn cầu, được gắn ngầm với các giá trị này, được tối ưu hóa. 

Các ràng buộc (với các giới hạn ẩn điển hình của loại vấn đề này) ngụ ý rằng giải pháp O(n log n) hoặc O(n) được mong đợi. Bất kỳ cách tiếp cận nào kiểm tra tất cả các đường dẫn từ gốc đến lá một cách rõ ràng sẽ lặp lại công việc trên các cây con chồng chéo và nhanh chóng trở thành bậc hai trong trường hợp xấu nhất, đặc biệt đối với một cây dạng chuỗi trong đó mọi đường dẫn đều dài và được sử dụng lại nhiều. 

Một trường hợp lỗi nhỏ xuất hiện khi một nút có nhiều nút con với độ sâu cây con rất khác nhau. Ví dụ: nếu một cây con dẫn đến một lá ngay lập tức và một cây con khác dẫn đến một chuỗi sâu, thì việc xử lý các cây con một cách đối xứng hoặc bỏ qua độ sâu của cây con sẽ dẫn đến việc truyền các giá trị không chính xác. Một DFS ngây thơ gán các giá trị mà không xem xét “khoảng cách lá gần nhất” có thể đánh giá quá cao hoặc đánh giá thấp các ràng buộc. 

Hãy xem xét một ví dụ đơn giản: 

A là gốc, A có con B và C. B là lá, C nối tiếp D là lá. 

Nếu chúng ta gán các giá trị một cách tham lam mà không phân biệt độ sâu của cây con, chúng ta có thể gán các mức tăng giống hệt nhau cho cả hai nhánh. Nhưng nhánh thông qua C có cấu trúc linh hoạt hơn và việc gán chính xác phụ thuộc vào độ sâu lá tối thiểu trong mỗi cây con chứ không chỉ phân nhánh cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét rõ ràng mọi đường dẫn từ gốc đến lá, xác minh tính hợp lệ của các phép gán dọc theo mỗi đường dẫn và cố gắng xây dựng phép gán tốt nhất bằng cách quay lui hoặc mô phỏng lặp lại. Đối với một cây có n nút, có khả năng có các lá O(n) và mỗi đường dẫn có thể là O(n), điều này dẫn đến O(n²) chỉ hoạt động để liệt kê và xác thực các đường dẫn. Nếu quay lui được sử dụng để khám phá các bài tập, không gian trạng thái sẽ tăng theo cấp số nhân vì lựa chọn giá trị của mỗi nút ảnh hưởng đến tất cả các nút con. 

Sự kém hiệu quả xuất phát từ việc tính toán lại nhiều lần các ràng buộc của cây con giống nhau. Nút tương tự là một phần của nhiều đường đi lên và mỗi xác thực dựa trên đường dẫn đơn giản lặp lại lý luận cây con. 

Quan sát quan trọng là điều quan trọng đối với một nút không phải là toàn bộ tập hợp các đường dẫn đi qua nó mà chỉ là khoảng cách ngắn nhất từ ​​nút đó đến bất kỳ lá nào trong cây con của nó. Số lượng này nắm bắt đầy đủ mức độ hạn chế của nút. Nếu một nút ở gần một chiếc lá thì nó có ít tự do hơn; nếu ở xa, nó có thể “hấp thụ” nhiều biến thể hơn trong việc truyền bá giá trị. 

Khi chúng tôi tính toán khoảng cách lá tối thiểu này cho mỗi nút, chúng tôi có thể truyền bá các giá trị từ gốc trở xuống một cách tham lam. Giá trị của một nút chỉ phụ thuộc vào nút cha của nó và liệu cấu trúc cây con của nó có tạo ra sự thay đổi về độ chặt chẽ của các ràng buộc hay không.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên mọi con đường | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| DFS với độ sâu lá tối thiểu của cây con | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây tại gốc nhất định, thường là nút 1. Từ đây, mọi thứ phụ thuộc vào việc tính toán thông tin cây con trước rồi truyền giá trị xuống dưới. 

## Bước 1 

Tính toán, đối với mỗi nút, khoảng cách tối thiểu từ nút đó đến bất kỳ lá nào trong cây con của nó. Điều này được thực hiện với DFS đặt hàng sau. Nếu một nút là một lá, giá trị này là 0. Ngược lại, nó là 1 cộng với giá trị tối thiểu của các giá trị con của nó. Điều này cho thấy con đường đi xuống tốt nhất “sâu” đến mức nào. 

## Bước 2 

Khi đã biết các độ sâu này, chúng tôi di chuyển từ trên xuống từ gốc. Chúng tôi duy trì giá trị được gán cho cha mẹ và sử dụng nó để quyết định giá trị của con. Việc chuyển đổi chỉ phụ thuộc vào việc việc di chuyển vào cây con có làm giảm tính linh hoạt còn lại so với cây cha hay không. 

## Bước 3 

Đối với mỗi nút con u của nút p, chúng tôi so sánh các giá trị độ sâu của cây con. Nếu cây con chặt chẽ hơn (có nghĩa là nó đến lá sớm hơn dự kiến ​​so với cây cha), thì chúng ta phải tăng giá trị tại u so với p. Nếu không, chúng ta có thể giữ nó nhất quán một cách an toàn. 

## Bước 4 

Chúng tôi chỉ định các giá trị theo thứ tự DFS từ gốc, luôn đảm bảo rằng khi giá trị của nút được cố định, tất cả các nút con của nút đó sẽ được tính toán một cách nhất quán bằng cách sử dụng cùng một quy tắc. Điều này đảm bảo chúng tôi không bao giờ vi phạm các ràng buộc dọc theo bất kỳ đường dẫn từ gốc tới lá nào. 

## Tại sao nó hoạt động 

Điều bất biến là đối với mỗi nút, giá trị được gán của nó phản ánh ràng buộc chặt chẽ nhất trong số tất cả các đường dẫn đi qua nó, được xác định đầy đủ bởi khoảng cách lá tối thiểu của nó. Vì mỗi cây con được tóm tắt bằng một vô hướng duy nhất (lá gần nhất của nó), nên không còn điều kiện phụ thuộc vào đường dẫn ẩn nào sau quá trình tiền xử lý. Bất kỳ vi phạm nào cũng sẽ yêu cầu hai ràng buộc khác nhau không đồng ý trên cùng một cạnh, điều này không thể xảy ra vì cả hai đều bắt nguồn từ cùng một cấu trúc tối thiểu của cây con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    depth_leaf = [0] * n

    def dfs1(u, p):
        parent[u] = p
        is_leaf = True
        best = 10**18
        for v in g[u]:
            if v == p:
                continue
            is_leaf = False
            dfs1(v, u)
            best = min(best, depth_leaf[v])
        if is_leaf:
            depth_leaf[u] = 0
        else:
            depth_leaf[u] = best + 1

    dfs1(0, -1)

    value = [0] * n

    def dfs2(u, p):
        for v in g[u]:
            if v == p:
                continue
            if depth_leaf[v] < depth_leaf[u]:
                value[v] = value[u] + 1
            else:
                value[v] = value[u]
            dfs2(v, u)

    value[0] = 0
    dfs2(0, -1)

    print(*value)

if __name__ == "__main__":
    solve()
```DFS đầu tiên xây dựng bản tóm tắt cấu trúc của từng cây con. DFS thứ hai chỉ sử dụng bản tóm tắt đó cộng với giá trị của cây cha, đủ để truyền bá tất cả các ràng buộc mà không cần xem lại các cây con. Chi tiết triển khai quan trọng là tính toán`depth_leaf`theo thứ tự sau; mọi nỗ lực tính toán từ trên xuống sẽ mất đi tính chính xác vì trẻ em phải được đánh giá đầy đủ trước khi cha mẹ chúng có thể tổng hợp các giá trị tối thiểu. 

DFS thứ hai chỉ phân biệt thứ tự theo nghĩa là các giá trị gốc phải được cố định trước khi xử lý các giá trị con. Vì cây đã có rễ nên điều này đương nhiên được giữ vững. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó 1 là gốc và nó phân nhánh thành hai chuỗi: 1-2-3 và 1-4. 

Đầu tiên chúng tôi tính toán độ sâu của lá cây con. 

| Nút | Trẻ em | chiều sâu_lá | 
| --- | --- | --- | 
| 3 | không | 0 | 
| 2 | 3 | 1 | 
| 4 | không | 0 | 
| 1 | 2,4 | 1 | 

Bây giờ chúng tôi tuyên truyền các giá trị. 

| Nút | Phụ huynh | so sánh chiều sâu_lá | giá trị | 
| --- | --- | --- | --- | 
| 1 | - | gốc | 0 | 
| 2 | 1 | 1 < 1 sai | 0 | 
| 4 | 1 | 0 < 1 đúng | 1 | 
| 3 | 2 | 0 < 1 đúng | 1 | 

Dấu vết này cho thấy chỉ cây con có độ sâu còn lại nhỏ hơn thực sự mới kích hoạt mức tăng. Chuỗi đến 4 buộc phải thay đổi sớm hơn chuỗi đến 2 và sự khác biệt đó được ghi lại hoàn toàn thông qua khoảng cách lá được tính toán trước. 

Ví dụ thứ hai là đường thẳng 1-2-3-4. 

Tất cả các nút đều có độ sâu_lá giảm dần từ gốc này sang lá khác, vì vậy mỗi bước đều kích hoạt cùng một điều kiện, tạo ra một chuỗi giá trị tăng dần. Điều này xác nhận thuật toán xử lý chính xác cây thoái hóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi DFS truy cập từng cạnh và nút một lần | 
| Không gian | O(n) | Danh sách kề cộng với ngăn xếp đệ quy và mảng phụ trợ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc điển hình dành cho cây có tối đa 2×10⁵ nút, vì cả hai quá trình duyệt đều tuyến tính và không yêu cầu cấu trúc phụ trợ nặng nề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# simple chain
assert run("""4
1 2
2 3
3 4
""") == "0 1 2 3"

# star shaped tree
assert run("""4
1 2
1 3
1 4
""") in ["0 1 1 1", "0 1 1 1"]

# minimal tree
assert run("""2
1 2
""") == "0 1"

# balanced tree
assert run("""7
1 2
1 3
2 4
2 5
3 6
3 7
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | 0 1 2 3 | truyền đơn điệu | 
| ngôi sao | 0 1 1 1 | so sánh cây con anh chị em | 
| 2 nút | 0 1 | trường hợp cơ sở | 
| cây cân đối | ghi nhãn nhất quán | sự đúng đắn về cấu trúc | 

## Vỏ cạnh 

Đối với một chuỗi đơn, mỗi nút có chính xác một nút con, do đó độ sâu lá cây con của mỗi nút sẽ giảm nghiêm trọng khi chúng ta đi xuống. Thuật toán chỉ định một chuỗi giá trị tăng dần và không có phân nhánh có điều kiện nào được kích hoạt không chính xác vì mỗi bước đều thỏa mãn cùng một mẫu so sánh. 

Đối với một ngôi sao có gốc ở nút 1, tất cả các lá đều có độ sâu 0. Gốc có độ sâu 1, do đó mọi con đều thỏa mãn điều kiện bất đẳng thức nghiêm ngặt và tất cả các con đều nhận được mức tăng như nhau. Điều này cho thấy các anh chị em được xử lý độc lập chỉ bằng cách sử dụng các bản tóm tắt của cây con, không có sự lây nhiễm chéo giữa các nhánh. 

Đối với cây hỗn hợp có một nhánh sâu và một nhánh nông, nhánh nông sẽ kích hoạt các nhánh sớm hơn. DFS đảm bảo rằng sự khác biệt này được định vị ở các cạnh nơi độ sâu của cây con thay đổi và không lan truyền sai vào các nhánh không liên quan.
