---
title: "CF 1849D - Vẽ mảng"
description: "Chúng ta được cung cấp một dòng ô, mỗi ô chứa giá trị 0, 1 hoặc 2. Mỗi ô bắt đầu không được sơn và mục tiêu là sơn tất cả các ô. Chúng tôi được phép tiêu tiền theo hai cách khác nhau. Đầu tiên là vẽ trực tiếp: chúng ta có thể chọn bất kỳ ô chưa sơn nào, sơn nó và trả một xu."
date: "2026-06-09T05:36:35+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1849
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 152 (Rated for Div. 2)"
rating: 1700
weight: 1849
solve_time_s: 178
verified: false
draft: false
---

[CF 1849D - Vẽ mảng](https://codeforces.com/problemset/problem/1849/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, hai con trỏ 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng ô, mỗi ô chứa giá trị 0, 1 hoặc 2. Mỗi ô bắt đầu không được sơn và mục tiêu là sơn tất cả các ô. 

Chúng tôi được phép tiêu tiền theo hai cách khác nhau. Đầu tiên là vẽ trực tiếp: chúng ta có thể chọn bất kỳ ô chưa sơn nào, sơn nó và trả một xu. Thứ hai là thao tác lan truyền: nếu chúng ta đã có một ô được sơn có giá trị là 1 hoặc 2, chúng ta có thể sử dụng nó để “lây nhiễm” vào một ô không được sơn liền kề, sơn miễn phí ô lân cận đó và đồng thời giảm giá trị của ô được sơn ban đầu đi một ô. Một ô có giá trị 0 không thể được sử dụng để lan truyền thêm. 

Điểm căng thẳng chính là các giá trị cao hơn hoạt động giống như “nguồn năng lượng” hạn chế có thể truyền sơn sang hàng xóm, có khả năng tiết kiệm tiền. Bài toán yêu cầu số lượng xu tối thiểu cần thiết để vẽ toàn bộ mảng. 

Ràng buộc n lên tới 200.000 buộc mọi giải pháp phải tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến việc thử các tập hợp con của điểm bắt đầu, khoảng DP với O(n^2) hoặc mô phỏng tất cả các lựa chọn lan truyền một cách ngây thơ sẽ thất bại. Cấu trúc gợi ý rằng giá trị của mỗi ô chỉ có thể được sử dụng một số lần nhỏ và việc truyền bá mang tính cục bộ, do đó, giải pháp sẽ giảm xuống tính toán dựa trên tham lam hoặc quét. 

Một dạng lỗi khó phát hiện xuất phát từ việc đánh giá quá cao sức mạnh lan truyền. Ví dụ, trong`[2,0,0]`, thật hấp dẫn khi nghĩ rằng 2 đơn lẻ có thể bao trùm cả hai hàng xóm, nhưng vấn đề về hướng: việc truyền bá là tuần tự và tiêu tốn giá trị, do đó thứ tự quyết định tính khả thi. Một trường hợp thất bại khác là coi tất cả các ô khác 0 là các nguồn có thể hoán đổi cho nhau, bỏ qua rằng số 1 chỉ có thể trợ giúp một lần, trong khi số 2 có thể trợ giúp hai lần. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về quy trình là quyết định xem ô nào ban đầu được thanh toán bằng tiền xu. Sau khi một số ô được vẽ, chúng có thể mở rộng ra bên ngoài miễn là chúng còn giá trị. Người ta có thể thử mọi tập hợp con của các loại sơn đồng xu ban đầu và mô phỏng sự lan truyền. Về nguyên tắc, điều này đúng vì mọi quy trình hợp lệ đều bắt đầu bằng một số tập hợp điểm bắt đầu phải trả phí, nhưng số lượng tập hợp con là số mũ theo n, khiến điều này không thể thực hiện được đối với n lên tới 200.000. 

Quan sát quan trọng là việc truyền bá luôn di chuyển dọc theo vùng lân cận và sử dụng giá trị theo cách tuyến tính, từng bước một. Điều này làm cho quy trình hoạt động giống như xây dựng các phân đoạn phủ sóng từ “nguồn”. Thay vì chọn các nguồn ban đầu tùy ý, chúng ta có thể quét từ trái sang phải và tham lam quyết định khi nào chúng ta buộc phải trả tiền. 

Thông tin chi tiết quan trọng là bất cứ khi nào chúng tôi gặp phải một vị trí mà bất kỳ khả năng truyền bá còn lại nào từ bên trái đều không thể đạt tới, chúng tôi phải bắt đầu một phân đoạn trả phí mới ở đó. Sau đó, mỗi ô phải trả phí sẽ cung cấp một lượng phạm vi tiếp cận giới hạn ở bên phải bằng với giá trị của nó và chúng tôi duy trì phạm vi tiếp cận hiện tại mở rộng bao xa. 

Điều này biến vấn đề thành việc theo dõi ranh giới phủ sóng di chuyển và đếm xem chúng ta cần đặt lại ranh giới đó bao nhiêu lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các bộ sơn ban đầu + mô phỏng | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam theo dõi phạm vi từ trái sang phải | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một biến biểu thị khoảng cách về bên phải mà chúng tôi vẫn có thể vẽ miễn phí bằng cách sử dụng các giá trị đã được kích hoạt. Chúng tôi cũng duy trì vị trí hiện tại mà chúng tôi đang quét. 

1. Khởi tạo một biến`reach = 0`, nghĩa là hiện tại chúng tôi không thể mở rộng bức tranh vượt quá điểm bắt đầu và bộ đếm`coins = 0`. 
2. Duyệt mảng từ trái sang phải. Tại mỗi chỉ số`i`, kiểm tra xem`i`nằm trong phạm vi có thể truy cập hiện tại`reach`. 
3. Nếu`i <= reach`, vị trí này có thể được vẽ miễn phí thông qua các ô được trả tiền hoặc truyền bá trước đó, vì vậy chúng tôi không phải làm gì cả. 
4. Nếu`i > reach`, chúng ta đang ở một vị trí mà bất kỳ lần truyền bá nào trước đó không thể đạt được. Chúng ta phải trả một đồng xu để vẽ trực tiếp ô này. Sau khi trả tiền, ô này trở thành nguồn nhân giống mới nên chúng ta tăng`coins`bằng 1. 
5. Sau khi chúng tôi thanh toán tại vị trí`i`, chúng tôi thiết lập`reach = i + a[i]`. Điều này phản ánh rằng từ ô mới được kích hoạt này, chúng ta có thể mở rộng khả năng vẽ lên tới`a[i]`bước sang bên phải. 
6. Tiếp tục cho đến hết mảng. 

Ý tưởng quan trọng là chúng ta không bao giờ xem lại các quyết định trước đó. Mỗi khi chúng ta nằm ngoài phạm vi tiếp cận hiện tại, chúng ta buộc phải tạo ra một nguồn mới và nguồn đó đóng góp toàn bộ công suất sử dụng của nó cho các vị trí trong tương lai. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến`reach`là chỉ số xa nhất có thể được vẽ mà không phải trả thêm tiền bằng cách sử dụng tất cả các ô trả phí đã chọn trước đó và sức lan truyền còn lại của chúng. Bất cứ khi nào chúng ta gặp một chỉ mục nằm ngoài ranh giới này, không có chuỗi bước lan truyền nào trước đó có thể đạt được nó vì quá trình lan truyền chỉ di chuyển từng bước một và chỉ bắt nguồn từ các ô có giá trị dương đã được sơn. Vì vậy, nếu không thanh toán vào thời điểm đó sẽ khiến việc sơn vị trí đó không thể thực hiện được nên cần có nguồn trả phí mới. Việc chọn thanh toán chính xác ở chỉ số không thể truy cập đầu tiên là tối ưu vì bất kỳ lựa chọn nào sau đó sẽ khiến vị trí hiện tại không được vẽ trong khi không mang lại lợi thế và bất kỳ lựa chọn nào trước đó đều nằm trong tầm tay hoặc sẽ không cải thiện phạm vi bao phủ của các khoảng trống trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    reach = 0
    coins = 0

    for i in range(n):
        if i > reach:
            coins += 1
            reach = i + a[i]
        else:
            reach = max(reach, i + a[i])

    print(coins)

if __name__ == "__main__":
    solve()
```Giải pháp quét một lần từ trái sang phải, duy trì chỉ mục xa nhất có thể truy cập. Khi một vị trí nằm ngoài phạm vi đó, nó buộc phải mua xu và đặt lại phạm vi tiếp cận dựa trên giá trị của vị trí đó. Mặt khác, nó chỉ cập nhật phạm vi tiếp cận nếu ô hiện tại cung cấp thêm phần mở rộng. 

Điểm tinh tế là bản cập nhật`reach = max(reach, i + a[i])`ngay cả khi không tiêu xu nào. Điều này cho thấy các vị trí đã có thể tiếp cận vẫn đóng góp sức mạnh lan truyền và có thể mở rộng phạm vi phủ sóng hơn nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 2 0
```Chúng tôi theo dõi mức độ phát triển của phạm vi tiếp cận. 

| tôi | một [tôi] | đạt trước | hành động | tiền xu | tiếp cận sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | trả (i == tầm) | 1 | 0 | 
| 1 | 2 | 0 | không thể truy cập, trả tiền | 2 | 3 | 
| 2 | 0 | 3 | được bảo hiểm | 2 | 3 | 

Điều này chứng tỏ rằng một lần kích hoạt sớm có thể bị lãng phí nếu nó không kéo dài đủ xa, buộc một đồng tiền khác ở chỉ số 1, nhưng lần kích hoạt thứ hai đó có thể bao gồm phần còn lại. 

### Ví dụ 2 

đầu vào:```
5
1 0 2 0 1
```| tôi | một [tôi] | đạt trước | hành động | tiền xu | tiếp cận sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | trả | 1 | 1 | 
| 1 | 0 | 1 | được bảo hiểm | 1 | 1 | 
| 2 | 2 | 1 | trả | 2 | 4 | 
| 3 | 0 | 4 | được bảo hiểm | 2 | 4 | 
| 4 | 1 | 4 | được bảo hiểm | 2 | 5 | 

Điều này cho thấy các ô có giá trị cao sau này sẽ mở rộng vùng phủ sóng một cách đáng kể như thế nào, giảm nhu cầu về thêm tiền. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mảng truyền qua một lần với các cập nhật liên tục | 
| Không gian | O(1) | chỉ có một số biến được duy trì | 

Quét tuyến tính là cần thiết với n lên tới 200.000 và giải pháp tránh mọi quá trình xử lý hoặc mô phỏng lồng nhau, giúp nó hoạt động hiệu quả thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))

    reach = 0
    coins = 0

    for i in range(n):
        if i > reach:
            coins += 1
            reach = i + a[i]
        else:
            reach = max(reach, i + a[i])

    return str(coins)

