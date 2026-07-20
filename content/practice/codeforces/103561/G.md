---
title: "CF 103561G - Ruby Rạng Rỡ"
description: "Chúng ta bắt đầu với cây có gốc có kích thước V, trong đó cây có cấu trúc kiểu nhị phân nhưng về mặt hình thức vẫn chỉ là cây có gốc. Mỗi lá của cây này sau đó được ghép với một lá tương ứng trong một bản sao phản ánh của cùng một cây."
date: "2026-07-03T05:24:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103561
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 02-11-22 Div. 1 (Advanced)"
rating: 0
weight: 103561
solve_time_s: 44
verified: true
draft: false
---

[CF 103561G - Ruby rạng rỡ](https://codeforces.com/problemset/problem/103561/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với cây có gốc có kích thước V, trong đó cây có cấu trúc kiểu nhị phân nhưng về mặt hình thức vẫn chỉ là cây có gốc. Mỗi lá của cây này sau đó được ghép với một lá tương ứng trong một bản sao phản ánh của cùng một cây. Sau sự phản chiếu này, các cạnh bổ sung được đưa vào giữa các lá tương ứng, hợp nhất một cách hiệu quả hai cây giống hệt nhau dọc theo các lá ranh giới của chúng. 

Điều này tạo ra một đồ thị vô hướng mới không còn là một cây nữa. Do các kết nối lá giữa bản gốc và bản sao được phản chiếu, biểu đồ chứa các chu trình. Nhiệm vụ là đếm có bao nhiêu chu trình đơn giản riêng biệt tồn tại trong biểu đồ được xây dựng này. 

Sự thay đổi giải thích quan trọng là chúng ta không xây dựng hai cây và đếm chu trình một cách rõ ràng trong một biểu đồ chung. Thay vào đó, mỗi chu trình được hình thành bằng cách lấy hai đường đi từ gốc tới lá riêng biệt trong cây ban đầu và “đóng chúng” thông qua cấu trúc được phản chiếu. Vì vậy, các chu trình tương ứng với sự kết hợp cấu trúc của các đường dẫn lá chứ không phải là các vòng lặp đi qua đồ thị tùy ý. 

Kích thước đầu vào V có thể lên tới 10^6, do đó, bất kỳ giải pháp nào cố gắng liệt kê các chu trình hoặc thậm chí xây dựng rõ ràng biểu đồ phản chiếu đều không khả thi ngay lập tức. Ngay cả việc truyền tải đồ thị tuyến tính trên một cấu trúc nhân đôi cũng phải cực kỳ cẩn thận với các hằng số và bố cục bộ nhớ. Điều này đã loại trừ bất kỳ cách tiếp cận nào cố gắng phát hiện các chu trình sử dụng DFS trên biểu đồ cuối cùng, vì số cạnh sau khi phản chiếu vẫn có thể là tuyến tính nhưng cấu trúc chu trình dày đặc về mặt tổ hợp. 

Một cách giải thích ngây thơ sẽ là xây dựng cây được phản chiếu một cách rõ ràng và kết nối các lá tương ứng, sau đó chạy thuật toán liệt kê chu trình. Ngay cả một phép liệt kê cạnh sau dựa trên DFS cũng sẽ giảm xuống hành vi hàm mũ vì mỗi cạnh lá được thêm vào sẽ tạo ra nhiều chu kỳ chồng chéo thông qua các đường dẫn cây dùng chung. 

Một trường hợp khó phát hiện khi cây thoái hóa thành một chuỗi. Trong trường hợp này chỉ có hai lá và tất cả các chu kỳ đều thu gọn lại thành một chu kỳ lớn bên ngoài. Bất kỳ cách tiếp cận không chính xác nào giả định rằng mỗi kết nối lá tạo thành một chu trình độc lập sẽ bị tính quá mức. 

Một trường hợp cạnh khác là khi một nút chỉ có một nút con trong chuỗi phân nhánh đơn phân lớn. Mặc dù vấn đề được mô tả là “có dạng cây nhị phân”, đầu vào không đảm bảo nghiêm ngặt một cây nhị phân đầy đủ, do đó giả sử cấu trúc nhị phân hoàn hảo sẽ dẫn đến logic đếm không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực xây dựng cấu trúc được phản ánh đầy đủ một cách rõ ràng. Chúng tôi nhân đôi cây, kết nối các lá tương ứng và sau đó cố gắng đếm tất cả các chu trình đơn giản bằng DFS hoặc phương pháp trích xuất cơ sở chu trình. Về nguyên tắc, điều này đúng vì mỗi chu trình là một bước đi khép kín đơn giản trong biểu đồ thu được. 

Tuy nhiên, số lượng chu trình đơn giản trong biểu đồ có kết nối lá L tăng lên nhanh chóng. Mỗi kết nối lá giới thiệu một chu trình cơ bản có cấu trúc bên trong phụ thuộc vào các tiền tố chung của các đường dẫn từ gốc đến lá. Trong trường hợp xấu nhất, khi nhiều lá có chung tổ tiên lâu đời, phép liệt kê dựa trên DFS liên tục xem lại các cây con được chia sẻ và đếm các chu kỳ cấu trúc giống nhau nhiều lần. Điều này dẫn đến sự bùng nổ theo cấp số nhân trong quá trình phân rã chu trình chồng chéo. 

Điểm mấu chốt là đồ thị cuối cùng về cơ bản vẫn là một cây cộng với L cạnh bổ sung, trong đó mỗi cạnh bổ sung kết nối hai lá có đường dẫn chung tổ tiên thấp nhất duy nhất. Mỗi cạnh như vậy đóng góp chính xác một chu trình cơ bản trong không gian chu trình của đồ thị. Do đó, vấn đề giảm xuống còn việc đếm số lượng các chu kỳ độc lập này được tạo ra bởi các cặp lá.

Thay vì xây dựng các chu trình một cách rõ ràng, chúng tôi tính toán xem có bao nhiêu cặp lá tồn tại và bao nhiêu kết nối cấu trúc riêng biệt mà chúng ngụ ý thông qua việc tổng hợp LCA. Cấu trúc được nhân đôi đảm bảo rằng mỗi lá tham gia hiệu quả vào chính xác một cặp, do đó số chu kỳ trở thành một hàm số của số lá cây con được truyền lên cây. 

Điều này làm giảm vấn đề xuống còn việc truyền tải theo thứ tự sau trong đó mỗi cây con đóng góp một số lượng “kết nối lá mở” lan truyền lên trên và bất cứ khi nào hai đóng góp như vậy gặp nhau, chúng sẽ tạo ra các chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ rõ ràng + liệt kê chu trình DFS) | O(2^V) trường hợp xấu nhất | O(V) | Quá chậm | 
| Cây DP khi nhân giống cặp lá | O(V) | O(V) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và xử lý nó bằng DFS đặt hàng sau. Đối tượng trung tâm mà chúng tôi duy trì là số lượng lá trong mỗi cây con và cách các lá này góp phần hình thành chu kỳ khi xem xét các kết nối được nhân đôi. 

