---
title: "CF 69C - Trò chơi"
description: "Mỗi người chơi sở hữu nhiều bộ hiện vật. Một số hiện vật là cơ bản và có thể được mua trực tiếp. Một số là hỗn hợp và có thể được chế tạo từ một số hiện vật cơ bản với số lượng cố định. Bất cứ khi nào người chơi mua một hiện vật cơ bản, nó sẽ được thêm vào kho của họ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 69
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 63 (Div. 2)"
rating: 2000
weight: 69
solve_time_s: 135
verified: true
draft: false
---

[CF 69C - Trò chơi](https://codeforces.com/problemset/problem/69/C) 

**Xếp hạng:** 2000 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi sở hữu nhiều bộ hiện vật. Một số hiện vật là cơ bản và có thể được mua trực tiếp. Một số là hỗn hợp và có thể được chế tạo từ một số hiện vật cơ bản với số lượng cố định. 

Bất cứ khi nào người chơi mua một hiện vật cơ bản, nó sẽ được thêm vào kho của họ. Ngay sau lần mua đó, bạn có thể chế tạo chính xác một hiện vật tổng hợp. Việc chế tạo tiêu tốn số lượng nguyên liệu cần thiết và thêm một bản sao của tạo tác tổng hợp. Tuyên bố đảm bảo rằng sau mỗi lần mua sẽ có nhiều nhất một tạo tác tổng hợp có thể chế tạo được, vì vậy chúng ta không bao giờ cần phải lựa chọn giữa nhiều công thức nấu ăn. 

Nhiệm vụ là mô phỏng tất cả các giao dịch mua và cuối cùng là in kho đồ của mọi người chơi. Chỉ những hiện vật có số lượng dương mới xuất hiện và tên phải được in theo thứ tự từ điển. 

Các ràng buộc đủ nhỏ để mô phỏng trực tiếp có thể thực hiện được. Có nhiều nhất 100 người chơi, nhiều nhất 50 công thức nấu ăn và nhiều nhất 500 lần mua. Ngay cả một giải pháp quét tất cả các công thức nấu ăn sau mỗi lần mua cũng chỉ thực hiện khoảng 500 × 50 = 25.000 lần kiểm tra công thức, một con số rất nhỏ. 

Khó khăn chính không phải là hiệu quả hoạt động mà là việc ghi sổ kế toán chính xác. Quá trình chế tạo sẽ loại bỏ các thành phần, vì vậy hàng tồn kho phải hoạt động giống như nhiều bộ thay vì các bộ đơn giản. Một điểm tinh tế khác là bản thân các hiện vật tổng hợp có thể tồn tại trong kho và cùng tồn tại với các hiện vật cơ bản. 

Một lỗi phổ biến là quên trừ nguyên liệu sau khi chế tạo. Hãy xem xét đầu vào này:```
1 2 1 2
wood
stone
axe: wood 1, stone 1
1 wood
1 stone
```Hàng tồn kho cuối cùng đúng là:```
1
axe 1
```Việc triển khai có lỗi có thể xuất ra cả hai`wood`,`stone`, Và`axe`, bởi vì nó chế tạo vật phẩm nhưng lại quên tiêu thụ nguyên liệu. 

Một sai lầm dễ mắc phải khác là chỉ kiểm tra công thức nấu ăn một lần trên toàn cầu thay vì sau mỗi lần mua cho người chơi tương ứng. Ví dụ:```
1 1 1 2
gem
crown: gem 2
1 gem
1 gem
```Lần mua đầu tiên không làm gì cả. Việc mua thứ hai tạo ra`crown`. Đầu ra đúng là:```
1
crown 1
```Nếu chúng tôi không kiểm tra lại sau mỗi lần mua, công thức sẽ không bao giờ được kích hoạt. 

Ngoài ra còn có một vấn đề phân tích cú pháp tinh tế. Các dòng công thức chứa dấu phẩy và dấu hai chấm ở định dạng văn bản dạng tự do:```
sword: iron 2, wood 1
```Một trình phân tích cú pháp dễ hỏng chỉ phân tách bằng dấu cách sẽ để lại dấu chấm câu kèm theo các tên như`"2,"`hoặc`"sword:"`. Cách tiếp cận an toàn nhất trước tiên là chia quanh dấu hai chấm, sau đó chia danh sách thành phần xung quanh dấu phẩy. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng đơn giản. 

Đối với mỗi lần mua, hãy thêm hiện vật đã mua vào kho của người chơi. Sau đó quét mọi công thức và kiểm tra xem người chơi có sở hữu đủ mọi nguyên liệu cần thiết hay không. Nếu một số công thức có thể chế tạo được, hãy trừ đi số lượng cần thiết và thêm một bản sao của tạo phẩm tổng hợp. 

Điều này đúng vì tuyên bố đảm bảo rằng sau mỗi lần mua, nhiều nhất một công thức có thể chế tạo được. Chúng ta không bao giờ phải đối mặt với những lựa chọn xung đột. 

Phương pháp vũ phu đã phù hợp một cách thoải mái trong giới hạn. Cho phép`q`là số lượng mua hàng,`m`số lượng công thức nấu ăn và`n`số lượng thành phần tối đa trong một công thức. Công việc trong trường hợp xấu nhất là đại khái`q × m × n`, nhiều nhất là`500 × 50 × 50 = 1,250,000`kiểm tra thành phần. 

Vì các ràng buộc rất nhỏ nên không cần xử lý biểu đồ nâng cao hoặc theo dõi phụ thuộc. Công việc thực sự nằm ở việc thực hiện mô phỏng một cách cẩn thận. 

Quan sát quan trọng là việc chế tạo chỉ phụ thuộc vào kho đồ hiện tại của một người chơi. Các giao dịch mua không bao giờ tương tác giữa những người chơi, vì vậy mỗi kho đồ có thể được lưu trữ độc lập. Sau khi chúng tôi thể hiện kho đồ của mỗi người chơi dưới dạng bản đồ tần suất, việc kiểm tra công thức sẽ trở thành so sánh trực tiếp giữa số lượng cần thiết với số lượng sở hữu. 

Giải pháp tối ưu về cơ bản là mô phỏng giống nhau nhưng được triển khai rõ ràng bằng bản đồ băm cho kho lưu trữ và cấu trúc công thức được phân tích cú pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q × m × n) | O(k × A) | Đã chấp nhận | 
| Tối ưu | O(q × m × n) | O(k × A) | Đã chấp nhận | 

