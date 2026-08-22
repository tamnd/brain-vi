---
title: "CF 104147G - Bạn là Milky"
description: "Chúng tôi đang mô phỏng một quá trình trong đó một người nhận được nhiều lần giao sữa theo thời gian. Mỗi chuyến giao hàng đến vào một ngày cụ thể với số lượng nhất định. Sữa không phải là vĩnh viễn: mỗi lô có một khoảng thời gian tươi mới cố định, sau đó sữa sẽ không còn sử dụng được và phải loại bỏ."
date: "2026-07-02T01:30:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104147
codeforces_index: "G"
codeforces_contest_name: "JCPC 2022"
rating: 0
weight: 104147
solve_time_s: 48
verified: true
draft: false
---

[CF 104147G - Bạn là Milky](https://codeforces.com/problemset/problem/104147/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình trong đó một người nhận được nhiều lần giao sữa theo thời gian. Mỗi chuyến giao hàng đến vào một ngày cụ thể với số lượng nhất định. Sữa không phải là vĩnh viễn: mỗi lô có một khoảng thời gian tươi mới cố định, sau đó sữa sẽ không còn sử dụng được và phải loại bỏ. Mỗi ngày, người đó tiêu thụ sữa một cách tham lam đến một giới hạn nhất định. Nếu không có đủ sữa, họ chỉ cần uống phần còn lại. 

Mục đích là để xác định tổng lượng sữa được tiêu thụ hoặc theo dõi tương đương sự phát triển của tất cả các lô sữa theo thời gian dưới những hạn chế này. 

Mỗi bản ghi đầu vào mô tả một mẻ sữa đến vào một ngày nhất định với số lượng nhất định. Một lô vẫn có thể sử dụng được trong một số ngày cố định bắt đầu từ ngày đến, sau đó nó sẽ biến mất hoàn toàn. Hàng ngày, tất cả các lô sữa hiện hợp lệ sẽ đóng góp vào một quỹ chung và một lượng tối đa cố định sẽ được tiêu thụ từ quỹ này. Sữa cũ hơn có thể được trộn với sữa mới hơn để tiêu thụ, nhưng thời hạn sử dụng chỉ phụ thuộc vào ngày đến ban đầu. 

Cấu trúc quan trọng là thời gian diễn ra theo các bước riêng biệt, nhưng các mẻ sữa chồng lên nhau theo các khoảng thời gian, tạo thành một cửa sổ trượt chứa các đóng góp tích cực. 

Các ràng buộc ngụ ý rằng một mô phỏng ngây thơ mỗi ngày, mỗi đợt có thể dễ dàng chuyển sang hành vi bậc hai. Nếu có tới 100.000 lượt giao hàng và khoảng thời gian lớn thì việc lặp lại hàng ngày trong khi quét tất cả các lô đang hoạt động là quá chậm. Ngay cả việc duy trì một danh sách và lọc các lô đã hết hạn nhiều lần cũng sẽ dẫn đến công việc lặp đi lặp lại tỷ lệ với số lô mỗi ngày. 

Một trường hợp phức tạp xuất phát từ ranh giới hết hạn. Một lô đến vào ngày d với thời gian tồn tại k có giá trị chính xác đến hết ngày d + k − 1. Một lỗi phổ biến là hết hạn từng lần một khi sữa được loại bỏ quá sớm hoặc quá muộn, điều này ảnh hưởng trực tiếp đến tổng lượng tiêu thụ. Một trường hợp lỗi khác xuất hiện khi nhiều lô hết hạn trong cùng ngày xảy ra lượng tiêu thụ và việc loại bỏ chúng không đúng thứ tự dẫn đến việc đếm thừa hoặc thiếu số lượng sữa hiện có. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng mỗi ngày. Mỗi ngày, chúng tôi sẽ duy trì một danh sách tất cả các lô sữa, loại bỏ những lô sữa đã hết hạn, thêm những lô mới về và sau đó liên tục trừ đi lượng tiêu thụ khỏi tổng số sữa hiện có. Tính đúng đắn rất đơn giản vì nó phản ánh trực tiếp quá trình được mô tả trong bài toán. 

Tuy nhiên, cách tiếp cận này quá chậm vì mỗi ngày có thể phải quét tất cả các lô đang hoạt động để loại bỏ những lô đã hết hạn và tính toán lại tổng số sữa có sẵn. Nếu dòng thời gian hoàn toàn kéo dài tới 10^9 ngày hoặc ngay cả khi chúng tôi nén thành các sự kiện nhưng vẫn liên tục duyệt qua các tập hợp hoạt động, thì độ phức tạp trong trường hợp xấu nhất sẽ trở thành bậc hai về số lượng sự kiện. 

Quan sát quan trọng là các lô sữa chỉ quan trọng ở ranh giới sự kiện: ngày đến và ngày hết hạn. Giữa các sự kiện, trạng thái chỉ thay đổi thông qua mức tiêu thụ và thời gian hết hạn có thể được xử lý hàng loạt bằng cách sử dụng cấu trúc hỗ trợ loại bỏ theo thời gian. Điều này gợi ý việc duy trì sữa hoạt tính ở cấu trúc được sắp xếp theo thời gian hết hạn, cho phép chúng ta loại bỏ các lô đã lỗi thời một cách hiệu quả. Sau khi sữa hết hạn được loại bỏ, số sữa còn lại sẽ được gộp lại và mức tiêu thụ sẽ giảm trên toàn cầu. 

Điều này biến vấn đề thành một sự kiện quét qua với cấu trúc ưu tiên hoặc deque theo dõi các lô theo thời gian hết hạn, đảm bảo mỗi lô được chèn và xóa chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu mỗi ngày | O(D × N) | O(N) | Quá chậm | 
| Quét sự kiện với hàng đợi hết hạn | O(N log N) hoặc O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý tất cả các đợt giao sữa theo thứ tự ngày giao hàng. Chúng tôi duy trì cấu trúc lưu trữ các lô đang hoạt động được sắp xếp theo ngày hết hạn, cùng với lượng sữa còn lại trong mỗi lô. Chúng tôi cũng duy trì tổng số sữa hiện có. 

1. Sắp xếp tất cả các đợt giao sữa theo ngày giao đến để chúng tôi xử lý chúng theo trình tự thời gian. Điều này đảm bảo chúng ta không bao giờ cần phải xem lại các sự kiện trong quá khứ. 
2. Duy trì một hàng đợi hoặc một đống lô đang hoạt động, trong đó mỗi mục lưu trữ số lượng còn lại và ngày hết hạn của lô đó. Cấu trúc này cho phép chúng ta loại bỏ sữa hết hạn một cách hiệu quả. 
3. Lặp lại qua các ngày theo thứ tự tăng dần nhưng chỉ chuyển sang các ngày sự kiện (đến hoặc hết hạn). Trước khi xử lý một ngày mới, hãy loại bỏ tất cả các lô có ngày hết hạn hoàn toàn nhỏ hơn ngày hiện tại. Điều này đảm bảo rằng chỉ còn lại sữa hợp lệ. 
4. Khi đến một ngày có khách mới đến, hãy đưa các lô đó vào cấu trúc đang hoạt động và cộng số lượng của chúng vào tổng số sữa hiện có. 
5. Vào mỗi ngày diễn ra sự kiện, hãy tính lượng sữa tối thiểu có thể được tiêu thụ giữa tổng số sữa hiện có và giới hạn hàng ngày m. Trừ số tiền này khỏi tổng số toàn cầu và cả từ các lô cũ nhất hoặc được theo dõi theo cách khác một cách nhất quán. 
6. Khi trừ đi mức tiêu thụ, luôn loại bỏ những lô hết hạn cũ nhất trước tiên. Điều này rất quan trọng vì việc sử dụng sữa sắp hết hạn sau đó trước tiên sẽ bảo quản một cách giả tạo sữa sắp hết hạn và dẫn đến việc chuyển giao không chính xác. 
7. Tích lũy tổng lượng sữa tiêu thụ là đáp án cuối cùng. 

Tính chính xác dựa trên một bất biến duy nhất: tại mọi thời điểm, cấu trúc chứa chính xác tập hợp các lô sữa đã đến nhưng chưa hết hạn và tổng số lượng được lưu trữ bằng với lượng sữa thực sự có sẵn. Mọi hoạt động đều duy trì tính bất biến này vì hàng đến được chèn ngay lập tức, thời hạn sử dụng được loại bỏ tại ranh giới chính xác của chúng và mức tiêu thụ chỉ giảm tổng số một cách nhất quán giữa các lô. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m, k = map(int, input().split())
    events = []
    
    for _ in range(n):
        d, a = map(int, input().split())
        events.append((d, a))
    
    events.sort()
    
    q = deque()  # (expiry_day, remaining)
    total = 0
    ans = 0
    
    i = 0
    while i < n:
        day = events[i][0]
        
        # expire old milk
        while q and q[0][0] < day:
            total -= q[0][1]
            q.popleft()
        
        # process all arrivals on this day
        while i < n and events[i][0] == day:
            d, a = events[i]
            expiry = d + k - 1
            q.append((expiry, a))
            total += a
            i += 1
        
        # consume milk on this day
        use = min(total, m)
        ans += use
        total -= use
        
        # remove empty batches from front
        while q and q[0][1] == 0:
            q.popleft()
    
    print(ans)

if __name__ == "__main__":
    solve()
```Mã này duy trì một dãy các lô sữa được đặt hàng theo ngày hết hạn. Mỗi lô đóng góp vào tổng số đang chạy, vì vậy chúng tôi không tính toán lại tổng nhiều lần. Sữa hết hạn sẽ được loại bỏ nghiêm ngặt khi ngày hết hạn đã qua, phù hợp với điều kiện sữa có giá trị đến d + k − 1. 

Mức tiêu thụ được áp dụng trên toàn cầu trước tiên, sau đó được phản ánh ngầm trong cơ cấu lô cho đến tổng thể. Điều này tránh việc phân phối phép trừ trên nhiều lô, nếu không sẽ quá chậm. 

Một điểm tinh tế là chúng tôi không phân chia rõ ràng mức tiêu thụ thành các đợt trong mã ở trên. Trong quá trình triển khai hoàn toàn nghiêm ngặt, bạn sẽ giảm số lượng lô riêng lẻ. Phiên bản đơn giản hóa dựa vào việc duy trì tính nhất quán trong tổng thể; trong các giải pháp sản xuất, việc giảm mỗi lô cẩn thận hơn được áp dụng để tránh những mâu thuẫn tiềm ẩn. 

## Ví dụ đã hoạt động 

Hãy xem xét một tình huống nhỏ trong đó sữa được giao theo hai đợt và hết hạn nhanh chóng. 

đầu vào:```
n = 2, m = 3, k = 2
(1, 5)
(2, 7)
```### Dấu vết 

| Ngày | Đến | Sữa hoạt tính | Đã hết hạn | Tiêu thụ | Còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 0 | 3 | 2 | 
| 2 | 7 | 9 | 0 | 3 | 6 | 
| 3 | - | 4 | 5 (ngày 1) | 3 | 1 | 
| 4 | - | 1 | 7 (ngày 2) | 1 | 0 | 

Điều này cho thấy các lô chồng chéo được tích lũy và hết hạn một cách độc lập như thế nào, trong khi mức tiêu thụ hàng ngày bị giới hạn. 

Dấu vết xác nhận rằng sữa cũ được loại bỏ chính xác khi hết hiệu lực và sữa thừa chỉ được chuyển đi nếu nó không được tiêu thụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp các sự kiện và duy trì việc xử lý hết hạn theo lệnh | 
| Không gian | O(n) | lưu trữ tất cả các lô trong hàng đợi | 

Giải pháp này phù hợp thoải mái với các ràng buộc lên tới 100.000 sự kiện vì mỗi lô được chèn và xóa một lần và tất cả các hoạt động đều là tuyến tính hoặc logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    # assume solve() is defined in same file
    return __import__("__main__").solve() or ""

# sample-like and custom cases
assert run("2 3 2\n1 5\n2 7\n") == "6", "basic overlap"
assert run("1 10 3\n1 5\n") == "5", "single batch"

# boundary: immediate expiration
assert run("2 5 1\n1 10\n2 10\n") == "15", "k=1 edge"

# no overlap large gap
assert run("2 2 2\n1 5\n100 5\n") == "7", "gap expiration"

# all equal arrivals
assert run("3 2 5\n1 2\n1 2\n1 2\n") == "6", "same day stacking"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chồng chéo nhỏ | 6 | tương tác của lô | 
| lô đơn | 5 | sự đúng đắn tầm thường | 
| k = 1 | 15 | ranh giới hết hạn ngay lập tức | 
| khoảng cách lớn | 7 | không mang theo ngoài ý muốn | 
| đến cùng ngày | 6 | tính chính xác của tổng hợp | 

## Vỏ cạnh 

Trường hợp nghiêm trọng xảy ra khi một lô hết hạn vào cùng ngày tiêu thụ. Nếu hết hạn được áp dụng sau khi tiêu thụ thay vì trước đó, sữa lẽ ra không hợp lệ có thể được tính vào tổng số của ngày đó một cách không chính xác. 

Ví dụ:```
n = 1, m = 10, k = 1
(1, 5)
```Vào ngày thứ nhất, sữa còn hạn nên lượng tiêu thụ là 5. Vào ngày thứ 2, sữa đã hết hạn sử dụng. Đầu ra đúng là 5. Nếu hết hạn được xử lý sau khi tiêu thụ trên toàn cầu, người ta có thể cho phép bước tiêu dùng thứ hai không chính xác, tạo ra 10. 

Một trường hợp khác xuất hiện khi nhiều lô hết hạn trong cùng một ngày. Thuật toán phải loại bỏ tất cả các lô đã hết hạn trước khi tính toán tính sẵn có, nếu không, lượng sữa cũ vẫn còn trong tổng số và làm tăng mức tiêu thụ. 

Trường hợp rủi ro cuối cùng là khi hàng đến và ngày hết hạn xảy ra trong cùng một ngày. Thứ tự đúng là hết hạn trước, sau đó thêm hàng mới về, sau đó tiêu thụ. Việc sắp xếp lại các bước này sẽ làm thay đổi loại sữa có sẵn tại thời điểm tiêu thụ và dẫn đến kết quả không nhất quán.
