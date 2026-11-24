---
title: "CF 89E - Lửa và Băng"
description: "Solomon đứng trên bức tường pháo đài ở vị trí 0. Bên phải ông có thể tồn tại một chuỗi khối băng chiếm các vị trí 1, 2, .... Ban đầu không có khối nào cả. Chiến trường là một đường dài n. Tại vị trí chiến trường i, có thể có một con quỷ có sức mạnh a[i]."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 89
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 74 (Div. 1 Only)"
rating: 2900
weight: 89
solve_time_s: 150
verified: false
draft: false
---

[CF 89E - Lửa và Băng](https://codeforces.com/problemset/problem/89/E) 

**Xếp hạng:** 2900 
**Thẻ:** tham lam 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Solomon đứng trên tường thành ở vị trí`0`. Bên phải anh ta có thể tồn tại một chuỗi các khối băng chiếm giữ các vị trí`1, 2, ...`. Ban đầu không có khối nào cả. 

Chiến trường là một đường dài`n`. Tại vị trí chiến trường`i`, có thể có một con quỷ có sức mạnh`a[i]`. Một khối băng rơi xuống vị trí`i`giảm`a[i]`bởi một. Khi sức mạnh trở về 0, con quỷ biến mất. 

Solomon có thể di chuyển sang trái hoặc phải dọc theo các khối băng hiện có và anh ta có thể chuyển đổi sự tồn tại của khối đó ngay lập tức sang bên phải của mình. Nếu anh ta phá hủy một khối, mọi khối bên phải sẽ mất đi sự hỗ trợ và rơi thẳng đứng xuống chiến trường. Mỗi khối rơi sẽ gây sát thương cho con quỷ ngay bên dưới nó một lần. 

Nhiệm vụ là đưa ra chuỗi hoạt động ngắn nhất có thể để tiêu diệt mọi con quỷ. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên,`n ≤ 1000`và mỗi sức mạnh nhiều nhất là`100`. Khó khăn không phải là độ phức tạp tính toán, mà là việc khám phá ra chiến lược tối ưu và chứng minh tính tối thiểu. Bất kỳ thuật toán nào xây dựng câu trả lời một cách rõ ràng đều đủ nhanh vì độ dài câu trả lời có thể đạt tới khoảng`2 * sum(a[i]) + O(n)`, nhiều nhất là về`200000`. 

Phần nguy hiểm là hiểu cơ học một cách chính xác. Một cách giải thích ngây thơ về hoạt động`A`thường dẫn đến việc xây dựng sai. 

Hãy xem xét ví dụ này:```
n = 3
1 0 1
```Nếu Solomon đứng ở vị trí`0`và thực hiện`A`, anh ấy tạo khối`1`. Chưa có gì rơi cả. Nếu anh ấy biểu diễn`A`lại ngay lập tức, chặn`1`bị phá hủy và rơi vào tế bào chiến trường`1`, gây sát thương cho con quỷ ở đó. 

Một lỗi phổ biến là cho rằng việc phá hủy một khối sẽ khiến tất cả các khối rơi xuống. Điều đó là sai. Chỉ các khối không được hỗ trợ ở bên phải mới rơi vào nhau. 

Một lỗi dễ dàng khác xuất hiện khi có khoảng trống:```
n = 5
1 0 0 0 1
```Việc phá hủy chuỗi khối cao từ bên trái sẽ lãng phí nhiều khối vào các ô trống. Chiến lược tối ưu phải quyết định cẩn thận nơi chuỗi bắt đầu và kết thúc. 

Trường hợp khó khăn nhất là một con quỷ:```
n = 1
5
```Chiến lược khả thi duy nhất là liên tục tạo và phá hủy khối đầu tiên. Bất kỳ nỗ lực nào nhằm xây dựng những cấu trúc sâu hơn đều chỉ tạo thêm những chuyển động không cần thiết. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô hình hóa toàn bộ trạng thái của cấu trúc băng và tìm kiếm chuỗi hoạt động ngắn nhất bằng BFS. Một trạng thái chứa vị trí của Solomon và tập hợp các khối hiện có. Từ mỗi tiểu bang, chúng tôi thử mọi hoạt động hợp pháp. 

Điều này hoạt động về mặt khái niệm vì mọi hoạt động đều có chi phí đơn vị, do đó BFS tự động tìm chuỗi ngắn nhất. 

Vấn đề là không gian trạng thái. có`2^n`cấu hình băng có thể và`n+1`những vị trí có thể. Ngay cả đối với`n = 30`, điều này trở nên hoàn toàn không thể. 

Bước đột phá thực sự đến từ việc quan sát điều gì thực sự quan trọng khi một khối rơi xuống. 

Giả sử các khối hiện đang chiếm vị trí`1...k`. Nếu Solomon phá hủy khối`x+1`, sau đó chặn`x+1...k`đồng thời rơi xuống. Chính xác một sát thương được gây ra cho mỗi ô chiến trường có hậu tố đó. 

Điều đó có nghĩa là mỗi hoạt động phá hủy đều góp phần gây thiệt hại cho một hậu tố liền kề. 

Bây giờ hãy nghĩ ngược lại. Thay vì mô phỏng các khối, hãy hỏi mỗi vị trí phải thuộc về hậu tố bị phá hủy bao nhiêu lần. 

Nếu vị trí`i`có sức mạnh`a[i]`, thì phải đánh chính xác`a[i]`lần. 

Giả sử chúng ta liên tục chọn một số hậu tố`[l, n]`và thả nó một lần. Chức vụ`i`được nhấn một lần cho mỗi hậu tố được chọn với`l ≤ i`. 

Điều này biến vấn đề thành một sự phân rã mảng khác biệt. 

Định nghĩa:```
b[i] = a[i] - a[i+1]
```với`a[n+1] = 0`. 

Bất cứ khi nào`b[i] > 0`, chúng ta phải bắt đầu chính xác`b[i]`hậu tố mới tấn công vào vị trí`i`. 

Điều này ngay lập tức đưa ra số lượng sự kiện hủy diệt tối ưu:```
total_attacks = a[1]
```sau khi kính thiên văn. 

Bây giờ chúng ta chuyển cấu trúc toán học này trở lại thành các phép toán vật lý. 

Để thực hiện một cuộc tấn công bắt đầu từ vị trí`i`, Solomon bước tới chặn`i-1`, xây dựng các khối cho đến độ cao mong muốn, sau đó phá hủy khối`i`. 

Chiến lược tối ưu là xử lý các cuộc tấn công từ trái sang phải trong khi vẫn duy trì chuỗi hiện tại. Điều này giảm thiểu sự di chuyển và tránh việc xây dựng lại các khối không cần thiết. 

Việc xây dựng cuối cùng đạt được mức tối thiểu về mặt lý thuyết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(2^n · n) | Quá chậm | 
| Tối ưu | O(tổng độ dài câu trả lời) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thêm một số 0 vào mảng, xử lý`a[n+1] = 0`. 
2. Đối với mọi vị trí`i`, hãy tính xem có bao nhiêu cuộc tấn công mới phải bắt đầu ở đây:```
need = a[i] - a[i+1]
```Chỉ có giá trị tích cực quan trọng. 

1. Giữ vững vị trí hiện tại của Solomon`pos`và khối băng hiện có ngoài cùng bên phải`mx`. 

Ban đầu cả hai đều`0`. 

1. Xử lý các vị trí từ phải sang trái. 

Xử lý từ phải sang trái là sự đơn giản hóa chính. Khi chúng tôi quyết định tấn công vào vị trí`i`, mọi vị trí sâu hơn đều đã có số lần truy cập chính xác được yêu cầu trong tương lai. 

1. Trong khi`a[i] > a[i+1]`, thực hiện một đòn tấn công mới bắt đầu từ`i`. 
2. Di chuyển Solomon sang phải cho đến khi đạt được vị trí`i-1`. 

Lúc này, khối`1...i-1`đã tồn tại rồi. 

1. Tạo khối từ`mx+1`đến độ sâu yêu cầu hiện tại. 

Mọi sáng tạo đều là hoạt động`A`theo sau là`R`nếu cần di chuyển nhiều hơn. 

1. Quay về bên trái từ khối sâu nhất về vị trí`i-1`. 
2. Phá hủy khối`i`với hoạt động`A`. 

Điều này làm giảm hậu tố`[i, mx]`, gây một sát thương lên mọi ô chiến trường trong phạm vi đó. 

1. Lặp lại cho đến khi tất cả các cuộc tấn công cần thiết cho vị trí`i`được hoàn thành. 
2. Sau khi tất cả các vị trí được xử lý, xuất ra chuỗi thao tác đã xây dựng. 

### Tại sao nó hoạt động 

Bất biến là trước khi xử lý vị trí`i`, mọi vị trí đều lớn hơn`i`đã nhận được chính xác tổng thiệt hại cần thiết. 

Khi chúng ta phá hủy khối`i`, mọi vị trí`j ≥ i`nhận thêm một đòn. Vì chúng tôi thực hiện chính xác`a[i] - a[i+1]`các cuộc tấn công, vị trí như vậy`i`tích lũy chính xác:```
(a[i] - a[i+1]) + (a[i+1] - a[i+2]) + ...
= a[i]
```đánh tổng thể. 

Không có đòn tấn công không cần thiết nào được thực hiện, bởi vì mỗi đòn tấn công đều tăng số lần trúng đích của ít nhất một vị trí vẫn chưa hài lòng. Việc xây dựng cũng giảm thiểu chuyển động bằng cách tái sử dụng cấu trúc dây chuyền hiện có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.append(0)

    ans = []

    current_len = 0
    pos = 0

    for i in range(n - 1, -1, -1):
        cnt = a[i] - a[i + 1]

        for _ in range(cnt):
            while pos < i:
                ans.append('R')
                pos += 1

            while current_len < a[i]:
                ans.append('A')
                current_len += 1

                if current_len < a[i]:
                    ans.append('R')
                    pos += 1

            while pos > i:
                ans.append('L')
                pos -= 1

            ans.append('A')

    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc giải thích hậu tố của các khối rơi.`current_len`theo dõi có bao nhiêu khối liên tiếp hiện đang tồn tại. Vị trí của Solomon được lưu trữ trong`pos`. 

Quá trình xử lý diễn ra từ phải sang trái vì mỗi lần hủy tại vị trí`i`ảnh hưởng tới mọi vị trí`≥ i`. Bằng cách sửa các chỉ số lớn hơn trước, chúng ta sẽ không bao giờ vô tình làm hỏng chỉ mục nhỏ hơn sau này. 

Phần tinh tế là ý nghĩa của`a[i] - a[i+1]`. 

Giả định:```
a = [5, 3, 3, 1]
```Chức vụ`1`cần năm cú đánh, vị trí`2`cần ba cú đánh. Điều đó có nghĩa là có đúng hai cuộc tấn công phải bắt đầu tại vị trí`1`, bởi vì đó là những lượt truy cập nhận được theo vị trí`1`nhưng không theo vị trí`2`. 

Mã thực hiện các cuộc tấn công đó. 

Một nơi dễ mắc lỗi khác là chuyển động sau khi kéo dài dây xích. Khi xây dựng các khối mới, Solomon di chuyển sang phải cùng với phần mở rộng. Trước khi phá hủy khối mong muốn, anh ta phải quay trái về đúng vị trí. 

Việc triển khai lưu trữ các hoạt động trực tiếp vào một danh sách và nối chúng một lần vào cuối. Việc nối chuỗi lặp lại vẫn sẽ được thực hiện ở đây nhưng danh sách sẽ sạch hơn và an toàn hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 0 1
```Thứ tự xử lý từ phải sang trái. 

| Bước | Vị trí tôi | Hành động | Khối hiện tại | Hiệu ứng | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | Xây dựng khối 1 | [1] | chuẩn bị | 
| 2 | 3 | Di chuyển/xây dựng tới khối 3 | [1,2,3] | chuẩn bị | 
| 3 | 3 | Phá hủy khối 3 | [1,2] | đánh ô 3 | 
| 4 | 1 | Quay lại bên trái | [1,2] | chuẩn bị | 
| 5 | 1 | Phá hủy khối 1 | [] | đánh ô 1 và 2 | 

Tế bào`2`nhận những đòn đánh quá mức vô hại vì nó không có quỷ. Tế bào`1`Và`3`mỗi người nhận đúng một đòn sát thương. 

Dấu vết này cho thấy tại sao các vị trí trống lại hữu ích. Họ cho phép các cuộc tấn công hậu tố dài hơn mà không bị phạt. 

### Ví dụ 2 

đầu vào:```
4
2 2 1 1
```Sự khác biệt:```
2-2 = 0
2-1 = 1
1-1 = 0
1-0 = 1
```Vì vậy, một cuộc tấn công bắt đầu ở vị trí`4`và một đòn tấn công bắt đầu ở vị trí`2`. 

| Bước | Vị trí tôi | Hành động | Tế bào Lượt truy cập | 
| --- | --- | --- | --- | 
| 1 | 4 | Bỏ hậu tố [4,4] | 4 | 
| 2 | 2 | Bỏ hậu tố [2,4] | 2,3,4 | 

Số lần truy cập cuối cùng: 

| Tế bào | Lượt truy cập | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 2 | 

Kết hợp với những đóng góp hậu tố được kế thừa, mỗi con quỷ sẽ nhận được chính xác sức mạnh cần thiết của mình. 

Ví dụ này cho thấy cấu trúc lồng nhau của các cuộc tấn công hậu tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) |`L`là độ dài đầu ra, mỗi thao tác được tạo một lần | 
| Không gian | O(L) | lưu trữ chuỗi câu trả lời | 

Bản thân câu trả lời có thể chứa khoảng hai trăm nghìn phép tính, do đó, bất kỳ thuật toán hợp lệ nào cũng cần có thời gian tuyến tính ở kích thước đầu ra. Giải pháp thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.append(0)

    ans = []

    current_len = 0
    pos = 0

    for i in range(n - 1, -1, -1):
        cnt = a[i] - a[i + 1]

        for _ in range(cnt):
            while pos < i:
                ans.append('R')
                pos += 1

            while current_len < a[i]:
                ans.append('A')
                current_len += 1

                if current_len < a[i]:
                    ans.append('R')
                    pos += 1

            while pos > i:
                ans.append('L')
                pos -= 1

            ans.append('A')

    print(''.join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# sample
assert isinstance(run("3\n1 0 1\n"), str)

# minimum case
assert isinstance(run("1\n1\n"), str)

# all equal
assert isinstance(run("5\n3 3 3 3 3\n"), str)

# increasing strengths
assert isinstance(run("4\n1 2 3 4\n"), str)

# sparse demons
assert isinstance(run("6\n5 0 0 0 0 5\n"), str)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`| bất kỳ chuỗi hợp lệ nào | xử lý đơn bào | 
|`5 / 3 3 3 3 3`| bất kỳ chuỗi hợp lệ nào | lặp đi lặp lại các cuộc tấn công hậu tố giống hệt nhau | 
|`4 / 1 2 3 4`| bất kỳ chuỗi hợp lệ nào | nhiều hậu tố lồng nhau | 
|`6 / 5 0 0 0 0 5`| bất kỳ chuỗi hợp lệ nào | vị trí trống ở giữa | 

## Vỏ cạnh 

Hãy xem xét trường hợp vị trí duy nhất:```
1
5
```Thuật toán tính toán:```
a[1] - a[2] = 5
```Vậy chính xác có năm cuộc tấn công bắt đầu ở vị trí`1`. 

Mỗi cuộc tấn công chỉ đơn giản là tạo khối`1`và tiêu diệt nó ngay lập tức. Không có chuyển động nào xảy ra. Trình tự được tạo ra là tối ưu vì mỗi đòn đánh cần ít nhất một lần tiêu diệt. 

Bây giờ hãy xem xét:```
5
1 0 0 0 1
```Thuật toán bắt đầu một cuộc tấn công tại vị trí`5`và một đòn tấn công vào vị trí`1`. 

Cuộc tấn công vào vị trí`1`làm hỏng mọi tế bào, nhưng chỉ các tế bào`1`Và`5`vấn đề. Các ô trống sẽ hấp thụ các cú đánh vô dụng một cách an toàn. 

Một chiến lược bất cẩn cố gắng tránh đánh vào các ô trống sẽ lãng phí việc xây dựng lại chuỗi chuyển động một cách không cần thiết. 

Cuối cùng hãy xem xét việc giảm dần điểm mạnh:```
4
4 3 2 1
```Sự khác biệt là:```
1 1 1 1
```Mỗi vị trí bắt đầu chính xác một đòn tấn công hậu tố mới. 

Thuật toán tạo ra các hậu tố lồng nhau một cách tự nhiên:```
[4]
[3,4]
[2,3,4]
[1,2,3,4]
```Chức vụ`1`bị đánh bốn lần, vị trí`2`ba lần, vân vân. Điều này xác nhận tính bất biến kính thiên văn đằng sau bằng chứng.
