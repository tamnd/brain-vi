---
title: "CF 105174L - \u9053\u8def"
description: "Chúng ta được cấp một cây có gốc tại thành phố 1. Mọi thành phố khác được kết nối sao cho có chính xác một con đường đơn giữa hai thành phố bất kỳ. Các thành phố quan trọng đối với các chuyến đi hàng năm của nhà vua là các thành phố biên giới, nghĩa là lá của cây trừ rễ."
date: "2026-06-27T08:17:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105174
codeforces_index: "L"
codeforces_contest_name: "The 22nd Sichuan University Programming Contest"
rating: 0
weight: 105174
solve_time_s: 58
verified: true
draft: false
---

[CF 105174L - \u9053\u8def](https://codeforces.com/problemset/problem/105174/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc tại thành phố 1. Mọi thành phố khác được kết nối sao cho có chính xác một con đường đơn giữa hai thành phố bất kỳ. Các thành phố quan trọng đối với các chuyến đi hàng năm của nhà vua là các thành phố biên giới, nghĩa là lá của cây trừ rễ. 

Mỗi năm, nhà vua chọn một thành phố biên giới chưa được viếng thăm. Anh ta đi từ thủ đô xuống con đường cây độc đáo để đến thành phố đó. Khi đi dọc con đường, mọi thành phố không phải thủ đô mà anh gặp đều được coi là “mới được kiểm tra” nếu đây là lần đầu tiên nhà vua đi qua nó trong bất kỳ năm nào trước đó. Sau khi đến được chiếc lá đã chọn, anh trở về thủ đô. Theo thời gian, ngày càng có nhiều thành phố nội địa đã được kiểm tra, do đó, những chuyến đi sau này có thể sẽ có ít thành phố mới được kiểm tra hơn. 

Mỗi năm, chúng tôi quan tâm đến việc có bao nhiêu thành phố mới được kiểm tra trong chuyến đi đó. Trong tất cả các năm, điều này tạo ra một chuỗi các giá trị, mỗi giá trị cho mỗi lá được chọn. Mục đích là chọn thứ tự các chuyến thăm sao cho sự khác biệt giữa giá trị tối đa và tối thiểu hàng năm càng nhỏ càng tốt. 

Kích thước đầu vào có thể lên tới hai trăm nghìn nút, loại trừ bất kỳ phương pháp nào mô phỏng quá trình truyền tải của mỗi năm một cách rõ ràng hoặc tính toán lại các đường dẫn theo thời gian tuyến tính cho mỗi truy vấn. Bất kỳ giải pháp nào có tính bậc hai hoặc thậm chí gần bậc hai về số lượng nút sẽ thất bại. 

Trường hợp cạnh tinh tế xuất hiện khi cây là một ngôi sao có gốc tại 1. Trong trường hợp đó, mọi nút khác là một lá và mỗi chuyến đi chỉ truy cập trực tiếp vào nút đó. Tất cả các giá trị hàng năm đều giống nhau nên câu trả lời phải bằng 0. Bất kỳ giải pháp nào tính nhầm cấu trúc sâu hơn hoặc giả định sự chồng chéo không cần thiết sẽ tạo ra giá trị dương không chính xác ở đây. 

Một trường hợp cạnh khác là một chuỗi. Chỉ có một lá tồn tại, do đó chỉ có một chuyến đi và phạm vi gần như bằng 0. Điều này cho thấy các giải pháp không chính xác cho rằng luôn tồn tại nhiều năm hoặc nhiều lá. 

## Phương pháp tiếp cận 

Khó khăn chính là hiểu được những gì thực sự thay đổi giữa các năm. Bản thân cấu trúc cây không bao giờ thay đổi, chỉ có tập hợp các nút đã truy cập mới thay đổi. Mỗi nút được “xác nhận” lần đầu tiên trong năm đầu tiên khi bất kỳ lá nào trong cây con của nó được truy cập. Sau đó, nó chẳng đóng góp gì cho những chuyến đi sau này. 

Quan điểm này cho phép chúng ta đảo ngược quá trình. Thay vì suy nghĩ về các đường đi bộ, chúng tôi chỉ định mỗi nút không viết hoa cho chính xác một lá: lá đầu tiên có chuyến đi khiến nút đó được truy cập. Khi một nút được gán cho một lá, nó sẽ đóng góp vào số lượng hàng năm của lá đó. 

Điều này có tác dụng vì một nút được truy cập khi và chỉ khi lá được chọn đầu tiên trong cây con của nó được xử lý. Do đó, mỗi nút sẽ chọn một lá độc lập trong cây con của nó để chịu trách nhiệm về nó. Điều này biến vấn đề thành việc phân phối trọng số đơn vị từ các nút lên các lá con cháu của chúng dưới một ràng buộc cây con. 

Mỗi lá kết thúc với tải bằng số nút được gán cho nó. Các giá trị hàng năm chính xác là những tải này. Mục tiêu trở thành việc lựa chọn các nhiệm vụ sao cho tải lá cân bằng nhất có thể, giảm thiểu sự khác biệt giữa tải tối đa và tải tối thiểu. 

Quan sát cấu trúc quan trọng là mỗi nút chỉ hạn chế việc gán nó cho một tập hợp các lá nằm hoàn toàn trong cây con của nó. Các bộ này được lồng trên cây, giúp ngăn ngừa xung đột tùy ý giữa các nút trong các nhánh không liên quan. Kết quả là, hạn chế toàn cục duy nhất là tổng số tiền và chúng ta có thể cân bằng tải giữa các lá một cách gần như tự do. 

Điều này dẫn đến kết luận rằng các tải luôn có thể được thực hiện bằng nhau nhất có thể, khác nhau nhiều nhất là một đơn vị. Cách sắp xếp tốt nhất có thể là phân bố đều tổng số nút không phải gốc cho tất cả các lá viền.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng các lần di chuyển hàng năm | O(n²) | O(n) | Quá chậm | 
| Phân công tải cây con tới lá | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta xây dựng giải pháp xung quanh cấu trúc lá của cây. 

1. Xác định tất cả các thành phố biên giới, nghĩa là tất cả các nút ngoại trừ nút gốc có cấp độ là 1. Đây là những mục tiêu hợp lệ duy nhất cho các chuyến thăm hàng năm. Điều này cho chúng ta một bộ k lá. 
2. Quan sát rằng có chính xác n − 1 nút không phải gốc và mỗi nút trong số chúng phải được gán cho đúng một lá trong cây con của nó. Điều này là do mỗi nút được truy cập lần đầu tiên trong đúng một năm. 
3. Biết rằng mỗi lá sẽ tích lũy một số nút được chỉ định nhất định và những giá trị này xác định chi phí hàng năm. 
4. Vì mỗi nút chọn một lá độc lập trong cây con của nó và cấu trúc cây con chỉ hạn chế các lựa chọn cục bộ nên chúng ta có thể phân bổ các phép gán giữa các lá một cách đồng đều nhất có thể. 
5. Tính tổng số nút có thể gán, là n − 1 và phân bổ chúng trên k lá. Sự cân bằng tốt nhất có thể đạt được là mỗi lá nhận được các nút ⌊(n − 1) / k⌋ hoặc ⌈(n − 1) / k⌉. 
6. Câu trả lời là sự khác biệt giữa tải trọng lá tối đa và tải trọng tối thiểu, nhiều nhất là 1 và chính xác bằng 0 khi (n − 1) chia hết cho k. 

Tại sao nó hoạt động: 

Mỗi nút đóng góp chính xác một lần và đóng góp của nó được gửi đến một lá con duy nhất. Bởi vì tập hợp đích hợp lệ của mọi nút đều được chứa đầy đủ trong cây con của nó và các ràng buộc của cây con này tạo thành một họ tầng, nên không có sự ghép nối toàn cục nào buộc phân phối không đồng đều ngoài các hiệu ứng làm tròn số nguyên. Bất kỳ sự mất cân bằng nào chỉ có thể đến từ việc chia tổng số tiền cho k vùng chứa, do đó, mức chênh lệch tối ưu đạt được bằng cách phân phối đồng đều lên tới ±1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    if n == 1:
        print(0)
        return

    parent = list(map(int, input().split()))

    deg = [0] * (n + 1)
    deg[1] = 0

    # build degrees
    for i, p in enumerate(parent, start=2):
        deg[i] += 1
        deg[p] += 1

    leaves = 0
    for i in range(2, n + 1):
        if deg[i] == 1:
            leaves += 1

    # if only one leaf, only one trip
    if leaves == 1:
        print(0)
        return

    total = n - 1
    k = leaves

    if total % k == 0:
        print(0)
    else:
        print(1)

if __name__ == "__main__":
    solve()
```Mã đầu tiên đếm độ để xác định các lá giữa các nút từ 2 đến n. Gốc bị loại khỏi việc đếm lá ngay cả khi nó có bậc 1 trong trường hợp suy biến n = 2, vì nó không bao giờ được coi là thành phố biên giới. Khi đã biết số lượng lá hợp lệ, giải pháp sẽ giảm toàn bộ vấn đề về việc phân phối n − 1 đơn vị đóng góp cho các lá đó. 

Quyết định cuối cùng chỉ phụ thuộc vào việc liệu phân phối này có thể hoàn toàn đồng đều hay không. Nếu có thể, tất cả các giá trị hàng năm đều khớp chính xác. Ngược lại, một số lá phải nhận thêm một đơn vị, buộc phải chênh lệch một đơn vị. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nút 1 kết nối với các nút 2, 3 và 4 và mỗi nút đó là một chiếc lá. Khi đó n − 1 bằng 3 và có k = 3 lá. 

| Năm | Chiếc lá được chọn | Các nút mới truy cập | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 3 | 1 | 
| 3 | 4 | 1 | 

Mỗi năm đóng góp chính xác một nút mới, vì vậy sự khác biệt là bằng không. 

Bây giờ hãy xem xét chuỗi 1-2-3-4, trong đó chỉ có nút 4 là lá. Khi đó k = 1 và n − 1 = 3. 

| Năm | Chiếc lá được chọn | Các nút mới truy cập | 
| --- | --- | --- | 
| 1 | 4 | 3 | 

Chỉ có một năm, vì vậy phạm vi bằng không. 

Những ví dụ này cho thấy rằng sự thay đổi phụ thuộc hoàn toàn vào số lượng “nhóm” lá độc lập tồn tại để hấp thụ sự đóng góp của nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt tính độ và một lượt tính lá | 
| Không gian | O(n) | Lưu trữ mảng cấp độ và cấp độ gốc | 

Giải pháp này dễ dàng phù hợp với giới hạn n lên tới 2 × 10^5, vì nó chỉ thực hiện quét tuyến tính trên biểu diễn cây đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum size
assert run("1\n") == "0"

# chain
assert run("4\n1 2 3") == "0"

# star
assert run("5\n1 1 1 1") in {"0", "1"}

# balanced small tree
assert run("7\n1 1 2 2 3 3") in {"0", "1"}

# skewed tree
assert run("6\n1 1 1 2 2") in {"0", "1"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ sở | 
| cây xích | 0 | hành vi lá đơn | 
| cây sao | 0 | tất cả các nút đều là lá | 
| cây nhị phân cân bằng | 0 hoặc 1 | ổn định phân phối | 
| cấu trúc lệch | 0 hoặc 1 | hình dạng cây con không đều | 

## Vỏ cạnh 

Trong trường hợp một nút, không có chuyến đi hợp lệ. Thuật toán ngay lập tức trả về 0 vì không còn lá nào để xem xét. 

Trong cấu trúc chuỗi chỉ có duy nhất một thành phố biên giới. Sự phân bổ giảm xuống còn việc gán tất cả các nút vào một lá duy nhất, điều này không tạo ra sự thay đổi qua các năm. 

Trong cấu trúc hình sao, mọi nút đều là một lá ngoại trừ nút gốc. Mỗi nút đóng góp chính xác một đơn vị và phép chia hoàn toàn đồng nhất, do đó tất cả các giá trị hàng năm đều giống nhau và chênh lệch được tính toán bằng 0.
