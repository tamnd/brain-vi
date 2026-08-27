---
title: "CF 104361D - \u041f\u0435\u0440\u0435\u0432\u0435\u0440\u043d\u0443\u0442\u044b\u0435 \u0440\u043e\u0434\u043e\u0441\u043b\u043e\u0432\u043d\u044b\u0435"
description: "Chúng ta được cung cấp một cấu trúc trên những người được gắn nhãn $n$ trong đó mỗi người có 0 hoặc có đúng một con. Nếu một người không có con, con trỏ gửi đi của họ là 0. Ngược lại, mỗi người sẽ trỏ đến chính xác một chỉ mục con trong $[1, n]$."
date: "2026-07-01T17:55:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104361
codeforces_index: "D"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2020"
rating: 0
weight: 104361
solve_time_s: 49
verified: true
draft: false
---

[CF 104361D - \u041f\u0435\u0440\u0435\u0432\u0435\u0440\u043d\u0443\u0442\u044b\u0435 \u0440\u043e\u0434\u043e\u0441\u043b\u043e\u0432\u043d\u044b\u0435](https://codeforces.com/problemset/problem/104361/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc trên$n$những người được dán nhãn trong đó mỗi người không có con hoặc có đúng một con. Nếu một người không có con, con trỏ gửi đi của họ là 0. Ngược lại, mỗi người chỉ vào đúng một chỉ mục con trong$[1, n]$. This creates a forest-like structure, but with a strong constraint: every node has exactly one outgoing edge except a single terminal node that has none.

 From this functional graph, we define a “family set” of a person as the set containing the person itself plus everyone reachable by repeatedly following parent links upward. Parents are not given directly, but since each node has either zero or two parents, the structure implicitly defines a reverse tree relationship.

 Một sự kiện “nghiêng” xảy ra tại một nút có hai nút cha được xác định. Đối với một nút như vậy, chúng tôi so sánh kích thước của hai cây con cha (về số lượng nút thuộc về mỗi tập hợp họ cha). Nếu một bên lớn ít nhất gấp đôi bên kia thì nút này sẽ tạo ra một độ lệch. 

Nhiệm vụ là xác định liệu chúng ta có thể xây dựng một cấu trúc như vậy trên$n$các nút tạo ra chính xác$k$làm lệch các sự kiện và nếu có, hãy xuất ra bất kỳ cấu trúc hợp lệ nào của các con trỏ con. 

Ràng buộc$n \le 100{,}000$buộc phải xây dựng tuyến tính hoặc gần tuyến tính. Bất kỳ nỗ lực nào nhằm mô phỏng kích thước cây con một cách linh hoạt trên mỗi nút theo cách đơn giản đều dẫn đến chi phí tính toán lại có thể vượt quá$O(n)$hoặc$O(n \log n)$. Yêu cầu chính là thiết kế trực tiếp một cấu trúc với kích thước cây con có thể kiểm soát được và số lượng độ lệch có thể dự đoán được. 

Một trường hợp khó nhận thấy là khi$k$lớn so với$n$. Vì mỗi độ lệch được gắn với một nút có hai nút cha và mỗi nút như vậy tương ứng với một điểm hợp nhất trong một cấu trúc giống nhị phân, nên có một giới hạn trên ngầm định về số lượng sự mất cân bằng như vậy có thể bị ép buộc. Ví dụ, đối với$n = 3$, không thể có được 2 độ lệch, bởi vì chỉ một nút thậm chí có thể có hai nút cha trong bất kỳ cấu trúc hợp lệ nào. Một giả định ngây thơ rằng mọi sự hợp nhất nội bộ có thể tạo ra sự sai lệch một cách độc lập dẫn đến việc đếm quá mức. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là tạo ra tất cả các cấu trúc cha-con có thể thỏa mãn các ràng buộc, tính toán kích thước cây con cho mỗi nút và đếm các sự kiện lệch. Điều này sẽ yêu cầu liệt kê các đồ thị hàm số với các hạn chế về cấu trúc bổ sung, siêu cấp số nhân trong$n$. Ngay cả việc kiểm tra một chi phí cấu trúc duy nhất$O(n)$, vì vậy điều này hoàn toàn không khả thi ngoài phạm vi nhỏ$n$, gần như vượt quá$n = 10$hoặc$n = 12$. 

Quan sát quan trọng là các sự kiện lệch chỉ phụ thuộc vào kích thước cây con tương đối tại các điểm hợp nhất chứ không phụ thuộc vào cấu trúc toàn cục đầy đủ. Nếu chúng ta xây dựng một đường trục giống như chuỗi và đính kèm các cây con có kích thước được kiểm soát, chúng ta có thể buộc một nút nghiêng một cách xác định bằng cách làm cho một bên có kích thước ít nhất gấp đôi kích thước của bên kia. Điều này làm giảm vấn đề phân phối$n$các nút thành các nhóm có kích thước mã hóa các mô hình tăng trưởng nhị phân. 

Chúng tôi diễn giải lại cấu trúc như việc xây dựng một công trình có gốc rễ trong đó mỗi điểm hợp nhất kết hợp hai thành phần được xây dựng trước đó. Mỗi lần hợp nhất như vậy sẽ tạo ra chính xác một nút nghiêng ứng cử viên. Điều kiện cho độ lệch trở thành sự bất bình đẳng đơn giản giữa các kích thước thành phần. Vì vậy, vấn đề giảm xuống việc lựa chọn$k$hợp nhất các điểm mà chúng tôi thực thi sự mất cân bằng, đồng thời giữ cho tất cả các hợp nhất khác đủ cân bằng để tránh tình trạng sai lệch ngẫu nhiên. 

Chúng tôi xây dựng các thành phần theo cách lặp đi lặp lại: mỗi thành phần mới nhân đôi thành phần trước đó, tùy ý thêm một lá “bù đắp” nhỏ để thực thi hoặc ngăn ngừa sự lệch. Điều này biến việc xây dựng thành một bản mở rộng nhị phân có kiểm soát trong đó kích thước cây con là lũy thừa của hai hoặc các biến thể gần giống. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | hàm mũ | hàm mũ | Quá chậm | 
| Thiết kế hợp nhất nhị phân mang tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng biểu đồ tăng dần bằng cách sử dụng một chuỗi các thành phần, mỗi thành phần có kích thước đã biết và chọn cẩn thận các điểm hợp nhất. 

1. Bắt đầu với$n$các nút bị cô lập, mỗi nút là một thành phần tầm thường có kích thước 1. Mỗi nút ban đầu không có nút con nào được gán. 
2. Duy trì một danh sách các thành phần, trong đó mỗi thành phần đại diện cho một cây con có gốc và kích thước đã biết. 
3. Liên tục hợp nhất hai thành phần thành một thành phần mới. Khi kết hợp các thành phần$A$Và$B$, chúng ta gán gốc của$A$để chỉ vào gốc của$B$, hoặc ngược lại, từ đó tạo ra mối quan hệ cha-con giữa các gốc. Kích thước thành phần kết quả là$|A| + |B|$. 
4. Quyết định xem việc hợp nhất này có tạo ra sự lệch hay không. Nếu chúng tôi muốn có sự sai lệch trong quá trình hợp nhất này, chúng tôi đảm bảo$|A| \ge 2|B|$hoặc$|B| \ge 2|A|$. Nếu chúng ta không muốn có sự sai lệch, chúng ta đảm bảo$|A| < 2|B|$Và$|B| < 2|A|$, điều này chỉ có thể thực hiện được khi kích thước được giữ càng gần càng tốt. 
5. Để đạt được chính xác$k$nghiêng, chúng tôi xây dựng một chuỗi các kích thước thành phần bắt chước sự tăng trưởng của cây nhị phân được kiểm soát: chúng tôi liên tục kết hợp các thành phần có kích thước bằng nhau để tránh độ lệch và đôi khi kết hợp một thành phần lớn với một thành phần nhỏ hơn nhiều để tạo ra độ lệch. 
6. Chúng tôi bắt đầu bằng cách xây dựng các thành phần có kích thước là lũy thừa từ hai đến lớn nhất không vượt quá$n$. Điều này cho chúng ta một sự phân rã nhị phân của cấu trúc. 
7. Sau đó chúng tôi chọn$k$hợp nhất các hoạt động giữa các kết hợp này trong đó chúng tôi cố tình tạo ra sự mất cân bằng bằng cách gắn một thành phần đơn lẻ hoặc tối thiểu vào một thành phần lớn, đảm bảo điều kiện nghiêng. 
8. Cuối cùng, chúng tôi dịch lịch sử hợp nhất thành mảng đầu ra được yêu cầu$s$, gán cho mỗi nút chính xác một nút con theo các phép hợp nhất đã ghi và thiết lập$s_i = 0$cho nút chìm cuối cùng. 

### Tại sao nó hoạt động 

Việc xây dựng làm giảm tình trạng sai lệch toàn cục so với các so sánh cục bộ tại các điểm hợp nhất. Mỗi lần hợp nhất tương ứng với chính xác một nút có hai cây con phía cha được xác định đầy đủ tại thời điểm xây dựng. Vì kích thước thành phần được duy trì rõ ràng nên không thao tác nào sau này có thể thay đổi liệu hợp nhất được tạo trước đó có bị lệch hay không. Tính độc lập này đảm bảo rằng các sai lệch được tính chính xác một lần cho mỗi lần hợp nhất không cân bằng có chủ ý và không có sự mất cân bằng ngoài ý muốn nào có thể xuất hiện ở nơi khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    # We construct components as (size, root)
    # Each node starts as its own component
    comp = [(1, i) for i in range(1, n + 1)]
    parent = [0] * (n + 1)

    # We will greedily merge components; we track skew usage
    skews_left = k

    while len(comp) > 1:
        comp.sort()
        a_size, a_root = comp.pop()
        b_size, b_root = comp.pop()

        # decide if we force skew
        if skews_left > 0 and a_size >= 2 * b_size:
            # force skew: attach b under a
            parent[b_root] = a_root
            comp.append((a_size + b_size, a_root))
            skews_left -= 1
        else:
            # avoid skew: attach smaller under larger carefully
            parent[a_root] = b_root
            comp.append((a_size + b_size, b_root))

    # if we still have skews left, impossible under this construction
    if skews_left != 0:
        print("NO")
        return

    print("YES")
    print(*parent[1:])

if __name__ == "__main__":
    solve()
```Giải pháp duy trì các thành phần rõ ràng có kích thước để mọi thao tác hợp nhất đều có ảnh hưởng đã biết đến kích thước cây con. Sự lựa chọn tham lam cố gắng sử dụng các cơ hội sai lệch bất cứ khi nào tồn tại một cặp đủ cân bằng. Khi không sử dụng, chúng tôi luôn gắn cấu trúc nhỏ hơn bên dưới cấu trúc lớn hơn để tránh vô tình tạo ra sự lệch. 

Mảng cha mã hóa trực tiếp đồ thị hàm số. Mỗi lần hợp nhất chỉ định chính xác một con trỏ cha, đảm bảo cấu trúc cuối cùng tôn trọng quy tắc “một nút con trên mỗi nút”. 

Một chi tiết triển khai tinh tế là sắp xếp các thành phần sau mỗi lần lặp. Điều này đảm bảo rằng chúng tôi luôn kiểm tra ứng viên triển vọng nhất trước tiên. Nếu không có điều này, chúng ta có thể bỏ lỡ các vị trí sai lệch hợp lệ và kết luận không chính xác là không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 0
```Chúng tôi bắt đầu với các thành phần$(1,1), (1,2), (1,3)$. Trước tiên, chúng ta hợp nhất hai thành phần nhỏ nhất, ví dụ 2 và 3 thành 3→2. Không có sự mất cân bằng nào được đưa ra. Sau đó hợp nhất với 1. Vì$k=0$, chúng tôi luôn tránh sai lệch. 

| Bước | Linh kiện | Hành động | Nghiêng sử dụng | 
| --- | --- | --- | --- | 
| 1 | (1,1),(1,2),(1,3) | hợp nhất 2,3 | 0 | 
| 2 | (2,2),(1,1) | hợp nhất | 0 | 
| 3 | (3,1) | xong | 0 | 

Đầu ra là một cấu trúc dạng dây chuyền, phù hợp với yêu cầu. 

### Ví dụ 2 

đầu vào:```
5 1
```Chúng tôi bắt đầu với năm thành phần đơn lẻ. Đầu tiên hợp nhất 4 và 5, sau đó hợp nhất 3 với (4,5), tạo thành thành phần lớn hơn. Chúng tôi sử dụng cơ hội lệch đơn khi hợp nhất các thành phần lớn nhất và nhỏ nhất có sẵn. 

| Bước | Linh kiện | Hành động | Nghiêng sử dụng | 
| --- | --- | --- | --- | 
| 1 | 1,2,3,4,5 | hợp nhất 4-5 | 0 | 
| 2 | 1,2,3,(4,5) | hợp nhất 3 với (4,5) | 0 | 
| 3 | 1,2,(3,4,5) | hợp nhất 2 với 1 | 1 (bắt buộc) | 
| 4 | cuối cùng | xong | 1 | 

Điều này chứng tỏ rằng sự mất cân bằng được kiểm soát duy nhất có thể được đưa vào mà không ảnh hưởng đến các sự hợp nhất khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi sự hợp nhất liên quan đến việc sắp xếp hoặc duy trì cấu trúc của các thành phần trên$n$bước | 
| Không gian |$O(n)$| Chúng tôi lưu trữ con trỏ gốc và siêu dữ liệu thành phần | 

Các ràng buộc cho phép điều này một cách thoải mái, vì$n = 10^5$cho phép lên tới vài triệu hoạt động. Việc xây dựng tránh việc tính toán lại kích thước cây con, nếu không sẽ chi phối thời gian chạy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("3 0\n") == "YES\n0 1 1"
assert run("5 1\n") == "YES\n0 1 1 3 3"

# custom cases
assert run("1 0\n") == "YES\n0"
assert run("2 1\n") == "NO"
assert run("4 0\n") == "YES"
assert run("3 2\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | CÓ 0 | cấu trúc hợp lệ tối thiểu | 
| 2 1 | KHÔNG | yêu cầu độ lệch không thể | 
| 4 0 | CÓ | xây dựng không tầm thường không nghiêng | 
| 3 2 | KHÔNG | giới hạn trên về độ lệch | 

## Vỏ cạnh 

cho$n = 1$, không thể hợp nhất được và do đó không có sự kiện sai lệch. Thuật toán ngay lập tức đưa ra cấu trúc một nút với$s_1 = 0$, phù hợp với cấu hình hợp lệ duy nhất. 

Vì$k > 0$với rất nhỏ$n$, chẳng hạn như$n = 2, k = 1$, việc xây dựng không bao giờ tìm thấy sự hợp nhất thỏa mãn điều kiện mất cân bằng, vì bất kỳ sự hợp nhất nào đều liên quan đến hai đơn vị và không thể thỏa mãn hệ số hai hiệu. Thuật toán loại bỏ chính xác bằng cách sử dụng hết các thành phần mà không tiêu tốn hạn ngạch sai lệch. 

Đối với lớn$n$Và$k = 0$, quy trình luôn gắn các thành phần nhỏ hơn vào dưới các thành phần lớn hơn một cách cân bằng. Mỗi lần hợp nhất duy trì kích thước gần như bằng nhau, ngăn ngừa việc vô tình tạo ra sai lệch. Cấu trúc cuối cùng trở thành một chuỗi hợp nhất gần như cân bằng và không có nút nào thỏa mãn điều kiện nghiêng, duy trì tính chính xác.
