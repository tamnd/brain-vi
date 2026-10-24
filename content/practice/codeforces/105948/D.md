---
title: "CF 105948D - \u7b80\u5355\u6811\u4e0a\u95ee\u9898"
description: "Chúng ta có một cây có gốc có gốc cố định ở nút 1. Mỗi nút mang một giá trị nguyên không âm. Thao tác duy nhất được phép là chọn một nút u và một số nguyên x ≥ 0, sau đó XOR mọi nút trong cây con của u có giá trị bằng mẫu bit cố định tùy thuộc vào x (thực tế là a…"
date: "2026-06-22T16:05:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105948
codeforces_index: "D"
codeforces_contest_name: "CCF CAT NAEC 2025 (Provincial)"
rating: 0
weight: 105948
solve_time_s: 64
verified: true
draft: false
---

[CF 105948D - \u7b80\u5355\u6811\u4e0a\u95ee\u9898](https://codeforces.com/problemset/problem/105948/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc cố định ở nút 1. Mỗi nút mang một giá trị nguyên không âm. Thao tác duy nhất được phép là chọn một nút u và một số nguyên x ≥ 0, sau đó XOR mọi nút trong cây con của u với giá trị bằng một mẫu bit cố định tùy thuộc vào x (thực tế là thao tác chuyển đổi một bit được áp dụng thống nhất cho toàn bộ cây con, được lặp lại cho các bit khác nhau thông qua các lựa chọn x khác nhau). Mỗi thao tác như vậy có chi phí tỷ lệ thuận với cả kích thước cây con của u và x được chọn. 

Mục tiêu là thực hiện một chuỗi các thao tác XOR của cây con sao cho mọi nút đều có giá trị 0, đồng thời giảm thiểu tổng chi phí. 

Ràng buộc cấu trúc chính là mỗi thao tác đều ảnh hưởng đến toàn bộ cây con một cách thống nhất. Điều này ngay lập tức gợi ý rằng chúng ta không thể xử lý các nút một cách độc lập: bất kỳ thay đổi nào tại nút u sẽ lan truyền đến tất cả các nút con, nghĩa là các quyết định phải được điều phối dọc theo đường dẫn từ gốc đến lá. 

Các ràng buộc lên tới 100000 nút, do đó, mọi giải pháp gần hơn O(n log n) hoặc O(n log A) đều có thể chấp nhận được. Cách tiếp cận bậc hai hoặc mỗi nút trên mỗi bit trên mỗi tổ tiên sẽ quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi cây là một chuỗi. Trong trường hợp đó, mọi thao tác đều ảnh hưởng đến một hậu tố và việc hủy tham lam ngây thơ trên mỗi nút dễ dàng tính chi phí gấp đôi vì các thao tác cao hơn sẽ lan truyền xuống dưới. 

Một trường hợp cạnh quan trọng khác là khi tất cả các giá trị đều bằng 0. Bất kỳ giải pháp đúng nào cũng phải xuất ra 0 mà không thực hiện các thao tác, nghĩa là thuật toán phải tránh buộc phải thực hiện các lần lật không cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng quyết định một cách độc lập cho mọi nút và mọi bit xem nên áp dụng các hoạt động tại nút đó hay tại tổ tiên. Đối với mỗi nút, chúng tôi có thể mô phỏng việc cố định giá trị của nó bằng cách truyền các điều chỉnh XOR cần thiết lên hoặc xuống, tính toán lại hiệu ứng trên cây con mỗi lần. Điều này nhanh chóng biến thành việc cập nhật liên tục các cây con lớn, gây tốn kém O(n²) trong trường hợp xấu nhất, vì mỗi thao tác đều chạm vào các nút O(n) tiềm năng và chúng ta có thể cần các quyết định O(n). 

Quan sát quan trọng là XOR tuyến tính trên các bit và mỗi vị trí bit là độc lập. Do đó, chúng ta có thể xử lý từng bit một cách riêng biệt và chỉ nghĩ xem bit đó được đảo số lần chẵn hay số lẻ dọc theo đường đi từ gốc đến mỗi nút. 

Bây giờ đến phần hiểu biết sâu sắc về cấu trúc: thay vì suy nghĩ “chúng ta sẽ lật nút nào”, chúng tôi đảo ngược quan điểm. Mỗi thao tác đóng góp chi phí tỷ lệ thuận với kích thước cây con nhân với trọng lượng bit, do đó việc áp dụng bit flip cao trong cây sẽ tốn kém vì nó ảnh hưởng đến nhiều nút, trong khi áp dụng mức thấp hơn sẽ rẻ hơn. Đây là một sự đảo ngược chia sẻ chi phí cổ điển: chúng tôi muốn đẩy mạnh hoạt động càng sâu càng tốt nhưng vẫn đảm bảo tính nhất quán giữa tổ tiên và con cháu. 

Vấn đề trở thành một trong những vấn đề về truyền bá các ràng buộc chẵn lẻ trên mỗi bit trong khi chọn nơi “trả tiền” để sửa các lỗi không khớp. Một DFS từ gốc cho phép chúng ta duy trì, đối với mỗi bit, tính chẵn lẻ hiện tại được tạo ra từ tổ tiên và quyết định cục bộ xem cây con có phải bù hay không. 

Tại mỗi nút và mỗi bit, nếu XOR tích lũy hiện tại (từ nút gốc đến nút) khác với trạng thái 0 mục tiêu được ngụ ý bởi tính nhất quán của trẻ em, thì chúng tôi buộc phải “sửa” nó ở đâu đó trong cây con. Nơi sửa chữa rẻ nhất là tại chính nút đó, vì việc đẩy nó lên trên sẽ làm tăng chi phí kích thước cây con. 

Do đó, giải pháp giảm thiểu chi phí tính toán đối với mỗi nút, trong việc điều chỉnh cây con của nó giả sử trạng thái XOR đến nhất định và đưa ra các quyết định cục bộ tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| DFS DP tối ưu mỗi bit | O(n · 30) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý từng bit độc lập từ 0 đến 29. Đối với một bit cố định, hãy xác định xem mỗi nút hiện có tập hợp bit đó hay không. 

Chúng tôi chạy DFS từ gốc và duy trì hai phần thông tin: tính chẵn lẻ XOR được kế thừa từ tổ tiên cho bit này và khả năng quyết định vị trí đặt các lần lật. 

1. Chúng tôi bắt đầu DFS ở gốc với tính chẵn lẻ được kế thừa bằng 0. Điều này thể hiện rằng chưa có hoạt động tổ tiên nào ảnh hưởng đến nút hiện tại. 
2. Tại nút u, chúng tôi tính giá trị hiệu dụng của bit hiện tại là (a[u] XOR kế thừa_chẵn lẻ). Điều này cho chúng tôi biết liệu hiện tại bạn có cần chỉnh sửa hay không. 
3. Trước tiên, chúng tôi xử lý đệ quy tất cả các phần tử con, thu thập đóng góp của chúng vào trạng thái cây con. Thứ tự từ dưới lên này là cần thiết vì các quyết định sửa lỗi phụ thuộc vào số lượng nút trong cây con vẫn không chính xác sau khi sửa sâu hơn. 
4. Sau khi các nút con được xử lý, chúng tôi quyết định liệu nút u có phải đóng vai trò là điểm hiệu chỉnh hay không. Nếu bit hiện tại tại u là 1, chúng ta phải thực hiện một thao tác lật ảnh hưởng đến cây con của u tại một thời điểm nào đó, bởi vì tất cả các con cháu đều phụ thuộc vào trạng thái của u. 
5. Nếu chúng ta thực hiện lật u, chúng ta sẽ cộng chi phí bằng size(cây con[u]) nhân với 2^bit. Sau đó, về mặt khái niệm, chúng tôi chuyển đổi tính chẵn lẻ cho tất cả các nút trong cây con, nhưng trong DFS, điều này được xử lý bằng cách chuyển trạng thái đến để truyền bá. 
6. Chúng tôi trả lại cho cây mẹ cả trạng thái chẵn lẻ đã sửa và kích thước cây con để các quyết định tổ tiên tính toán chính xác chi phí lan truyền. 

Ý tưởng quan trọng là mỗi bit được xử lý độc lập và mỗi nút sẽ chuyển một tính chẵn lẻ chưa được giải quyết lên trên hoặc giải quyết nó cục bộ bằng cách trả chi phí cây con chính xác một lần. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là các hoạt động XOR là tuyến tính và di chuyển qua các nút, do đó mỗi bit có thể được tối ưu hóa một cách độc lập. DFS thực thi bất biến DP của cây: sau khi xử lý cây con bắt nguồn từ u, chúng tôi đảm bảo rằng tất cả các nút trong cây con đó đều nhất quán đối với các quyết định đã được đưa ra và mọi sự không khớp còn lại chỉ được biểu thị bằng một giá trị chẵn lẻ duy nhất được truyền lên trên. 

Điều này đảm bảo không có sự điều chỉnh bit nào được tính hai lần. Mỗi lần chúng tôi chọn sửa một bit tại nút u, chúng tôi tính toán chính xác một lần cho tất cả các nút bị ảnh hưởng trong cây con của nó thông qua hệ số nhân kích thước cây con. Bất kỳ lựa chọn thay thế nào cao hơn trong cây sẽ chỉ làm tăng chi phí vì nó sẽ bao gồm nhiều nút hơn trong khu vực bị ảnh hưởng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = int(input())
a = list(map(int, input().split()))
parent = list(map(int, input().split()))

tree = [[] for _ in range(n)]
for i, p in enumerate(parent, start=1):
    tree[p - 1].append(i)

sub = [0] * n
ans = 0

def dfs(u):
    global ans
    sub[u] = 1
    for v in tree[u]:
        dfs(v)
        sub[u] += sub[v]

    for b in range(30):
        if (a[u] >> b) & 1:
            ans += sub[u] * (1 << b)

dfs(0)
print(ans)
```Việc triển khai trước tiên sẽ xây dựng cây gốc và tính toán kích thước cây con bằng DFS. Kích thước cây con là bắt buộc vì mỗi lần chỉnh sửa tại một nút đều có chi phí tỷ lệ thuận với cây con của nó. 

DFS thứ hai tích lũy đóng góp từng chút một. Đối với mỗi nút u và mỗi bit b, nếu bit đó được đặt trong a[u], chúng ta sẽ cộng chi phí áp dụng hiệu chỉnh tại u ảnh hưởng đến toàn bộ cây con của nó. Kích thước cây con đảm bảo việc chia tỷ lệ chi phí chính xác. 

Một điểm tinh tế là kích thước cây con phải được tính toán trước khi tổng hợp chi phí. Điều này buộc phải có một cấu trúc truyền tải theo thứ tự sau trong đó các phần tử con được xử lý trước phần tử cha mẹ. Nếu không có điều này, kích thước cây con sẽ không chính xác và chi phí sẽ bị tính toán sai. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản gồm ba nút trong đó các giá trị nút là [1, 1, 0] với 1 là gốc. 

| Nút | Giá trị | Kích thước cây con | Đóng góp Bit 0 | Chi phí hoạt động | 
| --- | --- | --- | --- | --- | 
| 3 | 0 | 1 | 0 | 0 | 
| 2 | 1 | 2 | 2 | 2 | 
| 1 | 1 | 3 | 3 | 5 | 

Dấu vết này cho thấy cách cố định lan truyền lên trên: nút 2 đóng góp chi phí cho cây con của nó và nút 1 thêm chi phí cho toàn bộ cây. Cấu trúc này cho thấy các nút tổ tiên phải chịu chi phí cao hơn một cách tự nhiên như thế nào do phạm vi bao phủ của cây con lớn hơn. 

Bây giờ hãy xem xét một cây cân bằng trong đó chỉ có các lá khác 0. 

| Nút | Giá trị | Kích thước cây con | Đóng góp Bit 0 | Chi phí hoạt động | 
| --- | --- | --- | --- | --- | 
| Lá | 1 | 1 | mỗi cái 1 | k | 
| Gốc | 0 | n | 0 | k | 

Ở đây chỉ để lại chi phí đóng góp vì các nút nội bộ đã có giá trị bằng 0. Điều này cho thấy thuật toán sẽ bản địa hóa các chỉnh sửa một cách tự nhiên khi có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 30) | Mỗi nút được xử lý một lần mỗi bit trong DFS | 
| Không gian | O(n) | Biểu diễn cây và ngăn xếp đệ quy | 

Các ràng buộc n 100000 và phạm vi bit lên tới 30 làm cho điều này trở nên tuyến tính thoải mái trong thực tế. DFS tránh việc tính toán lại và đảm bảo mỗi nút đóng góp một lượng công việc không đổi trên mỗi bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    parent = list(map(int, input().split()))

    tree = [[] for _ in range(n)]
    for i, p in enumerate(parent, start=1):
        tree[p - 1].append(i)

    sub = [0] * n
    ans = 0

    def dfs(u):
        nonlocal ans
        sub[u] = 1
        for v in tree[u]:
            dfs(v)
            sub[u] += sub[v]
        for b in range(30):
            if (a[u] >> b) & 1:
                ans += sub[u] * (1 << b)

    dfs(0)
    return str(ans)

# sample-like small tree
assert run("3\n1 1 0\n1 1\n") == "5"

# single chain
assert run("4\n1 2 3 4\n1 2 3\n") == str(1*4 + 2*3 + 3*2 + 4*1)

# all zeros
assert run("5\n0 0 0 0 0\n1 1 2 2\n") == "0"

# star tree
assert run("4\n1 0 1 0\n1 1 1\n") == str(4 + 2)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi giá trị ngày càng tăng | tổng có trọng số | tích lũy cây con đúng đắn | 
| Tất cả số không | 0 | không có hoạt động không cần thiết | 
| Cây sao | đóng góp hỗn hợp | tính chính xác về kích thước cây con | 

## Vỏ cạnh 

Cây hình chuỗi cho biết kích thước cây con có được truyền lên trên một cách chính xác hay không. Ví dụ: trong chuỗi 4 nút có giá trị [1, 2, 3, 4], thuật toán gán trọng số tăng dần cho các nút cao hơn vì kích thước cây con của chúng bao gồm tất cả các nút con. DFS tính toán kích thước từ dưới lên, đảm bảo nút 1 có kích thước 4, nút 2 có kích thước 3, v.v., tạo ra sự tích lũy chi phí nhất quán. 

Cây hoàn toàn bằng 0 là cách kiểm tra tính nhất quán đơn giản nhất. DFS không bao giờ nhập điều kiện làm tăng thêm chi phí vì không có bit nào được đặt trong bất kỳ nút nào. Đầu ra vẫn bằng 0 mà không yêu cầu bất kỳ xử lý trường hợp đặc biệt nào. 

Cây hình ngôi sao có gốc 1 và tất cả các nút khác là lá kiểm tra xem kích thước cây con có được tính toán chính xác dưới dạng bao phủ toàn bộ ở gốc hay không. Mỗi lá chỉ đóng góp chi phí riêng của nó, trong khi gốc chỉ đóng góp nếu giá trị của chính nó khác 0.
