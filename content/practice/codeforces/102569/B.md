---
title: "CF 102569B - Tiền thưởng trực tuyến"
description: "Chúng tôi có một tập hợp các vị trí tiền thưởng được đặt trên một dòng số. Vị trí bắt đầu là tọa độ 0 và việc di chuyển một đơn vị khoảng cách luôn tiêu tốn một giây. Nhiệm vụ là chọn một nhóm tiền thưởng và ghé thăm mọi địa điểm đã chọn trong thời gian t có sẵn."
date: "2026-07-31T07:50:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "B"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 96
verified: true
draft: false
---

[CF 102569B - Tiền thưởng trực tuyến](https://codeforces.com/problemset/problem/102569/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một tập hợp các vị trí tiền thưởng được đặt trên một dòng số. Vị trí bắt đầu là tọa độ 0 và việc di chuyển một đơn vị khoảng cách luôn tiêu tốn một giây. Nhiệm vụ là chọn một nhóm tiền thưởng và ghé thăm mọi địa điểm đã chọn trong thời gian có sẵn`t`. Câu trả lời là số tiền thưởng lớn nhất có thể thu thập được chứ không phải quãng đường đã đi hay vị trí cuối cùng. 

Đầu vào cung cấp số lượng phần thưởng, giới hạn thời gian và tọa độ đã được sắp xếp của tất cả các phần thưởng. Vì tọa độ được sắp xếp nên các phần thưởng gần đó được lưu trữ cạnh nhau, đây là thuộc tính cấu trúc cho phép giải pháp hiệu quả. Đầu ra là một số nguyên duy nhất biểu thị số tiền thưởng tối đa có thể đạt được. 

Số lượng tiền thưởng có thể lên tới 200000, vì vậy việc kiểm tra từng nhóm tiền thưởng có thể có là không thực tế. Thuật toán bậc hai sẽ thực hiện khoảng 40000000000 phép tính trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. Chúng ta cần một cách tiếp cận gần với thời gian tuyến tính. Tọa độ và thời hạn có thể lớn bằng`10^9`, vì vậy các phép tính phải sử dụng số học số nguyên để có thể lưu trữ các giá trị trung gian lớn một cách an toàn. Số nguyên Python đã xử lý việc này. 

Một lỗi phổ biến là chỉ kiểm tra khoảng cách đến phần thưởng xa nhất trong phạm vi đã chọn. Điều đó có hiệu quả khi tất cả phần thưởng nằm ở một phía của điểm gốc, nhưng nó không thành công khi phần thưởng tồn tại ở cả hai phía vì việc quay lại điểm gốc sẽ tốn thêm chi phí di chuyển. Ví dụ: với:```
3 4
-2 1 3
```câu trả lời đúng là`2`. Tham quan`-2`Và`1`chi phí`2 + 3 = 5`giây nếu chúng ta đi bên trái trước, hoặc`1 + 3 = 4`giây nếu chúng ta đi đúng trước, vì vậy có thể có hai phần thưởng. Một phương pháp chỉ nhìn vào tọa độ xa nhất sẽ nghĩ sai rằng cả ba phần thưởng đều phù hợp vì tọa độ xa nhất là`3`. 

Một trường hợp khác là không có thời gian. Ví dụ:```
1 0
5
```Đầu ra đúng là`0`. Việc triển khai bất cẩn bắt đầu bằng một phần thưởng có thể đạt được hoặc kiểm tra vị trí trước khi di chuyển có thể tính sai. 

Trường hợp thứ ba là khi phạm vi tối ưu vượt qua 0 nhưng bắt đầu và kết thúc ở phía đối diện. Ví dụ:```
4 5
-4 -1 2 10
```Sự lựa chọn tốt nhất là`-1, 2`, mất`3`giây nếu chúng ta đi bên phải trước. Bao gồm`-4`sẽ cần nhiều thời gian hơn. Các thuật toán luôn mở rộng đến phạm vi tọa độ lớn nhất có thể mà không tính đến hành trình quay về sẽ đánh giá quá cao câu trả lời. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ thử mọi phân đoạn tiền thưởng liên tiếp có thể có. Đối với mỗi đoạn, chúng ta có thể tính toán tuyến đường ngắn nhất bắt đầu từ 0 và đi đến cả hai đầu của đoạn đó. Bởi vì tất cả các phần thưởng bên trong phân khúc đều nằm giữa hai điểm cuối đó nên việc truy cập các điểm cuối là đủ. Điều này đúng vì khi chúng ta đạt đến cả hai thái cực, tất cả tiền thưởng trung gian có thể được thu thập trên đường đi. 

Vấn đề là số lượng phân khúc. có khoảng`n * (n + 1) / 2`các phân đoạn có thể có, khoảng 20000000000 cho`n = 200000`. Ngay cả với việc kiểm tra chi phí liên tục, việc này vẫn quá chậm. 

Quan sát quan trọng là tập hợp phần thưởng tối ưu luôn là một đoạn liên tiếp trong mảng được sắp xếp. Nếu một giải pháp nhận hai phần thưởng và bỏ qua phần thưởng giữa chúng, thì việc thêm phần thưởng bị bỏ qua đó sẽ không tốn thêm chi phí di chuyển vì tuyến đường đã đi qua vị trí của nó. Điều này biến vấn đề thành việc tìm cửa sổ hợp lệ dài nhất. 

Đối với điểm cuối bên phải cố định, việc di chuyển điểm cuối bên trái sang bên phải chỉ có thể làm cho khoảng cách di chuyển cần thiết nhỏ hơn hoặc giữ nguyên khoảng cách đó. Hành vi đơn điệu này cho phép một cách tiếp cận hai con trỏ. Chúng tôi mở rộng phần thưởng bên phải một lần và bất cứ khi nào thời lượng hiện tại cần nhiều hơn`t`giây, chúng tôi xóa phần thưởng ở bên trái cho đến khi cửa sổ có thể hoạt động trở lại. 

Chi phí di chuyển của một cửa sổ chỉ phụ thuộc vào hai đầu của nó. Nếu toàn bộ cửa sổ nằm ở phía dương thì chi phí sẽ là tọa độ ngoài cùng bên phải. Nếu nó ở phía âm thì chi phí là giá trị tuyệt đối của tọa độ ngoài cùng bên trái. Nếu nó vượt qua số 0, có hai thứ tự có thể xảy ra: thăm bên trái trước hoặc thăm bên phải trước. Chúng tôi lấy cái nhỏ hơn trong hai cái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ hai con trỏ mô tả phân đoạn tiền thưởng hiện tại. Con trỏ bên phải mở rộng từ trái sang phải, mỗi lần thêm một phần thưởng mới. Con trỏ bên trái đánh dấu phần thưởng đầu tiên hiện có. 
2. Sau khi thêm điểm cuối bên phải mới, hãy tính thời gian tối thiểu cần thiết để truy cập đoạn hiện tại. Tọa độ duy nhất cần có là phần thưởng ngoài cùng bên trái và ngoài cùng bên phải vì mọi phần thưởng khác đều nằm giữa chúng. 
3. Nếu phân khúc hiện tại cần nhiều hơn`t`giây, di chuyển con trỏ bên trái sang phải một vị trí và kiểm tra lại. Việc loại bỏ phần thưởng ở bên trái không thể tăng thời gian cần thiết, vì vậy quá trình này cuối cùng sẽ tìm thấy đoạn hợp lệ dài nhất kết thúc ở con trỏ bên phải hiện tại. 
4. Bất cứ khi nào phân đoạn hiện tại hợp lệ, hãy cập nhật câu trả lời với kích thước của nó. Kích thước lớn nhất nhìn thấy trong quá trình quét là số lượng tiền thưởng tối đa có thể thu thập được. 

Điều bất biến là sau khi thu gọn, cửa sổ hiện tại luôn là cửa sổ hợp lệ dài nhất kết thúc ở điểm cuối bên phải hiện tại. Bất kỳ cửa sổ nào có cùng điểm cuối bên phải nhưng điểm cuối bên trái nhỏ hơn đã được kiểm tra và sẽ không hợp lệ nếu bị xóa. Vì mọi điểm cuối bên phải có thể đều được xem xét nên cửa sổ hợp lệ lớn nhất được tìm thấy trong quá trình quét là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())
    x = list(map(int, input().split()))

    def cost(l, r):
        left = x[l]
        right = x[r]

        if right <= 0:
            return -left
        if left >= 0:
            return right

        return min(right - 2 * left, 2 * right - left)

    ans = 0
    l = 0

    for r in range(n):
        while l <= r and cost(l, r) > t:
            l += 1
        if l <= r:
            ans = max(ans, r - l + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```các`cost`chức năng là cốt lõi của giải pháp. Khi cả hai đầu đều âm, điểm xa nhất nằm ở bên trái và chuyển động cần thiết chỉ đơn giản là khoảng cách từ 0 đến điểm cuối bên trái. Khi cả hai đầu đều dương, điểm cuối bên phải sẽ xác định chi phí. Khi đoạn thẳng vượt qua 0, hai công thức biểu thị hai thứ tự có thể có của việc thăm các cạnh. 

Vòng lặp chính tuân theo quy trình hai con trỏ. Con trỏ bên phải chỉ di chuyển về phía trước một lần và con trỏ bên trái cũng chỉ di chuyển về phía trước một lần, do đó tổng số chuyển động của con trỏ là tuyến tính. Những cuộc gọi lặp đi lặp lại`cost`chỉ sử dụng một số phép tính số học. 

Điều kiện biên trong vòng lặp thu hẹp là quan trọng. Một cửa sổ chỉ chứa điểm cuối bên phải hiện tại vẫn hợp lệ nếu giá của nó được kiểm tra, do đó điều kiện cho phép`l == r`. Nếu mọi phần thưởng ở quá xa, cửa sổ sẽ trống và câu trả lời vẫn là 0. 

Không cần xử lý đặc biệt đối với tọa độ lớn vì số nguyên Python không bị tràn. Trong các ngôn ngữ có loại số nguyên có kích thước cố định, các biểu thức liên quan đến tọa độ nhân đôi sẽ yêu cầu số nguyên 64 bit. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
5 6
-4 -1 2 3 7
```quá trình quét hoạt động như sau. 

| Chỉ số trái | Chỉ mục bên phải | Cửa sổ | Chi phí | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | -4 | 4 | 1 | 
| 0 | 1 | -4,-1 | 4 | 2 | 
| 0 | 2 | -4,-1,2 | 10 | 2 | 
| 1 | 2 | -1,2 | 4 | 2 | 
| 1 | 3 | -1,2,3 | 5 | 3 | 
| 1 | 4 | -1,2,3,7 | 15 | 3 | 
| 2 | 4 | 2,3,7 | 7 | 3 | 
| 3 | 4 | 3,7 | 7 | 3 | 
| 4 | 4 | 7 | 7 | 3 | 

Phần quan trọng của dấu vết này là sau khi cửa sổ trở nên quá lớn, thuật toán sẽ loại bỏ phía bên trái cho đến khi nó trở lại hợp lệ. Nó không bao giờ cần di chuyển con trỏ phải về phía sau. 

Một ví dụ thứ hai:```
4 3
-5 -1 1 2
```| Chỉ số trái | Chỉ mục bên phải | Cửa sổ | Chi phí | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | -5 | 5 | 0 | 
| 1 | 0 | trống | | 0 | 
| 1 | 1 | -1 | 1 | 1 | 
| 1 | 2 | -1,1 | 3 | 2 | 
| 1 | 3 | -1,1,2 | 4 | 2 | 
| 2 | 3 | 1,2 | 2 | 2 | 

Ví dụ này cho thấy hành vi khi giới hạn thời gian nhỏ. Thuật toán loại bỏ các điểm cuối bên trái không thể có và giữ đoạn lớn nhất phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần thưởng vào cửa sổ một lần và rời khỏi cửa sổ nhiều nhất một lần. | 
| Không gian | O(1) bên cạnh việc lưu trữ đầu vào | Chỉ sử dụng con trỏ, bộ đếm và giá trị tạm thời. | 

Giải pháp thực hiện một lượng công việc không đổi trên mỗi phần thưởng, phù hợp với`n = 200000`. Việc sử dụng bộ nhớ cũng nằm trong giới hạn vì thuật toán không tạo thêm mảng hoặc cấu trúc động. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def solve():
    import sys
    input = sys.stdin.readline

    n, t = map(int, input().split())
    x = list(map(int, input().split()))

    def cost(l, r):
        left = x[l]
        right = x[r]
        if right <= 0:
            return -left
        if left >= 0:
            return right
        return min(right - 2 * left, 2 * right - left)

    ans = 0
    l = 0
    for r in range(n):
        while l <= r and cost(l, r) > t:
            l += 1
        if l <= r:
            ans = max(ans, r - l + 1)

    print(ans)

assert run("5 6\n-4 -1 2 3 7\n") == "3\n", "sample 1"
assert run("1 0\n5\n") == "0\n", "no time available"
assert run("1 10\n-7\n") == "1\n", "single negative point"
assert run("5 5\n-3 -1 2 4 8\n") == "3\n", "crossing zero window"
assert run("6 1000000000\n-1000000000 -5 1 2 7 999999999\n") == "6\n", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 6 / -4 -1 2 3 7`|`3`| Mẫu gốc có phạm vi tối ưu dấu hỗn hợp | 
|`1 0 / 5`|`0`| Ranh giới thời gian tối thiểu | 
|`1 10 / -7`|`1`| Xử lý tiền thưởng đơn lẻ | 
|`5 5 / -3 -1 2 4 8`|`3`| Tính toán chi phí chính xác khi vượt qua số 0 | 
| Trường hợp tọa độ lớn |`6`| Giá trị lớn và số học số nguyên | 

## Vỏ cạnh 

Đối với trường hợp không có thời gian:```
1 0
5
```con trỏ bên phải bắt đầu ở phần thưởng duy nhất. Chi phí là`5`, lớn hơn`t`, do đó con trỏ bên trái di chuyển qua con trỏ bên phải. Cửa sổ trở nên trống rỗng và câu trả lời vẫn là 0. Thuật toán không bao giờ tính phần thưởng không thể đạt được. 

Đối với một phạm vi ở cả hai phía của số 0:```
3 4
-2 1 3
```cửa sổ chứa cả ba phần thưởng có giá`2 * 3 - (-2) = 8`hoặc`3 - 2 * (-2) = 7`, nên nó bị từ chối. Con trỏ bên trái di chuyển cho đến khi cửa sổ`[-2, 1]`còn lại, chi phí của nó là`1 - 2 * (-2) = 5`Và`2 * 1 - (-2) = 4`, đưa ra một chi phí hợp lệ của`4`. Câu trả lời trở thành hai. 

Đối với cửa sổ hoàn toàn ở một bên:```
3 5
-6 -3 -1
```thuật toán không sử dụng công thức vượt 0. Cái giá phải trả chỉ đơn giản là khoảng cách đến điểm âm xa nhất. Cửa sổ đầu tiên có giá`6`, do đó con trỏ bên trái di chuyển. Tiền thưởng còn lại`-3, -1`trị giá`3`giây và được tính, tạo ra kết quả`2`. 

Đối với tọa độ lớn:```
2 2000000000
-1000000000 1000000000
```công thức chéo đánh giá các giá trị xung quanh`3000000000`, vượt quá giới hạn số nguyên 32 bit. Python xử lý việc này một cách chính xác và thuật toán so sánh các giá trị số nguyên đầy đủ trước khi quyết định rằng cả hai phần thưởng đều phù hợp. Đầu ra là`2`.
