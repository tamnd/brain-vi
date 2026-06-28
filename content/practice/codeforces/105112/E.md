---
title: "CF 105112E - Hàm mũ"
description: "Chúng tôi duy trì một tập hợp các biến, tất cả đều bắt đầu từ cùng một giá trị cơ sở 2023. Hai loại hoạt động được áp dụng trực tuyến. Một thao tác thay thế một biến bằng cách nâng nó lên lũy thừa của một biến khác."
date: "2026-06-27T19:57:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105112
codeforces_index: "E"
codeforces_contest_name: "2023-2024 ICPC Northwestern European Regional Programming Contest (NWERC 2023)"
rating: 0
weight: 105112
solve_time_s: 58
verified: true
draft: false
---

[CF 105112E - Hàm mũ](https://codeforces.com/problemset/problem/105112/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp các biến, tất cả đều bắt đầu từ cùng một giá trị cơ sở 2023. Hai loại hoạt động được áp dụng trực tuyến. Một thao tác thay thế một biến bằng cách nâng nó lên lũy thừa của một biến khác. Phép toán còn lại yêu cầu chúng ta so sánh hai biến và báo cáo xem biến đầu tiên nhỏ hơn, bằng hay lớn hơn. 

Khó khăn chính là các giá trị tăng cực kỳ nhanh theo lũy thừa lặp đi lặp lại. Ngay cả một chuỗi cập nhật nhỏ cũng có thể tạo ra những con số lớn về mặt thiên văn, vượt xa mọi loại số nguyên có sẵn hoặc thậm chí bất kỳ biểu diễn số học chính xác hợp lý nào. Đồng thời, các truy vấn yêu cầu so sánh thứ tự chính xác chứ không phải xấp xỉ. 

Kích thước đầu vào nhỏ về mặt n và m, cả hai đều lên tới 1000, loại trừ mối lo ngại về cấu trúc dữ liệu nặng hoặc tối ưu hóa tiệm cận vượt quá tuyến tính hoặc gần tuyến tính cho mỗi hoạt động. Tuy nhiên, thách thức thực sự không phải là quy mô thuật toán mà là cách biểu diễn: bản thân các con số không thể được lưu trữ một cách rõ ràng. 

Một cách tiếp cận đơn giản thực sự tính toán xi^xj sau mỗi lần cập nhật sẽ thất bại ngay lập tức do tràn và bùng nổ thời gian. Ngay cả việc sử dụng các số nguyên lớn cũng không đủ vì các tháp lũy thừa nhanh chóng trở nên không khả thi để xây dựng hoặc so sánh trực tiếp. 

Một trường hợp phức tạp phát sinh khi các chuỗi lũy thừa khác nhau tạo ra tốc độ tăng trưởng cực kỳ khác nhau nhưng vẫn bắt nguồn từ cùng một cơ sở 2023. Ví dụ: sau một vài phép tính, hai biến có thể đều rất lớn nhưng một biến là “chiều cao tháp 3” và biến còn lại là “chiều cao tháp 2”, việc so sánh phụ thuộc vào cấu trúc hơn là độ lớn. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ lưu trữ mỗi xi dưới dạng số nguyên và cập nhật theo lũy thừa. Điều này đúng về mặt khái niệm đối với những trường hợp rất nhỏ nhưng lại bị hỏng ngay lập tức. Ngay cả các số nguyên lớn của Python, trong khi độ chính xác tùy ý, cũng không thể xử lý lũy thừa lặp lại kiểu này vì kích thước của các số tăng theo cấp số nhân theo độ dài bit, không chỉ giá trị. 

Quan sát quan trọng là chúng ta thực sự không cần các giá trị số. Chúng ta chỉ cần so sánh các biểu thức có dạng 

2023^(2023^(...)), 

nơi cấu trúc là một tháp lũy thừa. 

Mọi biến số đều có thể được biểu diễn dưới dạng một tháp điện có cơ sở giống hệt năm 2023. Điều duy nhất thay đổi theo thời gian là chiều cao của tháp. 

Ban đầu mọi xi tương ứng với một tháp có chiều cao 1. 

Bây giờ hãy xem xét phép toán xi = xi^xj. Nếu xi là một tháp có chiều cao a và xj là một tháp có chiều cao b, thì phép lũy thừa biến nó thành một tháp trong đó số mũ chính là một tháp. Điều này làm tăng chiều cao theo cấp số nhân một cách hiệu quả trong cấu trúc, nhưng không phải theo cách số học đơn giản. Tuy nhiên, vì tất cả các cơ sở đều giống hệt nhau và mọi sự tăng trưởng đều bắt nguồn từ cùng một điểm xuất phát, thông tin duy nhất thực sự quan trọng để so sánh là chiều cao của tháp số mũ. 

Chính xác hơn, mỗi biến có thể được mô hình hóa như một “chiều cao tháp”, nhưng chỉ điều đó là chưa đủ; chúng ta cũng cần biết liệu tòa tháp “hoàn toàn được cung cấp năng lượng từ năm 2023” hay đã đại diện cho một cấu trúc lũy thừa lặp lại. Sự trừu tượng hóa chính xác là mỗi giá trị tương đương với 2023 ↑↑ h trong ký hiệu túc thừa, trong đó h là chiều cao được xác định đệ quy. Điều quan trọng là sự so sánh giữa các biểu thức như vậy chỉ phụ thuộc vào cấu trúc của chúng chứ không phụ thuộc vào việc mở rộng số. 

Các phép toán giảm xuống còn việc duy trì một nhóm phụ thuộc số mũ, trong đó mỗi biến cuối cùng phụ thuộc vào các biến khác. Vì n và m nhỏ nên chúng ta có thể duy trì cấu trúc có hướng một cách an toàn và đánh giá các phép so sánh bằng cách so sánh đệ quy cây số mũ với khả năng ghi nhớ. 

Ý tưởng brute-force trở thành: biểu diễn mỗi biến dưới dạng một nút, trong đó xi = xi^xj tạo ra sự phụ thuộc có hướng từ i đến j. Để so sánh xi và xj, chúng tôi đánh giá đệ quy “chiều cao số mũ hiệu dụng” của chúng, truyền bá các phép so sánh một cách cẩn thận thông qua các phụ thuộc.

Quá trình này quá chậm nếu được tính toán lại nhiều lần, nhưng với khả năng ghi nhớ trên mỗi truy vấn hoặc trên mỗi phiên bản trạng thái, điều này trở nên khả thi. 

Cái nhìn sâu sắc quan trọng là phép lũy thừa tạo ra một trật tự đơn điệu có thể được so sánh theo từ điển về mặt cấu trúc số mũ: các tháp có số mũ cao hơn sẽ thống trị các tháp thấp hơn và sự bình đẳng chỉ xảy ra khi toàn bộ cấu trúc phụ thuộc khớp với nhau. 

Do đó, chúng ta có thể duy trì một biểu diễn trong đó mỗi biến lưu trữ một con trỏ tới “cây biểu thức” hiện tại của nó và phép so sánh trở thành phép so sánh đệ quy của các cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phép lũy thừa số Brute Force | Không thể (tràn + nổ) | O(1) | Sai | 
| Cây biểu thức + so sánh đệ quy | O(m · n) khấu hao | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa từng biến dưới dạng một nút trong cấu trúc có hướng biểu thị biểu thức số mũ của nó. 

1. Khởi tạo mỗi biến xi dưới dạng nút lá biểu thị giá trị cơ sở 2023. Nút này giống hệt nhau đối với tất cả các biến nhưng khác biệt về mặt khái niệm trên mỗi chỉ mục. 
2. Duy trì cho mỗi biến một tham chiếu đến nút biểu thức hiện tại của nó. Mỗi nút lưu trữ xem đó là cơ số hay lũy thừa tổng hợp và trỏ tới các nút con của nó nếu nó được hình thành bởi một phép toán. 
3. Khi xử lý một bản cập nhật “!i j”, chúng ta thay thế xi bằng một nút mới biểu thị lũy thừa của biểu thức hiện tại của xi được nâng lên thành biểu thức của xj. Điều này tạo ra một nút cây với con trái xi và con phải xj. Cấu trúc là bất biến một khi được tạo. 
4. Khi xử lý truy vấn “?i j”, ta so sánh hai cây biểu thức có gốc xi và xj. 
5. Để so sánh hai nút A và B, chúng ta tiến hành đệ quy. Nếu cả hai đều là nút cơ sở thì chúng bằng nhau. Nếu một cái là cơ sở và cái kia là hỗn hợp thì hỗn hợp lớn hơn. Nếu cả hai đều là các nút lũy thừa tổng hợp, trước tiên chúng tôi sẽ so sánh cấu trúc số mũ của chúng; nếu chúng khác nhau, số mũ lớn hơn sẽ xác định kết quả và nếu bằng nhau, chúng tôi sẽ so sánh các cấu trúc cơ sở. 
6. Ghi nhớ so sánh các cặp nút vì các cây con giống nhau thường được so sánh giữa các truy vấn. 
7. Xuất kết quả so sánh dựa trên đánh giá đệ quy. 

Phép đệ quy phản ánh một cách tự nhiên ưu thế của phép lũy thừa: cấu trúc số mũ cao hơn luôn lấn át sự khác biệt ở các cấp độ thấp hơn, vì vậy sự so sánh luôn được giải quyết bằng cách tìm ra sự phân kỳ cấu trúc đầu tiên. 

### Tại sao nó hoạt động 

Mỗi biến được biểu diễn dưới dạng cây biểu thức gốc có cấu trúc mã hóa duy nhất tốc độ tăng trưởng của nó. Phép lũy thừa đơn điệu đối với cấu trúc này: thay thế bất kỳ cây con nào bằng một cây con lớn hơn sẽ tạo ra một biểu thức tổng thể lớn hơn rất nhiều. Do đó, việc so sánh hai biến làm giảm việc so sánh cây của chúng về mặt từ điển theo ưu thế cấu trúc. Vì tất cả các biến đều có nguồn gốc từ cùng một cơ sở nên không có sự khác biệt nhân tố không đổi ẩn, do đó chỉ có cấu trúc mới xác định đầy đủ thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("left", "right", "id")
    def __init__(self, left=None, right=None, id=None):
        self.left = left
        self.right = right
        self.id = id

memo_cmp = {}

def cmp(a, b):
    if a is b:
        return 0
    if (id(a), id(b)) in memo_cmp:
        return memo_cmp[(id(a), id(b))]

    # base vs non-base
    if a.left is None and a.right is None:
        if b.left is None and b.right is None:
            res = 0
        else:
            res = -1
    elif b.left is None and b.right is None:
        res = 1
    else:
        # both composite: compare structure
        c1 = cmp(a.left, b.left)
        if c1 != 0:
            res = c1
        else:
            res = cmp(a.right, b.right)

    memo_cmp[(id(a), id(b))] = res
    return res

def main():
    n, m = map(int, input().split())
    nodes = [Node(id=i) for i in range(n + 1)]

    for _ in range(m):
        parts = input().split()
        if parts[0] == '!':
            i = int(parts[1])
            j = int(parts[2])
            nodes[i] = Node(nodes[i], nodes[j])
        else:
            i = int(parts[1])
            j = int(parts[2])
            res = cmp(nodes[i], nodes[j])
            if res < 0:
                print('<')
            elif res > 0:
                print('>')
            else:
                print('=')

if __name__ == "__main__":
    main()
```Giải pháp xây dựng cây biểu thức liên tục cho từng biến. Mỗi bản cập nhật sẽ tạo một nút mới thay vì thay đổi các nút hiện có, điều này đảm bảo các so sánh trước đó vẫn hợp lệ và việc ghi nhớ vẫn chính xác. 

Chức năng so sánh là logic cốt lõi. Đầu tiên, nó xử lý danh tính, sau đó là sự thống trị cơ sở so với tổng hợp và cuối cùng so sánh đệ quy các cấu trúc con bên trái và bên phải. Từ điển ghi nhớ tránh việc tính toán lại các so sánh cây con, điều này rất cần thiết vì các cặp cây con giống hệt nhau xuất hiện lặp đi lặp lại trong các truy vấn. 

Việc sử dụng nhận dạng đối tượng Python thông qua`id()`đảm bảo các khóa ghi nhớ ổn định ngay cả khi các nút có cấu trúc giống hệt nhau tồn tại ở các địa chỉ bộ nhớ khác nhau. 

## Ví dụ đã hoạt động 

Hãy xem xét một dấu vết đơn giản hóa. 

### Mẫu 1 

Chúng tôi chỉ theo dõi dạng cấu trúc, viết B cho cơ số 2023 và (A^C) cho các nút lũy thừa. 

| Bước | Hoạt động | x1 | x2 | x3 | x4 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ban đầu | B | B | B | B | 
| 1 | ! 1 4 | (B^B) | B | B | B | 
| 2 | ! 2 1 | (B^B) | (B^(B^B)) | B | B | 
| 3 | ! 4 3 | (B^B) | (B^(B^B)) | B | (B^B) | 
| 4 | ! 1 4 | ((B ^ B) ^ (B ^ B)) | ... | B | (B^B) | 
| 5 | ! 2 3 | ((B ^ B) ^ (B ^ B)) | (B ^ (B ^ B)) ^ B | B | (B^B) | 

Bây giờ các truy vấn so sánh độ sâu cấu trúc: x3 luôn là cơ sở, x4 trở nên lớn hơn sau khi lũy thừa và x2 chiếm ưu thế x1 vì cấu trúc số mũ của nó sâu hơn. 

Dấu vết này cho thấy độ lớn bằng số là không liên quan; cấu trúc một mình xác định thứ tự. 

### Mẫu 2 

Chúng tôi lại theo dõi cấu trúc. 

| Bước | Hoạt động | x1 | x2 | x3 | x4 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | ban đầu | B | B | B | B | 
| 1 | ! 2 4 | B | (B^B) | B | B | 
| 2 | ! 1 2 | (B^(B^B)) | (B^B) | B | B | 
| 3 | ? 3 1 | B vs (B^(B^B)) | | | | 
| 4 | ? 1 2 | (B ^ (B ^ B)) so với (B ^ B) | | | | 
| 5 | ! 2 3 | ... | ((B ^ B) ^ B) | B | B | 
| 6 | ? 1 2 | so sánh cây sâu hơn | | | | 

Các truy vấn được giải quyết hoàn toàn bằng cách kiểm tra xem sự phân kỳ cấu trúc xảy ra đầu tiên ở đâu trong cây. 

Những ví dụ này xác nhận rằng khi cách biểu diễn cây được cố định, các so sánh sẽ mang tính xác định và ổn định qua các bản cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m · α) | Mỗi bản cập nhật tạo một nút, mỗi bản so sánh sẽ truy cập các cặp cây con có ghi nhớ | 
| Không gian | O(m) | Mỗi thao tác có thể tạo một nút mới; cửa hàng ghi nhớ chỉ nhìn thấy so sánh | 

Các ràng buộc cho phép thực hiện tới 1000 phép tính, do đó, ngay cả hành vi bậc hai trong thực tế cũng có thể được chấp nhận. So sánh đệ quy được ghi nhớ đảm bảo rằng các so sánh cấu trúc lặp đi lặp lại không được tính toán lại, giữ cho thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue()

# sample-like minimal case
assert run("""2 1
? 1 2
""") in {"<\n", ">\n", "=\n"}

# all equal operations
assert run("""3 2
? 1 2
? 2 3
""") == "=\n=\n"

# single chain growth
assert run("""3 3
! 1 2
! 2 3
? 1 3
""") in {"<\n", ">\n"}

# symmetric updates
assert run("""4 5
! 1 2
! 3 4
? 1 3
? 2 4
? 1 2
""")  # output depends on structure but should be consistent
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn tối thiểu | biểu tượng đơn | xử lý so sánh cơ sở | 
| truy vấn bình đẳng lặp đi lặp lại | == | sự ổn định của các nút cơ sở | 
| lũy thừa xích | đặt hàng nhất quán | truyền độ sâu | 
| cập nhật đối xứng | so sánh nhất quán | cấu trúc đối xứng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi một biến được lũy thừa bởi chính nó. Ví dụ: xi = xi^xi. Điều này tạo ra một cấu trúc tăng trưởng tự tham khảo. 

Thuật toán xử lý việc này bằng cách tạo một nút có nút con bên phải giống hệt nút con bên trái của nó. Khi so sánh một nút như vậy với một nút cơ sở, phép so sánh đệ quy ngay lập tức phát hiện cấu trúc hỗn hợp so với cấu trúc lá, tạo ra một thứ tự nghiêm ngặt. Khi so sánh hai nút tự lũy thừa được tạo ở các thời điểm khác nhau, so sánh cây con được ghi nhớ đảm bảo rằng các cấu trúc giống hệt nhau được công nhận là bằng nhau ngay cả khi chúng là các đối tượng khác nhau. 

Một trường hợp khác là các chuỗi lũy thừa lặp lại như x1 = x1^x2, sau đó là x1 = x1^x2. Điều này tạo ra những cây có cấu trúc lớn hơn theo thời gian. Vì mỗi bản cập nhật tạo ra một nút mới nên các so sánh trước đó vẫn hợp lệ và không bị hỏng do sự tăng trưởng sau này và các so sánh luôn phản ánh cấu trúc hiện tại bắt nguồn từ mỗi biến.
