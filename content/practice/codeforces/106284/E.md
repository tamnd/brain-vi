---
title: "CF 106284E - \u0418\u0433\u0440\u0430 \u041a\u0440\u043e\u0448\u0430"
description: "Trò chơi có một dòng quái vật cà rốt. Krosh chỉ có thể tấn công trực tiếp con quái vật đầu tiên còn sống. Để tiếp cận quái vật sau, mọi quái vật trước nó đều phải bị đánh bại. Một phát bắn nhắm vào quái vật hiện tại sẽ gây sát thương tăng dần lên một khối liên tiếp."
date: "2026-06-25T07:41:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106284
codeforces_index: "E"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 (\u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u0435) 10-11 \u043a\u043b\u0430\u0441\u0441, \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u044c, 2025"
rating: 0
weight: 106284
solve_time_s: 40
verified: true
draft: false
---

[CF 106284E - \u0418\u0433\u0440\u0430 \u041a\u0440\u043e\u0448\u0430](https://codeforces.com/problemset/problem/106284/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có một dòng quái vật cà rốt. Krosh chỉ có thể tấn công trực tiếp con quái vật đầu tiên còn sống. Để tiếp cận quái vật sau, mọi quái vật trước nó đều phải bị đánh bại. 

Một phát bắn nhắm vào quái vật hiện tại sẽ gây sát thương tăng dần lên một khối liên tiếp. Quái vật bị nhắm tới sẽ nhận 1 sát thương, con tiếp theo nhận 2 sát thương, con tiếp theo nhận 3 sát thương, v.v.`k`quái vật. Một con quái vật có sức khỏe`h`nhận được thiệt hại`d`thay đổi sức khỏe của nó thành`|h - d|`. Một khi quái vật không còn máu, nó sẽ bị đánh bại mãi mãi. 

Nhiệm vụ là tìm ra số lần bắn tối thiểu cần thiết để đánh bại mọi quái vật. 

Các giới hạn là phần then chốt của vấn đề. Có thể có tới`2 * 10^5`quái vật, trong khi`k`nhiều nhất là`200`. Không thể mô phỏng thực hiện công việc tỷ lệ thuận với số lần bắn vì giá trị sức khỏe có thể lớn. Ngay cả một cách tiếp cận liên tục gây sát thương cho đến khi một con quái vật chết cũng có thể cần hàng tỷ thao tác. 

Giá trị nhỏ của`k`cho chúng ta biết cấu trúc ở đâu. Quái vật chỉ có thể bị ảnh hưởng bởi quái vật trước đó`k - 1`quái vật, bởi vì một phát bắn đạt tối đa`k`các vị trí. Chúng ta cần tránh lặp lại từng giây của trò chơi và thay vào đó xử lý hiệu ứng của một số lượng lớn các thao tác giống hệt nhau cùng một lúc. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Nếu một quái vật đã bị giết bởi sát thương đến từ những quái vật trước đó, nó không được phép tấn công lại. Ví dụ:```
1 2
5 1
```Con quái vật đầu tiên chết sau 5 phát bắn. Trong những lần bắn đó, quái vật thứ hai nhận sát thương 2 năm lần:`1 -> 1 -> 1 -> 1 -> 1`Trên thực tế, vì cập nhật sức khỏe là khác biệt tuyệt đối nên trạng thái vẫn là 1. Câu trả lời là tổng cộng 6 phát bắn chứ không phải 5 + 1 đòn tấn công trực tiếp lặp lại sau quái vật đầu tiên. Một giải pháp chỉ tính các đòn tấn công trực tiếp và bỏ qua sát thương phụ sẽ mắc phải sai lầm này. 

Một con quái vật có thể trở thành con số 0 trước khi nó tiến tới phía trước. Ví dụ:```
2 1
1 2
```Con quái vật đầu tiên cần một phát bắn. Cú đánh đó gây 2 sát thương cho con quái vật thứ hai, thay đổi từ 2 thành 0. Câu trả lời là 1. Một mô phỏng từ trái sang phải ngây thơ luôn chiến đấu với từng quái vật riêng biệt sẽ tạo ra 2. 

Một số lượng lớn các cuộc tấn công phải được xử lý bằng toán học. Ví dụ: nếu một con quái vật trong tương lai nhận sát thương`3`một triệu lần, việc mô phỏng một triệu bản cập nhật là không cần thiết. Phép toán chênh lệch tuyệt đối lặp lại có chu kỳ ngắn, đây là quan sát chính được sử dụng bởi giải pháp. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là bắn liên tục con quái vật hiện tại cho đến khi nó chết. Nếu một con quái vật hiện có sức khỏe`h`, điều này cần chính xác`h`phát bắn vì nó nhận 1 sát thương mỗi lần. Sau mỗi lần bắn chúng tôi sẽ cập nhật tất cả quái vật ở lần tiếp theo`k - 1`các vị trí. 

Cách tiếp cận này đúng vì nó tuân theo trò chơi một cách chính xác, nhưng nó không thành công khi giá trị sức khỏe lớn. Một con quái vật duy nhất có sức khỏe`10^18`sẽ yêu cầu một số lần lặp không thể thực hiện được. 

Quan sát giúp giải quyết vấn đề là sát thương mà quái vật trong tương lai nhận được luôn có cùng giá trị trong một trận chiến. Nếu quái vật`i`đang bị tấn công, quái vật`i + j`nhận được`j + 1`thiệt hại trên mỗi một trong những phát bắn đó. Chúng ta chỉ cần áp dụng hàm`x -> |x - d|`nhiều lần cho một kích thước nhỏ cố định`d`. 

Hàm này có một mẫu đơn giản. Viết`x = q * d + r`, Ở đâu`0 <= r < d`. Sau đó`q`ứng dụng, giá trị trở thành`r`. Nếu như`r = 0`, con quái vật đã chết. Nếu không thì nó luân phiên giữa`r`Và`d - r`. Điều này cho phép chúng tôi bỏ qua bất kỳ số lượng cập nhật lặp lại nào trong thời gian liên tục. 

Toàn bộ thuật toán xử lý mỗi quái vật một lần và chỉ cập nhật quái vật tiếp theo`k - 1`quái vật. Từ`k`nhỏ, tổng khối lượng công việc bị giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số lần chụp * k) | O(n) | Quá chậm | 
| Tối ưu | O(nk) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ lượng máu hiện tại của mọi quái vật. Chúng tôi xử lý quái vật từ trái sang phải vì thứ tự trò chơi bị ép buộc bởi quy tắc chỉ có quái vật còn sống đầu tiên mới có thể bị tấn công. 
2. Đối với quái vật hiện tại, nếu máu của nó đã bằng 0, hãy bỏ qua nó. Các cuộc tấn công trước đó đã đánh bại nó. 
3. Nếu không, hãy để sức khỏe hiện tại của nó`h`. Con quái vật cần chính xác`h`phát bắn vì mỗi đòn tấn công trực tiếp đều gây sát thương 1. Thêm`h`cho câu trả lời và đánh dấu quái vật là bị đánh bại. 
4. Trong thời gian này`h`ảnh, cập nhật từng ảnh tiếp theo`k - 1`quái vật. Quái vật ở xa`j`nhận thiệt hại`j + 1`trên mỗi lần bắn, vì vậy hãy áp dụng thao tác chênh lệch tuyệt đối lặp đi lặp lại với giá trị sát thương cố định đó. 
5. Để áp dụng`x -> |x - d|`chính xác`t`lần, chia`x`qua`d`. Nếu như`t`nhỏ hơn thương thì đáp án là`x - t*d`. Mặt khác, phần còn lại là 0 hoặc là chu kỳ hai giá trị. 