Đây`A`là tổng số tên hiện vật riêng biệt. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các tên hiện vật cơ bản. 

Chúng ta thực sự không cần phải phân biệt các tạo phẩm cơ bản và tổng hợp trong quá trình mô phỏng, nhưng việc đọc các tên sẽ thiết lập nên một loạt các định danh tạo tác. 
2. Phân tích mọi công thức tổng hợp. 

Mỗi công thức chứa một thành phần tạo tác và danh sách các cặp đếm thành phần. Lưu trữ chúng dưới dạng:```
result_name -> list of (ingredient, required_count)
```Phân tích cú pháp cẩn thận là vấn đề quan trọng vì dấu phẩy và dấu hai chấm là một phần của định dạng đầu vào. 
3. Tạo một bản đồ kho đồ cho mỗi người chơi. 

Mỗi kho lưu trữ:```
artifact_name -> quantity
```4. Xử lý từng giao dịch mua hàng một. 

Để mua hàng`(player, artifact)`: 

- Tăng số lượng hiện vật đã mua của người chơi đó. 
- Quét tất cả các công thức nấu ăn. 
- Đối với mỗi công thức, hãy kiểm tra xem người chơi có sở hữu đủ mọi nguyên liệu hay không. 
- Nếu một công thức có thể chế tạo được: 

- Trừ đi tất cả số lượng thành phần. 
- Thêm một bản sao của tạo tác tổng hợp. 
- Dừng quét công thức nấu ăn. 

Chúng tôi dừng ngay lập tức vì tuyên bố đảm bảo rằng tối đa một công thức có thể có sẵn sau khi mua. 
5. Sau khi mua xong, hãy in kho đồ của mỗi người chơi. 

Chỉ nên in các hiện vật có số lượng dương. Đầu ra phải được sắp xếp theo từ điển theo tên tạo tác. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý mỗi lần mua, kho đồ của mọi người chơi đều khớp chính xác với quy tắc trò chơi. 

Khi giao dịch mua diễn ra, trước tiên chúng tôi sẽ thêm hiện vật đã mua. Sau đó, chúng tôi kiểm tra tất cả các công thức nấu ăn. Nếu một số công thức có thể chế tạo được, chúng tôi tiêu thụ chính xác các thành phần cần thiết và thêm chính xác một tạo phẩm thu được. Bởi vì tuyên bố đảm bảo tính duy nhất của công thức mới có sẵn nên việc dừng lại sau khi chế tạo thành công đầu tiên phù hợp với hành vi dự định của trò chơi. 