1. Chạy DFS từ gốc và tính toán cho mỗi nút số lượng nút lá trong cây con của nó. Một nút là một chiếc lá nếu nó không có nút con. 
2. Đối với mỗi nút, hãy xem xét từng cây con con của nó. Mỗi cây con đóng góp một số lượng lá nhất định mà sau này sẽ được ghép nối thông qua cấu trúc được phản chiếu. 
3. Khi kết hợp các cây con con tại một nút, chúng tôi duy trì tổng số đóng góp lá được thấy cho đến nay. Đối với mỗi cây con mới đóng góp k lá, chúng ta tạo thành k * (lá tích lũy hiện tại) chu kỳ mới. Điều này thể hiện việc ghép các lá trên các nhánh khác nhau thông qua các kết nối do gương tạo ra. 
4. Sau khi xử lý tất cả các nút con của một nút, chúng ta trả về tổng số lá trong cây con này cho nút cha của nó. 
5. Tích lũy các đóng góp của chu trình tại mỗi nút thành một câu trả lời toàn cục. 

Ý tưởng chính là các chu trình được hình thành bất cứ khi nào hai lá từ các cây con khác nhau được kết nối thông qua việc nhận dạng lá được phản chiếu. Điểm thấp nhất nơi đường đi của chúng phân kỳ chính xác là nơi chu kỳ được tính, do đó mỗi cặp được tính chính xác một lần tại LCA của chúng. 

### Tại sao nó hoạt động 

Mỗi chu kỳ trong đồ thị cuối cùng tương ứng duy nhất với một cặp lá có đường đi trong cây ban đầu phân kỳ tại một tổ tiên chung thấp nhất nào đó. Cấu trúc phản chiếu đảm bảo rằng hai lá này được kết nối theo cách khép lại một chu kỳ thông qua cấu trúc phản chiếu. Không có chu trình nào có thể được hình thành nếu không chọn hai lá riêng biệt và mỗi cặp như vậy xác định chính xác một chu trình đơn giản trong biểu đồ kết quả. 

Bởi vì DFS xử lý mỗi nút dưới dạng LCA duy nhất cho các cặp lá từ các cây con khác nhau nên mỗi chu kỳ được tính chính xác một lần tại điểm phân kỳ cao nhất. Điều này ngăn cản việc đếm hai lần và đảm bảo tính đầy đủ. 

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

    ans = 0

    def dfs(u):
        nonlocal ans
        leaf_count = 0

        for v in g[u]:
            sub = dfs(v)

            ans += leaf_count * sub
            leaf_count += sub

        if leaf_count == 0:
            return 1
        return leaf_count

    dfs(1)
    print(ans)

