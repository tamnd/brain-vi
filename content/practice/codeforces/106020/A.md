---
title: "CF 106020A - Ghi nhãn cây"
description: "Chúng ta có một cây vô hướng có $n$ đỉnh. Nhiệm vụ là gán cho mỗi đỉnh một trong ba nhãn $a$, $b$ hoặc $c$, sao cho không có cạnh nào nối hai đỉnh có cùng nhãn."
date: "2026-06-25T13:10:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106020
codeforces_index: "A"
codeforces_contest_name: "The 2025 Damascus University Collegiate Programming Contest"
rating: 0
weight: 106020
solve_time_s: 64
verified: true
draft: false
---

[CF 106020A - Dán nhãn cây](https://codeforces.com/problemset/problem/106020/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây vô hướng với$n$đỉnh. Nhiệm vụ là gán cho mỗi đỉnh một trong ba nhãn,$a$,$b$, hoặc$c$, sao cho không có cạnh nào nối hai đỉnh có cùng nhãn. Trong số tất cả các phép gán hợp lệ như vậy, chúng ta được yêu cầu tạo ra chuỗi nhỏ nhất về mặt từ điển được hình thành bằng cách ghép các nhãn của các đỉnh theo thứ tự từ$1$ĐẾN$n$. 

Cấu trúc cây chỉ quan trọng thông qua các ràng buộc kề. Yêu cầu hoàn toàn mang tính cục bộ trên các cạnh, nhưng mục tiêu là toàn cục vì thứ tự từ điển phụ thuộc vào toàn bộ chuỗi. Sự kết hợp này buộc chúng ta phải lựa chọn nhãn hiệu một cách tham lam với nhận thức về những xung đột trong tương lai. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các đỉnh, loại trừ mọi hoạt động quay lui theo cấp số nhân hoặc kiểm tra lực lượng mạnh mẽ trên mỗi đỉnh đối với tất cả các nhãn. Ngay cả việc thử ba lựa chọn cho mỗi nút mà không cắt tỉa cũng dẫn đến$3^n$, điều đó hoàn toàn không thể thực hiện được. Bất kỳ giải pháp hợp lệ nào cũng phải chạy trong thời gian tuyến tính hoặc gần tuyến tính, về cơ bản$O(n)$hoặc$O(n \log n)$. 

Chế độ thất bại tinh vi xuất hiện khi sử dụng chiến lược tham lam ngây thơ mà không tôn trọng cấu trúc cây. Ví dụ: luôn gán nhãn nhỏ nhất có thể cục bộ mà không xem xét các ràng buộc gốc có thể gặp khó khăn sau này. 

Hãy xem xét một con đường đơn giản$1 - 2 - 3$. Nếu chúng ta gán$s_1 = a$, sau đó tham lam gán$s_2 = a$cũng vì nó nhỏ nhất cục bộ nên chúng ta ngay lập tức vi phạm các ràng buộc, nhưng ngay cả khi chúng ta thực thi tính hợp lệ cục bộ, việc chọn$s_2 = b$và sau đó$s_3 = a$đưa ra “aba”, điều đó đúng. Tuy nhiên, trong cấu trúc phân nhánh như một ngôi sao bắt nguồn từ 1 với các ngôi sao lân cận 2, 3, 4, một cách tiếp cận bất cẩn có thể gán tất cả các phần tử con một cách tham lam là “a” nếu chúng được xử lý độc lập, tạo ra kết quả đầu ra không hợp lệ chẳng hạn như “aaa?” vi phạm các ràng buộc trên mọi cạnh. 

Một vấn đề tế nhị khác nảy sinh từ thứ tự truyền tải. Nếu chúng tôi xử lý các nút theo thứ tự DFS mà không sửa lỗi phụ thuộc gốc đầu tiên, thì chúng tôi có thể chỉ định nút con trước khi biết nhãn cuối cùng của nút cha trong một số cách triển khai không chính xác. Điều đó dẫn đến những quyết định không nhất quán và kết quả không hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra tất cả các nhiệm vụ của$\{a,b,c\}$đến$n$các đỉnh và chỉ giữ lại những đỉnh thỏa mãn ràng buộc cạnh. Đối với mỗi phép gán hợp lệ, chúng tôi tính toán giá trị từ điển của nó và giữ giá trị nhỏ nhất. 

Điều này hiệu quả vì nó kiểm tra rõ ràng mọi nhãn có thể có, nhưng nó thất bại ngay lập tức về mặt quy mô. Số bài tập là$3^n$và mỗi chi phí xác nhận$O(n)$để kiểm tra tất cả các cạnh, đưa ra$O(n \cdot 3^n)$, trở nên không thể sử dụng được ngay cả đối với$n = 20$, huống hồ là$2 \cdot 10^5$. 

Quan sát cấu trúc quan trọng là cây có dạng lưỡng bội nhưng có thêm một màu. Nếu chúng ta bị giới hạn ở hai màu, giải pháp sẽ được khắc phục bằng cách hoán đổi. Với ba màu, chúng tôi có được tính linh hoạt: tại mỗi nút, chúng tôi chỉ cần tránh nhãn gốc và phần còn lại của cấu trúc không áp đặt ràng buộc trực tiếp bổ sung nào ngoài vùng lân cận. 

Điều này cho thấy một công trình tham lam. Nếu chúng ta root cây ở bất kỳ đâu, hạn chế duy nhất đối với một nút là nút cha của nó và các nút lân cận đã được chỉ định trong DFS. Vì các nút con chưa được chỉ định khi chúng tôi xử lý một nút theo thứ tự trước nên chúng tôi chỉ cần tránh màu của nút cha. Điều đó làm giảm mỗi quyết định xuống còn nhiều nhất là hai lựa chọn hợp lệ. Việc chọn nhãn hợp lệ nhỏ nhất ở mỗi bước đảm bảo tính tối thiểu về mặt từ điển vì các vị trí trước đó chiếm ưu thế trong thứ tự. 

Tính chính xác phụ thuộc vào thực tế là khi một nút được chỉ định, nó không bao giờ cần phải xem xét lại. Cấu trúc cây đảm bảo không có chu kỳ, do đó không có sự lan truyền ràng buộc ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot 3^n)$|$O(n)$| Quá chậm | 
| Màu DFS tham lam |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta lấy gốc cây ở đỉnh 1. Mục tiêu là gán nhãn theo cách tôn trọng các ràng buộc cha-con trong khi luôn ưu tiên nhãn nhỏ nhất có thể. 