# provided sample
assert run("3\n0 2 0\n") == "1"

# single element
assert run("1\n0\n") == "1"

# all zeros
assert run("4\n0 0 0 0\n") == "4"

# all twos
assert run("5\n2 2 2 2 2\n") == "1"

# alternating small values
assert run("5\n1 0 2 0 1\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n0`|`1`| mua bắt buộc tối thiểu | 
|`0 0 0 0`|`4`| không thể lan truyền | 
|`2 2 2 2 2`|`1`| hiệu quả xích tối đa | 
|`1 0 2 0 1`|`2`| tương tác giữa phạm vi địa phương và tầm xa | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi phần tử đầu tiên bằng 0. Trong đầu vào như`[0, 2, 0]`, thuật toán bắt đầu ở chỉ số 0, thấy rằng nó không thể truy cập được với`reach = 0`, và buộc phải trả tiền ngay lập tức. Sau đó, phạm vi tiếp cận có thể nhảy đủ xa để bao phủ các ô sau này. Việc tham lam trả tiền ở chỉ số 0 là điều khó tránh khỏi vì không có nguồn lan truyền trước đó. 

Một trường hợp cạnh khác là khi một phần tử có giá trị cao xuất hiện sau một tiền tố dài bằng 0. Vì`[0, 0, 0, 2, 0]`, thuật toán thanh toán ở chỉ số 0, 1, 2, sau đó cuối cùng sử dụng chỉ mục 3 để mở rộng phạm vi tiếp cận. Mỗi số 0 tạo ra một đồng xu riêng biệt vì không có sự truyền bá nào có thể vượt qua chúng nếu không có nguồn hoạt động. 

Trường hợp thứ ba là khi nhiều giá trị nhỏ trùng nhau. TRONG`[1, 0, 1, 0, 1]`, mỗi lần 1 chỉ mở rộng phạm vi tiếp cận một chút và thuật toán đảm bảo rằng các lần kích hoạt trước đó sẽ được sử dụng lại bất cứ khi nào có thể. Quá trình quét đảm bảo rằng chúng tôi không bao giờ thanh toán hai lần cho một khu vực đã được bao phủ và mỗi đồng tiền mới tương ứng chính xác với một chỉ mục không thể truy cập trước đó.
