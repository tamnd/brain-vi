---
title: "CF 75B - Tường ưu tiên Facetook"
description: "Chúng tôi được cung cấp tên của người dùng trên mạng xã hội và một chuỗi tin nhắn hoạt động giữa những người dùng. Mỗi hoạt động đều đóng góp một số điểm nhất định giữa hai người tham gia."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 75
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 67 (Div. 2)"
rating: 1500
weight: 75
solve_time_s: 134
verified: false
draft: false
---

[CF 75B - Tường ưu tiên Facetook](https://codeforces.com/problemset/problem/75/B) 

**Đánh giá:** 1500 
**Thẻ:** phân tích cú pháp biểu thức, triển khai, chuỗi 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tên của người dùng trên mạng xã hội và một chuỗi tin nhắn hoạt động giữa những người dùng. Mỗi hoạt động đều đóng góp một số điểm nhất định giữa hai người tham gia. 

Ba hoạt động có thể là: 

- đăng lên tường của ai đó, trị giá 15 điểm 
- bình luận bài viết của ai đó, trị giá 10 điểm 
- thích bài viết của ai đó, trị giá 5 điểm 

Điểm số mối quan hệ là đối xứng. Nếu như`alice`bình luận về`bob`bài đăng của cả hai`alice`Và`bob`giành được 10 điểm cho nhau. 

Nhiệm vụ của chúng tôi không phải là tính điểm giữa mỗi cặp người dùng. Chúng tôi chỉ quan tâm đến điểm số liên quan đến người dùng nhất định. Sau khi xử lý tất cả các hành động, chúng tôi phải in từng người riêng biệt xuất hiện trong nhật ký, ngoại trừ người dùng chính, được sắp xếp theo điểm giảm dần với người dùng đó. Nếu nhiều người có cùng số điểm, chúng tôi sẽ phá vỡ mối quan hệ theo từ điển. 

Định dạng đầu vào thoạt nhìn có vẻ đơn giản nhưng các chuỗi hơi phức tạp. Tên người dùng thứ hai xuất hiện với`"'s"`kèm theo, chẳng hạn như`fatma's`. Trình phân tích cú pháp bất cẩn lưu trữ trực tiếp mã thông báo này sẽ xử lý`"fatma"`Và`"fatma's"`như những cái tên khác nhau. 

Những hạn chế là rất nhỏ. Có tối đa 100 hành động và mỗi tên người dùng có độ dài tối đa là 10. Ngay cả việc triển khai không hiệu quả cũng có thể trôi qua một cách thoải mái. Điều này có nghĩa là vấn đề chủ yếu nằm ở việc phân tích chuỗi và ghi sổ kế toán cẩn thận chứ không phải ở việc tối ưu hóa. 

Một lỗi phổ biến là quên bao gồm những người dùng có điểm với người dùng chính bằng 0. 

Hãy xem xét đầu vào này:```
ahmed
2
mona likes sara's post
sara commented on mona's post
```Không có tương tác nào liên quan đến`ahmed`, vậy cả hai`mona`Và`sara`có điểm 0 với anh ta. Đầu ra đúng là:```
mona
sara
```Một lỗi dễ xảy ra khác là việc phân tích sai tên người dùng thứ hai.```
ahmed
1
ahmed posted on fatma's wall
```Tên người dùng chính xác là`fatma`, không`fatma's`. Nếu chúng ta quên loại bỏ hậu tố, việc sắp xếp và tích lũy điểm sẽ sai vì`"fatma"`Và`"fatma's"`được coi là những người dùng riêng biệt. 

Hòa giải cũng có vấn đề.```
ahmed
2
ahmed likes sara's post
ahmed likes mona's post
```Cả hai người dùng đều nhận được 5 điểm, vì vậy kết quả đầu ra phải là:```
mona
sara
```Một giải pháp chỉ sắp xếp theo điểm giảm dần sẽ tạo ra thứ tự không xác định. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng mọi hành động chính xác như được viết. Đối với mỗi dòng, chúng tôi trích xuất hai tên người dùng và xác định hoạt động nào đã xảy ra. Nếu người dùng chính tham gia tương tác, chúng tôi sẽ cộng giá trị tương ứng vào điểm của người khác. Trong khi đó, chúng tôi duy trì một tập hợp mọi tên người dùng riêng biệt xuất hiện. 

Chiến lược bạo lực này đã hoàn toàn được chấp nhận vì kích thước đầu vào rất nhỏ. Với tối đa 100 dòng, việc quét liên tục các chuỗi và sắp xếp tên gần như không tốn kém gì. Toàn bộ công việc bị chi phối bởi việc sắp xếp tối đa vài trăm tên. 

Thử thách thực sự không phải là hiệu suất mà là sự đại diện. Các dòng hoạt động có ba cấu trúc câu khác nhau:```
X posted on Y's wall
X commented on Y's post
X likes Y's post
```Quan sát hữu ích là tất cả các định dạng đều trở nên dễ dàng khi chúng ta chia dòng thành các từ. Mã thông báo đầu tiên luôn là`X`, mã thông báo thứ hai xác định loại hành động và mã thông báo thứ tư chứa`Y's`. Xóa hai ký tự cuối cùng sẽ cung cấp tên người dùng thực tế. 

Sau khi trích xuất tên người dùng và loại hành động, vấn đề sẽ giảm xuống mức tích lũy điểm trong từ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

Giải pháp "tối ưu" về cơ bản là giống nhau vì các ràng buộc vốn đã rất nhỏ. Phần quan trọng là tổ chức phân tích cú pháp rõ ràng và sắp xếp chính xác. 

## Hướng dẫn thuật toán 

1. Đọc tên người dùng chính và số lượng hành động. 
2. Tạo một từ điển ánh xạ từng từ hành động theo điểm của nó. 

Chúng tôi sử dụng: 

-`"posted"`→ 15 
-`"commented"`→ 10 
-`"likes"`→ 5 
3. Duy trì một bộ chứa mọi tên người dùng riêng biệt ngoại trừ người dùng chính. 

Điều này đảm bảo rằng ngay cả những người dùng có điểm bằng 0 cũng xuất hiện trong kết quả cuối cùng. 
4. Duy trì điểm lưu trữ từ điển khác liên quan đến người dùng chính. 

Các mục bị thiếu được coi là bằng không. 
5. Đối với từng dòng hành động: 

1. Chia dòng thành các từ. 
2. Trích xuất: 

-`x = words[0]`-`action = words[1]`-`y = words[3][:-2]`Thao tác cuối cùng sẽ xóa dấu vết`"'s"`. 

1. Thêm cả hai tên người dùng vào nhóm nếu họ không phải là người dùng chính. 
2. Tra cứu giá trị điểm của hành động. 
3. Nếu`x`là người dùng chính, hãy thêm điểm vào`y`. 
4. Nếu`y`là người dùng chính, hãy thêm điểm vào`x`. 

Bởi vì mối quan hệ là đối xứng nên cả hai hướng đều đóng góp như nhau. 
6. Chuyển đổi nhóm tên người dùng thành một danh sách và sắp xếp nó bằng cách sử dụng: 

- điểm giảm dần 
- thứ tự từ điển như một sự ràng buộc 
7. In tên đã sắp xếp. 

### Tại sao nó hoạt động 

Thuật toán xử lý mọi tương tác chính xác một lần. Đối với mỗi hành động, điểm số duy nhất quan trọng là điểm liên quan đến người dùng chính. Nếu không người tham gia nào là người dùng chính thì sự tương tác không thể ảnh hưởng đến xếp hạng cuối cùng, vì vậy chúng tôi bỏ qua phần đóng góp điểm số của nó trong khi vẫn ghi lại tên người dùng. 

Từ điển điểm luôn lưu trữ tổng giá trị tương tác tích lũy giữa người dùng chính và mọi người dùng khác được xử lý cho đến nay. Vì mỗi hành động đóng góp độc lập và chính xác một lần nên từ điển cuối cùng chứa tổng số chính xác. Quy tắc sắp xếp cuối cùng phù hợp trực tiếp với tuyên bố: điểm cao hơn trước, thứ tự bảng chữ cái cho các mối quan hệ. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    me = input().strip()
    n = int(input())

    points = {
        "posted": 15,
        "commented": 10,
        "likes": 5
    }

    score = defaultdict(int)
    people = set()

    for _ in range(n):
        words = input().split()

        x = words[0]
        action = words[1]
        y = words[3][:-2]

        if x != me:
            people.add(x)

        if y != me:
            people.add(y)

        value = points[action]

        if x == me:
            score[y] += value

        if y == me:
            score[x] += value

    result = sorted(people, key=lambda name: (-score[name], name))

    print("\n".join(result))

solve()
```các`points`từ điển chuyển đổi các từ hành động thành giá trị số. Điều này tránh các câu lệnh có điều kiện lặp đi lặp lại và làm cho mã dễ mở rộng hơn. 

Trình phân tích cú pháp dựa vào cấu trúc câu cố định. Sau khi phân tách bằng dấu cách, tên người dùng thứ hai luôn xuất hiện ở vị trí`3`có đính kèm`"'s"`. Cắt lát bằng`[:-2]`loại bỏ chính xác hai ký tự đó. 

các`people`bộ này tách biệt với`score`từ điển vì một số người dùng có thể không bao giờ tương tác trực tiếp với người dùng chính. Nếu chúng ta chỉ dựa vào các khóa trong`score`, người dùng có điểm 0 sẽ biến mất khỏi đầu ra. 

sử dụng`defaultdict(int)`loại bỏ nhu cầu khởi tạo điểm còn thiếu theo cách thủ công. 

Phím sắp xếp rất tinh tế. Chúng tôi sắp xếp theo`(-score[name], name)`thay vì sắp xếp hai lần. Việc phủ định điểm sẽ đưa ra thứ tự số giảm dần, trong khi thành phần thứ hai tự động xử lý các mối quan hệ từ điển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ahmed
3
ahmed posted on fatma's wall
fatma commented on ahmed's post
mona likes ahmed's post
```Xử lý dấu vết: 

| Bước | Hành động | Người dùng được trích xuất | Thay đổi Điểm | Điểm hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | ahmed đăng trên tường của Fatma | ahmed, béo | béo +15 | béo: 15 | 
| 2 | fatma đã bình luận về bài viết của ahmed | bà béo, ahmed | béo +10 | béo: 25 | 
| 3 | mona thích bài viết của ahmed | mona, ahmed | mona +5 | Fatma: 25, mona: 5 | 

Phân loại cuối cùng: 

| Tên | Điểm | 
| --- | --- | 
| béo | 25 | 
| mona | 5 | 

Đầu ra:```
fatma
mona
```Ví dụ này cho thấy các tương tác đóng góp bất kể hướng nào. Cả hai hành động liên quan đến`fatma`tăng điểm mối quan hệ tương tự. 

### Ví dụ 2 

đầu vào:```
alex
4
bob likes clara's post
alex likes diana's post
eric commented on frank's post
george posted on alex's wall
```Xử lý dấu vết: 

| Bước | Hành động | Người dùng được trích xuất | Thay đổi Điểm | Điểm hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | bob thích bài viết của clara | bob, clara | không | trống | 
| 2 | alex thích bài viết của diana | alex, diana | Diana +5 | Diana: 5 | 
| 3 | eric đã bình luận về bài viết của Frank | eric, thẳng thắn | không | Diana: 5 | 
| 4 | George đăng trên tường của Alex | George, Alex | George +15 | Diana: 5, George: 15 | 

Tất cả người dùng được phát hiện: 

| Người dùng | Điểm | 
| --- | --- | 
| bob | 0 | 
| clara | 0 | 
| diana | 5 | 
| eric | 0 | 
| thẳng thắn | 0 | 
| George | 15 | 

Đầu ra được sắp xếp:```
george
diana
bob
clara
eric
frank
```Dấu vết này chứng minh lý do tại sao chúng ta phải lưu trữ từng tên người dùng được nhìn thấy riêng biệt khỏi từ điển điểm số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Hành động xử lý là O(n), việc sắp xếp tên người dùng chiếm ưu thế | 
| Không gian | O(n) | Từ điển và bộ lưu trữ cho hầu hết tất cả người dùng riêng biệt | 

Với tối đa 100 hành động, giải pháp này dễ dàng nằm trong giới hạn. Ngay cả bước sắp xếp cũng rất nhỏ vì số lượng tên người dùng riêng biệt rất ít. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        me = input().strip()
        n = int(input())

        points = {
            "posted": 15,
            "commented": 10,
            "likes": 5
        }

        score = defaultdict(int)
        people = set()

        for _ in range(n):
            words = input().split()

            x = words[0]
            action = words[1]
            y = words[3][:-2]

            if x != me:
                people.add(x)

            if y != me:
                people.add(y)

            value = points[action]

            if x == me:
                score[y] += value

            if y == me:
                score[x] += value

        result = sorted(people, key=lambda name: (-score[name], name))

        return "\n".join(result)

    return solve()

# provided sample
assert run(
"""ahmed
3
ahmed posted on fatma's wall
fatma commented on ahmed's post
mona likes ahmed's post
"""
) == "fatma\nmona", "sample 1"

# minimum input
assert run(
"""alex
1
bob likes clara's post
"""
) == "bob\nclara", "minimum size with zero scores"

# tie breaking
assert run(
"""alex
2
alex likes sara's post
alex likes mona's post
"""
) == "mona\nsara", "lexicographical tie breaking"

# repeated interactions
assert run(
"""alex
3
alex posted on bob's wall
bob commented on alex's post
alex likes bob's post
"""
) == "bob", "score accumulation"

# users unrelated to main user
assert run(
"""alex
4
bob likes clara's post
diana likes eric's post
frank commented on george's post
harry posted on alex's wall
"""
) == "harry\nbob\nclara\ndiana\neric\nfrank\ngeorge", "include zero-score users"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tương tác đơn lẻ không liên quan |`bob clara`đặt hàng | Người dùng có điểm 0 vẫn phải xuất hiện | 
| Điểm bằng nhau | Thứ tự từ điển | Thắt dây đúng cách | 
| Nhiều tương tác với cùng một người dùng | Tổng tích lũy đơn | Tổng hợp điểm | 
| Người dùng hỗn hợp có liên quan và không liên quan | Điểm tích cực đầu tiên | Sắp xếp và đưa vào đúng cách | 

## Vỏ cạnh 

### Người dùng không có điểm tương tác 

đầu vào:```
ahmed
2
mona likes sara's post
sara commented on mona's post
```Không hành động nào liên quan đến`ahmed`, do đó không có cập nhật điểm nào xảy ra. 

Thuật toán vẫn chèn cả`mona`Và`sara`vào`people`bộ. Điểm số của họ vẫn là giá trị mặc định`0`. 

Sắp xếp mang lại:```
mona
sara
```Một giải pháp chỉ lưu trữ người dùng bên trong từ điển điểm số sẽ không in sai gì cả. 

### Phân tích tên kết thúc bằng "'s" 

đầu vào:```
ahmed
1
ahmed posted on fatma's wall
```Sau khi tách: 

| Chỉ mục | Giá trị | 
| --- | --- | 
| 0 | ahmed | 
| 1 | đã đăng | 
| 2 | trên | 
| 3 | của Fatma | 
| 4 | bức tường | 

Thuật toán sử dụng`words[3][:-2]`, sản xuất`fatma`. 

Trạng thái điểm: 

| Người dùng | Điểm | 
| --- | --- | 
| béo | 15 | 

Đầu ra:```
fatma
```Nếu không loại bỏ hậu tố, tên được lưu trữ sẽ trở thành không chính xác`fatma's`. 

### Phá vỡ theo thứ tự từ điển 

đầu vào:```
alex
2
alex likes sara's post
alex likes mona's post
```Cả hai người dùng đều nhận được chính xác 5 điểm. 

Sắp xếp các giá trị khóa: 

| Người dùng | Chìa khóa | 
| --- | --- | 
| mona | (-5, "mona") | 
| Sara | (-5, "sara") | 

Vì các phần số bằng nhau nên việc so sánh chuỗi thông thường sẽ xác định thứ tự. 

Đầu ra đúng:```
mona
sara
```Việc sắp xếp giải pháp chỉ theo điểm có thể tạo ra thứ tự sai tùy thuộc vào thứ tự chèn.