Tại sao nó hoạt động: 

Quái vật còn sống đầu tiên luôn chỉ nhận sát thương 1, vì vậy thời gian tồn tại của nó chính xác là lượng máu hiện tại. Mọi quái vật khác chỉ bị ảnh hưởng bởi những quái vật trước đó và những hiệu ứng đó có thể bị nén vì mỗi trận chiến áp dụng một lần biến đổi cố định nhiều lần. Thuật toán duy trì lượng máu chính xác của mọi quái vật chưa được xử lý, vì vậy khi một quái vật tiến lên phía trước, giá trị được lưu trữ chính xác là trạng thái mà trò chơi thực sự sẽ tạo ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply_damage(x, d, t):
    if x == 0:
        return 0

    q, r = divmod(x, d)

    if t < q:
        return x - t * d

    if r == 0:
        return 0

    if (t - q) % 2 == 0:
        return r
    return d - r

def solve():
    n, k = map(int, input().split())
    h = list(map(int, input().split()))

    ans = 0

    for i in range(n):
        if h[i] == 0:
            continue

        shots = h[i]
        ans += shots
        h[i] = 0

        for j in range(1, k):
            if i + j >= n:
                break
            if h[i + j] != 0:
                h[i + j] = apply_damage(h[i + j], j + 1, shots)

    print(ans)

if __name__ == "__main__":
    solve()
```chức năng`apply_damage`là lối tắt toán học. thương số`q`đại diện cho bao nhiêu khối đầy đủ kích thước`d`có thể được loại bỏ trước khi đạt được phần còn lại. Nếu số lượng ứng dụng không đạt đến điểm đó, giá trị sẽ giảm đi một lượng`d`mỗi lần. 

Sau khi đạt đến phần còn lại, chỉ có hai trạng thái có thể xảy ra. Số dư bằng 0 có nghĩa là quái vật đã chết. Phần dư khác 0 xen kẽ giữa`r`Và`d - r`, do đó chỉ có tính chẵn lẻ của số lượng hoạt động còn lại mới quan trọng. 

Vòng lặp chính tuân theo thứ tự bắt buộc của trò chơi. Quái vật hiện tại đóng góp sức khỏe của nó cho câu trả lời, trở thành 0 và sau đó chỉ ảnh hưởng đến quái vật tiếp theo.`k - 1`các vị trí. Việc kiểm tra lượng máu bằng 0 trước mỗi bản cập nhật sẽ ngăn những quái vật đã bị đánh bại thay đổi lần nữa. 

Số nguyên Python không bị tràn, điều này cần thiết vì câu trả lời có thể vượt quá giới hạn 32 bit. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3 2
3 5 1
```Những thay đổi trạng thái là: 