Vì hàng tồn kho được cập nhật ngay sau mỗi sự kiện nên trạng thái trước lần mua tiếp theo luôn chính xác. Bằng quy nạp đối với tất cả các lần mua hàng, lượng hàng tồn kho cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    k, n, m, q = map(int, input().split())

    basic = [input().strip() for _ in range(n)]

    recipes = []

    for _ in range(m):
        line = input().strip()

        result, rest = line.split(": ")

        ingredients = []

        parts = rest.split(", ")

        for part in parts:
            name, cnt = part.rsplit(" ", 1)
            ingredients.append((name, int(cnt)))

        recipes.append((result, ingredients))

    inventories = [defaultdict(int) for _ in range(k)]

    for _ in range(q):
        player, artifact = input().split()
        player = int(player) - 1

        inventories[player][artifact] += 1

        for result, ingredients in recipes:
            ok = True

            for name, need in ingredients:
                if inventories[player][name] < need:
                    ok = False
                    break

            if ok:
                for name, need in ingredients:
                    inventories[player][name] -= need

                inventories[player][result] += 1
                break

    out = []

    for inv in inventories:
        items = []

        for name in sorted(inv.keys()):
            if inv[name] > 0:
                items.append((name, inv[name]))

        out.append(str(len(items)))

        for name, cnt in items:
            out.append(f"{name} {cnt}")

    print("\n".join(out))

solve()
```Giải pháp tuân theo mô phỏng trực tiếp. 

Công thức nấu ăn được lưu trữ dưới dạng cặp tên kết quả và danh sách thành phần. Việc sử dụng phân tích cú pháp`split(": ")`đầu tiên là vì dấu hai chấm phân biệt hiện vật được chế tạo với yêu cầu của nó. Sau đó phần thành phần được chia cho`", "`để tách biệt từng yêu cầu. Cuối cùng,`rsplit(" ", 1)`tách tên thành phần khỏi số lượng của nó. sử dụng`rsplit`an toàn hơn vì tên tạo tác là các chuỗi tùy ý. 

Kho đồ của mỗi người chơi là một`defaultdict(int)`, tự động coi các thành phần bị thiếu là số lượng bằng 0. 

Sau mỗi lần mua, chúng tôi quét công thức nấu ăn theo thứ tự đầu vào. Đối với mỗi công thức, chúng tôi xác minh xem mọi thành phần đều có sẵn hay không. Nếu vậy, chúng tôi trừ số lượng cần thiết và thêm hiện vật được chế tạo. 

Một chi tiết triển khai tinh vi sẽ dừng ngay sau khi tạo. Tuyên bố đảm bảo rằng không có nhiều hơn một công thức có sẵn sau khi mua hàng. Việc tiếp tục quét có thể cho phép nhiều thủ công từ một sự kiện không chính xác. 

Khi in kết quả, chúng tôi lọc ra số 0 vì các thành phần được tiêu thụ trong quá trình chế tạo có thể vẫn còn là khóa trong từ điển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 2 5
desolator
refresher
perseverance
vanguard: desolator 1, refresher 1
maelstorm: perseverance 2
1 desolator
2 perseverance
1 refresher
2 desolator
2 perseverance
```### Dấu vết 

| Bước | Người chơi | Mua hàng | Hàng tồn kho trước khi chế tạo | Chế tác | Hàng tồn kho Sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | kẻ tàn phá | kẻ phá hoại=1 | không | kẻ phá hoại=1 | 
| 2 | 2 | sự kiên trì | sự kiên trì=1 | không | sự kiên trì=1 | 
| 3 | 1 | bồi dưỡng | desolator=1, bồi dưỡng=1 | tiên phong | tiên phong=1 | 
| 4 | 2 | kẻ tàn phá | sự kiên trì=1, kẻ tàn phá=1 | không | sự kiên trì=1, kẻ tàn phá=1 | 
| 5 | 2 | sự kiên trì | sự kiên trì=2, kẻ tàn phá=1 | cơn bão | cơn bão=1, kẻ tàn phá=1 | 

Đầu ra cuối cùng:```
1
vanguard 1
2
desolator 1
maelstorm 1
```Ví dụ này cho thấy mức tiêu thụ thành phần. Sau khi chế tạo`vanguard`, cả hai`desolator`Và`refresher`biến mất khỏi kho của người chơi 1. 

### Ví dụ 2 

đầu vào:```
1 2 1 4
wood
stone
axe: wood 2, stone 1
1 wood
1 stone
1 wood
1 wood
```### Dấu vết 

| Bước | Mua hàng | Hàng tồn kho trước khi chế tạo | Chế tác | Hàng tồn kho Sau | 
| --- | --- | --- | --- | --- | 
| 1 | gỗ | gỗ=1 | không | gỗ=1 | 
| 2 | đá | gỗ=1, đá=1 | không | gỗ=1, đá=1 | 
| 3 | gỗ | gỗ=2, đá=1 | rìu | rìu=1 | 
| 4 | gỗ | rìu=1, gỗ=1 | không | rìu=1, gỗ=1 | 

Đầu ra cuối cùng:```
2
axe 1
wood 1
```Dấu vết này cho thấy rằng quá trình chế tạo diễn ra ngay lập tức khi các yêu cầu được đáp ứng và chỉ tiêu thụ đúng số lượng cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q × m × n) | Mỗi lần mua sẽ quét tất cả công thức nấu ăn và tất cả thành phần công thức | 
| Không gian | O(k × A) | Mỗi người chơi lưu trữ số lượng hiện vật thuộc sở hữu | 