if __name__ == "__main__":
    solve()
```DFS trả về số lá trong mỗi cây con. Khi xử lý một nút, chúng tôi tổng hợp các nút con một cách tuần tự. Mỗi khi chúng tôi kết hợp một cây con mới, chúng tôi ghép các lá của nó với tất cả các lá đã thấy trước đó trong các cây con khác nhau, tương ứng chính xác với việc đếm các chu kỳ được hình thành bởi các kết nối lá trên cấu trúc được phản chiếu. 

Một chi tiết triển khai tinh tế là chúng tôi coi các nút không có nút con là các lá quay về 1. Đây chính là mầm mống cho sự lan truyền của quá trình hình thành chu kỳ đi lên. 

Thứ tự tích lũy rất quan trọng: chúng ta phải thêm các đóng góp trước khi cập nhật số lượng lá đang chạy, nếu không chúng ta sẽ đếm không chính xác các cặp tự ghép bên trong một cây con. 

## Ví dụ đã hoạt động 

Xét một cây nhỏ có gốc 1 có hai con 2 và 3, cả 2 và 3 đều là lá. 

Đối với nút 1: 

| Bước | Đang xử lý con | Lá đóng góp | Chạy lá_count | Đã thêm chu kỳ mới | 
| --- | --- | --- | --- | --- | 
| 1 | con 2 | 1 | 0 → 1 | 0 | 
| 2 | con 3 | 1 | 1 → 2 | 1 | 

Kết quả là 1 chu kỳ được hình thành bằng cách ghép lá 2 và lá 3. 

Điều này chứng tỏ rằng các chu kỳ chỉ được tính khi hợp nhất các cây con riêng biệt. 

Bây giờ hãy xem xét chuỗi 1 → 2 → 3 trong đó chỉ có nút 3 là lá. 

Tại nút 3, chúng ta trả về 1. Tại nút 2, chỉ có một con nên không xảy ra ghép đôi. Tại nút 1, lại chỉ có một cây con đóng góp nên không có chu trình nào được hình thành. Câu trả lời cuối cùng là 0, phù hợp với thực tế là không có cặp lá riêng biệt nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V) | Mỗi cạnh được truy cập một lần trong DFS và mỗi nút thực hiện tổng hợp theo thời gian không đổi trên các nút con | 
| Không gian | O(V) | Danh sách kề và ngăn xếp đệ quy | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc lên tới 10^6 nút vì nó tránh mọi phép liệt kê theo cặp và giảm tất cả việc đếm chu kỳ thành tập hợp cây tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # embedded solution
    sys.setrecursionlimit(10**7)

    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)

    ans = 0

    def dfs(u):
        nonlocal ans
        leaf_count = 0
        for v in g[u]:
            sub = dfs(v)
            ans += leaf_count * sub
            leaf_count += sub
        if leaf_count == 0:
            return 1
        return leaf_count

    dfs(1)
    return str(ans)

# sample-style small trees
assert run("3\n1 2\n1 3") == "1"
assert run("4\n1 2\n1 3\n3 4") == "1"

# chain
assert run("4\n1 2\n2 3\n3 4") == "0"

# star
assert run("5\n1 2\n1 3\n1 4\n1 5") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây sao | 6 | nhiều cặp lá độc lập | 
| chuỗi | 0 | không phân nhánh, không có chu kỳ | 
| chia nhỏ | 1 | hình thành chu trình LCA đơn | 

## Vỏ cạnh 

Trong một chuỗi thuần túy, mỗi nút có nhiều nhất một nút con, do đó DFS không bao giờ gặp phải hai nhóm lá độc lập để kết hợp. Số lượng lá lan truyền lên trên mà không kích hoạt bất kỳ phép nhân chéo nào, do đó số chu kỳ tích lũy vẫn bằng 0. 

Trong một ngôi sao hoàn hảo, tất cả các lá đều được kết nối trực tiếp với gốc. Ở gốc, mỗi lá mới đóng góp các chu kỳ bằng số lá đã thấy trước đó, tạo ra số cặp hình tam giác. DFS tích lũy chính xác điều này vì mỗi lá được coi là một đóng góp của cây con riêng biệt ở cấp gốc. 

Nếu cây rất mất cân bằng nhưng vẫn phân nhánh ở gần ngọn thì số chu kỳ vẫn chỉ được xác định tại các điểm phân kỳ. DFS đảm bảo rằng không có cặp nào được tính hai lần vì mỗi cặp lá có một tổ tiên chung thấp nhất duy nhất nơi đóng góp của chúng gặp nhau lần đầu tiên.