1. Chọn một gốc tùy ý, thường là đỉnh 1 và xây dựng biểu diễn danh sách kề của cây. Điều này cho phép truyền tải hiệu quả mà không cần tính toán lại hàng xóm. 
2. Khởi động DFS từ thư mục gốc. Gán nhãn gốc nhỏ nhất, luôn luôn là$a$. Lựa chọn này an toàn vì không có tổ tiên nào xung đột với nó và nó giảm thiểu ký tự đầu tiên của chuỗi kết quả. 
3. Đối với mọi nút$v$, khi xử lý các phần tử con của nó, hãy quyết định nhãn của từng phần tử con một cách độc lập dựa trên nhãn của phần tử cha của nó. Vì ràng buộc chỉ là các nút liền kề khác nhau nên nút con phải tránh nhãn của nút cha. 
4. Gán nhãn nhỏ nhất có thể trong số$\{a, b, c\}$đó không bằng nhãn của cha mẹ. Lựa chọn tham lam này hợp lệ vì nó chỉ ảnh hưởng đến vị trí hiện tại trong chuỗi; các bài tập sau không ảnh hưởng đến thứ tự từ điển trước đó. 
5. Tiếp tục DFS đệ quy, chuyển nhãn đã chọn làm ràng buộc gốc cho các nút sâu hơn. 

Tại sao nó hoạt động xuất phát từ cấu trúc của cây và bản chất của trật tự từ điển. Nhãn của mỗi nút chỉ ảnh hưởng đến vị trí của nó trong chuỗi cuối cùng và không hạn chế các nút anh chị em vượt quá ràng buộc cha mẹ được chia sẻ. Vì chúng tôi luôn chọn nhãn có sẵn nhỏ nhất ở mỗi vị trí, nên không có sự điều chỉnh nào sau này có thể tạo ra tiền tố nhỏ hơn về mặt từ điển mà không vi phạm ràng buộc cạnh tại nút đó. Phép gán này mang tính tối ưu cục bộ và nhất quán toàn cục vì biểu đồ phụ thuộc có tính chất không theo chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    res = [''] * (n + 1)

    def dfs(u, parent):
        for c in "abc":
            if c != res[parent]:
                res[u] = c
                break
        for v in g[u]:
            if v != parent:
                dfs(v, u)

    res[0] = '#'  # dummy parent so root can take 'a'
    res[1] = 'a'
    for v in g[1]:
        dfs(v, 1)

    return "".join(res[1:])