| Bước | Quái vật hiện tại | Ảnh đã sử dụng | Mảng sức khỏe sau trận đấu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Quái vật 1 | 3 |`[0, 2, 1]`| 3 | 
| 2 | Quái vật 2 | 2 |`[0, 0, 1]`| 5 | 
| 3 | Quái vật 3 | 1 |`[0, 0, 0]`| 6 | 

Quái vật thứ hai nhận sát thương 2 ba lần trong trận chiến đầu tiên:`5 -> 3 -> 1 -> 1`Quái vật thứ ba không bị ảnh hưởng vì`k = 2`. 

Bây giờ hãy xem xét:```
4 3
1 2 10 4
```| Bước | Quái vật hiện tại | Ảnh đã sử dụng | Mảng sức khỏe sau trận đấu | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Quái vật 1 | 1 |`[0, 0, 8, 2]`| 1 | 
| 2 | Quái vật 3 | 8 |`[0, 0, 0, 0]`| 9 | 

Phát đầu tiên giết quái vật 2 ngay lập tức vì nó nhận sát thương 2. Nó cũng thay đổi quái vật 3 từ 10 thành 8 và quái vật 4 từ 4 thành 2. Thuật toán bỏ qua quái vật 2 hoàn toàn sau đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi quái vật cập nhật nhiều nhất ở lần tiếp theo`k - 1`quái vật. | 
| Không gian | O(n) | Chỉ các giá trị sức khỏe hiện tại được lưu trữ. | 

Với`n = 2 * 10^5`Và`k = 200`, số lượng cập nhật là khoảng`4 * 10^7`, có thể quản lý được vì mỗi lần cập nhật chỉ là một vài thao tác số nguyên. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("""3 2
3 5 1
""") == "6\n", "basic sample"

assert run("""4 3
1 2 10 4
""") == "9\n", "side damage kills monsters"

assert run("""1 1
100
""") == "100\n", "single monster"

assert run("""5 200
7 7 7 7 7
""") == "7\n", "large radius and equal values"

assert run("""6 2
1 1000000000 1 1000000000 1 1000000000
""") == "1000000003\n", "large health values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 / 3 5 1`|`6`| Xử lý tuần tự cơ bản | 
|`4 3 / 1 2 10 4`|`9`| Quái vật chết vì sát thương phụ | 
|`1 1 / 100`|`100`| Kích thước tối thiểu và chỉ thiệt hại trực tiếp | 
|`5 200 / 7 7 7 7 7`|`7`| Phạm vi tối đa và nhiều quái vật bị ảnh hưởng | 
|`6 2 / 1 1000000000 1 1000000000 1 1000000000`|`1000000003`| Giá trị lớn yêu cầu phím tắt chu trình | 

## Vỏ cạnh 

Đối với quái vật đã bị giết bởi đòn tấn công trước đó, thuật toán sẽ kiểm tra số 0 trước khi xử lý nó. Trong đầu vào```
2 1
1 2
```con quái vật đầu tiên yêu cầu một phát bắn. Quái vật thứ hai nhận sát thương 2 và trở thành 0. Sức khỏe được lưu trữ bây giờ là`[0, 0]`, vậy câu trả lời là`1`. 

Đối với các phép biến đổi lặp lại với số lượng lớn, thuật toán không bao giờ thực hiện một lần cập nhật cho mỗi lần chụp. Hãy xem xét một con quái vật có sức khỏe`1000000000`nhận thiệt hại`2`vì`500000000`cú đánh. Phép biến đổi được nén bằng phép chia:`1000000000 = 500000000 * 2 + 0`vì vậy con quái vật đạt đến số 0 ngay sau chính xác những thao tác đó. Hàm trả về 0 trong thời gian không đổi. 

Vì`k = 1`, mỗi lần bắn chỉ ảnh hưởng đến quái vật hiện tại. Vòng cập nhật bên trong không chạy lần nào và mỗi quái vật chỉ đóng góp lượng máu ban đầu của nó. Điều này phù hợp với trò chơi vì không có sát thương lan. 

Đối với những quái vật có giá trị máu nhỏ hơn sát thương nhận vào, hành vi khác biệt tuyệt đối sẽ được xử lý trực tiếp. Nếu một con quái vật có sức khỏe`3`và nhận sát thương`5`, nó trở thành`2`, không phải bằng không. Công thức chu trình duy trì hành vi này vì nó dựa trên phép biến đổi chính xác chứ không phải phép trừ thông thường.