Công việc tối đa là khoảng 1,25 triệu lượt kiểm tra thành phần, rất nhỏ đối với Python trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng rất nhỏ vì tổng số tên tạo tác riêng biệt nhiều nhất là 100. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out_capture = io.StringIO()

    k, n, m, q = map(int, input().split())

    basic = [input().strip() for _ in range(n)]

    recipes = []

    for _ in range(m):
        line = input().strip()

        result, rest = line.split(": ")

        ingredients = []

        for part in rest.split(", "):
            name, cnt = part.rsplit(" ", 1)
            ingredients.append((name, int(cnt)))

        recipes.append((result, ingredients))

    inventories = [defaultdict(int) for _ in range(k)]

    for _ in range(q):
        player, artifact = input().split()
        player = int(player) - 1

        inventories[player][artifact] += 1

        for result, ingredients in recipes:
            ok = True

            for name, need in ingredients:
                if inventories[player][name] < need:
                    ok = False
                    break

            if ok:
                for name, need in ingredients:
                    inventories[player][name] -= need

                inventories[player][result] += 1
                break

    ans = []

    for inv in inventories:
        items = []

        for name in sorted(inv.keys()):
            if inv[name] > 0:
                items.append((name, inv[name]))

        ans.append(str(len(items)))

        for name, cnt in items:
            ans.append(f"{name} {cnt}")

    return "\n".join(ans)

# provided sample
assert run(
"""2 3 2 5
desolator
refresher
perseverance
vanguard: desolator 1, refresher 1
maelstorm: perseverance 2
1 desolator
2 perseverance
1 refresher
2 desolator
2 perseverance
"""
) == (
"""1
vanguard 1
2
desolator 1
maelstorm 1"""
), "sample 1"

# minimum input
assert run(
"""1 1 0 1
a
1 a
"""
) == (
"""1
a 1"""
), "minimum case"

# repeated crafting
assert run(
"""1 1 1 4
gem
crown: gem 2
1 gem
1 gem
1 gem
1 gem
"""
) == (
"""1
crown 2"""
), "multiple crafts over time"

# leftover ingredients
assert run(
"""1 2 1 3
wood
stone
axe: wood 2, stone 1
1 wood
1 stone
1 wood
"""
) == (
"""1
axe 1"""
), "exact consumption"

# independent player inventories
assert run(
"""2 1 1 2
gem
crown: gem 2
1 gem
2 gem
"""
) == (
"""1
gem 1
1
gem 1"""
), "players separated"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào tối thiểu | Hiện vật duy nhất thuộc sở hữu | Khởi tạo cơ sở | 
| Chế tạo lặp đi lặp lại |`crown 2`| Việc chế tạo có thể diễn ra nhiều lần trong quá trình mua hàng | 
| Thành phần còn sót lại | Chỉ một`axe 1`| Thành phần được tiêu thụ chính xác | 
| Hàng tồn kho độc lập | Mỗi người chơi giữ viên ngọc riêng | Không có trạng thái chia sẻ ngẫu nhiên | 

## Vỏ cạnh 

Hãy xem xét trường hợp chế tạo tiêu thụ chính xác tất cả các thành phần:```
1 2 1 2
wood
stone
axe: wood 1, stone 1
1 wood
1 stone
```Sau lần mua thứ hai, hàng tồn kho sẽ trở thành:```
wood=1, stone=1
```Công thức có thể chế tạo được nên cả hai số đếm đều giảm xuống 0 và`axe=1`được thêm vào. Trong quá trình xuất, các mục không đếm được sẽ bị bỏ qua. Kết quả cuối cùng là:```
1
axe 1
```Bây giờ hãy xem xét việc chế tạo bị trì hoãn:```
1 1 1 3
gem
crown: gem 2
1 gem
1 gem
1 gem
```Sau lần mua đầu tiên, việc chế tạo là không thể. Sau lần thứ hai, hàng tồn kho đạt`gem=2`, Vì thế`crown`được chế tạo và cả hai viên đá quý đều biến mất. Sau lần mua thứ ba, hàng tồn kho sẽ trở thành:```
crown=1, gem=1
```Thuật toán kiểm tra công thức nấu ăn sau mỗi lần mua, do đó thời gian được xử lý chính xác. 

Cuối cùng, hãy xem xét nhiều người chơi:```
2 1 1 2
gem
crown: gem 2
1 gem
2 gem
```Người chơi 1 sở hữu một viên ngọc và người chơi 2 sở hữu một viên ngọc. Cũng không thể thủ công`crown`, bởi vì hàng tồn kho là độc lập. Thuật toán lưu trữ một từ điển riêng cho mỗi người chơi, do đó số lượng người chơi không bao giờ bị trộn lẫn.