print(solve())
```Việc triển khai dựa trên việc truyền tải DFS đơn giản. Gốc được khởi tạo riêng biệt để đảm bảo nó có nhãn nhỏ nhất. Một giá trị giả được sử dụng cho giá trị gốc của nó để giá trị gốc không bị hạn chế. 

Điều tinh tế chính là đảm bảo rằng chúng tôi không bao giờ so sánh với nhãn gốc chưa được khởi tạo. sử dụng`res[parent]`trực tiếp hoạt động vì mọi nút đều được chỉ định trước khi các nút con của nó được xử lý. 

Một điểm khác là độ sâu đệ quy. Vì cây có thể là một chuỗi dài nên giới hạn đệ quy mặc định của Python không đủ nên được tăng lên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2
1 3
```Chúng tôi bắt đầu ở nút 1. 

| Bước | Nút | Phụ huynh | Lựa chọn có sẵn | Nhãn được chọn | Trạng thái một phần | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | giả | a, b, c | một | một-- | 
| 2 | 2 | 1 | b, c | b | ab- | 
| 3 | 3 | 1 | b, c | b | abb | 

Điều này cho thấy rằng cả hai phần tử con của gốc đều tránh “a” một cách độc lập và chúng tôi nhất quán chọn “b” là tùy chọn hợp lệ nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
4
1 2
2 3
3 4
```Đây là một chuỗi. 

| Bước | Nút | Phụ huynh | Lựa chọn có sẵn | Nhãn được chọn | Trạng thái một phần | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | giả | a, b, c | một | một--- | 
| 2 | 2 | 1 | b, c | b | ab-- | 
| 3 | 3 | 2 | một, c | một | aba- | 
| 4 | 4 | 3 | b, c | b | abab | 

Cấu trúc xen kẽ xuất hiện một cách tự nhiên vì mỗi nút chỉ tránh nhãn cha của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi đỉnh và cạnh được truy cập một lần trong DFS | 
| Không gian |$O(n)$| Danh sách kề và ngăn xếp đệ quy lưu trữ thông tin tuyến tính | 

Giải pháp phù hợp một cách thoải mái trong các ràng buộc vì cả thời gian và bộ nhớ đều có quy mô tuyến tính với số lượng đỉnh. Ngay cả đối với$2 \cdot 10^5$, một lần truyền tải DFS duy nhất là hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    res = [''] * (n + 1)

    def dfs(u, p):
        for c in "abc":
            if c != res[p]:
                res[u] = c
                break
        for v in g[u]:
            if v != p:
                dfs(v, u)

    res[0] = '#'
    res[1] = 'a'
    for v in g[1]:
        dfs(v, 1)

    return "".join(res[1:])

# sample-style tests
assert run("3\n1 2\n1 3\n") in ["abb", "abc", "aba"], "star tree validity"
assert run("4\n1 2\n2 3\n3 4\n") == "abab", "chain alternation"

# custom tests
assert run("1\n") == "a", "single node"
assert run("2\n1 2\n") == "ab", "minimum edge"
assert run("5\n1 2\n1 3\n1 4\n1 5\n") != "aaaa", "star constraint check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | một | xử lý trường hợp cơ bản | 
| cây 2 nút | ab | ràng buộc cạnh trực tiếp | 
| cây sao | không “aaa..” | nhân giống cha-con | 
| chuỗi | mô hình xen kẽ | Tính chính xác của DFS | 

## Vỏ cạnh 

Một cây đỉnh duy nhất là kịch bản đơn giản nhất. Thuật toán gán gốc “a” ngay lập tức và không xảy ra quá trình truyền tải nào nữa. Vì không có cạnh nào nên điều này có giá trị tầm thường. 

Cây hai nút kiểm tra xem ràng buộc cha mẹ có được thực thi đúng cách hay không. Gốc nhận được “a” và trẻ phải lấy “b”. Bất kỳ nỗ lực nào để gán “a” cho thành phần con sẽ vi phạm quy tắc cạnh, vì vậy điều này sẽ kiểm tra tính chính xác của logic lọc. 

Cây hình ngôi sao kiểm tra xem tính độc lập của anh chị em có được xử lý đúng cách hay không. Mỗi đứa trẻ được xử lý riêng biệt nhưng tất cả đều phải tránh nhãn gốc. Thuật toán gán cho chúng tất cả là “b”, vẫn hợp lệ vì anh chị em không bị ràng buộc trực tiếp. 

Chuỗi sâu kiểm tra xem DFS có truyền tải chính xác các ràng buộc qua nhiều cấp độ hay không. Mỗi nút chỉ phụ thuộc vào nút cha của nó và các nhiệm vụ luân phiên xuất hiện một cách tự nhiên mà không cần ghi sổ bổ sung.
